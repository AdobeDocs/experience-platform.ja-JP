---
keywords: Experience Platform；ホーム；人気のあるトピック；API;API;XDM;XDMシステム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；スキーマレジストリ；スキーマ；スキーマ；スキーマ；スキーマ；スキーマ；作成
solution: Experience Platform
title: スキーマ API エンドポイント
description: スキーマレジストリAPIの/schemasエンドポイントを使用すると、エクスペリエンスアプリケーション内のXDMスキーマをプログラムで管理できます。
topic-legacy: developer guide
exl-id: d0bda683-9cd3-412b-a8d1-4af700297abf
source-git-commit: 8133804076b1c0adf2eae5b748e86a35f3186d14
workflow-type: tm+mt
source-wordcount: '1454'
ht-degree: 18%

---

# スキーマエンドポイント

スキーマは、Adobe Experience Platformに取り込むデータのブループリントと見なすことができます。 各スキーマは、クラスと0個以上のスキーマフィールドグループで構成されます。 [!DNL Schema Registry] APIの`/schemas`エンドポイントを使用すると、エクスペリエンスアプリケーション内のスキーマをプログラムで管理できます。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Schema Registry] API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) の一部です。続行する前に、[はじめにのガイド](./getting-started.md)を参照して、関連ドキュメントへのリンク、このドキュメントのAPI呼び出し例の読み方、およびExperience PlatformAPIを正しく呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## スキーマのリストの取得 {#list}

`global`コンテナまたは`tenant`コンテナの下にあるすべてのスキーマをリストするには、それぞれ`/global/schemas`または`/tenant/schemas`にGETリクエストを送信します。

