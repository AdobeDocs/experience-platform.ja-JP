---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: オブジェクトの検索
topic: developer guide
translation-type: tm+mt
source-git-commit: 4dcd174eda98fee1e8cf668819809bd061c6e8bb

---


# オブジェクトの検索

特定のCatalogオブジェクトの固有な識別子がわかっている場合は、そのオブジェクトの詳細を表示に対してGETリクエストを実行できます。

>[!NOTE] 特定のオブジェクトを表示する場合も、プロパティでフィルタ [ーし](filter-data.md) 、目的のプロパティのみを返すことをお勧めします。

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

次のリクエストでは、IDを使用してデータセットを取得し、そ `name`の、、、、お `description`よびプ `state`ロパテ `tags`ィを返 `files` します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets/5ba9452f7de80400007fc52a?properties=name,description,state,tags,files' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、要求されたデータセットのみを本文に含む指 `properties` 定されたデータセットを返します。

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

>[!NOTE] 値の先頭に相互関連オブジェクトを表すプ `@` リフィックスを持つプロパティ。 これらのオブジェクトの詳細を表示 [する手順については、](appendix.md#view-interrelated-objects) 「相互関連オブジェクトの表示」の付録の節を参照してください。
