---
keywords: Experience Platform；ホーム；人気のトピック；カタログ；複数オブジェクト参照；api
solution: Experience Platform
title: 複数のカタログ オブジェクトを検索する
description: オブジェクトごとに 1 つのリクエストを実行する代わりに、複数のオブジェクトを表示する場合、カタログには同じ種類のオブジェクトを複数リクエストするためのシンプルなショートカットが用意されています。1 つの GET リクエストで複数のオブジェクトを返すには、リクエストに ID のコンマ区切りリストを含めます。
exl-id: b2329b32-6139-4557-aff3-a584e03b09f3
source-git-commit: 99837f7aa803f3f992dce2127089bff6279c7008
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 49%

---

# 複数のカタログ オブジェクトを検索する

オブジェクトごとに 1 つのリクエストを実行するのではなく、複数の特定のオブジェクトを表示する場合は、同じタイプの複数のオブジェクト [!DNL Catalog] リクエストするための簡単なショートカットが用意されています。 1 つの GET リクエストで複数のオブジェクトを返すには、リクエストに ID のコンマ区切りリストを含めます。

>[!NOTE]
>
>特定の [!DNL Catalog] オブジェクトをリクエストする場合でも、必要なプロパティのみを返すクエリパラメーター `properties` 使用することをお勧めします。

**API 形式**

```http
GET /{OBJECT_TYPE}/{ID_1},{ID_2},{ID_3},{ID_4}
GET /{OBJECT_TYPE}/{ID_1},{ID_2},{ID_3},{ID_4}?properties={PROPERTY_1},{PROPERTY_2},{PROPERTY_3}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{OBJECT_TYPE}` | 取得 [!DNL Catalog] るオブジェクトのタイプ。 有効なオブジェクトは次のとおりです。 <ul><li>`batches`</li><li>`dataSets`</li><li>`dataSetFiles`</li></ul> |
| `{ID}` | 取得する特定のオブジェクトの 1 つの識別子。 |

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
>返されたオブジェクトに、`properties` クエリで示されている要求されたプロパティが 1 つ以上含まれていない場合、次の ***`Sample Dataset 3`*** と ***`Sample Dataset 4`*** に示すように、応答は、含まれている要求されたプロパティのみを返します。

```json
{
    "5ba9452f7de80400007fc52a": {
        "name": "Sample Dataset 1",
        "description": "Description of dataset.",
        "files": "@/dataSetFiles?dataSetId=5ba9452f7de80400007fc52a"
    },
    "5bb276b03a14440000971552": {
        "name": "Sample Dataset 2",
        "description": "Description of dataset.",
        "files": "@/dataSetFiles?dataSetId=5bb276b03a14440000971552"
    },
    "5bceaa4c26c115000039b24b": {
        "name": "Sample Dataset 3"
    },
    "5bda3a4228babc0000126377": {
        "name": "Sample Dataset 4",
        "files": "@/dataSetFiles?dataSetId=5bda3a4228babc0000126377"
    },
    "5bde21511dd27b0000d24e95": {
        "name": "Sample Dataset 5",
        "description": "Description of dataset.",
        "files": "@/dataSetFiles?dataSetId=5bde21511dd27b0000d24e95"
    }
}
```
