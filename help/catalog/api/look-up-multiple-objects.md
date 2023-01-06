---
keywords: Experience Platform；ホーム；人気の高いトピック；カタログ；複数のオブジェクト参照；api
solution: Experience Platform
title: 複数のカタログオブジェクトの検索
description: オブジェクトごとに 1 つのリクエストを実行する代わりに、複数のオブジェクトを表示する場合、カタログには同じ種類のオブジェクトを複数リクエストするためのシンプルなショートカットが用意されています。1 つの GET リクエストで複数のオブジェクトを返すには、リクエストに ID のコンマ区切りリストを含めます。
exl-id: b2329b32-6139-4557-aff3-a584e03b09f3
source-git-commit: 74867f56ee13430cbfd9083a916b7167a9a24c01
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 49%

---

# 複数のカタログオブジェクトの検索

オブジェクトごとに 1 回のリクエストを行うのではなく、複数の特定のオブジェクトを表示する場合は、 [!DNL Catalog] には、同じタイプの複数のオブジェクトをリクエストするためのシンプルなショートカットが用意されています。 1 つの GET リクエストで複数のオブジェクトを返すには、リクエストに ID のコンマ区切りリストを含めます。

>[!NOTE]
>
>特定の [!DNL Catalog] オブジェクトの場合は、 `properties` クエリパラメーターを使用して、必要なプロパティのみを返します。

**API 形式**

```http
GET /{OBJECT_TYPE}/{ID_1},{ID_2},{ID_3},{ID_4}
GET /{OBJECT_TYPE}/{ID_1},{ID_2},{ID_3},{ID_4}?properties={PROPERTY_1},{PROPERTY_2},{PROPERTY_3}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{OBJECT_TYPE}` | のタイプ [!DNL Catalog] 取得するオブジェクト。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{ID}` | 取得するいずれかの特定のオブジェクトの識別子。 |

**リクエスト**

次のリクエストには、データセット ID のコンマ区切りリストと、各データセットに対して返されるプロパティのコンマ区切りリストが含まれています。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets/5bde21511dd27b0000d24e95,5bda3a4228babc0000126377,5bceaa4c26c115000039b24b,5bb276b03a14440000971552,5ba9452f7de80400007fc52a?properties=name,description,files' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合、指定したデータセットのリストと、それぞれに対して要求したプロパティ（`name`、`description` および `files`）が返されます。

>[!NOTE]
>
>返されるオブジェクトに、 `properties` クエリを指定した場合、応答は、に示すように、含まれるリクエストされたプロパティのみを返します。 ***`Sample Dataset 3`*** および ***`Sample Dataset 4`*** 下

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
