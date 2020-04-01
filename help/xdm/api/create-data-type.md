---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ型の作成
topic: developer guide
translation-type: tm+mt
source-git-commit: b0d8c8ee4df11d601d8feb122c70a9cd5d7d5b77

---


# データ型の作成

組織が複数の方法で使用する一般的なデータ構造がある場合は、データタイプを定義します。 データ型を使用すると、複数フィールド構造を一貫して使用でき、ミックスインよりも柔軟に使用できます。これは、データ型をフィールドとして追加することで、スキーマ内の任意の場所に組み込める `type` からです。

つまり、データ型を使用すると、1つのオブジェクト階層を1回定義し、他のスカラー型と同じ方法でフィールド内で参照できます。

**API形式**

```http
POST /tenant/datatypes
```

**リクエスト**

データ型を定義する際に、フィールドやフィー `meta:extends` ルドは必 `meta:intendedToExtend` 要なく、フィールドの入れ子にする必要もありません。

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

成功した応答は、HTTPステータス201（作成済み）と、、およびを含む、新しく作成されたデータタイプの詳細を含むペイロ `$id`ードを `meta:altId`返しま `version`す。 これらの3つの値は読み取り専用で、割り当てはスキーマレジストリです。

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

テナントコンテナ内のすべてのリスト型に対してGET要求を実行すると、プロパティ構築のデータ型が含まれるようになりました。 また、URLエンコードされた `$id` URIを使用して参照(GET)リクエストを実行し、新しいデータ型を直接表示することもできます。 参照リクエストのAcceptヘッ `version` ダーにを必ず含めてください。
