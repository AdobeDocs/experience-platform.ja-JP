---
keywords: Experience Platform；ホーム；人気のトピック；API;XDM;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；スキーマレジストリ；スキーマ；スキーマ；関係；関係；記述子；参照 ID;
title: スキーマレジストリ API を使用した 2 つのスキーマ間の関係の定義
description: このドキュメントでは、スキーマレジストリ API を使用して、組織が定義した 2 つのスキーマ間の 1 対 1 の関係を定義するためのチュートリアルを提供します。
topic-legacy: tutorial
type: Tutorial
exl-id: ef9910b5-2777-4d8b-a6fe-aee51d809ad5
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '1367'
ht-degree: 33%

---

# を使用して 2 つのスキーマ間の関係を定義 [!DNL Schema Registry] API

様々なチャネルでの顧客とブランドとの関係を理解する能力は、Adobe Experience Platform の重要な部分です。の構造内でこれらの関係を定義する [!DNL Experience Data Model] (XDM) スキーマを使用すると、顧客データに関する複雑なインサイトを得ることができます。

スキーマの関係は、和集合スキーマと [!DNL Real-Time Customer Profile]同じクラスを共有するスキーマにのみ適用されます。 異なるクラスに属する 2 つのスキーマ間の関係を確立するには、宛先スキーマの ID を参照するソーススキーマに、専用の関係フィールドを追加する必要があります。

