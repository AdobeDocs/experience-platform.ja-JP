---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: サンドボックスのリセット
topic: developer guide
translation-type: tm+mt
source-git-commit: 974e93b1c24493734848151b9be00758f6a84578

---


# サンドボックスのリセット

開発サンドボックスには「ファクトリのリセット」機能があり、この機能によりサンドボックスからデフォルト以外のすべてのリソースが削除されます。 サンドボックスをリセットするには、そのサンドボックスをリクエストパスに含むPUTリクエストを作 `name` 成する必要があります。

**API形式**

```http
PUT /sandboxes/{SANDBOX_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{SANDBOX_NAME}` | リセッ `name` トするサンドボックスのプロパティです。 |

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

成功した応答は、更新されたサンドボックスの詳細を返し、サンドボックスが「リセッ `state` ト中」であることを示します。

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

>[!NOTE] サンドボックスがリセットされると、システムによってプロビジョニングされるまで約15分かかります。 プロビジョニングが完了すると、サンドボッ `state` クスは「アクティブ」または「失敗」になります。