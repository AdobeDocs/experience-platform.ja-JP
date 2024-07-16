---
keywords: Experience Platform；ホーム；人気のトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；スキーマレジストリ；スキーマレジストリ；スキーマ；スキーマ；スキーマ；関係；関係記述子；関係記述子；参照 ID;
title: Schema Registry API を使用した 2 つのスキーマ間の関係の定義
description: このドキュメントでは、Schema Registry API を使用して組織で定義された 2 つのスキーマ間に 1 対 1 の関係を定義するためのチュートリアルを提供します。
type: Tutorial
exl-id: ef9910b5-2777-4d8b-a6fe-aee51d809ad5
source-git-commit: 7021725e011a1e1d95195c6c7318ecb5afe05ac6
workflow-type: tm+mt
source-wordcount: '1379'
ht-degree: 27%

---

# [!DNL Schema Registry] API を使用した 2 つのスキーマ間の関係の定義

様々なチャネルでの顧客とブランドとの関係を理解する能力は、Adobe Experience Platform の重要な部分です。[!DNL Experience Data Model] （XDM）スキーマの構造でこれらの関係を定義すると、顧客データに対する複雑なインサイトを得ることができます。

スキーマの関係は、結合スキーマと [!DNL Real-Time Customer Profile] を使用して推論できますが、同じクラスを共有するスキーマにのみ適用されます。 異なるクラスに属する 2 つのスキーマ間の関係を確立するには、専用の関係フィールドを **ソーススキーマ** に追加する必要があります。これは、別の **参照スキーマ** の ID を示します。

>[!NOTE]
>
>Schema Registry API は、参照スキーマを「宛先スキーマ」と呼びます。 これらは、[ データ準備マッピングセット ](../../data-prep/mapping-set.md) の宛先スキーマや [ 宛先接続 ](../../destinations/home.md) のスキーマと混同しないでください。

