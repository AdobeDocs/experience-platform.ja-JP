---
keywords: Experience Platform；ホーム；人気のあるトピック；サンドボックスの削除
solution: Experience Platform
title: APIでのサンドボックスの削除
topic: 開発ガイド
description: サンドボックスの名前を要求パスに含むDELETE要求を行うことで、サンドボックスを削除できます。
translation-type: tm+mt
source-git-commit: e7a80dbfdd2d59e4997f6e227b5c2cf336e5a0f6
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 66%

---


# APIでのサンドボックスの削除

サンドボックスを削除するには、リクエストパスにサンドボックスの `name` を含む DELETE リクエストを実行します。

>[!NOTE]
>
>この API 呼び出しを実行すると、サンドボックスの `status` プロパティが「deleted」に更新され、サンドボックスが非アクティブになります。サンドボックスが削除された後も、GET リクエストを使用して、その詳細を取得できます。

**API 形式**

```http
DELETE /sandboxes/{SANDBOX_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{SANDBOX_NAME}` | 削除するサンドボックスの `name`。 |

**リクエスト**

次のリクエストでは、「dev-2」という名前のサンドボックスが削除されます。

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes/dev-2 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答**

リクエストが成功した場合は、サンドボックスの更新された情報が返され、サンドボックスの `state` が「deleted」になったことが示されます。

```json
{
    "name": "dev-2",
    "title": "Development 2",
    "state": "deleted",
    "type": "development",
    "region": "VA7"
}
```
