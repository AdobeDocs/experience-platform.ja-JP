---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: オブジェクトの検索
topic: developer guide
translation-type: tm+mt
source-git-commit: 4dcd174eda98fee1e8cf668819809bd061c6e8bb
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 2%

---


# オブジェクトの検索

特定のカタログオブジェクトの固有な識別子がわかっている場合は、そのオブジェクトの詳細を表示するためにGETリクエストを実行できます。

>[!NOTE] 特定のオブジェクトを表示する場合は、プロパティで [フィルタし](filter-data.md) 、目的のプロパティのみを返すことをお勧めします。

**API形式**

```http
GET /{OBJECT_TYPE}/{OBJECT_ID}
GET /{OBJECT_TYPE}/{OBJECT_ID}?properties={PROPERTY_1},{PROPERTY_2},{PROPERTY_3}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 取得するCatalogオブジェクトのタイプ。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{OBJECT_ID}` | 取得する特定のオブジェクトの識別子。 |

**リクエスト**

次のリクエストでは、IDを使用してデータセットを取得し、 `name`、、、、、および `description`各プ `state``tags``files` ロパティを返します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets/5ba9452f7de80400007fc52a?properties=name,description,state,tags,files' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、本文にリクエストされたデータセットのみを含む指定されたデータセット `properties` を返します。

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

>[!NOTE] 値のプリフィックスが付けられたプロパティは、相互に関連するオブジェクトを `@` 表します。 これらのオブジェクトの詳細を表示する手順については、 [相互に関連するオブジェクトの表示に関する付録の節を参照してください](appendix.md#view-interrelated-objects) 。
