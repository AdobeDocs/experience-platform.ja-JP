---
keywords: Experience Platform;home;popular topics;api;API;XDM;XDM system;;experience data model;Experience data model;Experience Data Model;data model;Data Model;schema registry;Schema Registry;schema;Schema;schemas;Schemas;create
solution: Experience Platform
title: スキーマの作成
description: スキーマレジストリAPIの/スキーマエンドポイントを使用すると、エクスペリエンスアプリケーション内のXDMスキーマをプログラムで管理できます。
topic: developer guide
translation-type: tm+mt
source-git-commit: 0b55f18eabcf1d7c5c233234c59eb074b2670b93
workflow-type: tm+mt
source-wordcount: '1386'
ht-degree: 18%

---


# スキーマエンドポイント

スキーマは、Adobe Experience Platformに取り込むデータの青写真と考えることができます。 各スキーマは、クラスと 0 個以上の mixin で構成されます。APIの `/schemas`[!DNL Schema Registry] エンドポイントを使用すると、エクスペリエンスアプリケーション内のスキーマをプログラムで管理できます。

## はじめに

The API endpoint used in this guide is part of the [[!DNL Schema Registry] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml). 先に進む前に、 [はじめに](./getting-started.md) 、関連ドキュメントへのリンク、このドキュメントのサンプルAPI呼び出しを読むためのガイド、Experience PlatformAPIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報を確認してください。

## Retrieve a list of schemas {#list}

また、スキーマに対してリストリクエストを行うことで、 `global` または `tenant` コンテナ下のすべてのGETをそれぞれ `/global/schemas` または `/tenant/schemas`に対して行うことができます。

