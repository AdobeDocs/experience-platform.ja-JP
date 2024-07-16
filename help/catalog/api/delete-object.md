---
keywords: Experience Platform；ホーム；人気のトピック；オブジェクトの削除；catalog service;api
solution: Experience Platform
title: API でのオブジェクトの削除
description: DELETE リクエストのパスに ID を指定することで、カタログオブジェクトを削除できます。
exl-id: 2ac9c378-2340-43e1-8279-7c365df652e4
source-git-commit: d88336d314e767a068ef6524161baeb642a58433
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 48%

---

# API でのオブジェクトの削除

DELETEリクエストのパスに ID を指定することで、[!DNL Catalog] オブジェクトを削除できます。

>[!WARNING]
>
>オブジェクトを削除する際は元に戻すことができず、[!DNL Experience Platform] の他の場所で重大な変更が発生する可能性があるので、慎重に行ってください。

**API 形式**

```http
DELETE /{OBJECT_TYPE}/{OBJECT_ID}
```

>[!IMPORTANT]
>
>`DELETE /batches/{ID}` エンドポイントは非推奨（廃止予定）になりました。 バッチを削除するには、[ バッチ取得 API](../../ingestion/batch-ingestion/api-overview.md#delete-a-batch) を使用する必要があります。

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 削除するオブジェ [!DNL Catalog] トのタイプ。 有効なオブジェクトは次のとおりです。 <ul><li>`dataSets`</li><li>`dataSetFiles`</li></ul> |
| `{OBJECT_ID}` | 更新する特定のオブジェクトの識別子。 |

**リクエスト**

次のリクエストを実行すると、リクエストパスで ID が指定されたデータセットが削除されます。

```shell
curl -X DELETE \
  'https://platform.adobe.io/data/foundation/catalog/dataSets/5ba9452f7de80400007fc52a' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
>リクエストで指定された ID に一致する [!DNL Catalog] オブジェクトがない場合でも、HTTP ステータスコード 200 を受け取ることはできますが、応答配列は空になります。
