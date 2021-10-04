---
keywords: Experience Platform；ホーム；人気のあるトピック；フィルター；フィルター；データのフィルター；データのフィルター
solution: Experience Platform
title: カタログオブジェクトのリスト
topic-legacy: developer guide
description: 1 回の API 呼び出しで、特定のタイプの使用可能なすべてのオブジェクトのリストを取得できます。ベストプラクティスは、応答のサイズを制限するフィルターを含めることです。
exl-id: 2c65e2bc-4ddd-445a-a52d-6ceb1153ccea
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 53%

---

# リストカタログオブジェクト

1 回の API 呼び出しで、特定のタイプの使用可能なすべてのオブジェクトのリストを取得できます。ベストプラクティスは、応答のサイズを制限するフィルターを含めることです。

**API 形式**

```http
GET /{OBJECT_TYPE}
GET /{OBJECT_TYPE}?{FILTER}={VALUE}&{FILTER_2}={VALUE}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 表示する [!DNL Catalog] オブジェクトのタイプ。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{FILTER}` | 応答で返された結果をフィルターするために使用されるクエリパラメーター。複数のパラメーターはアンパサンド（`&`）で区切られます。詳しくは、[カタログデータのフィルタリング](filter-data.md)に関するガイドを参照してください。 |

**リクエスト**

以下のリクエスト例では、データセットのリストを取得し、応答を 5 つの結果に減らす `limit` フィルターと、各データセットに対して表示されるプロパティを制限する `properties` フィルターを使用しています。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets?limit=5&properties=name,description,files' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、[!DNL Catalog] オブジェクトのリストをキーと値のペアの形式で返し、リクエストで指定されたクエリパラメーターでフィルタリングします。 キーと値のペアごとに、キーは問題の [!DNL Catalog] オブジェクトの一意の識別子を表します。これを別の呼び出しで使用して、 [ 特定のオブジェクト ](look-up-object.md) を表示して詳細を確認できます。

>[!NOTE]
>
>返されたオブジェクトに、`properties` クエリで指定された 1 つ以上のリクエストされたプロパティが含まれていない場合、次の ***`Sample Dataset 3`*** と ***`Sample Dataset 4`*** に示すように、応答は、含まれているリクエストされたプロパティのみを返します。

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
