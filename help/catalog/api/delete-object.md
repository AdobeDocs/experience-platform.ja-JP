---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: オブジェクトの削除
topic: developer guide
translation-type: tm+mt
source-git-commit: 85b497d87fcfb54f390302036ce24c98c6dba6ff

---


# オブジェクトの削除

DELETEリクエストのパスにIDを指定することで、Catalogオブジェクトを削除できます。

>[!WARNING] オブジェクトの削除は取り消しできず、エクスペリエンスプラットフォームの他の場所で中断的な変更が生じる場合があるので、注意が必要です。

**API形式**

```http
DELETE /{OBJECT_TYPE}/{OBJECT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 削除するカタログオブジェクトの種類を指定します。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{OBJECT_ID}` | 更新する特定のオブジェクトの識別子。 |

**リクエスト**

次のリクエストでは、リクエストパスでIDが指定されたデータセットを削除します。

```shell
curl -X DELETE \
  'https://platform.adobe.io/data/foundation/catalog/dataSets/5ba9452f7de80400007fc52a' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、HTTPステータス200(OK)と、削除されたデータセットのIDを含む配列を返します。 このIDは、DELETE要求で送信されたIDと一致する必要があります。 削除されたオブジェクトに対してGETリクエストを実行すると、HTTPステータス404（見つかりません）が返され、データセットが正常に削除されたことが確認されます。

```json
[
    "@/dataSets/5ba9452f7de80400007fc52a"
]
```

>[!NOTE] リクエストで指定されたIDに一致するCatalogオブジェクトがない場合、HTTPステータスコード200を引き続き受け取ることができますが、応答配列は空になります。
