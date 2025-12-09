---
keywords: Experience Platform；ホーム；人気のトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；スキーマレジストリ；スキーマ；スキーマ；作成
solution: Experience Platform
title: スキーマ API エンドポイント
description: Schema Registry API の/schemas エンドポイントを使用すると、エクスペリエンスアプリケーション内の XDM スキーマをプログラムで管理できます。
exl-id: d0bda683-9cd3-412b-a8d1-4af700297abf
source-git-commit: 491588dab1388755176b5e00f9d8ae3e49b7f856
workflow-type: tm+mt
source-wordcount: '2091'
ht-degree: 15%

---

# スキーマエンドポイント

スキーマは、Adobe Experience Platformに取り込むデータのブループリントと考えることができます。 各スキーマは、クラスと 0 個以上のスキーマフィールドグループで構成されます。 `/schemas` API の [!DNL Schema Registry] エンドポイントを使用すると、エクスペリエンスアプリケーション内のスキーマをプログラムで管理できます。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Schema Registry] API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) の一部です。先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## スキーマのリストの取得 {#list}

`global` または `tenant` に対してそれぞれGET リクエストをおこなうことで、`/global/schemas` コンテナまたは `/tenant/schemas` コンテナの下のすべてのスキーマをリストできます。

>[!NOTE]
>
>リソースをリストする場合、スキーマレジストリでは結果セットを 300 項目に制限しています。 この制限を超えるリソースを返すには、ページングパラメーターを使用する必要があります。 また、追加のクエリパラメーターを使用して結果をフィルタリングし、返されるリソースの数を減らすこともお勧めします。 詳しくは、付録のドキュメントの [ クエリパラメーター ](./appendix.md#query) に関する節を参照してください。

**API 形式**

```http
GET /{CONTAINER_ID}/schemas?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | 取得するスキーマを格納するコンテナ：Adobeで作成されたスキーマの場合は `global`、組織が所有するスキーマの場合は `tenant`。 |
| `{QUERY_PARAMS}` | 結果をフィルタリングするオプションのクエリパラメーター。 使用可能なパラメーターのリストについては、[ 付録ドキュメント ](./appendix.md#query) を参照してください。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、`tenant` クエリパラメーターを使用して結果を `orderby` 属性で並べ替え、`title` コンテナからスキーマのリストを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas?orderby=title \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、リクエストで送信される `Accept` ヘッダーによって異なります。 スキーマの一覧表示には、次の `Accept` ヘッダーを使用できます。

| `Accept` ヘッダー | 説明 |
| --- | --- |
| `application/vnd.adobe.xed-id+json` | 各リソースの短い概要を返します。 リソースをリストする場合は、このヘッダーをお勧めします。 （上限：300） |
| `application/vnd.adobe.xed+json` | 各リソースの完全な JSON スキーマを返します。元の `$ref` と `allOf` が含まれます。 （上限：300） |

{style="table-layout:auto"}

**応答**

上記のリクエストでは `application/vnd.adobe.xed-id+json` `Accept` ヘッダーを使用しているので、応答には各スキーマの `title`、`$id`、`meta:altId` および `version` 属性のみが含まれています。 もう一方の `Accept` ヘッダー（`application/vnd.adobe.xed+json`）を使用すると、各スキーマのすべての属性が返されます。 応答で必要な情報に応じて、適切な `Accept` ヘッダーを選択します。

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

パスにスキーマの ID を含むGET リクエストを実行することで、特定のスキーマを検索できます。

**API 形式**

```http
GET /{CONTAINER_ID}/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | 取得するスキーマを格納するコンテナ：Adobeで作成されたスキーマの場合は `global`、組織が所有するスキーマの場合は `tenant`。 |
| `{SCHEMA_ID}` | 検索するスキーマの `meta:altId` または URL エンコードされた `$id`。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、パス内の `meta:altId` 値で指定されたスキーマを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.f579a0b5f992c69458ea408ec36571f7da9de15901bab116 \
  -H 'Accept: application/vnd.adobe.xed+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、リクエストで送信される `Accept` ヘッダーによって異なります。 すべての参照リクエストには、`version` ヘッダーに `Accept` を含める必要があります。 次の `Accept` ヘッダーを使用できます。

| `Accept` ヘッダー | 説明 |
| ------- | ------------ |
| `application/vnd.adobe.xed+json; version=1` | `$ref` および `allOf` で生、タイトルと説明を含む |
| `application/vnd.adobe.xed-full+json; version=1` | `$ref` および `allOf` を解決、タイトルと説明を含む |
| `application/vnd.adobe.xed-notext+json; version=1` | `$ref` および `allOf` で生、タイトルや説明なし |
| `application/vnd.adobe.xed-full-notext+json; version=1` | `$ref` および `allOf` で解決、タイトルや説明なし |
| `application/vnd.adobe.xed-full-desc+json; version=1` | `$ref` および `allOf` で解決、説明を含む |
| `application/vnd.adobe.xed-deprecatefield+json; version=1` | `$ref` と `allOf` の解決には、タイトルと説明があります。 非推奨フィールドは、`meta:status` 属性が `deprecated` で示されます。 |

{style="table-layout:auto"}

**応答**

応答が成功すると、スキーマの詳細が返されます。 返されるフィールドは、リクエストで送信される `Accept` ヘッダーによって異なります。 様々な `Accept` ヘッダーを試して、応答を比較し、ユースケースに最適なヘッダーを判断します。

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

クラスやフィールドグループを含まないスキーマ（リレーショナルスキーマと呼ばれます）の作成方法については、[ リレーショナルスキーマの作成 ](#create-relational-schema) の節を参照してください。

>[!NOTE]
>
>以下の呼び出し例は、クラスの最小構成要件を持ち、フィールドグループを持たない、API でスキーマを作成する方法の基本的な例に過ぎません。 フィールドグループとデータタイプを使用してフィールドを割り当てる方法など、API でスキーマを作成する詳細な手順については、[ スキーマ作成チュートリアル ](../tutorials/create-schema-api.md) を参照してください。

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
| `allOf` | オブジェクトの配列。各オブジェクトは、スキーマがフィールドを実装するクラスまたはフィールドグループを参照します。 各オブジェクトには、新しいスキーマが実装するクラスまたはフィールドグループの `$ref` を表す値を持つ単一のプロパティ（`$id`）が含まれています。 1 つのクラスを指定し、0 個以上の追加のフィールドグループを含める必要があります。 上記の例では、`allOf` 配列の単一のオブジェクトがスキーマのクラスです。 |

{style="table-layout:auto"}

**応答**

正常な応答は、HTTP ステータス 201（作成済み）と、新しく作成されたスキーマの詳細（`$id`、`meta:altId`、`version`など）を含むペイロードを返します。これらの値は読み取り専用で、[!DNL Schema Registry] によって割り当てられます。

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

テナントコンテナで [ すべてのスキーマをリスト ](#list) するGET リクエストを実行すると、新しいスキーマが含まれるようになりました。 URL エンコードされた [ URI を使用して ](#lookup) ルックアップ （GET） リクエストを実行し `$id` 新しいスキーマを直接表示することができます。

スキーマにフィールドを追加するには、[PATCH操作を実行して ](#patch) スキーマの `allOf` 配列と `meta:extends` 配列にフィールドグループを追加します。

## リレーショナルスキーマを作成 {#create-relational-schema}

>[!AVAILABILITY]
>
>Data Mirrorおよびリレーショナルスキーマは、Adobe Journey Optimizer **オーケストレートキャンペーン** のライセンスホルダーが利用できます。 ライセンスとイネーブルメント機能に応じて、Customer Journey Analytics ユーザー向けの **限定リリース** としても使用できます。 アクセスについては、Adobe担当者にお問い合わせください。

`/schemas` エンドポイントに対して POST リクエストを実行することで、リレーショナルスキーマを作成します。 リレーショナルスキーマは、構造化されたリレーショナルスタイルのデータ **クラスまたはフィールドグループを含まない** を格納します。 スキーマ上で直接フィールドを定義し、論理動作タグを使用してスキーマをリレーショナルとして識別します。

>[!IMPORTANT]
>
>リレーショナルスキーマを作成するには、`meta:extends` を `"https://ns.adobe.com/xdm/data/adhoc-v2"` に設定します。 これは **論理的動作の識別子** であり、物理的な動作やクラスではありません。 **でクラスやフィールドグループを参照** ない `allOf` し、**でクラスやフィールドグループを含めない**`meta:extends` します。

`POST /tenant/schemas` で最初にスキーマを作成します。 次に、[Descriptors API （`POST /tenant/descriptors`）で必要な記述子を追加し ](../api/descriptors.md) す。

- [プライマリキー記述子 ](../api/descriptors.md#primary-key-descriptor): プライマリキーフィールドは **ルートレベル** にあり、**必要としてマークされている** 必要があります。
- [ バージョン記述子 ](../api/descriptors.md#version-descriptor): **必須** プライマリキーが存在する場合。
- [ 関係記述子 ](../api/descriptors.md#relationship-descriptor)：任意。結合を定義します。カーディナリティは取り込み時に適用されません。
- [ タイムスタンプ記述子 ](../api/descriptors.md#timestamp-descriptor)：時系列スキーマの場合、プライマリキーはタイムスタンプ フィールドを含む **複合** キーである必要があります。

>[!NOTE]
>
>UI スキーマエディターでは、バージョン記述子とタイムスタンプ記述子は、それぞれ「[!UICONTROL Version identifier]」と「[!UICONTROL Timestamp identifier]」として表示されます。

>[!CAUTION]
>
>リレーショナルスキーマは **結合スキーマと互換性がありません**。 リレーショナルスキーマを使用する場合は、`union` に `meta:immutableTags` タグを適用しないでください。 この設定は、UI ではブロックされますが、現在は API でブロックされていません。 結合スキーマの動作について詳しくは、[ 結合エンドポイントガイド ](./unions.md) を参照してください。

**API 形式**

```http
POST /tenant/schemas
```

**リクエスト**

```shell
curl --request POST \
  --url https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas \
  -H 'Accept: application/vnd.adobe.xed+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
  "title": "marketing.customers",
  "type": "object",
  "description": "Schema of the Marketing Customers table.",
  "definitions": {
    "customFields": {
      "type": "object",
      "properties": {
        "customer_id": {
          "title": "Customer ID",
          "description": "Primary key of the customer table.",
          "type": "string",
          "minLength": 1
        },
        "name": {
          "title": "Name",
          "description": "Name of the customer.",
          "type": "string"
        },
        "email": {
          "title": "Email",
          "description": "Email of the customer.",
          "type": "string",
          "format": "email",
          "minLength": 1
        }
      },
      "required": ["customer_id"]
    }
  },
  "allOf": [
    {
      "$ref": "#/definitions/customFields",
      "meta:xdmType": "object"
    }
  ],
  "meta:extends": ["https://ns.adobe.com/xdm/data/adhoc-v2"],
  "meta:behaviorType": "record"
}
'
```

### リクエスト本文のプロパティ

| プロパティ | タイプ | 説明 |
| ------------------------------- | ------ | --------------------------------------------------------- |
| `title` | 文字列 | スキーマの名前を表示します。 |
| `description` | 文字列 | スキーマの目的の簡単な説明。 |
| `type` | 文字列 | リレーショナルスキーマには `"object"` を使用する必要があります。 |
| `definitions` | オブジェクト | スキーマフィールドを定義するルートレベルのオブジェクトが含まれます。 |
| `definitions.<name>.properties` | オブジェクト | フィールド名とデータタイプ。 |
| `allOf` | 配列 | ルートレベルのオブジェクト定義（例：`#/definitions/marketing_customers`）を参照します。 |
| `meta:extends` | 配列 | スキーマをリレーショナルとして識別するには、`"https://ns.adobe.com/xdm/data/adhoc-v2"` を含める必要があります。 |
| `meta:behaviorType` | 文字列 | `"record"` に設定します。 `"time-series"` は、有効かつ適切な場合にのみ使用してください。 |

>[!IMPORTANT]
>
>リレーショナルスキーマのスキーマ進化は、標準スキーマと同じ追加ルールに従います。 PATCH リクエストで新しいフィールドを追加できます。 フィールドの名前変更や削除などの変更は、データがデータセットに取り込まれていない場合にのみ許可されます。

**応答**

リクエストが成功すると、**HTTP 201 （作成済み）** 作成されたスキーマが返されます。

>[!NOTE]
>
>リレーショナルスキーマは、事前シードされたフィールド（id、タイムスタンプ、eventType など）を継承しません。 すべての必須フィールドをスキーマで明示的に定義します。

**応答の例**

```json
{
  "$id": "https://ns.adobe.com/<TENANT_ID>/schemas/<SCHEMA_UUID>",
  "meta:altId": "_<SCHEMA_ALT_ID>",
  "meta:resourceType": "schemas",
  "version": "1.0",
  "title": "marketing.customers",
  "description": "Schema of the Marketing Customers table.",
  "type": "object",
  "definitions": {
    "marketing_customers": {
      "type": "object",
      "properties": {
        "customer_id": {
          "title": "Customer ID",
          "description": "Primary key of the customer table.",
          "type": "string",
          "minLength": 1
        },
        "name": {
          "title": "Name",
          "description": "Name of the customer.",
          "type": "string"
        },
        "email": {
          "title": "Email",
          "description": "Email of the customer.",
          "type": "string",
          "format": "email",
          "minLength": 1
        }
      },
      "required": ["customer_id"]
    }
  },
  "allOf": [
    { "$ref": "#/definitions/marketing_customers" }
  ],
  "meta:extends": ["https://ns.adobe.com/xdm/data/adhoc-v2"],
  "meta:behaviorType": "record",
  "meta:containerId": "tenant"
}
```

### 応答本文のプロパティ

| プロパティ | タイプ | 説明 |
| ------------------- | ------ | -------------------------- |
| `$id` | 文字列 | 作成されたスキーマの一意の URL。 後続の API 呼び出しでを使用します。 |
| `meta:altId` | 文字列 | スキーマの代替識別子。 |
| `meta:resourceType` | 文字列 | リソースタイプ （常に `"schemas"`）。 |
| `version` | 文字列 | 作成時に割り当てられるスキーマバージョン。 |
| `title` | 文字列 | スキーマの表示名。 |
| `description` | 文字列 | スキーマの目的の簡単な説明。 |
| `type` | 文字列 | スキーマタイプ。 |
| `definitions` | オブジェクト | スキーマで使用される再利用可能なオブジェクトまたはフィールドグループを定義します。 これは通常、メインデータ構造を含み、スキーマのルートを定義するために `allOf` 配列で参照されます。 |
| `allOf` | 配列 | 1 つ以上の定義（例：`#/definitions/marketing_customers`）を参照して、スキーマのルートオブジェクトを指定します。 |
| `meta:extends` | 配列 | スキーマをリレーショナル（`adhoc-v2`）として識別します。 |
| `meta:behaviorType` | 文字列 | 動作タイプ （有効な場合は `record` または `time-series`）。 |
| `meta:containerId` | 文字列 | スキーマが格納されるコンテナ（例：`tenant`）。 |

フィールドを作成した後でリレーショナルスキーマに追加するには、[PATCH リクエスト ](#patch) を実行します。 リレーショナルスキーマは、継承も自動進化もしません。 フィールド名の変更やフィールドの削除などの構造変更は、データがデータセットに取り込まれていない場合にのみ使用できます。 データが存在する場合は、**追加的な変更** （新しいフィールドの追加など）のみがサポートされます。

新しいルートレベルフィールド（ルート定義またはルート `properties` 定内）を追加できますが、既存のフィールドのタイプを削除、名前変更、変更することはできません。

>[!CAUTION]
>
>スキーマを使用してデータセットが初期化されると、スキーマ進化が制限されます。 データを取り込むと、フィールドを削除または変更することはできないので、事前にフィールド名とタイプを慎重に計画します。

## スキーマの更新 {#put}

PUT操作を通じて、スキーマ全体を置き換えることができます。つまり、リソースを基本的に書き換えます。 PUT リクエストを通じてスキーマを更新する場合、本文には、POST リクエストで [ 新しいスキーマの作成 ](#create) を行う際に必要なすべてのフィールドを含める必要があります。

>[!NOTE]
>
>スキーマを完全に置き換えるのではなく、一部のみを更新する場合は、[ スキーマの一部の更新 ](#patch) の節を参照してください。

**API 形式**

```http
PUT /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | 再書き込みするスキーマの `meta:altId` または URL エンコードされた `$id`。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、既存のスキーマを置き換えて、`title`、`description` および `allOf` 属性を変更します。

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

応答が成功すると、更新されたスキーマの詳細が返されます。

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

PATCH リクエストを使用して、スキーマの一部を更新できます。 [!DNL Schema Registry] は、`add`、`remove`、`replace` など、標準の JSON パッチ操作をすべてサポートしています。 JSON パッチについて詳しくは、[API の基本ガイド](../../landing/api-fundamentals.md#json-patch)を参照してください。

>[!NOTE]
>
>個々のフィールドを更新する代わりにリソース全体を新しい値に置き換える場合は、[PUT操作を使用したスキーマの置き換え ](#put) の節を参照してください。

最も一般的なPATCH操作の 1 つは、以前に定義したフィールドグループをスキーマに追加することです（以下の例に示を参照）。

**API 形式**

```http
PATCH /tenant/schemas/{SCHEMA_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | 更新するスキーマの URL エンコードされた `$id` URI または `meta:altId`。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエスト例では、新しいフィールドグループをスキーマに追加するには、そのフィールドグループの `$id` 値を `meta:extends` 配列と `allOf` 配列の両方に追加します。

リクエスト本文は配列の形式をとり、リストされた各オブジェクトは、個々のフィールドに対する特定の変更を表します。 各オブジェクトには、実行する操作（`op`）、操作を実行するフィールド（`path`）、その操作に含める情報（`value`）が含まれます。

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

応答には、両方の操作が正常に実行されたことが示されます。フィールドグループ `$id` が `meta:extends` 配列に追加され、フィールドグループ `$ref` への参照（`$id`）が `allOf` 配列に表示されるようになりました。

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

## リアルタイム顧客プロファイルで使用するスキーマを有効にする {#union}

スキーマを [ リアルタイム顧客プロファイル ](../../profile/home.md) に参加させるには、スキーマの `union` 配列に `meta:immutableTags` タグを追加する必要があります。 これを実現するには、該当するスキーマに対してPATCH リクエストを実行します。

>[!IMPORTANT]
>
>不変タグは、設定を目的としているが削除されないタグです。

**API 形式**

```http
PATCH /tenant/schemas/{SCHEMA_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | 有効にするスキーマの URL エンコードされた `$id` URI または `meta:altId`。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエスト例では、既存のスキーマに `meta:immutableTags` 配列を追加し、配列に `union` の単一の文字列値を指定して、プロファイルで使用できるようにします。

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

応答が成功すると、更新されたスキーマの詳細が返され、`meta:immutableTags` の配列が追加されたことが示されます。

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

これで、このスキーマのクラスの和集合を表示して、スキーマのフィールドが表されていることを確認できます。 詳しくは、[ 結合エンドポイントガイド ](./unions.md) を参照してください。

## スキーマの削除 {#delete}

場合によっては、スキーマをスキーマレジストリから削除する必要があります。 これを行うには、パスで指定されたスキーマ ID を使用してDELETE リクエストを実行します。

**API 形式**

```http
DELETE /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | 削除するスキーマの URL エンコードされた `$id` URI または `meta:altId`。 |

{style="table-layout:auto"}

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

スキーマに対して検索（GET）リクエストを試みることで、削除を確認できます。 リクエストには `Accept` ヘッダーを含める必要がありますが、スキーマがスキーマレジストリから削除されたので、HTTP ステータス 404 （見つかりません）が返されます。
