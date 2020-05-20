---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: サンドボックスの削除
topic: developer guide
translation-type: tm+mt
source-git-commit: 974e93b1c24493734848151b9be00758f6a84578
workflow-type: tm+mt
source-wordcount: '84'
ht-degree: 4%

---


# サンドボックスの削除

サンドボックスを削除するには、そのサンドボックスを要求パスに含むDELETE要求 `name` を行います。

>[!NOTE] このAPI呼び出しを行うと、サンドボックスの `status` プロパティが「削除済み」に更新され、非アクティブになります。 GET要求は、サンドボックスが削除された後も、サンドボックスの詳細を取得できます。

**API形式**

```http
DELETE /sandboxes/{SANDBOX_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{SANDBOX_NAME}` | 削除するサ `name` ンドボックスの名前です。 |

**リクエスト**

次の要求は、「dev-2」という名前のサンドボックスを削除します。

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes/dev-2 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常に応答すると、サンドボックスの更新された詳細が返され、サンドボックスが「削除された」こ `state` とが示されます。

```json
{
    "name": "dev-2",
    "title": "Development 2",
    "state": "deleted",
    "type": "development",
    "region": "VA7"
}
```
