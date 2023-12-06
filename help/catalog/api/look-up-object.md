---
keywords: Experience Platform；ホーム；人気の高いトピック；カタログ；オブジェクト参照；API
solution: Experience Platform
title: カタログオブジェクトの検索
description: 特定のカタログオブジェクトの一意の ID がわかっている場合は、GET リクエストを実行してそのオブジェクトの詳細を表示できます。
exl-id: fd6fbe72-0108-4be3-a065-c753e7a19d24
source-git-commit: 0331b6bbd22255cab92c93070dda1ffaed5bbbcb
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 44%

---

# カタログオブジェクトの検索

特定の [!DNL Catalog] オブジェクトの場合は、GETリクエストを実行して、そのオブジェクトの詳細を表示できます。

>[!NOTE]
>
>特定のオブジェクトを表示する場合は、次の操作を行うことをお勧めします。 [プロパティでフィルター](filter-data.md) を呼び出し、目的のプロパティのみを返します。

**API 形式**

```http
GET /{OBJECT_TYPE}/{OBJECT_ID}
GET /{OBJECT_TYPE}/{OBJECT_ID}?properties={PROPERTY_1},{PROPERTY_2},{PROPERTY_3}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 次のタイプの [!DNL Catalog] 取得するオブジェクト。 有効なオブジェクトは次のとおりです。 <ul><li>`batches`</li><li>`dataSets`</li><li>`dataSetFiles`</li></ul> |
| `{OBJECT_ID}` | 取得する特定のオブジェクトの識別子。 |

**リクエスト**

次のリクエストでは、ID でデータセットを取得し、その ID を返します `name`, `description`, `tags`、および `files` プロパティ。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets/5ba9452f7de80400007fc52a?properties=name,description,tags,files' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、指定されたデータセッを返します。本文にはリクエストされた `properties` のみが含まれます。

```json
{
    "5ba9452f7de80400007fc52a": {
        "name": "Sample Dataset",
        "description": "Sample dataset containing important data.",
        "tags": {
            "adobe/pqs/table": [
                "sample_dataset"
            ]
        },
        "files": "@/dataSetFiles?dataSetId=5ba9452f7de80400007fc52a"
    }
}
```

>[!NOTE]
>
>値の前にが付いているプロパティ `@` は、相互に関連するオブジェクトを表します。 これらのオブジェクトの詳細を表示する手順については、付録の「[相互関連オブジェクトの表示](appendix.md#view-interrelated-objects)」の節を参照してください。
