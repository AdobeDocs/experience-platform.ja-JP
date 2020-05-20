---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ型の作成
topic: developer guide
translation-type: tm+mt
source-git-commit: b0d8c8ee4df11d601d8feb122c70a9cd5d7d5b77
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%

---


# データ型の作成

組織で複数の方法で使用したい共通のデータ構造がある場合は、データタイプを定義する必要があります。 データ型を使用すると、複数フィールド構造を一貫して使用でき、ミックスインよりも柔軟に使用できます。これは、データ型をフィールドの一部として追加することで、スキーマ内のどこにでも含めるこ `type` とができるからです。

つまり、データ型を使用すると、1回だけオブジェクト階層を定義し、他のスカラー型と同じ方法でフィールド内で参照できます。

**API形式**

```http
POST /tenant/datatypes
```

**リクエスト**

データ型の定義には、フィールド `meta:extends` や `meta:intendedToExtend` フィールドは不要です。また、競合を避けるために、フィールドを入れ子にする必要もありません。

```SHELL
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/datatypes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "title":"Property Construction",
        "description":"Information related to the property construction",
        "type":"object",
        "properties": {
          "yearBuilt": {
            "type":"integer",
            "title": "Year Built",
            "description": "The year the property was constructed."
          },
          "propertyType": {
            "type":"string",
            "title": "Property Type",
            "description": "Type of building or structure in which the property exists.",
            "enum": [
              "freeStanding",
              "mall",
              "shoppingCenter"
            ],
            "meta:enum": {
              "freeStanding": "Free Standing Building",
              "mall": "Mall Space",
              "shoppingCentre": "Shopping Center"
            }
          }
        } 
      }'
```

**応答**

正常な応答は、HTTPステータス201（作成済み）と、、、など、新しく作成されたデータタイプの詳細を含むペイロード `$id`を返し `meta:altId`ま `version`す。 これらの3つの値は読み取り専用で、スキーマレジストリによって割り当てられます。

```JSON
{
    "title": "Property Construction",
    "description": "Information related to the property construction",
    "type": "object",
    "properties": {
        "yearBuilt": {
            "type": "integer",
            "title": "Year Built",
            "description": "The year the property was constructed.",
            "meta:xdmType": "int"
        },
        "constructionType": {
            "type": "string",
            "title": "Construction Type",
            "description": "Type of building or structure in which the property exists.",
            "enum": [
                "freeStanding",
                "mall",
                "shoppingCenter"
            ],
            "meta:enum": {
                "freeStanding": "Free Standing Building",
                "mall": "Mall Space",
                "shoppingCentre": "Shopping Center"
            },
            "meta:xdmType": "string"
        }
    },
    "meta:abstract": true,
    "meta:extensible": true,
    "meta:containerId": "tenant",
    "imsOrg": "{IMS_ORG}",
    "meta:altId": "_{TENANT_ID}.datatypes.24c643f618647344606222c494bd0102",
    "meta:xdmType": "object",
    "$id": "https://ns.adobe.com/{TENANT_ID}/datatypes/24c643f618647344606222c494bd0102",
    "version": "1.0",
    "meta:resourceType": "datatypes",
    "meta:registryMetadata": {
        "repo:createDate": 1552087079285,
        "repo:lastModifiedDate": 1552087079285,
        "xdm:createdClientId": "{CREATED_CLIENT}",
        "xdm:repositoryCreatedBy": "{CREATED_BY}"
    }
}
```

テナントコンテナ内のすべてのデータ型をリストするためのGET要求を実行すると、プロパティ構築のデータ型が含まれるようになりました。 また、URLエンコードされた `$id` URIを使用してルックアップ(GET)リクエストを実行し、新しいデータ型を直接表示することもできます。 ルックアップリクエストの場合は、必ず「同意する」ヘッダー `version` にを含めてください。
