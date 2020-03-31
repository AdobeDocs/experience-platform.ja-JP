---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リストオブジェクト
topic: developer guide
translation-type: tm+mt
source-git-commit: 71c73a3899ccdd1c024a811b36c411915a3b14be

---


# リストオブジェクト

1回のAPI呼び出しで、特定のタイプの使用可能なすべてのオブジェクトのリストを取得できます。ベストプラクティスは、応答のサイズを制限するフィルターを含めることです。

**API形式**

```http
GET /{OBJECT_TYPE}
GET /{OBJECT_TYPE}?{FILTER}={VALUE}&{FILTER_2}={VALUE}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 表示するCatalogオブジェクトのタイプ。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{FILTER}` | クエリに返された結果をフィルターするために使用される応答パラメーター。 複数のパラメーターはアンパサンド(`&`)で区切られます。 詳しくは、カタログデータのフィ [ルタリングに関するガイド](filter-data.md) （英語のみ）を参照してください。 |

**リクエスト**

以下のサンプルリクエストでは、データセットのリストを取得し、5つの結果に対する応答を減らすフィルターと、各データセットに対して表示されるプ `limit``properties` ロパティを制限するフィルターを使用しています。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets?limit=5&properties=name,description,files' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、キーと値のペアの形式でCatalogオブジェクトのリストを返し、リクエストで指定されたクエリパラメーターでフィルタリングされます。 各キーと値のペアに対して、キーは対象のCatalogオブジェクトの固有な識別子を表します。この識別子は、詳細について特定のオブジェクトを呼び出す別の [表示呼び出しで](look-up-object.md) 使用できます。

>[!NOTE] 返されたオブジェクトに、クエリが示す要求されたプロパティが1つ以上含まれていない場合、応答は、以下の「サンプルデータセット3」および「サンプルデータセット4」に示すように、要求されたプロパティのみを返します。 `properties`

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
