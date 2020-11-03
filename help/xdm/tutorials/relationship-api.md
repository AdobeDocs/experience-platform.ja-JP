---
keywords: Experience Platform;home;popular topics;api;API;XDM;XDM system;;experience data model;Experience data model;Experience Data Model;data model;Data Model;schema registry;Schema Registry;schema;Schema;schemas;Schemas;relationship;Relationship;relationship descriptor;Relationship descriptor;reference identity;Reference identity;
solution: Experience Platform
title: レジストリ API を使用した 2 つのスキーマ間の関係の定義
description: このドキュメントでは、スキーマレジストリ API を使用して、組織が定義した 2 つのスキーマ間の 1 対 1 の関係を定義するためのチュートリアルを提供します。
topic: tutorial
type: Tutorial
translation-type: tm+mt
source-git-commit: ce06550e9608163e6e5819d79cc73a4b1f92e915
workflow-type: tm+mt
source-wordcount: '1296'
ht-degree: 49%

---


# Define a relationship between two schemas using the [!DNL Schema Registry] API

様々なチャネルでの顧客とブランドとの関係を理解する能力は、Adobe Experience Platform の重要な部分です。Defining these relationships within the structure of your [!DNL Experience Data Model] (XDM) schemas allows you to gain complex insights into your customer data.

スキーマの関係は、和集合スキーマを使用して推論できますが、 [!DNL Real-time Customer Profile]これは同じクラスを共有するスキーマにのみ適用されます。 異なるクラスに属する2つのスキーマ間の関係を確立するには、目的のスキーマのIDを参照するソーススキーマに、専用の関係フィールドを追加する必要があります。