>[!NOTE]
>
>リソースをリストする場合、スキーマレジストリでは結果セットが300項目に制限されます。 この制限を超えるリソースを返すには、ページングパラメーターを使用する必要があります。 また、結果をフィルターし、返されるリソースの数を減らすために、追加のクエリパラメーターを使用することもお勧めします。 詳しくは、付録ドキュメントの [クエリパラメータ](./appendix.md#query) の節を参照してください。

**API 形式**

```http
GET /{CONTAINER_ID}/schemas?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | 取得するスキーマを格納するコンテナ。 `global` adobeが作成したスキーマ、または組織が所有するスキーマ `tenant` 用。 |
| `{QUERY_PARAMS}` | 結果をフィルターするオプションのクエリパラメーター。 使用可能なパラメーターのリストについては、 [付録ドキュメント](./appendix.md#query) を参照してください。 |

**リクエスト**

次のリクエストは、 `tenant` コンテナからスキーマのリストを取得し、 `orderby``title` クエリパラメーターを使用して属性で結果を並べ替えます。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas?orderby=title \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

The response format depends on the `Accept` header sent in the request. The following `Accept` headers are available for listing schemas:

| `Accept` ヘッダー | 説明 |
| --- | --- |
| `application/vnd.adobe.xed-id+json` | 各リソースの短い概要を返します。 リソースのリストを表示する際に推奨されるヘッダーです。 (制限：300) |
| `application/vnd.adobe.xed+json` | Returns full JSON schema for each resource, with original `$ref` and `allOf` included. (制限：300) |

**応答** 

The request above used the `application/vnd.adobe.xed-id+json` `Accept` header, therefore the response includes only the `title`, `$id`, `meta:altId`, and `version` attributes for each schema. もう一方の `Accept` ヘッダー(`application/vnd.adobe.xed+json`)を使用すると、各スキーマのすべての属性が返されます。 Select the appropriate `Accept` header depending on the information you require in your response.

```json
{
  "results": [
    {
      "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/0238be93d3e7a06aec5e0655955901ec",
      "meta:altId": "_{TENANT_ID}.schemas.0238be93d3e7a06aec5e0655955901ec",
      "version": "1.4",
      "title": "Hotels"
    },
    {
      "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/0ef4ce0d390f0809fad490802f53d30b",
      "meta:altId": "_{TENANT_ID}.schemas.0ef4ce0d390f0809fad490802f53d30b",
      "version": "1.0",
      "title": "Loyalty Members"
    }
  ],
  "_page": {
        "orderby": "title",
        "next": null,
        "count": 2
    },
    "_links": {
        "next": null,
        "global_schemas": {
            "href": "https://platform.adobe.io/data/foundation/schemaregistry/global/schemas"
        }
    }
}
```

## スキーマの検索 {#lookup}

パスにスキーマのIDを含むGETリクエストを作成して、特定のスキーマを検索できます。

**API 形式**

```http
GET /{CONTAINER_ID}/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | 取得するスキーマを格納するコンテナ。 `global` adobeが作成したスキーマ、または組織が所有するスキーマ `tenant` 用。 |
| `{SCHEMA_ID}` | 検索 `meta:altId` するスキーマ `$id` のURLエンコードまたはURLエンコード。 |

**リクエスト**

次のリクエストは、パスの `meta:altId` 値で指定されたスキーマを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.f579a0b5f992c69458ea408ec36571f7da9de15901bab116 \
  -H 'Accept: application/vnd.adobe.xed+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

The response format depends on the `Accept` header sent in the request. All lookup requests require a `version` be included in the `Accept` header. The following `Accept` headers are available:

| `Accept` ヘッダー | 説明 |
| ------- | ------------ |
| `application/vnd.adobe.xed+json; version={MAJOR_VERSION}` | `$ref` および `allOf` で生、タイトルと説明を含む |
| `application/vnd.adobe.xed-full+json; version={MAJOR_VERSION}` | `$ref` および `allOf` を解決、タイトルと説明を含む |
| `application/vnd.adobe.xed-notext+json; version={MAJOR_VERSION}` | `$ref` および `allOf` で生、タイトルや説明なし |
| `application/vnd.adobe.xed-full-notext+json; version={MAJOR_VERSION}` | `$ref` および `allOf` で解決、タイトルや説明なし |
| `application/vnd.adobe.xed-full-desc+json; version={MAJOR_VERSION}` | `$ref` および `allOf` で解決、説明を含む |

**応答** 

成功した場合は、スキーマの詳細が返されます。 The fields that are returned depend on the `Accept` header sent in the request. Experiment with different `Accept` headers to compare the responses and determine which header is best for your use case.

```json
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/20af3f1d4b175f27ba59529d1b51a0c79fc25df454117c80",
  "meta:altId": "_{TENANT_ID}.schemas.20af3f1d4b175f27ba59529d1b51a0c79fc25df454117c80",
  "meta:resourceType": "schemas",
  "version": "1.1",
  "title": "Example schema",
  "type": "object",
  "description": "An example schema created within the tenant container.",
  "allOf": [
      {
          "$ref": "https://ns.adobe.com/xdm/context/profile",
          "type": "object",
          "meta:xdmType": "object"
      },
      {
          "$ref": "https://ns.adobe.com/{TENANT_ID}/mixins/443fe51457047d958f4a97853e64e0eca93ef34d7990583b",
          "type": "object",
          "meta:xdmType": "object"
      }
  ],
  "imsOrg": "{IMS_ORG}",
  "meta:extensible": false,
  "meta:abstract": false,
  "meta:extends": [
      "https://ns.adobe.com/{TENANT_ID}/mixins/443fe51457047d958f4a97853e64e0eca93ef34d7990583b",
      "https://ns.adobe.com/xdm/common/auditable",
      "https://ns.adobe.com/xdm/data/record",
      "https://ns.adobe.com/xdm/context/profile"
  ],
  "meta:xdmType": "object",
  "meta:registryMetadata": {
      "repo:createdDate": 1602872911226,
      "repo:lastModifiedDate": 1603381419889,
      "xdm:createdClientId": "{CLIENT_ID}",
      "xdm:lastModifiedClientId": "{CLIENT_ID}",
      "xdm:createdUserId": "{USER_ID}",
      "xdm:lastModifiedUserId": "{USER_ID}",
      "eTag": "84b4da79b7445a4bf1c59269e718065effddb983c492f48e223d49c049c6d589",
      "meta:globalLibVersion": "1.15.4"
  },
  "meta:class": "https://ns.adobe.com/xdm/context/profile",
  "meta:containerId": "tenant",
  "meta:sandboxId": "ff0f6870-c46d-11e9-8ca3-036939a64204",
  "meta:sandboxType": "production",
  "meta:tenantNamespace": "_{TENANT_ID}"
}
```

## スキーマの作成 {#create}

スキーマ構成プロセスは、クラスの割り当てから開始されます。このクラスは、データ（レコードまたは時系列）の主要な動作要素と、取得されるデータを記述するために必要な最小フィールドを定義します。

>[!NOTE]
>
>以下の呼び出し例は、クラスの組版要件を最小限にし、ミックスインを使用しないで、APIでのスキーマの作成方法のベースラインの例に過ぎません。 ミックスインやデータ型を使用してフィールドを割り当てる方法など、APIでスキーマを作成する方法に関する完全な手順については、 [スキーマ作成のチュートリアルを参照してください](../tutorials/create-schema-api.md)。

**API 形式**

```http
POST /tenant/schemas
```

**リクエスト**

リクエストには、クラスの `$id` を参照する `allOf` 属性を含める必要があります。この属性は、スキーマが実装する「基本クラス」を定義します。この例では、基本クラスは、以前に作成された「プロパティ情報」クラスです。

```SHELL
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas \
  -H 'Authorization: Bearer {ACCESS_TOKEN' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "title":"Property Information",
        "description": "Property-related information.",
        "type": "object",
        "allOf": [ 
          { 
            "$ref": "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590" 
          } 
        ]
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `allOf` | オブジェクトの配列。各オブジェクトは、スキーマがフィールドを実装するクラスまたはミックスインを参照します。 各オブジェクトには、新しいスキーマが実装するクラスまたはミックスイン`$ref`の値を表す1つのプロパティ( `$id` )が含まれます。 1つのクラスを指定し、0個以上の追加ミックスインを追加する必要があります。 上記の例では、 `allOf` 配列内の1つのオブジェクトがスキーマのクラスです。 |

**応答**

正常な応答は、HTTP ステータス 201（作成済み）と、新しく作成されたスキーマの詳細（`$id`、`meta:altId`、`version`など）を含むペイロードを返します。These values are read-only and are assigned by the [!DNL Schema Registry].

```JSON
{
    "title": "Property Information",
    "description": "Property-related information.",
    "type": "object",
    "allOf": [
        {
            "$ref": "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590"
        }
    ],
    "meta:class": "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590",
    "meta:abstract": false,
    "meta:extensible": false,
    "meta:extends": [
        "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590",
        "https://ns.adobe.com/xdm/data/record"
    ],
    "meta:containerId": "tenant",
    "imsOrg": "{IMS_ORG}",
    "meta:altId": "_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67",
    "meta:xdmType": "object",
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/d5cc04eb8d50190001287e4c869ebe67",
    "version": "1.0",
    "meta:resourceType": "schemas",
    "meta:registryMetadata": {
        "repo:createDate": 1552088461236,
        "repo:lastModifiedDate": 1552088461236,
        "xdm:createdClientId": "{CREATED_CLIENT}",
        "xdm:repositoryCreatedBy": "{CREATED_BY}"
    }
}
```

Performing a GET request to [list all schemas](#list) in the tenant container would now include the new schema. You can perform a [lookup (GET) request](#lookup) using the URL-encoded `$id` URI to view the new schema directly.

スキーマにフィールドを追加するには、 [PATCH操作を実行して](#patch) 、スキーマのおよ `allOf` び `meta:extends` 配列にミックスインを追加します。

## スキーマの更新 {#put}

スキーマ全体をPUT操作で置き換え、基本的にリソースを書き直すことができます。 PUT要求を使用してスキーマを更新する場合、本文には、POST要求で新しいスキーマを [作成する際に必要となるすべてのフィールドを含める必要があります](#create) 。

>[!NOTE]
>
>If you only want to update part of a schema instead of replacing it entirely, see the section on [updating a portion of a schema](#patch).

**API 形式**

```http
PUT /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | 再書き込みす `meta:altId` るスキーマ `$id` のURLエンコードまたはURL。 |

**リクエスト**

次のリクエストは、既存のスキーマを置き換え、その `title`、 `description`および `allOf` 属性を変更します。

```SHELL
curl -X PUT \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67 \
  -H 'Authorization: Bearer {ACCESS_TOKEN' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "title":"Commercial Property Information",
        "description": "Information related to commercial properties.",
        "type": "object",
        "allOf": [ 
          { 
            "$ref": "https://ns.adobe.com/{TENANT_ID}/classes/01b7b1745e8ac4ed1e8784ec91b6afa7" 
          } 
        ]
      }'
```

**応答** 

正常に応答すると、更新されたスキーマの詳細が返されます。

```JSON
{
    "title":"Commercial Property Information",
    "description": "Information related to commercial properties.",
    "type": "object",
    "allOf": [
        {
            "$ref": "https://ns.adobe.com/{TENANT_ID}/classes/01b7b1745e8ac4ed1e8784ec91b6afa7"
        }
    ],
    "meta:class": "https://ns.adobe.com/{TENANT_ID}/classes/01b7b1745e8ac4ed1e8784ec91b6afa7",
    "meta:abstract": false,
    "meta:extensible": false,
    "meta:extends": [
        "https://ns.adobe.com/{TENANT_ID}/classes/01b7b1745e8ac4ed1e8784ec91b6afa7",
        "https://ns.adobe.com/xdm/data/record"
    ],
    "meta:containerId": "tenant",
    "imsOrg": "{IMS_ORG}",
    "meta:altId": "_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67",
    "meta:xdmType": "object",
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/d5cc04eb8d50190001287e4c869ebe67",
    "version": "1.0",
    "meta:resourceType": "schemas",
    "meta:registryMetadata": {
        "repo:createDate": 1552088461236,
        "repo:lastModifiedDate": 1552088470592,
        "xdm:createdClientId": "{CREATED_CLIENT}",
        "xdm:repositoryCreatedBy": "{CREATED_BY}"
    }
}
```

## Update a portion of a schema {#patch}

PATCHリクエストを使用して、スキーマの一部を更新できます。 の標準的なJSONパッチ操作(、、な [!DNL Schema Registry] ど)はすべて `add`サポートされ `remove``replace`ます。 JSONパッチについて詳しくは、 [APIの基本ガイドを参照してください](../../landing/api-fundamentals.md#json-patch)。

>[!NOTE]
>
>If you want to replace an entire resource with new values instead of updating individual fields, see the section on [replacing a schema using a PUT operation](#put).

最も一般的なPATCH操作の1つに、以下の例に示すように、スキーマに以前に定義したミックスインを追加する方法があります。

**API 形式**

```http
PATCH /tenant/schema/{SCHEMA_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | The URL-encoded `$id` URI or `meta:altId` of the schema you want to update. |

**リクエスト**

以下のリクエスト例では、との配列の両方にmixinの `$id` 値を追加することで、スキーマに新しいmixinを追加し `meta:extends` て `allOf` います。

リクエスト本体は配列の形をとり、リストに表示された各オブジェクトは個々のフィールドに対する特定の変更を表します。 各オブジェクトは、実行する操作(`op`)、操作を実行するフィールド(`path`)、およびその操作に含める情報(`value`)を含む。

```SHELL
curl -X PATCH\
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'content-type: application/json' \
  -d '[
        { 
          "op": "add",
          "path": "/meta:extends/-",
          "value":  "https://ns.adobe.com/{TENANT_ID}/mixins/e49cbb2eec33618f686b8344b4597ecf"
        },
        {
          "op": "add",
          "path": "/allOf/-",
          "value":  {
            "$ref": "https://ns.adobe.com/{TENANT_ID}/mixins/e49cbb2eec33618f686b8344b4597ecf"
          }
        }
      ]'
```

**応答**

応答には、両方の操作が正常に実行されたことが示されます。Mixin `$id` が `meta:extends` 配列に追加され、mixin `$id` への参照（`$ref`）が `allOf` 配列に表示されます。

```JSON
{
    "title": "Property Information",
    "description": "Property-related information.",
    "type": "object",
    "allOf": [
        {
            "$ref": "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590"
        },
        {
            "$ref": "https://ns.adobe.com/{TENANT_ID}/mixins/e49cbb2eec33618f686b8344b4597ecf"
        }
    ],
    "meta:class": "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590",
    "meta:abstract": false,
    "meta:extensible": false,
    "meta:extends": [
        "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590",
        "https://ns.adobe.com/xdm/data/record",
        "https://ns.adobe.com/{TENANT_ID}/mixins/e49cbb2eec33618f686b8344b4597ecf"
    ],
    "meta:containerId": "tenant",
    "imsOrg": "{IMS_ORG}",
    "meta:altId": "_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67",
    "meta:xdmType": "object",
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/d5cc04eb8d50190001287e4c869ebe67",
    "version": "1.1",
    "meta:resourceType": "schemas",
    "meta:registryMetadata": {
        "repo:createDate": 1552088461236,
        "repo:lastModifiedDate": 1552088649634,
        "xdm:createdClientId": "{CREATED_CLIENT}",
        "xdm:repositoryCreatedBy": "{CREATED_BY}"
    }
}
```

## Enable a schema for use in Real-time Customer Profile {#union}

スキーマが [リアルタイム顧客プロファイルに参加するには](../../profile/home.md)、スキーマの `union` アレイに `meta:immutableTags` タグを追加する必要があります。 これを行うには、対象のスキーマに対してPATCHリクエストを行います。

>[!IMPORTANT]
>
>Immutable タグは、設定を目的としたもので、削除には使用できません。

**API 形式**

```http
PATCH /tenant/schema/{SCHEMA_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | The URL-encoded `$id` URI or `meta:altId` of the schema you want to enable. |

**リクエスト**

次の例のリクエストでは、既存のスキーマに `meta:immutableTags` 配列を追加し、その配列に対してプロファイルでの使用を有効にするための単一の文字列値 `union` を与えています。

```SHELL
curl -X PATCH\
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'content-type: application/json' \
  -d '[
        {
          "op": "add",
          "path": "/meta:immutableTags",
          "value": ["union"]
        }
      ]'
```

**応答** 

正常に応答すると、更新されたスキーマの詳細が返され、 `meta:immutableTags` 配列が追加されたことが示されます。

```JSON
{
    "title": "Property Information",
    "description": "Property-related information.",
    "type": "object",
    "allOf": [
        {
            "$ref": "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590"
        },
        {
            "$ref": "https://ns.adobe.com/{TENANT_ID}/mixins/e49cbb2eec33618f686b8344b4597ecf"
        }
    ],
    "meta:class": "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590",
    "meta:abstract": false,
    "meta:extensible": false,
    "meta:extends": [
        "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590",
        "https://ns.adobe.com/xdm/data/record",
        "https://ns.adobe.com/{TENANT_ID}/mixins/e49cbb2eec33618f686b8344b4597ecf"
    ],
    "meta:containerId": "tenant",
    "imsOrg": "{IMS_ORG}",
    "meta:altId": "_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67",
    "meta:xdmType": "object",
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/d5cc04eb8d50190001287e4c869ebe67",
    "version": "1.1",
    "meta:resourceType": "schemas",
    "meta:registryMetadata": {
        "repo:createDate": 1552088461236,
        "repo:lastModifiedDate": 1552088649634,
        "xdm:createdClientId": "{CREATED_CLIENT}",
        "xdm:repositoryCreatedBy": "{CREATED_BY}"
    },
    "meta:immutableTags": [
      "union"
    ]
}
```

これで、このスキーマのクラスの和集合を表示して、スキーマのフィールドが表示されていることを確認できます。 See the [unions endpoint guide](./unions.md) for more information.

## スキーマの削除 {#delete}

スキーマレジストリからスキーマを削除する必要がある場合があります。 これは、パスで指定されたスキーマIDを使用してDELETEリクエストを実行することで行われます。

**API 形式**

```http
DELETE /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | The URL-encoded `$id` URI or `meta:altId` of the schema you want to delete. |

**リクエスト**

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、空白の本文とともに HTTP ステータス 204（コンテンツなし）を返します。

スキーマに対してルックアップ(GET)リクエストを実行して、削除を確認できます。 You will need to include an `Accept` header in the request, but should receive an HTTP status 404 (Not Found) because the schema has been removed from the Schema Registry.