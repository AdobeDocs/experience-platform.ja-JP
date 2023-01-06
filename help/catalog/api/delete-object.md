---
keywords: Experience Platform；ホーム；人気の高いトピック；オブジェクトの削除；カタログサービス；API
solution: Experience Platform
title: API でのオブジェクトの削除
description: DELETE リクエストのパスに ID を指定することで、カタログオブジェクトを削除できます。
exl-id: 2ac9c378-2340-43e1-8279-7c365df652e4
source-git-commit: 74867f56ee13430cbfd9083a916b7167a9a24c01
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 47%

---

# API でのオブジェクトの削除

次の項目を削除できます。 [!DNL Catalog] オブジェクト内に含める必要があります。

>[!WARNING]
>
>オブジェクトの削除は元に戻せず、内の他の場所で破壊的な変更が生じる可能性があるので、特に注意が必要です。 [!DNL Experience Platform].

**API 形式**

```http
DELETE /{OBJECT_TYPE}/{OBJECT_ID}
```

>[!IMPORTANT]
>
>この `DELETE /batches/{ID}` エンドポイントは非推奨になりました。 バッチを削除するには、 [バッチ取得 API](../../ingestion/batch-ingestion/api-overview.md#delete-a-batch).

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | のタイプ [!DNL Catalog] 削除するオブジェクト。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`connections`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
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
>指定しない場合 [!DNL Catalog] オブジェクトはリクエストで指定された ID に一致します。HTTP ステータスコード 200 が表示される場合がありますが、応答配列は空になります。
