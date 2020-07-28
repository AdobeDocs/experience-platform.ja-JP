---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 複数のオブジェクトの検索
topic: developer guide
translation-type: tm+mt
source-git-commit: 73a492ba887ddfe651e0a29aac376d82a7a1dcc4
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 50%

---


# 複数のオブジェクトの検索

If you wish to view several specific objects, rather than making one request per object, [!DNL Catalog] provides a simple shortcut for requesting multiple objects of the same type. 1 つの GET リクエストで複数のオブジェクトを返すには、リクエストに ID のコンマ区切りリストを含めます。

>[!NOTE]
>
>Even when requesting specific [!DNL Catalog] objects, it is still best practice to `properties` query parameter to return only the properties you need.

**API 形式**

```http
GET /{OBJECT_TYPE}/{ID_1},{ID_2},{ID_3},{ID_4}
GET /{OBJECT_TYPE}/{ID_1},{ID_2},{ID_3},{ID_4}?properties={PROPERTY_1},{PROPERTY_2},{PROPERTY_3}
```

| `{OBJECT_TYPE}` | The type of [!DNL Catalog] object to be retrieved. 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{ID}` | 取得するいずれかのオブジェクトの識別子。|

**リクエスト**

次のリクエストには、データセット ID のコンマ区切りリストと、各データセットに対して返されるプロパティのコンマ区切りリストが含まれています。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets/5bde21511dd27b0000d24e95,5bda3a4228babc0000126377,5bceaa4c26c115000039b24b,5bb276b03a14440000971552,5ba9452f7de80400007fc52a?properties=name,description,files' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合、指定したデータセットのリストと、それぞれに対して要求したプロパティ（`name`、`description` および `files`）が返されます。

>[!NOTE]
>
>If a returned object does not contain one ore more of the requested properties indicated by the `properties` query, the response returns only the requested properties that it does include, as shown in ***`Sample Dataset 3`*** and ***`Sample Dataset 4`*** below.

```json
{
    "5ba9452f7de80400007fc52a": {
        "name": "Sample Dataset 1",
        "description": "Description of dataset.",
        "files": "@/dataSets/5ba9452f7de80400007fc52a/views/5ba9452f7de80400007fc52b/files"
    },
    "5bb276b03a14440000971552": {
        "name": "Sample Dataset 2",
        "description": "Description of dataset.",
        "files": "@/dataSets/5bb276b03a14440000971552/views/5bb276b01250b012f9acc75b/files"
    },
    "5bceaa4c26c115000039b24b": {
        "name": "Sample Dataset 3"
    },
    "5bda3a4228babc0000126377": {
        "name": "Sample Dataset 4",
        "files": "@/dataSets/5bda3a4228babc0000126377/views/5bda3a4228babc0000126378/files"
    },
    "5bde21511dd27b0000d24e95": {
        "name": "Sample Dataset 5",
        "description": "Description of dataset.",
        "files": "@/dataSets/5bde21511dd27b0000d24e95/views/5bde21511dd27b0000d24e96/files"
    }
}
```
