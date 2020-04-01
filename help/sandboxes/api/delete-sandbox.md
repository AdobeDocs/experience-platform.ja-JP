---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: サンドボックスの削除
topic: developer guide
translation-type: tm+mt
source-git-commit: 974e93b1c24493734848151b9be00758f6a84578

---


# サンドボックスの削除

サンドボックスを削除するには、リクエストパスにサンドボックスを含むDELETEリクエストを `name` 作成します。

>[!NOTE] このAPI呼び出しを行うと、サンドボックスのプロパティが「 `status` 削除済み」に更新され、非アクティブになります。 GET要求は、削除後もサンドボックスの詳細を取得できます。

**API形式**

```http
DELETE /sandboxes/{SANDBOX_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{SANDBOX_NAME}` | 削除 `name` するサンドボックスの名前。 |

**リクエスト**

次のリクエストは、「dev-2」という名前のサンドボックスを削除します。

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes/dev-2 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、サンドボックスの更新された詳細を返し、サンドボックスが「削除さ `state` れた」ことを示します。

```json
{
    "name": "dev-2",
    "title": "Development 2",
    "state": "deleted",
    "type": "development",
    "region": "VA7"
}
```
