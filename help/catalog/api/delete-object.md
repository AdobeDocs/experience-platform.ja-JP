---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: オブジェクトの削除
topic: developer guide
translation-type: tm+mt
source-git-commit: 73a492ba887ddfe651e0a29aac376d82a7a1dcc4
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 50%

---


# オブジェクトの削除

You can delete a [!DNL Catalog] object by providing its ID in the path of a DELETE request.

>[!WARNING]
>
>Take extra care when deleting objects, as this cannot be undone and may produce breaking changes elsewhere in [!DNL Experience Platform].

**API 形式**

```http
DELETE /{OBJECT_TYPE}/{OBJECT_ID}
```

>[!IMPORTANT]
>
>エンドポイ `DELETE /batches/{ID}` ントは非推奨です。 バッチを削除するには、 [Batch Ingestion APIを使用する必要があります](../../ingestion/batch-ingestion/api-overview.md#delete-a-batch)。

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | The type of [!DNL Catalog] object to be deleted. 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`connections`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{OBJECT_ID}` | 更新する特定のオブジェクトの識別子。 |

**リクエスト**

次のリクエストを実行すると、リクエストパスで ID が指定されたデータセットが削除されます。

```shell
curl -X DELETE \
  'https://platform.adobe.io/data/foundation/catalog/dataSets/5ba9452f7de80400007fc52a' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、削除したデータセットの ID を含む配列と HTTP ステータス 200（OK）が返されます。この ID は、DELETE リクエストで送信された ID と一致します。削除したオブジェクトに対して GET リクエストを実行すると、HTTP ステータス 404（Not Found）が返され、データセットが正常に削除されたことがわかります。

```json
[
    "@/dataSets/5ba9452f7de80400007fc52a"
]
```

>[!NOTE]
>
>If no [!DNL Catalog] objects match the ID provided in your request, you may still receive an HTTP Status Code 200, but the response array will be empty.
