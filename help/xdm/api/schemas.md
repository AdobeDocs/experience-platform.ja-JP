---
keywords: Experience Platform；ホーム；人気のトピック；API;XDM;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；スキーマレジストリ；スキーマ；スキーマ；スキーマ；作成
solution: Experience Platform
title: スキーマ API エンドポイント
description: Schema Registry API の/schemas エンドポイントを使用すると、エクスペリエンスアプリケーション内の XDM スキーマをプログラムで管理できます。
exl-id: d0bda683-9cd3-412b-a8d1-4af700297abf
source-git-commit: 983682489e2c0e70069dbf495ab90fc9555aae2d
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 21%

---

# スキーマエンドポイント

スキーマは、Adobe Experience Platformに取り込むデータのブループリントと考えることができます。 各スキーマは、クラスと、0 個以上のスキーマフィールドグループで構成されます。 この `/schemas` エンドポイント [!DNL Schema Registry] API を使用すると、エクスペリエンスアプリケーション内のスキーマをプログラムで管理できます。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Schema Registry] API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) の一部です。先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## スキーマのリストの取得 {#list}

すべてのスキーマを `global` または `tenant` コンテナを作成するには、次に対してGETリクエストを実行します。 `/global/schemas` または `/tenant/schemas`、それぞれ。