>[!NOTE]
>
>リソースをリストする場合、スキーマレジストリでは結果セットが300項目に制限されます。 この制限を超えるリソースを返すには、ページングパラメーターを使用する必要があります。 また、追加のクエリパラメーターを使用して結果をフィルタリングし、返されるリソースの数を減らすこともお勧めします。 詳しくは、付録ドキュメントの[クエリパラメーター](./appendix.md#query)の節を参照してください。

**API 形式**

```http
GET /{CONTAINER_ID}/schemas?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | 取得するスキーマを格納するコンテナ：`global`(Adobeが作成したスキーマの場合)または`tenant`（組織が所有するスキーマの場合）。 |
| `{QUERY_PARAMS}` | 結果をフィルターするオプションのクエリパラメーター。使用可能なパラメーターのリストについては、付録の[ドキュメント](./appendix.md#query)を参照してください。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、`tenant`コンテナからスキーマのリストを取得し、`orderby`クエリパラメーターを使用して結果を`title`属性で並べ替えます。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas?orderby=title \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、リクエストで送信される`Accept`ヘッダーに応じて異なります。 スキーマのリストには、次の`Accept`ヘッダーを使用できます。

| `Accept` ヘッダー | 説明 |
| --- | --- |
| `application/vnd.adobe.xed-id+json` | 各リソースの短い概要を返します。 これは、リソースをリストする際に推奨されるヘッダーです。 (上限：300) |
| `application/vnd.adobe.xed+json` | 各リソースの完全なJSONスキーマを、元の`$ref`と`allOf`を含めて返します。 (上限：300) |

{style=&quot;table-layout:auto&quot;}

**応答**

上記のリクエストでは`application/vnd.adobe.xed-id+json` `Accept`ヘッダーが使用されていたので、応答には各スキーマの`title`、`$id`、`meta:altId`および`version`属性のみが含まれています。 他の`Accept`ヘッダー(`application/vnd.adobe.xed+json`)を使用すると、各スキーマのすべての属性が返されます。 応答で必要な情報に応じて、適切な`Accept`ヘッダーを選択します。

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
| `{CONTAINER_ID}` | 取得するスキーマを格納するコンテナ：`global`(Adobeが作成したスキーマの場合)または`tenant`（組織が所有するスキーマの場合）。 |
| `{SCHEMA_ID}` | 検索するスキーマの`meta:altId`またはURLエンコードされた`$id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、パス内の`meta:altId`値で指定されたスキーマを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.f579a0b5f992c69458ea408ec36571f7da9de15901bab116 \
  -H 'Accept: application/vnd.adobe.xed+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、リクエストで送信される`Accept`ヘッダーに応じて異なります。 すべての検索リクエストでは、`version`を`Accept`ヘッダーに含める必要があります。 次の`Accept`ヘッダーを使用できます。

| `Accept` ヘッダー | 説明 |
| ------- | ------------ |
| `application/vnd.adobe.xed+json; version=1` | `$ref` および `allOf` で生、タイトルと説明を含む |
| `application/vnd.adobe.xed-full+json; version=1` | `$ref` および `allOf` を解決、タイトルと説明を含む |
| `application/vnd.adobe.xed-notext+json; version=1` | `$ref` および `allOf` で生、タイトルや説明なし |
| `application/vnd.adobe.xed-full-notext+json; version=1` | `$ref` および `allOf` で解決、タイトルや説明なし |
| `application/vnd.adobe.xed-full-desc+json; version=1` | `$ref` および `allOf` で解決、説明を含む |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、スキーマの詳細を返します。 返されるフィールドは、リクエストで送信される`Accept`ヘッダーに応じて異なります。 様々な`Accept`ヘッダーを試して、応答を比較し、使用事例に最適なヘッダーを判断します。

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
>以下の呼び出し例は、APIでスキーマを作成する方法のベースライン例です（クラスの構成要件は最小限で、フィールドグループは不要）。 フィールドグループとデータ型を使用してフィールドを割り当てる方法など、APIでスキーマを作成する方法に関する完全な手順については、[スキーマ作成に関するチュートリアル](../tutorials/create-schema-api.md)を参照してください。

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
| `allOf` | オブジェクトの配列。各オブジェクトは、スキーマがフィールドを実装するクラスまたはフィールドグループを参照します。 各オブジェクトには、1つのプロパティ(`$ref`)が含まれます。このプロパティの値は、新しいスキーマが実装するクラスまたはフィールドグループの`$id`を表します。 1つのクラスに、0個以上の追加フィールドグループを指定する必要があります。 上記の例では、`allOf`配列内の単一のオブジェクトがスキーマのクラスです。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、HTTP ステータス 201（作成済み）と、新しく作成されたスキーマの詳細（`$id`、`meta:altId`、`version`など）を含むペイロードを返します。これらの値は読み取り専用で、[!DNL Schema Registry]によって割り当てられます。

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

[に対してGETリクエストを実行すると、テナントコンテナ内のすべてのスキーマ](#list)に新しいスキーマが含まれるようになります。 URLエンコードされた`$id` URIを使用して[検索(GET)リクエスト](#lookup)を実行し、新しいスキーマを直接表示できます。

PATCHにフィールドを追加するには、[スキーマ操作](#patch)を実行して、スキーマの`allOf`配列と`meta:extends`配列にフィールドグループを追加します。

## スキーマの更新 {#put}

スキーマ操作を使用してPUT全体を置き換え、基本的にリソースを書き直すことができます。 PUTリクエストを通じてスキーマを更新する場合、本文には、POSTリクエストで[新しいスキーマ](#create)を作成する際に必要となるすべてのフィールドを含める必要があります。

>[!NOTE]
>
>スキーマを完全に置き換えるのではなく、スキーマの一部のみを更新する場合は、[スキーマの一部の更新](#patch)の節を参照してください。

**API 形式**

```http
PUT /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | 書き直すスキーマの`meta:altId`またはURLエンコードされた`$id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、既存のスキーマを置き換え、`title`、`description`および`allOf`属性を変更します。

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

## スキーマの一部の更新 {#patch}

スキーマリクエストを使用して、スキーマの一部をPATCHできます。 [!DNL Schema Registry]は、`add`、`remove`、`replace`を含む、すべての標準的なJSONパッチ操作をサポートします。 JSONパッチの詳細については、『[APIの基本ガイド](../../landing/api-fundamentals.md#json-patch)』を参照してください。

>[!NOTE]
>
>個々のフィールドを更新する代わりに、リソース全体を新しい値に置き換える場合は、「PUT」操作](#put)を使用したスキーマの置き換え[の節を参照してください。

最も一般的なPATCH操作の1つは、次の例に示すように、以前に定義したフィールドグループをスキーマに追加することです。

**API 形式**

```http
PATCH /tenant/schema/{SCHEMA_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | 更新するスキーマのURLエンコードされた`$id` URIまたは`meta:altId`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエスト例では、フィールドグループの`$id`値を`meta:extends`配列と`allOf`配列の両方に追加して、新しいフィールドグループをスキーマに追加します。

リクエスト本文は配列の形式をとり、リストされた各オブジェクトは個々のフィールドに対する特定の変更を表します。 各オブジェクトには、実行する操作(`op`)、操作を実行するフィールド(`path`)、およびその操作に含める情報(`value`)が含まれます。

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

応答には、両方の操作が正常に実行されたことが示されます。フィールドグループ`$id`が`meta:extends`配列に追加され、フィールドグループ`$id`への参照(`$ref`)が`allOf`配列に表示されます。

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

## リアルタイム顧客プロファイルでのスキーマ使用の有効化 {#union}

スキーマが[リアルタイム顧客プロファイル](../../profile/home.md)に参加するには、スキーマの`meta:immutableTags`配列に`union`タグを追加する必要があります。 これは、対象のスキーマに対してPATCHリクエストを作成することで実現できます。

>[!IMPORTANT]
>
>Immutable タグは、設定を目的としたもので、削除には使用できません。

**API 形式**

```http
PATCH /tenant/schema/{SCHEMA_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | 有効にするスキーマのURLエンコードされた`$id` URIまたは`meta:altId`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次の例のリクエストでは、既存のスキーマに`meta:immutableTags`配列を追加し、配列に`union`という1つの文字列値を指定して、プロファイルでの使用を有効にします。

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

正常な応答は、更新されたスキーマの詳細を返し、`meta:immutableTags`配列が追加されたことを示します。

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

これで、このスキーマのクラスの和集合を表示して、スキーマのフィールドが表されていることを確認できます。 詳しくは、[和集合エンドポイントのガイド](./unions.md)を参照してください。

## スキーマの削除 {#delete}

スキーマレジストリからスキーマを削除する必要が生じる場合があります。 これは、パスで指定されたDELETEIDを使用してスキーマリクエストを実行することでおこなわれます。

**API 形式**

```http
DELETE /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | 削除するスキーマのURLエンコードされた`$id` URIまたは`meta:altId`。 |

{style=&quot;table-layout:auto&quot;}

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

スキーマに対して検索(GET)リクエストを試行すると、削除を確認できます。 リクエストに`Accept`ヘッダーを含める必要がありますが、スキーマがスキーマレジストリから削除されたので、HTTPステータス404（見つかりません）を受け取る必要があります。
