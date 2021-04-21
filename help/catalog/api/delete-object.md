---
keywords: Experience Platform；ホーム；人気のあるトピック；オブジェクトの削除；カタログサービス；api
solution: Experience Platform
title: APIでのオブジェクトの削除
topic-legacy: developer guide
description: DELETE リクエストのパスに ID を指定することで、カタログオブジェクトを削除できます。
exl-id: 2ac9c378-2340-43e1-8279-7c365df652e4
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 47%

---

# APIでのオブジェクトの削除

DELETEリクエストのパスに[!DNL Catalog]オブジェクトのIDを指定すると、オブジェクトを削除できます。

>[!WARNING]
>
>オブジェクトの削除は取り消しができず、[!DNL Experience Platform]の他の場所で改行の変更が行われる可能性があるので、注意が必要です。

**API 形式**

```http
DELETE /{OBJECT_TYPE}/{OBJECT_ID}
```

>[!IMPORTANT]
>
>`DELETE /batches/{ID}`エンドポイントは非推奨です。 バッチを削除するには、[バッチ取り込みAPI](../../ingestion/batch-ingestion/api-overview.md#delete-a-batch)を使用する必要があります。

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 削除する[!DNL Catalog]オブジェクトの種類です。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`connections`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
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
>リクエストで指定されたIDに一致する[!DNL Catalog]オブジェクトがない場合、HTTPステータスコード200を引き続き受け取ることができますが、応答配列は空になります。
