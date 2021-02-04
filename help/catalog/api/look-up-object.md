---
keywords: Experience Platform；ホーム；人気のあるトピック；カタログ；オブジェクト参照；api
solution: Experience Platform
title: オブジェクトの検索
topic: developer guide
description: '特定のカタログオブジェクトの一意の識別子がわかっている場合は、GET リクエストを実行してそのオブジェクトの詳細を表示できます。 '
translation-type: tm+mt
source-git-commit: dd1f508b93e8eac14e3c41fac9d8f49769d08f46
workflow-type: tm+mt
source-wordcount: '163'
ht-degree: 77%

---


# オブジェクトの検索

特定の[!DNL Catalog]オブジェクトの固有な識別子がわかっている場合は、そのオブジェクトの詳細を表示するためにGETリクエストを実行できます。

>[!NOTE]
>
> 特定のオブジェクトを表示する場合も、[プロパティでフィルター](filter-data.md)し、目的のプロパティのみを返すのがベストプラクティスです。

**API 形式**

```http
GET /{OBJECT_TYPE}/{OBJECT_ID}
GET /{OBJECT_TYPE}/{OBJECT_ID}?properties={PROPERTY_1},{PROPERTY_2},{PROPERTY_3}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 取得する[!DNL Catalog]オブジェクトの型です。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{OBJECT_ID}` | 取得する特定のオブジェクトの識別子。 |

**リクエスト**

次のリクエストでは、ID でデータセットを取得し、その `name`、`description`、`state`、`tags`、`files`プロパティを返します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets/5ba9452f7de80400007fc52a?properties=name,description,state,tags,files' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、指定されたデータセッを返します。本文にはリクエストされた `properties` のみが含まれます。

```json
{
    "5ba9452f7de80400007fc52a": {
        "name": "Sample Dataset",
        "description": "Sample dataset containing important data.",
        "state": "DRAFT",
        "tags": {
            "adobe/pqs/table": [
                "sample_dataset"
            ]
        },
        "files": "@/dataSets/5ba9452f7de80400007fc52a/views/5ba9452f7de80400007fc52b/files"
    }
}
```

>[!NOTE]
>
>値の前に `@` が付いているプロパティは、相互に関連するオブジェクトを表します。これらのオブジェクトの詳細を表示する手順については、付録の「[相互関連オブジェクトの表示](appendix.md#view-interrelated-objects)」の節を参照してください。