>[!NOTE]
>
>リソースをリストする場合、スキーマレジストリでは結果セットを 300 項目に制限します。 この制限を超えるリソースを返すには、ページングパラメーターを使用する必要があります。 また、追加のクエリパラメーターを使用して結果をフィルタリングし、返されるリソースの数を減らすことをお勧めします。 詳しくは、 [クエリパラメーター](./appendix.md#query) 詳しくは、付録のドキュメントを参照してください。

**API 形式**

```http
GET /{CONTAINER_ID}/schemas?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | 取得するスキーマを格納するコンテナ： `global` (Adobe作成スキーマの場合 ) または `tenant` ：組織が所有するスキーマの場合。 |
| `{QUERY_PARAMS}` | 結果をフィルターするオプションのクエリパラメーター。詳しくは、 [付録文書](./appendix.md#query) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、 `tenant` コンテナ、使用 `orderby` クエリパラメーターを使用して結果を並べ替える `title` 属性。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas?orderby=title \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、 `Accept` ヘッダーがリクエストで送信されました。 以下 `Accept` ヘッダーを使用してスキーマをリストできます。

| `Accept` ヘッダー | 説明 |
| --- | --- |
| `application/vnd.adobe.xed-id+json` | 各リソースの短い概要を返します。 リソースを一覧表示する場合は、これが推奨されるヘッダーです。 ( 制限：300) |
| `application/vnd.adobe.xed+json` | 各リソースの完全な JSON スキーマを元のと共に返します `$ref` および `allOf` 含まれる ( 制限：300) |

{style=&quot;table-layout:auto&quot;}

**応答**

上記のリクエストでは、 `application/vnd.adobe.xed-id+json` `Accept` したがって、応答には `title`, `$id`, `meta:altId`、および `version` 属性を指定します。 他の `Accept` ヘッダー (`application/vnd.adobe.xed+json`) は、各スキーマのすべての属性を返します。 適切な `Accept` ヘッダーを作成します。

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

パスにスキーマの ID を含むGETリクエストを作成して、特定のスキーマを検索できます。

**API 形式**

```http
GET /{CONTAINER_ID}/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | 取得するスキーマを格納するコンテナ。 `global` (Adobe作成のスキーマの場合 ) または `tenant` ：組織が所有するスキーマの場合。 |
| `{SCHEMA_ID}` | この `meta:altId` または URL エンコード済み `$id` 検索するスキーマの数を指定します。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、で指定されたスキーマを取得します。 `meta:altId` パスの値。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.f579a0b5f992c69458ea408ec36571f7da9de15901bab116 \
  -H 'Accept: application/vnd.adobe.xed+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、 `Accept` ヘッダーがリクエストで送信されました。 すべての検索リクエストには `version` 含まれる `Accept` ヘッダー。 以下 `Accept` ヘッダーを使用できます。

| `Accept` ヘッダー | 説明 |
| ------- | ------------ |
| `application/vnd.adobe.xed+json; version=1` | `$ref` および `allOf` で生、タイトルと説明を含む |
| `application/vnd.adobe.xed-full+json; version=1` | `$ref` および `allOf` を解決、タイトルと説明を含む |
| `application/vnd.adobe.xed-notext+json; version=1` | `$ref` および `allOf` で生、タイトルや説明なし |
| `application/vnd.adobe.xed-full-notext+json; version=1` | `$ref` および `allOf` で解決、タイトルや説明なし |
| `application/vnd.adobe.xed-full-desc+json; version=1` | `$ref` および `allOf` で解決、説明を含む |
| `application/vnd.adobe.xed-deprecatefield+json; version=1` | `$ref` および `allOf` を解決、タイトルと説明を含む非推奨のフィールドは、 `meta:status` 属性 `deprecated`. |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、スキーマの詳細を返します。 返されるフィールドは、 `Accept` ヘッダーがリクエストで送信されました。 異なるを使用した実験 `Accept` ヘッダーを使用して、応答を比較し、使用事例に最適なヘッダーを決定します。

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
  "imsOrg": "{ORG_ID}",
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
>以下の呼び出しの例は、API でスキーマを作成する方法のベースライン例です。クラスの構成要件は最小限で、フィールドグループは必要ありません。 フィールドグループとデータ型を使用してフィールドを割り当てる方法など、API でスキーマを作成する方法に関する完全な手順については、 [スキーマ作成チュートリアル](../tutorials/create-schema-api.md).

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `allOf` | 各オブジェクトが、スキーマが実装するフィールドを持つクラスまたはフィールドグループを参照する、オブジェクトの配列。 各オブジェクトには、1 つのプロパティ (`$ref`) の値が `$id` 新しいスキーマが実装するクラスまたはフィールドグループの。 1 つのクラスを、0 個以上の追加のフィールドグループと共に指定する必要があります。 上記の例では、 `allOf` array はスキーマのクラスです。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、HTTP ステータス 201（作成済み）と、新しく作成されたスキーマの詳細（`$id`、`meta:altId`、`version`など）を含むペイロードを返します。これらの値は読み取り専用で、 [!DNL Schema Registry].

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
    "imsOrg": "{ORG_ID}",
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

へのGETリクエストの実行 [すべてのスキーマのリスト](#list) これで、テナントコンテナに新しいスキーマが含まれます。 次の操作を実行できます。 [参照 (GET) リクエスト](#lookup) URL エンコードされたを使用 `$id` 新しいスキーマを直接表示する URI。

スキーマにフィールドを追加するには、 [PATCH操作](#patch) スキーマの `allOf` および `meta:extends` 配列

## スキーマの更新 {#put}

スキーマ全体を置き換えるには、PUT操作を使用します。つまり、リソースを書き直す必要があります。 スキーマリクエストを通じてPUTを更新する場合、本文には、 [新しいスキーマの作成](#create) POST。

>[!NOTE]
>
>スキーマ全体を置き換えるのではなく、スキーマの一部のみを更新する場合は、 [スキーマの一部の更新](#patch).

**API 形式**

```http
PUT /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | この `meta:altId` または URL エンコード済み `$id` 」で指定します。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、既存のスキーマを置き換え、変更します `title`, `description`、および `allOf` 属性。

```SHELL
curl -X PUT \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67 \
  -H 'Authorization: Bearer {ACCESS_TOKEN' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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

正常な応答は、更新されたスキーマの詳細を返します。

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
    "imsOrg": "{ORG_ID}",
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

## スキーマの一部を更新 {#patch}

スキーマリクエストを使用して、スキーマの一部を更新することがPATCHできます。 この [!DNL Schema Registry] は、以下を含むすべての標準的な JSON パッチ操作をサポートしています。 `add`, `remove`、および `replace`. JSON パッチの詳細については、 [API の基本事項ガイド](../../landing/api-fundamentals.md#json-patch).

>[!NOTE]
>
>個々のフィールドを更新する代わりに、リソース全体を新しい値に置き換える場合は、 [スキーマ操作を使用したPUTの置き換え](#put).

最も一般的なPATCH操作の 1 つは、次の例に示すように、以前に定義したフィールドグループをスキーマに追加することです。

**API 形式**

```http
PATCH /tenant/schemas/{SCHEMA_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | URL エンコードされた `$id` URI or `meta:altId` を設定します。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

以下のリクエスト例では、新しいフィールドグループをスキーマに追加する際に、そのフィールドグループの `$id` 値を `meta:extends` および `allOf` 配列

リクエスト本文は配列の形式をとり、リストに表示される各オブジェクトは個々のフィールドに対する特定の変更を表します。 各オブジェクトには、実行される操作 (`op`) を呼び出し、操作を実行する必要があるフィールド (`path`) と、その操作に含める必要のある情報 (`value`) をクリックします。

```SHELL
curl -X PATCH\
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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

応答には、両方の操作が正常に実行されたことが示されます。フィールドグループ `$id` が `meta:extends` 配列と参照 (`$ref`) をフィールドグループに追加します。 `$id` が `allOf` 配列。

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
    "imsOrg": "{ORG_ID}",
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

## リアルタイム顧客プロファイルでのスキーマ使用の有効化 {#union}

スキーマが参加するために [リアルタイム顧客プロファイル](../../profile/home.md)を追加する場合は、 `union` タグをスキーマの `meta:immutableTags` 配列。 これは、対象のスキーマに対してPATCHリクエストを実行することで実現できます。

>[!IMPORTANT]
>
>Immutable タグは、設定を目的としたもので、削除には使用できません。

**API 形式**

```http
PATCH /tenant/schemas/{SCHEMA_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | URL エンコードされた `$id` URI or `meta:altId` を設定します。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

以下のリクエスト例では、 `meta:immutableTags` 配列を既存のスキーマに割り当て、配列に次の単一の文字列値を渡す `union` をクリックして、プロファイルでの使用を有効にします。

```SHELL
curl -X PATCH\
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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

正常な応答は、更新されたスキーマの詳細を返し、 `meta:immutableTags` 配列が追加されました。

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
    "imsOrg": "{ORG_ID}",
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

これで、このスキーマのクラスの和集合を表示して、スキーマのフィールドが表されていることを確認できます。 詳しくは、 [unions endpoint guide](./unions.md) を参照してください。

## スキーマの削除 {#delete}

スキーマレジストリからスキーマを削除する必要が生じる場合があります。 これは、パスで指定されたDELETEID を使用してスキーマリクエストを実行することでおこなわれます。

**API 形式**

```http
DELETE /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | URL エンコードされた `$id` URI or `meta:altId` を設定します。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、HTTP ステータス 204（コンテンツなし）が空白の本文とともに返されます。

スキーマに対して検索 (GET) リクエストを試行して、削除を確認できます。 次を含める必要があります： `Accept` ヘッダーを使用しますが、スキーマレジストリからスキーマが削除されたので、HTTP ステータス 404(Not Found) を受け取る必要があります。