このドキュメントでは、[[!DNL Schema Registry API]](https://www.adobe.io/experience-platform-apis/references/schema-registry/) を使用して組織で定義された 2 つのスキーマ間に 1 対 1 の関係を定義するチュートリアルを提供します。

## はじめに

このチュートリアルでは、[!DNL Experience Data Model] （XDM）および [!DNL XDM System] について実際に理解している必要があります。 このチュートリアルを始める前に、次のドキュメントを確認してください。

* [Experience Platformにおける XDM システム ](../home.md):XDM と [!DNL Experience Platform] での実装の概要です。
   * [スキーマ構成の基本](../schema/composition.md)：XDM スキーマの構築ブロックの紹介。
* [[!DNL Real-Time Customer Profile]](../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
* [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

このチュートリアルを開始する前に、[ 開発者ガイド ](../api/getting-started.md) を参照して、[!DNL Schema Registry] API を正常に呼び出すために必要な重要な情報を確認してください。 そうした情報としては、`{TENANT_ID}`、「コンテナ」の概念、リクエストを行うのに必要なヘッダーなどがあります（ [!DNL Accept] ヘッダーとその取り得る値には特に注意を払います）。

## ソースおよび参照スキーマの定義 {#define-schemas}

この関係で定義される 2 つのスキーマが既に作成されていると想定されます。このチュートリアルでは、組織の現在のロイヤルティプログラム （「[!DNL Loyalty Members]」スキーマで定義）のメンバーと、お気に入りのホテル （「[!DNL Hotels]」スキーマで定義）のメンバー間の関係を作成します。

スキーマ関係は、**参照スキーマ** 内の別のフィールドを参照するフィールドを持つ **ソーススキーマ** で表されます。 以下の手順では、「[!DNL Loyalty Members]」がソーススキーマになり、「[!DNL Hotels]」が参照スキーマとして機能します。

>[!IMPORTANT]
>
>関係を確立するには、両方のスキーマにプライマリ ID が定義され、[!DNL Real-Time Customer Profile] が有効になっている必要があります。 スキーマを適切に設定する方法に関するガイダンスが必要な場合は、スキーマ作成チュートリアルの [ プロファイルで使用するスキーマの有効化 ](./create-schema-api.md#profile) に関する節を参照してください。

2 つのスキーマ間の関係を定義するには、まず両方のスキーマの `$id` 値を取得する必要があります。スキーマの表示名（`title`）がわかっている場合は、[!DNL Schema Registry] API の `/tenant/schemas` エンドポイントにGETリクエストを実行することで、`$id` の値を見つけることができます。

**API 形式**

```http
GET /tenant/schemas
```

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/vnd.adobe.xed-id+json'
```

>[!NOTE]
>
>[!DNL Accept] ヘッダー `application/vnd.adobe.xed-id+json` は、生成されるスキーマのタイトル、ID およびバージョンのみを返します。

**応答** 

正常な応答は、組織が定義したスキーマのリスト（組織の `name`、`$id`、`meta:altId`、`version` を含む）を返します。

```json
{
    "results": [
        {
            "title": "Newsletter Subscriptions",
            "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/192a66930afad02408429174c311ae73",
            "meta:altId": "_{TENANT_ID}.schemas.192a66930afad02408429174c311ae73",
            "version": "1.2"
        },
        {
            "title": "Loyalty Members",
            "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/2c66c3a4323128d3701289df4468e8a6",
            "meta:altId": "_{TENANT_ID}.schemas.2c66c3a4323128d3701289df4468e8a6",
            "version": "1.5"
        },
        {
            "title": "Hotels",
            "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/d4ad4b8463a67f6755f2aabbeb9e02c7",
            "meta:altId": "_{TENANT_ID}.schemas.d4ad4b8463a67f6755f2aabbeb9e02c7",
            "version": "1.0"
        }
    ],
    "_page": {
        "orderby": "updated",
        "next": null,
        "count": 3
    },
    "_links": {
        "next": null,
        "global_schemas": {
            "href": "https://platform-stage.adobe.io/data/foundation/schemaregistry/global/schemas"
        }
    }
}
```

関係を定義する 2 つのスキーマの `$id` 値を記録します。これらの値は、後の手順で使用します。

## ソーススキーマの参照フィールドを定義

[!DNL Schema Registry] 内では、関係記述子はリレーショナルデータベーステーブルの外部キーと同様に機能します。ソーススキーマのフィールドは、参照スキーマのプライマリ ID フィールドへの参照として機能します。 ソーススキーマにこのようなフィールドがない場合は、新しいフィールドを含むスキーマフィールドグループを作成し、スキーマに追加する必要がある可能性があります。 この新しいフィールドの `type` 値は `string` である必要があります。

>[!IMPORTANT]
>
>ソーススキーマは、そのプライマリ ID を参照フィールドとして使用できません。

このチュートリアルでは、参照スキーマ「[!DNL Hotels]」には、スキーマのプライマリ ID として機能する `hotelId` フィールドが含まれています。 ただし、ソーススキーマ「[!DNL Loyalty Members]」には、`hotelId` への参照として使用する専用フィールドがないので、スキーマに新しいフィールドを追加するには、カスタムフィールドグループを作成する必要があります（`favoriteHotel`）。

>[!NOTE]
>
>ソーススキーマに、参照フィールドとして使用する予定の専用フィールドが既に存在する場合は、[ 参照記述子の作成 ](#reference-identity) に関する手順に進みます。

### 新しいフィールドグループの作成

スキーマに新しいフィールドを追加するには、まずフィールドグループで定義する必要があります。 `/tenant/fieldgroups` エンドポイントにPOSTリクエストを行うことで、新しいフィールドグループを作成できます。

**API 形式**

```http
POST /tenant/fieldgroups
```

**リクエスト**

次のリクエストは、追加先のスキーマの `_{TENANT_ID}` 名前空間の下に `favoriteHotel` フィールドを追加する新しいフィールドグループを作成します。

```shell
curl -X POST\
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/fieldgroups \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'content-type: application/json' \
  -d '{
        "type": "object",
        "title": "Favorite Hotel",
        "meta:intendedToExtend": ["https://ns.adobe.com/xdm/context/profile"],
        "description": "Favorite hotel field group for the Loyalty Members schema.",
        "definitions": {
            "favoriteHotel": {
              "properties": {
                "_{TENANT_ID}": {
                  "type":"object",
                  "properties": {
                    "favoriteHotel": {
                      "title": "Favorite Hotel",
                      "type": "string",
                      "description": "Favorite hotel for a Loyalty Member."
                    }
                  }
                }
              }
            }
        },
        "allOf": [
            {
              "$ref": "#/definitions/favoriteHotel"
            }
        ]
      }'
```

**応答**

応答が成功すると、新しく作成されたフィールドグループの詳細が返されます。

```json
{
    "$id": "https://ns.adobe.com/{TENANT_ID}/mixins/3387945212ad76ee59b6d2b964afb220",
    "meta:altId": "_{TENANT_ID}.mixins.3387945212ad76ee59b6d2b964afb220",
    "meta:resourceType": "mixins",
    "version": "1.0",
    "type": "object",
    "title": "Favorite Hotel",
    "meta:intendedToExtend": [
        "https://ns.adobe.com/xdm/context/profile"
    ],
    "description": "Favorite hotel field group for the Loyalty Members schema.",
    "definitions": {
        "favoriteHotel": {
            "properties": {
                "_{TENANT_ID}": {
                    "type": "object",
                    "properties": {
                        "favoriteHotel": {
                            "title": "Favorite Hotel",
                            "type": "string",
                            "description": "Favorite hotel for a Loyalty Member.",
                            "meta:xdmType": "string"
                        }
                    },
                    "meta:xdmType": "object"
                }
            },
            "type": "object",
            "meta:xdmType": "object"
        }
    },
    "allOf": [
        {
            "$ref": "#/definitions/favoriteHotel"
        }
    ],
    "meta:xdmType": "object",
    "meta:abstract": true,
    "meta:extensible": true,
    "meta:containerId": "tenant",
    "meta:tenantNamespace": "_{TENANT_ID}",
    "meta:registryMetadata": {
        "eTag": "quM2aMPyb2NkkEiZHNCs/MG34E4=",
        "palm:sandboxName": "prod"
    }
}
```

| プロパティ | 説明 |
| --- | --- |
| `$id` | 新しいフィールドグループの読み取り専用の、システム生成された一意の ID。 URI の形式を取ります。 |

{style="table-layout:auto"}

フィールドグループの `$id` URI を記録します。これは、次の手順でフィールドグループをソーススキーマに追加する際に使用します。

### ソーススキーマへのフィールドグループの追加

フィールドグループを作成したら、`/tenant/schemas/{SCHEMA_ID}` エンドポイントに対して追加リクエストを行うことで、ソーススキーマにPATCHできます。

**API 形式**

```http
PATCH /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | URL エンコードされた `$id` URI またはソーススキーマの `meta:altId`。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストでは、「[!DNL Favorite Hotel]」フィールドグループを「[!DNL Loyalty Members]」スキーマに追加しています。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.533ca5da28087c44344810891b0f03d9 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '[
    { 
      "op": "add", 
      "path": "/allOf/-", 
      "value":  {
        "$ref": "https://ns.adobe.com/{TENANT_ID}/mixins/3387945212ad76ee59b6d2b964afb220"
      }
    }
  ]'
```

| プロパティ | 説明 |
| --- | --- |
| `op` | 実行する PATCH 操作。このリクエストでは、`add` 操作が使用されます。 |
| `path` | 新しいリソースを追加するスキーマフィールドへのパス。フィールドグループをスキーマに追加する場合、値は「/allOf/ –」である必要があります。 |
| `value.$ref` | 追加するフィールドグループの `$id`。 |

{style="table-layout:auto"}

**応答**

応答が成功すると、更新されたスキーマの詳細が返されます。この詳細には、`allOf` 配列の下に追加されたフィールドグループの `$ref` 値が含まれるようになりました。

```json
{
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/2c66c3a4323128d3701289df4468e8a6",
    "meta:altId": "_{TENANT_ID}.schemas.2c66c3a4323128d3701289df4468e8a6",
    "meta:resourceType": "schemas",
    "version": "1.1",
    "type": "object",
    "title": "Loyalty Members",
    "description": "",
    "allOf": [
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile-person-details"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/profile-personal-details"
        },
        {
            "$ref": "https://ns.adobe.com/{TENANT_ID}/mixins/ec16dfa484358f80478b75cde8c430d3"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/context/identitymap"
        },
        {
            "$ref": "https://ns.adobe.com/{TENANT_ID}/mixins/3387945212ad76ee59b6d2b964afb220"
        }
    ],
    "meta:containerId": "tenant",
    "meta:class": "https://ns.adobe.com/xdm/context/profile",
    "meta:abstract": false,
    "meta:extensible": false,
    "meta:tenantNamespace": "_{TENANT_ID}",
    "imsOrg": "{ORG_ID}",
    "meta:extends": [
        "https://ns.adobe.com/xdm/context/profile",
        "https://ns.adobe.com/xdm/data/record",
        "https://ns.adobe.com/xdm/context/identitymap",
        "https://ns.adobe.com/xdm/common/extensible",
        "https://ns.adobe.com/xdm/common/auditable",
        "https://ns.adobe.com/xdm/context/profile-person-details",
        "https://ns.adobe.com/xdm/context/profile-personal-details",
        "https://ns.adobe.com/{TENANT_ID}/mixins/ec16dfa484358f80478b75cde8c430d3",
        "https://ns.adobe.com/{TENANT_ID}/mixins/61969bc646b66a6230a7e8840f4a4d33"
    ],
    "meta:xdmType": "object",
    "meta:registryMetadata": {
        "repo:createdDate": 1557525483804,
        "repo:lastModifiedDate": 1566419670915,
        "xdm:createdClientId": "{API_KEY}",
        "xdm:lastModifiedClientId": "{CLIENT_ID}",
        "eTag": "ITNzu8BVTO5pw9wfCtTTpk6U4WY="
    }
}
```

## 参照 ID 記述子の作成 {#reference-identity}

関係で別のスキーマへの参照として使用される場合、スキーマフィールドには参照 ID 記述子を適用する必要があります。 「[!DNL Loyalty Members]」の `favoriteHotel` フィールドは「[!DNL Hotels]」の `hotelId` フィールドを参照するので、参照 ID 記述子を指定 `favoriteHotel` る必要があります。

`/tenant/descriptors` エンドポイントに対して参照リクエストを実行することで、ソーススキーマのPOST記述子を作成します。

**API 形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次のリクエストは、ソーススキーマ「[!DNL Loyalty Members]」の `favoriteHotel` フィールドの参照記述子を作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "@type": "xdm:descriptorReferenceIdentity",
    "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/533ca5da28087c44344810891b0f03d9",
    "xdm:sourceVersion": 1,
    "xdm:sourceProperty": "/_{TENANT_ID}/favoriteHotel",
    "xdm:identityNamespace": "Hotel ID"
  }'
```

| パラメーター | 説明 |
| --- | --- |
| `@type` | 定義する記述子のタイプ。 参照記述子の場合、値は `xdm:descriptorReferenceIdentity` にする必要があります。 |
| `xdm:sourceSchema` | ソーススキーマの `$id` URL。 |
| `xdm:sourceVersion` | ソーススキーマのバージョン番号。 |
| `sourceProperty` | 参照スキーマのプライマリ ID を参照するために使用されるソーススキーマ内のフィールドへのパス。 |
| `xdm:identityNamespace` | 参照フィールドの ID 名前空間。参照スキーマのプライマリ ID と同じ名前空間である必要があります。 詳しくは、「[ID 名前空間の概要](../../identity-service/home.md)」を参照してください。 |

{style="table-layout:auto"}

**応答**

応答が成功すると、ソースフィールドに対して新しく作成された参照記述子の詳細が返されます。

```json
{
    "@type": "xdm:descriptorReferenceIdentity",
    "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/533ca5da28087c44344810891b0f03d9",
    "xdm:sourceVersion": 1,
    "xdm:sourceProperty": "/_{TENANT_ID}/favoriteHotel",
    "xdm:identityNamespace": "Hotel ID",
    "meta:containerId": "tenant",
    "@id": "53180e9f86eed731f6bf8bf42af4f59d81949ba6"
}
```

## 関係記述子の作成 {#create-descriptor}

関係記述子は、ソーススキーマと参照スキーマの間に 1 対 1 の関係を確立します。 ソーススキーマ内の適切なフィールドに対して参照 ID 記述子を定義したら、`/tenant/descriptors` エンドポイントに対して関係リクエストを行うことで、新しいPOST記述子を作成できます。

**API 形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次のリクエストは、ソーススキーマとして「[!DNL Loyalty Members]」、参照スキーマとして「[!DNL Hotels]」を使用して、新しい関係記述子を作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "@type": "xdm:descriptorOneToOne",
    "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/2c66c3a4323128d3701289df4468e8a6",
    "xdm:sourceVersion": 1,
    "xdm:sourceProperty": "/_{TENANT_ID}/favoriteHotel",
    "xdm:destinationSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/d4ad4b8463a67f6755f2aabbeb9e02c7",
    "xdm:destinationVersion": 1,
    "xdm:destinationProperty": "/_{TENANT_ID}/hotelId"
  }'
