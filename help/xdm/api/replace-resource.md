---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リソースの置換
topic: developer guide
translation-type: tm+mt
source-git-commit: d04bf35e49488ab7d5e07de91eb77d0d9921b6fa
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 89%

---


# リソースの置換

The [!DNL Schema Registry] allows you to replace an entire resource through a PUT operation. この操作は基本的にリソースを書き換えるので、リクエスト本文には、POST リクエストを使用して新しいリソースを作成する際に必要となるフィールドをすべて含める必要があります。

この方法は、リソース内の多くの情報を一度に更新する場合に特に便利です。

>[!NOTE]
>
>リソース全体を置換するのではなく、リソースの一部のみを更新する場合は、[PATCH 操作を使用したリソースの更新](update-resource.md)に関するドキュメントを参照してください。

**API 形式**

PUT リクエストは、テナントコンテナで定義したリソースに対してのみ実行できます。

```http
PUT /tenant/{RESOURCE_TYPE}/{RESOURCE_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{RESOURCE_TYPE}` | The type of resource to be updated from the [!DNL Schema Library]. 有効なタイプは、`datatypes`、`mixins`、`schemas` および `classes` です。 |
| `{RESOURCE_ID}` | リソースの URL エンコードされた `$id` URI または `meta:altId`。 |

**リクエスト**

このサンプルリクエストでは、前の例で作成したプロパティ構築データイプが置換されます。リクエスト本文は、データタイプの作成に使用される POST リクエストに似ていますが、以前に定義された値に代わる新しい値を持つ更新済みのフィールドが含まれているという点が異なります。

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

リクエストが成功した場合、データタイプの詳細が返され、リクエストで指定された更新済みフィールドと値が示されます。

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
