---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リソースの置換
topic: developer guide
translation-type: tm+mt
source-git-commit: 67826f838951b3202a6a04321c28daa8ee883d20
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 1%

---


# リソースの置換

スキーマレジストリを使用すると、PUT操作でリソース全体を置き換えることができます。 この操作は基本的にリソースを書き直すので、リクエスト本文には、POSTリクエストを使用して新しいリソースを作成する際に必要となるフィールドをすべて含める必要があります。

この方法は、リソース内の多くの情報を一度に更新する場合に特に便利です。

>[!NOTE] リソース全体を置き換えるのではなく、リソースの一部だけを更新したい場合は、PATCH操作を使ってリソースを [更新するドキュメントを参照してください](update-resource.md)。

**API形式**

PUT要求は、テナントコンテナで定義したリソースに対してのみ実行できます。

```http
PUT /tenant/{RESOURCE_TYPE}/{RESOURCE_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{RESOURCE_TYPE}` | スキーマライブラリから更新するリソースの種類です。 有効なタイプは、 `datatypes`、、、お `mixins`よび `schemas``classes`です。 |
| `{RESOURCE_ID}` | URLエンコードされた `$id` URIまたはリソース `meta:altId` のURIです。 |

**リクエスト**

このサンプル要求は、前の例で作成されたプロパティ構築データ型を置き換えます。 リクエスト本文は、データ型の作成に使用されるPOSTリクエストと似ていますが、以前に定義した値に代わる新しい値を持つ更新済みのフィールドセットが含まれる点が異なります。

```SHELL
curl -X PUT \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/mixins/_{TENANT_ID}.datatypes.24c643f618647344606222c494bd0102 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "title":"Property Construction",
        "description":"Information related to the construction of the property.",
        "type":"object",
        "properties": {
          "dateOpened": {
            "type":"string",
            "format": "date",
            "title": "Date Opened",
            "description": "The date the property was first opened."
          },
          "propertyType": {
            "type":"string",
            "title": "Property Type",
            "description": "Type of building or structure in which the property exists.",
            "enum": [
              "standAlone",
              "highRise",
              "stripMall"
            ],
            "meta:enum": {
              "standAlone": "Stand-Alone Building",
              "highRise": "High Rise Building",
              "stripMall": "Strip Mall"
            }
          },
          "constructionCompany": {
            "type": "string",
            "title": "Construction Company",
            "description": "Name of the construction company that completed the construction of the property."
          },
          "totalSquareFootage": {
            "type": "integer",
            "title": "Total Square Footage",
            "description": "Total square footage of the property."
          }
        } 
      }'
```

**応答**

成功した応答は、データ型の詳細を返し、更新されたフィールドと値がリクエストに指定されたとおりに表示されます。

```JSON
{
    "title": "Property Construction",
    "description": "Information related to the construction of the property.",
    "type": "object",
    "properties": {
        "dateOpened": {
            "type": "string",
            "format": "date",
            "title": "Date Opened",
            "description": "The date the property was first opened.",
            "meta:xdmType": "date"
        },
        "propertyType": {
            "type": "string",
            "title": "Property Type",
            "description": "Type of building or structure in which the property exists.",
            "enum": [
                "standAlone",
                "highRise",
                "stripMall"
            ],
            "meta:enum": {
                "standAlone": "Stand-Alone Building",
                "highRise": "High Rise Building",
                "stripMall": "Strip Mall"
            },
            "meta:xdmType": "string"
        },
        "constructionCompany": {
            "type": "string",
            "title": "Construction Company",
            "description": "Name of the construction company that completed the construction of the property.",
            "meta:xdmType": "string"
        },
        "totalSquareFootage": {
            "type": "integer",
            "title": "Total Square Footage",
            "description": "Total square footage of the property.",
            "meta:xdmType": "int"
        }
    },
    "meta:abstract": true,
    "meta:extensible": true,
    "meta:containerId": "tenant",
    "imsOrg": "{IMS_ORG}",
    "meta:altId": "_{TENANT_ID}.datatypes.24c643f618647344606222c494bd0102",
    "meta:xdmType": "object",
    "$id": "https://ns.adobe.com/{TENANT_ID}/datatypes/24c643f618647344606222c494bd0102",
    "version": "1.2",
    "meta:resourceType": "datatypes",
    "meta:registryMetadata": {
        "repo:createDate": 1552087079285,
        "repo:lastModifiedDate": 1552090569013,
        "xdm:createdClientId": "{CREATED_CLIENT}",
        "xdm:repositoryCreatedBy": "{CREATED_BY}"
    }
}
```