```

| パラメーター | 説明 |
| --- | --- |
| `@type` | 作成する記述子のタイプ。関係記述子の `@type` 値は `xdm:descriptorOneToOne` です。 |
| `xdm:sourceSchema` | ソーススキーマの `$id` URL。 |
| `xdm:sourceVersion` | ソーススキーマのバージョン番号。 |
| `xdm:sourceProperty` | ソーススキーマの参照フィールドへのパス。 |
| `xdm:destinationSchema` | 参照スキーマの `$id` URL。 |
| `xdm:destinationVersion` | 参照スキーマのバージョン番号。 |
| `xdm:destinationProperty` | 参照スキーマのプライマリ ID フィールドへのパス。 |

{style="table-layout:auto"}

### 応答

正常な応答は、宛先スキーマの新しく作成された関係記述子の詳細を返します。

```json
{
    "@type": "xdm:descriptorOneToOne",
    "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/2c66c3a4323128d3701289df4468e8a6",
    "xdm:sourceVersion": 1,
    "xdm:sourceProperty": "/_{TENANT_ID}/favoriteHotel",
    "xdm:destinationSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/d4ad4b8463a67f6755f2aabbeb9e02c7",
    "xdm:destinationVersion": 1,
    "xdm:destinationProperty": "/_{TENANT_ID}/hotelId",
    "meta:containerId": "tenant",
    "@id": "76f6cc7105f4eaab7eb4a5e1cb4804cadc741669"
}
```

## 次の手順

このチュートリアルでは、2 つのスキーマ間に 1 対 1 の関係を作成しました。[!DNL Schema Registry] API を使用した記述子の操作について詳しくは、『 [ スキーマレジストリ開発者ガイド ](../api/descriptors.md) 』を参照してください。 UI でスキーマの関係を定義する手順については、[スキーマエディタを使用したスキーマの関係の定義](relationship-ui.md)に関するチュートリアルを参照してください。