このドキュメントでは、 [[!DNL Schema Registry API]](https://www.adobe.io/experience-platform-apis/references/schema-registry/).

## はじめに

このチュートリアルでは、 [!DNL Experience Data Model] (XDM) と [!DNL XDM System]. このチュートリアルを始める前に、次のドキュメントを確認してください。

* [XDM システムのExperience Platform](../home.md):XDM と、 [!DNL Experience Platform].
   * [スキーマ構成の基本](../schema/composition.md)：XDM スキーマの構築ブロックの紹介。
* [[!DNL Real-Time Customer Profile]](../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
* [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

このチュートリアルを開始する前に、 [開発者ガイド](../api/getting-started.md) を正しく呼び出すために知っておく必要がある重要な情報については、を参照してください。 [!DNL Schema Registry] API これには、`{TENANT_ID}`、「コンテナ」の概念、リクエストを行うのに必要なヘッダー（ ヘッダーと使用可能な値には特に注意を払う）が含まれます。[!DNL Accept]

## ソースと宛先のスキーマの定義 {#define-schemas}

この関係で定義される 2 つのスキーマが既に作成されていると想定されます。このチュートリアルでは、組織の現在のロイヤルティプログラム (「[!DNL Loyalty Members]「 」スキーマ ) とそのお気に入りのホテル（「 」で定義）[!DNL Hotels]&quot;スキーマ ) です。

スキーマ関係は、**宛先スキーマ**&#x200B;内の別のフィールドを参照するフィールドを有する&#x200B;**ソーススキーマ**&#x200B;で表されます。次の手順で、「[!DNL Loyalty Members]」がソーススキーマになり、「[!DNL Hotels]「 」が宛先スキーマとして機能します。

>[!IMPORTANT]
>
>関係を確立するには、両方のスキーマでプライマリ ID が定義され、 [!DNL Real-Time Customer Profile]. 詳しくは、 [プロファイルで使用するスキーマの有効化](./create-schema-api.md#profile) スキーマを適切に設定する方法に関するガイダンスが必要な場合は、スキーマ作成のチュートリアルを参照してください。

2 つのスキーマ間の関係を定義するには、まず両方のスキーマの `$id` 値を取得する必要があります。表示名 (`title`) を使用して、スキーマの `$id` の値を指定するために、 `/tenant/schemas` エンドポイント [!DNL Schema Registry] API

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
>この [!DNL Accept] ヘッダー `application/vnd.adobe.xed-id+json` は、結果として生成されるスキーマのタイトル、ID およびバージョンのみを返します。

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

内 [!DNL Schema Registry]関係記述子は、リレーショナルデータベーステーブルの外部キーと同様に機能します。ソーススキーマ内のフィールドは、宛先スキーマのプライマリ id フィールドへの参照として機能します。 ソーススキーマにこの目的のフィールドがない場合は、新しいフィールドを含むスキーマフィールドグループを作成し、スキーマに追加する必要がある場合があります。 この新しいフィールドには、 `type` 値 `string`.

>[!IMPORTANT]
>
>ソーススキーマは、そのプライマリ ID を参照フィールドとして使用できません。

このチュートリアルでは、宛先スキーマ「 」[!DNL Hotels]「 」には次が含まれます `hotelId` スキーマのプライマリ id として機能するフィールド。 ただし、ソーススキーマ「[!DNL Loyalty Members]」には、 `hotelId`と呼ばれ、スキーマに新しいフィールドを追加するには、カスタムフィールドグループを作成する必要があります。 `favoriteHotel`.

>[!NOTE]
>
>ソーススキーマに、参照フィールドとして使用する専用のフィールドが既に存在する場合は、 [参照記述子の作成](#reference-identity).

### 新しいフィールドグループを作成

スキーマに新しいフィールドを追加するには、まずフィールドグループで定義する必要があります。 新しいフィールドグループを作成するには、 `/tenant/fieldgroups` endpoint.

**API 形式**

```http
POST /tenant/fieldgroups
```

**リクエスト**

次のリクエストでは、新しいフィールドグループを作成し、 `favoriteHotel` 下のフィールド `_{TENANT_ID}` 名前空間に含まれます。

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
| `$id` | システムが生成した、新しいフィールドグループの一意の ID です。 URI の形式を取ります。 |

{style=&quot;table-layout:auto&quot;}

次を記録： `$id` 次の手順でフィールドグループをソーススキーマに追加する際に使用するフィールドグループの URI。

### フィールドグループをソーススキーマに追加する

フィールドグループを作成したら、 `/tenant/schemas/{SCHEMA_ID}` endpoint.

**API 形式**

```http
PATCH /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | URL エンコードされた `$id` URI またはソーススキーマの `meta:altId`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストでは、[!DNL Favorite Hotel]&quot;フィールドグループを&quot;[!DNL Loyalty Members]&quot;スキーマ。

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
| `path` | 新しいリソースを追加するスキーマフィールドへのパス。フィールドグループをスキーマに追加する場合、値は「/allOf/ — 」である必要があります。 |
| `value.$ref` | この `$id` 追加するフィールドグループの |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、更新されたスキーマの詳細を返します。この詳細には、 `$ref` 追加されたフィールドグループの値を、その下に `allOf` 配列。

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

スキーマフィールドは、関係内の別のスキーマへの参照として使用される場合、参照 ID 記述子を適用する必要があります。 以降 `favoriteHotel` フィールドの「[!DNL Loyalty Members]」が `hotelId` フィールドの「[!DNL Hotels]&quot;, `favoriteHotel` は、参照 ID 記述子を与える必要があります。

に対してPOSTリクエストを実行して、ソーススキーマの参照記述子を作成します。 `/tenant/descriptors` endpoint.

**API 形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次のリクエストは、 `favoriteHotel` ソーススキーマのフィールド[!DNL Loyalty Members]&quot;.

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
| `@type` | 定義する記述子のタイプ。参照記述子の場合、値は `xdm:descriptorReferenceIdentity`. |
| `xdm:sourceSchema` | ソーススキーマの `$id` URL。 |
| `xdm:sourceVersion` | ソーススキーマのバージョン番号。 |
| `sourceProperty` | 宛先スキーマのプライマリ ID を参照するために使用される、ソーススキーマ内のフィールドへのパス。 |
| `xdm:identityNamespace` | 参照フィールドの ID 名前空間。これは、宛先スキーマのプライマリ ID と同じ名前空間である必要があります。 詳しくは、「[ID 名前空間の概要](../../identity-service/home.md)」を参照してください。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、ソースフィールドに対して新しく作成された参照記述子の詳細を返します。

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

関係記述子は、ソース記述子と宛先スキーマとの間に 1 対 1 の関係を確立します。ソーススキーマ内の適切なフィールドの参照 ID 記述子を定義したら、に対してPOSTリクエストを作成して、新しい関係記述子を作成できます。 `/tenant/descriptors` endpoint.

**API 形式**

```http
POST /tenant/descriptors
```

**リクエスト**

次のリクエストは、「[!DNL Loyalty Members]&quot;をソーススキーマとして、&quot;[!DNL Hotels]」を宛先スキーマとして追加します。

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
| `xdm:sourceProperty` | ソーススキーマ内の参照フィールドへのパス。 |
| `xdm:destinationSchema` | 宛先スキーマの `$id` URL。 |
| `xdm:destinationVersion` | 宛先スキーマのバージョン番号。 |
| `xdm:destinationProperty` | 宛先スキーマのプライマリ ID フィールドへのパス。 |

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

このチュートリアルでは、2 つのスキーマ間に 1 対 1 の関係を作成しました。を使用した記述子の操作の詳細 [!DNL Schema Registry] API( [スキーマレジストリ開発者ガイド](../api/descriptors.md). UI でスキーマの関係を定義する手順については、[スキーマエディタを使用したスキーマの関係の定義](relationship-ui.md)に関するチュートリアルを参照してください。