This document provides a tutorial for defining a one-to-one relationship between two schemas defined by your organization using the [[!DNL Schema Registry API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml).

## はじめに

このチュートリアルでは、 [!DNL Experience Data Model] (XDM)との詳細を理解している必要があり [!DNL XDM System]ます。 このチュートリアルを始める前に、次のドキュメントを確認してください。

* [Experience PlatformのXDMシステム](../home.md):XDMとその実装の概要を、で説明し [!DNL Experience Platform]ます。
   * [スキーマ構成の基本](../schema/composition.md)：XDM スキーマの構築ブロックの紹介。
* [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [サンドボックス](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

Before starting this tutorial, please review the [developer guide](../api/getting-started.md) for important information that you need to know in order to successfully make calls to the [!DNL Schema Registry] API. これには、`{TENANT_ID}`、「コンテナ」の概念、リクエストをおこなうために必要なヘッダー（ ヘッダーとその可能な値に特に注意）が含まれます。[!DNL Accept]

## ソースと宛先スキーマの定義 {#define-schemas}

この関係で定義される 2 つのスキーマが既に作成されていると想定されます。This tutorial creates a relationship between members of an organization&#39;s current loyalty program (defined in a &quot;[!DNL Loyalty Members]&quot; schema) and their favorite hotels (defined in a &quot;[!DNL Hotels]&quot; schema).

スキーマ関係は、**宛先スキーマ**&#x200B;内の別のフィールドを参照するフィールドを有する&#x200B;**ソーススキーマ**&#x200B;で表されます。In the steps that follow, &quot;[!DNL Loyalty Members]&quot; will be the source schema, while &quot;[!DNL Hotels]&quot; will act as the destination schema.

>[!IMPORTANT]
>
>関係を確立するには、両方のスキーマがプライマリIDを定義し、有効にする必要があり [!DNL Real-time Customer Profile]ます。 スキーマをそれに応じて設定する方法のガイダンスが必要な場合は、「プロファイルの作成」チュートリアルの「スキーマをスキーマで使用できるようにする」のセクションを参照して [](./create-schema-api.md#profile) ください。

2 つのスキーマ間の関係を定義するには、まず両方のスキーマの `$id` 値を取得する必要があります。If you know the display names (`title`) of the schemas, you can find their `$id` values by making a GET request to the `/tenant/schemas` endpoint in the [!DNL Schema Registry] API.

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
>The [!DNL Accept] header `application/vnd.adobe.xed-id+json` returns only the titles, IDs, and versions of the resulting schemas.

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

Within the [!DNL Schema Registry], relationship descriptors work similarly to foreign keys in relational database tables: a field in the source schema acts as a reference to the primary identity field of a destination schema. ソーススキーマにこの目的のフィールドがない場合は、新しいフィールドを使用してミックスインを作成し、スキーマに追加する必要があります。 This new field must have a `type` value of &quot;[!DNL string]&quot;.

>[!IMPORTANT]
>
>宛先スキーマとは異なり、ソーススキーマは、その主IDを参照フィールドとして使用できません。

このチュートリアルでは、宛先スキーマ「[!DNL Hotels]」に、スキーマの主IDとしての役割を果たす `hotelId` フィールドが含まれているので、その参照フィールドとしても機能します。 ただし、ソーススキーマ「[!DNL Loyalty Members]」には参照として使用する専用のフィールドがないため、スキーマに新しいフィールドを追加する新しいミックスインを与える必要があります。 `favoriteHotel`.

>[!NOTE]
>
>ソーススキーマに、参照フィールドとして使用する専用のフィールドが既に存在する場合は、参照記述子の [作成の手順に進むことができます](#reference-identity)。

### 新しい mixin の作成

スキーマに新しいフィールドを追加するには、まず mixin で定義する必要があります。`/tenant/mixins` エンドポイントに対して POST リクエストを作成することで、新しい mixin を作成できます。

**API 形式**

```http
POST /tenant/mixins
```

**リクエスト**

次のリクエストでは、新しい mixin を作成し、追加先の任意のスキーマの `_{TENANT_ID}` 名前空間の下に `favoriteHotel` フィールドを追加します。

```shell
curl -X POST\
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/mixins \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'content-type: application/json' \
  -d '{
        "type": "object",
        "title": "Favorite Hotel",
        "meta:intendedToExtend": ["https://ns.adobe.com/xdm/context/profile"],
        "description": "Favorite hotel mixin for the Loyalty Members schema.",
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

正常な応答は、新しく作成された mixin の詳細を返します。

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
    "description": "Favorite hotel mixin for the Loyalty Members schema.",
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
| `$id` | 新しい mixin の一意の識別子（システムで生成され、読み取り専用）。URI の形式を取ります。 |

次の手順で mixin をソーススキーマに追加する際に使用する、mixin の `$id` URI を記録します。

### ソーススキーマへの mixin の追加

Mixin を作成したら、`/tenant/schemas/{SCHEMA_ID}` エンドポイントに対して PATCH リクエストを作成して、その mixin をソーススキーマに追加できます。

**API 形式**

```http
PATCH /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | URL エンコードされた `$id` URI またはソーススキーマの `meta:altId`。 |

**リクエスト**

The following request adds the &quot;[!DNL Favorite Hotel]&quot; mixin to the &quot;[!DNL Loyalty Members]&quot; schema.

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
| `path` | 新しいリソースを追加するスキーマフィールドへのパス。スキーマにミックスインを追加する場合、値は「/allOf/ — 」にする必要があります。 |
| `value.$ref` | 追加する mixin の `$id`。 |

**応答** 

正常な応答は、更新されたスキーマの詳細を返します。これには、追加された mixin の `$ref` 値が `allOf` 配列に含まれます。

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

スキーマフィールドは、関係内の他の要素からの参照として使用される場合、参照 ID 記述子を適用する必要があります。Since the `favoriteHotel` field in &quot;[!DNL Loyalty Members]&quot; will refer to the `hotelId` field in &quot;[!DNL Hotels]&quot;, `hotelId` must be given a reference identity descriptor.

`/tenant/descriptors` エンドポイントに対して POST リクエストをおこなって、宛先スキーマの参照記述子を作成します。

**API 形式**

```http
POST /tenant/descriptors
```

**リクエスト**

The following request creates a reference descriptor for the `hotelId` field in the destination schema &quot;[!DNL Hotels]&quot;.

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
| `@type` | 定義する記述子のタイプ。参照記述子の場合、値は&quot;xdm:descriptorReferenceIdentity&quot;でなければなりません。 |
| `xdm:sourceSchema` | 宛先スキーマの `$id` URL。 |
| `xdm:sourceVersion` | 宛先スキーマのバージョン番号。 |
| `sourceProperty` | 宛先スキーマのプライマリ ID フィールドへのパス。 |
| `xdm:identityNamespace` | 参照フィールドの ID 名前空間。この名前空間は、フィールドをスキーマのプライマリIDとして定義する場合と同じにする必要があります。 詳しくは、「[ID 名前空間の概要](../../identity-service/home.md)」を参照してください。 |

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

関係記述子は、ソース記述子と宛先スキーマとの間に 1 対 1 の関係を確立します。宛先スキーマの参照記述子を定義すると、エンドポイントにPOSTリクエストを作成して、新しい関係記述子を作成でき `/tenant/descriptors` ます。

**API 形式**

```http
POST /tenant/descriptors
```

**リクエスト**

The following request creates a new relationship descriptor, with &quot;[!DNL Loyalty Members]&quot; as the source schema and &quot;[!DNL Legacy Loyalty Members]&quot; as the destination schema.

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
| `@type` | 作成する記述子のタイプ。The `@type` value for relationship descriptors is &quot;xdm:descriptorOneToOne&quot;. |
| `xdm:sourceSchema` | ソーススキーマの `$id` URL。 |
| `xdm:sourceVersion` | ソーススキーマのバージョン番号。 |
| `xdm:sourceProperty` | ソーススキーマの参照フィールドへのパス。 |
| `xdm:destinationSchema` | 宛先スキーマの `$id` URL。 |
| `xdm:destinationVersion` | 宛先スキーマのバージョン番号。 |
| `xdm:destinationProperty` | コピー先スキーマの参照フィールドへのパス。 |

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

このチュートリアルでは、2 つのスキーマ間に 1 対 1 の関係を作成しました。For more information on working with descriptors using the [!DNL Schema Registry] API, see the [Schema Registry developer guide](../api/descriptors.md). UI でスキーマの関係を定義する手順については、[スキーマエディタを使用したスキーマの関係の定義](relationship-ui.md)に関するチュートリアルを参照してください。
