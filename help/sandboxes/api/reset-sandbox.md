---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: サンドボックスのリセット
topic: developer guide
translation-type: tm+mt
source-git-commit: 974e93b1c24493734848151b9be00758f6a84578
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 4%

---


# サンドボックスのリセット

開発サンドボックスには「ファクトリリセット」機能があり、これによりサンドボックスからデフォルト以外のすべてのリソースが削除されます。 サンドボックスをリセットするには、そのサンドボックスを要求パスに含むPUT要求 `name` を行います。

**API形式**

```http
PUT /sandboxes/{SANDBOX_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{SANDBOX_NAME}` | リセットするサンドボックスの `name` プロパティです。 |

**リクエスト**

次の要求は、「dev-2」という名前のサンドボックスをリセットします。

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
| `action` | サンドボックスをリセットするには、このパラメーターを要求ペイロードで「reset」の値と共に指定する必要があります。 |

**応答**

正常に応答すると、更新されたサンドボックスの詳細が返され、「リセット中」であるこ `state` とが示されます。

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

>[!NOTE] サンドボックスをリセットすると、システムによるプロビジョニングが15分ほどかかります。 プロビジョニングが完了すると、Sandboxの「アクティブ」または「失敗」 `state` になります。