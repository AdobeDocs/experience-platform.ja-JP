---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: オブジェクトの削除
topic: developer guide
translation-type: tm+mt
source-git-commit: 327be13cbaaa40e4d0409cbb49a051b7067759bf
workflow-type: tm+mt
source-wordcount: '178'
ht-degree: 2%

---


# オブジェクトの削除

DELETEリクエストのパスにカタログオブジェクトのIDを指定すると、そのカタログオブジェクトを削除できます。

>[!WARNING] オブジェクトの削除は元に戻せず、Experience Platform内の別の場所で改行の変更が行われる場合があるので、注意が必要です。

**API形式**

```http
DELETE /{OBJECT_TYPE}/{OBJECT_ID}
```

>[!IMPORTANT]
>
>エンドポイ `DELETE /batches/{ID}` ントは非推奨です。 バッチを削除するには、 [Batch Ingestion APIを使用する必要があります](../../ingestion/batch-ingestion/api-overview.md#delete-a-batch)。

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 削除するカタログオブジェクトの種類です。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`connections`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{OBJECT_ID}` | 更新する特定のオブジェクトの識別子。 |

**リクエスト**

次のリクエストでは、リクエストパスでIDが指定されているデータセットを削除します。

```shell
curl -X DELETE \
  'https://platform.adobe.io/data/foundation/catalog/dataSets/5ba9452f7de80400007fc52a' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常に応答すると、HTTPステータス200(OK)と、削除されたデータセットのIDを含む配列が返されます。 このIDは、DELETE要求で送信されたIDと一致する必要があります。 削除したオブジェクトに対してGETリクエストを実行すると、データセットが正常に削除されたことを確認するHTTPステータス404（見つかりません）が返されます。

```json
[
    "@/dataSets/5ba9452f7de80400007fc52a"
]
```

>[!NOTE] リクエストで指定されたIDに一致するCatalogオブジェクトがない場合、HTTPステータスコード200を引き続き受け取ることができますが、応答配列は空になります。
