---
keywords: Experience Platform；ホーム；人気のあるトピック；API;API;XDM;XDMシステム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；スキーマレジストリ；スキーマ；スキーマ；スキーマ；関係；関係記述子；参照ID;
solution: Experience Platform
title: スキーマレジストリAPIを使用した2つのスキーマ間の関係の定義
description: このドキュメントでは、スキーマレジストリ API を使用して、組織が定義した 2 つのスキーマ間の 1 対 1 の関係を定義するためのチュートリアルを提供します。
topic-legacy: tutorial
type: Tutorial
exl-id: ef9910b5-2777-4d8b-a6fe-aee51d809ad5
source-git-commit: 8133804076b1c0adf2eae5b748e86a35f3186d14
workflow-type: tm+mt
source-wordcount: '1365'
ht-degree: 37%

---

# [!DNL Schema Registry] APIを使用して2つのスキーマ間の関係を定義する

様々なチャネルでの顧客とブランドとの関係を理解する能力は、Adobe Experience Platform の重要な部分です。[!DNL Experience Data Model](XDM)スキーマの構造内でこれらの関係を定義すると、顧客データに関する複雑なインサイトを得ることができます。

和集合スキーマと[!DNL Real-time Customer Profile]を使用してスキーマの関係を推論することはできますが、これは同じクラスを共有するスキーマにのみ適用されます。 異なるクラスに属する2つのスキーマ間の関係を確立するには、宛先スキーマのIDを参照するソーススキーマに、専用の関係フィールドを追加する必要があります。

