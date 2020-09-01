---
keywords: Experience Platform;home;popular topics;api;API;XDM;XDM system;;experience data model;Experience data model;Experience Data Model;data model;Data Model;schema registry;Schema Registry;schema;Schema;schemas;Schemas;create
solution: Experience Platform
title: スキーマの作成
description: 'スキーマは、Experience Platform に取得するデータの青写真と見なすことができます。各スキーマは、クラスと 0 個以上の mixin で構成されます。つまり、スキーマを定義するために mixin を追加する必要はありませんが、ほとんどの場合、少なくとも 1 つの mixin が使用されます。 '
topic: developer guide
translation-type: tm+mt
source-git-commit: 74a4a3cc713cc068be30379e8ee11572f8bb0c63
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 91%

---


# スキーマの作成

A schema can be thought of as the blueprint for the data you wish to ingest into [!DNL Experience Platform]. 各スキーマは、クラスと 0 個以上の mixin で構成されます。つまり、スキーマを定義するために mixin を追加する必要はありませんが、ほとんどの場合、少なくとも 1 つの mixin が使用されます。

スキーマ構成プロセスは、クラスの割り当てから開始されます。このクラスは、データ（レコードまたは時系列）の主要な動作要素と、取得されるデータを記述するために必要な最小フィールドを定義します。

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
| `allOf > $ref` | 新しいスキーマが実装するクラスの `$id` 値。 |

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

GET リクエストを実行してテナントコンテナ内のすべてのスキーマを一覧表示すると、プロパティ情報スキーマが含まれるようになります。または、URL エンコードされた `$id` URI を使用してルックアップ（GET）リクエストを実行し、新しいスキーマを直接表示できます。すべてのルックアップリクエストの Accept ヘッダーには必ず `version` を含めてください。
