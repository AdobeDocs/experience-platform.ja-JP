---
keywords: Experience Platform；ホーム；人気のあるトピック；サンドボックスのリセット
solution: Experience Platform
title: APIでのサンドボックスのリセット
topic: 開発ガイド
description: 開発サンドボックスには「出荷時リセット」機能があり、この機能によりサンドボックスからデフォルト以外のすべてのリソースが削除されます。サンドボックスの名前を要求パスに含むPUT要求を行うことで、サンドボックスをリセットできます。
translation-type: tm+mt
source-git-commit: 62ce5ac92d03a6e85589fc92e8d953f7fc1d8f31
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 76%

---


# APIでのサンドボックスのリセット

開発サンドボックスには「出荷時リセット」機能があり、この機能によりサンドボックスからデフォルト以外のすべてのリソースが削除されます。サンドボックスをリセットするには、そのサンドボックスの `name` をリクエストパスに含む PUT リクエストを作成する必要があります。

**API 形式**

```http
PUT /sandboxes/{SANDBOX_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{SANDBOX_NAME}` | リセットするサンドボックスの `name` プロパティです。 |

**リクエスト**

次のリクエストは、「dev-2」という名前のサンドボックスをリセットします。

```shell
curl -X PUT \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes/dev-2 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "action": "reset"
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `action` | サンドボックスをリセットするには、このパラメーターをリクエストペイロードで値「reset」と共に指定する必要があります。 |

**応答**

正常な応答は、更新されたサンドボックスの詳細を返し、`state` が「リセット中」であることを示します。

```json
{
    "id": "d8184350-dbf5-11e9-875f-6bf1873fec16",
    "name": "dev-2",
    "title": "Development 2",
    "state": "resetting",
    "type": "development",
    "region": "VA7"
}
```

>[!NOTE]
>
> サンドボックスがリセットされると、システムによってプロビジョニングされるまで約 15 分かかります。プロビジョニングが完了すると、サンドボックスの `state` は「アクティブ」または「失敗」になります。