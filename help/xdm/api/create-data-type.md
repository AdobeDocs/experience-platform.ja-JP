---
keywords: Experience Platform;home;popular topics;data type;data types;Data types;Data type;datatype;Datatype
solution: Experience Platform
title: データタイプの作成
topic: developer guide
description: '組織が複数の方法で使用する一般的なデータ構造がある場合は、データタイプを定義します。データ型を使用すると、複数フィールド構造を一貫して使用でき、ミックスインよりも柔軟に使用できます。これは、データ型をフィールドの「型」として追加することで、スキーマ内のどこにでも含めることができるからです。 '
translation-type: tm+mt
source-git-commit: cc81d590f308c7e2677cec000c27e8aca42437f5
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 83%

---


# データタイプの作成

組織が複数の方法で使用する一般的なデータ構造がある場合は、データタイプを定義します。データタイプは、フィールドの `type` として追加することでスキーマの任意の場所に含めることができるため、mixin よりも柔軟性が高く、マルチフィールド構造の一貫した使用を可能にします。

つまり、データタイプを使用すると、1 つのオブジェクト階層を 1 回定義するだけで、他のスカラー型と同じ方法でフィールド内で参照できます。

**API 形式**

```http
POST /tenant/datatypes
```

**リクエスト**

データ型を定義する際に、`meta:extends` フィールドや `meta:intendedToExtend` フィールドは必要なく、衝突を避けるためにフィールドを入れ子にする必要もありません。

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

正常な応答は、HTTP ステータス 201（作成済み）と、新しく作成されたデータタイプの詳細（`$id`、`meta:altId`、`version`を含む）を含むペイロードを返します。These three values are read-only and are assigned by the [!DNL Schema Registry].

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

GET リクエストを実行してテナントコンテナ内のすべてのデータタイプを一覧表示すると、プロパティ構築データタイプが含まれるようになります。また、URL エンコードされた `$id` URI を使用して検索（GET）リクエストを実行し、新しいデータタイプを直接表示することもできます。検索リクエストの Accept ヘッダーに `version` を必ず含めてください。
