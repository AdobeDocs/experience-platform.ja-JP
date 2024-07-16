---
keywords: Experience Platform；ホーム；人気のトピック；フィルター；データのフィルター；データのフィルター
solution: Experience Platform
title: カタログオブジェクトのリスト
description: 1 回の API 呼び出しで、特定のタイプの使用可能なすべてのオブジェクトのリストを取得できます。ベストプラクティスは、応答のサイズを制限するフィルターを含めることです。
exl-id: 2c65e2bc-4ddd-445a-a52d-6ceb1153ccea
source-git-commit: 409d052c119814a1cf6b79ff4448d1100b18e199
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 53%

---

# List Catalog objects

1 回の API 呼び出しで、特定のタイプの使用可能なすべてのオブジェクトのリストを取得できます。ベストプラクティスは、応答のサイズを制限するフィルターを含めることです。

**API 形式**

```http
GET /{OBJECT_TYPE}
GET /{OBJECT_TYPE}?{FILTER}={VALUE}&{FILTER_2}={VALUE}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | リストされるオブジェク [!DNL Catalog] のタイプ。 有効なオブジェクトは次のとおりです。 <ul><li>`batches`</li><li>`dataSets`</li><li>`dataSetFiles`</li></ul> |
| `{FILTER}` | 応答で返された結果をフィルターするために使用されるクエリパラメーター。複数のパラメーターはアンパサンド（`&`）で区切られます。詳しくは、[カタログデータのフィルタリング](filter-data.md)に関するガイドを参照してください。 |

**リクエスト**

以下のリクエスト例では、データセットのリストを取得し、応答を 5 つの結果に減らす `limit` フィルターと、各データセットに対して表示されるプロパティを制限する `properties` フィルターを使用しています。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets?limit=5&properties=name,description,files' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、[!DNL Catalog] オブジェクトのリストがキーと値のペアの形式で返され、リクエストで提供されるクエリパラメーターでフィルタリングされます。 キーと値のペアごとに、キーは、対象となる [!DNL Catalog] オブジェクトの一意の ID を表します。さらに詳しくは、別の呼び出しで使用して [ その特定のオブジェクトを表示 ](look-up-object.md) できます。

>[!NOTE]
>
>返されたオブジェクトに、`properties` クエリで示されるリクエストされたプロパティが 1 つ以上含まれていない場合、次の ***`Sample Dataset 3`*** と ***`Sample Dataset 4`*** に示すように、応答は、含まれているリクエストされたプロパティのみを返します。

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