このドキュメントでは、[[!DNL Schema Registry API]](https://www.adobe.io/experience-platform-apis/references/schema-registry/)を使用して、組織で定義された2つのスキーマ間の1対1の関係を定義するためのチュートリアルを提供します。

## はじめに

このチュートリアルでは、[!DNL Experience Data Model](XDM)と[!DNL XDM System]に関する十分な知識が必要です。 このチュートリアルを始める前に、次のドキュメントを確認してください。

* [Experience PlatformのXDMシステム](../home.md):XDMと、での実装の概要で [!DNL Experience Platform]す。
   * [スキーマ構成の基本](../schema/composition.md)：XDM スキーマの構築ブロックの紹介。
* [[!DNL Real-time Customer Profile]](../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
* [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

このチュートリアルを開始する前に、『[開発者ガイド](../api/getting-started.md)』を参照し、[!DNL Schema Registry] APIを正しく呼び出すために知っておく必要がある重要な情報を確認してください。 これには、`{TENANT_ID}`、「コンテナ」の概念、リクエストをおこなうために必要なヘッダー（ ヘッダーとその可能な値に特に注意）が含まれます。[!DNL Accept]

## ソースと宛先のスキーマの定義 {#define-schemas}

この関係で定義される 2 つのスキーマが既に作成されていると想定されます。このチュートリアルでは、組織の現在のロイヤルティプログラム（「[!DNL Loyalty Members]」スキーマで定義）のメンバーと、お気に入りのホテル（「[!DNL Hotels]」スキーマで定義）との間に関係を作成します。

スキーマ関係は、**宛先スキーマ**&#x200B;内の別のフィールドを参照するフィールドを有する&#x200B;**ソーススキーマ**&#x200B;で表されます。次の手順では、「[!DNL Loyalty Members]」がソーススキーマで、「[!DNL Hotels]」が宛先スキーマとして機能します。

>[!IMPORTANT]
>
>関係を確立するには、両方のスキーマでプライマリIDが定義され、[!DNL Real-time Customer Profile]に対して有効になっている必要があります。 スキーマを適切に設定する方法に関するガイダンスが必要な場合は、『スキーマ作成チュートリアル』の「プロファイル](./create-schema-api.md#profile)でのスキーマの使用」に関する節を参照してください。[

2 つのスキーマ間の関係を定義するには、まず両方のスキーマの `$id` 値を取得する必要があります。スキーマの表示名(`title`)がわかっている場合は、[!DNL Schema Registry] APIの`/tenant/schemas`エンドポイントにGETリクエストを送信して、`$id`の値を見つけることができます。

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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/vnd.adobe.xed-id+json'
```

>[!NOTE]
>
>[!DNL Accept]ヘッダー`application/vnd.adobe.xed-id+json`は、結果として得られるスキーマのタイトル、IDおよびバージョンのみを返します。

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

## ソーススキーマの参照フィールドの定義

[!DNL Schema Registry]内では、リレーショナルデータベーステーブルの外部キーと同様に関係記述子が機能します。ソーススキーマ内のフィールドは、宛先スキーマのプライマリidフィールドへの参照として機能します。 ソーススキーマにこの目的のフィールドがない場合は、新しいフィールドを含むスキーマフィールドグループを作成し、スキーマに追加する必要があります。 この新しいフィールドの値は、「[!DNL string]」にする必要があります。`type`

>[!IMPORTANT]
>
>宛先スキーマとは異なり、ソーススキーマはプライマリIDを参照フィールドとして使用できません。

このチュートリアルでは、宛先スキーマ「[!DNL Hotels]」には、スキーマのプライマリIDとして機能する`hotelId`フィールドが含まれているので、参照フィールドとしても機能します。 ただし、ソーススキーマ「[!DNL Loyalty Members]」には参照として使用する専用のフィールドがないので、スキーマに新しいフィールドを追加する新しいフィールドグループを指定する必要があります。`favoriteHotel`.

>[!NOTE]
>
>ソーススキーマに、参照フィールドとして使用する専用のフィールドが既に存在する場合は、[参照記述子](#reference-identity)の作成の手順に進むことができます。

### 新しいフィールドグループを作成する

スキーマに新しいフィールドを追加するには、まずフィールドグループで定義する必要があります。 `/tenant/fieldgroups`エンドポイントにPOSTリクエストを送信して、新しいフィールドグループを作成できます。

**API 形式**

```http
POST /tenant/fieldgroups
```

**リクエスト**

次のリクエストでは、新しいフィールドグループを作成し、追加先のスキーマの`_{TENANT_ID}`名前空間の下に`favoriteHotel`フィールドを追加します。

```shell
curl -X POST\
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/fieldgroups \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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

正常な応答は、新しく作成されたフィールドグループの詳細を返します。

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
| `$id` | 新しいフィールドグループの一意の識別子（システムで生成された読み取り専用）。 URI の形式を取ります。 |

{style=&quot;table-layout:auto&quot;}

次の手順でフィールドグループをソーススキーマに追加する際に使用する、フィールドグループの`$id` URIを記録します。

### フィールドグループをソーススキーマに追加する

フィールドグループを作成したら、`/tenant/schemas/{SCHEMA_ID}`エンドポイントにPATCHリクエストを送信して、そのフィールドグループをソーススキーマに追加できます。

**API 形式**

```http
PATCH /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | URL エンコードされた `$id` URI またはソーススキーマの `meta:altId`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストでは、「[!DNL Favorite Hotel]」フィールドグループを「[!DNL Loyalty Members]」スキーマに追加します。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.533ca5da28087c44344810891b0f03d9 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `path` | 新しいリソースを追加するスキーマフィールドへのパス。フィールドグループをスキーマに追加する場合、値は「/allOf/ — 」にする必要があります。 |
| `value.$ref` | 追加するフィールドグループの`$id`。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、更新されたスキーマの詳細を返します。この詳細には、追加されたフィールドグループの`$ref`値が`allOf`配列に含まれます。

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
    "imsOrg": "{IMS_ORG}",
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

スキーマフィールドは、関係内の他の要素からの参照として使用される場合、参照 ID 記述子を適用する必要があります。「[!DNL Loyalty Members]」の`favoriteHotel`フィールドは「[!DNL Hotels]」の`hotelId`フィールドを参照するので、`hotelId`には参照ID記述子を指定する必要があります。

`/tenant/descriptors` エンドポイントに対して POST リクエストをおこなって、宛先スキーマの参照記述子を作成します。

**API 形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次のリクエストは、宛先スキーマ「[!DNL Hotels]」の`hotelId`フィールドの参照記述子を作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "@type": "xdm:descriptorReferenceIdentity",
    "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/d4ad4b8463a67f6755f2aabbeb9e02c7",
    "xdm:sourceVersion": 1,
    "xdm:sourceProperty": "/_{TENANT_ID}/hotelId",
    "xdm:identityNamespace": "Hotel ID"
  }'
```

| パラメーター | 説明 |
| --- | --- |
| `@type` | 定義する記述子のタイプ。参照記述子の場合、値は「xdm:descriptorReferenceIdentity」である必要があります。 |
| `xdm:sourceSchema` | 宛先スキーマの `$id` URL。 |
| `xdm:sourceVersion` | 宛先スキーマのバージョン番号。 |
| `sourceProperty` | 宛先スキーマのプライマリ ID フィールドへのパス。 |
| `xdm:identityNamespace` | 参照フィールドの ID 名前空間。これは、フィールドをスキーマのプライマリIDとして定義する際に使用する名前空間と同じにする必要があります。 詳しくは、「[ID 名前空間の概要](../../identity-service/home.md)」を参照してください。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、宛先スキーマの新しく作成された参照記述子の詳細を返します。

```json
{
    "@type": "xdm:descriptorReferenceIdentity",
    "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/d4ad4b8463a67f6755f2aabbeb9e02c7",
    "xdm:sourceVersion": 1,
    "xdm:sourceProperty": "/_{TENANT_ID}/hotelId",
    "xdm:identityNamespace": "Hotel ID",
    "meta:containerId": "tenant",
    "@id": "53180e9f86eed731f6bf8bf42af4f59d81949ba6"
}
```

## 関係記述子の作成 {#create-descriptor}

関係記述子は、ソース記述子と宛先スキーマとの間に 1 対 1 の関係を確立します。宛先スキーマの参照記述子を定義したら、`/tenant/descriptors`エンドポイントにPOSTリクエストを作成して、新しい関係記述子を作成できます。

**API 形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次のリクエストは、ソーススキーマが「[!DNL Loyalty Members]」、宛先スキーマが「[!DNL Legacy Loyalty Members]」の新しい関係記述子を作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `@type` | 作成する記述子のタイプ。関係記述子の`@type`値は「xdm:descriptorOneToOne」です。 |
| `xdm:sourceSchema` | ソーススキーマの `$id` URL。 |
| `xdm:sourceVersion` | ソーススキーマのバージョン番号。 |
| `xdm:sourceProperty` | ソーススキーマ内の参照フィールドへのパス。 |
| `xdm:destinationSchema` | 宛先スキーマの `$id` URL。 |
| `xdm:destinationVersion` | 宛先スキーマのバージョン番号。 |
| `xdm:destinationProperty` | 宛先スキーマ内の参照フィールドへのパス。 |

{style=&quot;table-layout:auto&quot;}

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

このチュートリアルでは、2 つのスキーマ間に 1 対 1 の関係を作成しました。[!DNL Schema Registry] APIを使用した記述子の操作について詳しくは、『[スキーマレジストリ開発者ガイド](../api/descriptors.md)』を参照してください。 UI でスキーマの関係を定義する手順については、[スキーマエディタを使用したスキーマの関係の定義](relationship-ui.md)に関するチュートリアルを参照してください。
