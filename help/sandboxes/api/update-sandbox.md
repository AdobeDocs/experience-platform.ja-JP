---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: サンドボックスの更新
topic: developer guide
translation-type: tm+mt
source-git-commit: 974e93b1c24493734848151b9be00758f6a84578

---


# サンドボックスの更新

要求パスにサンドボックスを含むPATCH要求を作成し、要求ペイロードで更新するプロパティを作成することで、サンドボックス内の1つ以上の `name` フィールドを更新できます。

>[!NOTE] 現在、サンドボックスのプロパティの `title` みを更新できます。

**API形式**

```http
PATCH /sandboxes/{SANDBOX_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{SANDBOX_NAME}` | 更新 `name` するサンドボックスのプロパティ。 |

**リクエスト**

次の要求は、「dev-2」 `title` という名前のサンドボックスのプロパティを更新します。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes/dev-2 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "title": "Development B"
  }'
```

**応答**

成功した応答は、新しく更新されたサンドボックスの詳細を含むHTTPステータス200(OK)を返します。

```json
{
    "name": "dev-2",
    "title": "Development B",
    "state": "active",
    "type": "development",
    "region": "VA7"
}
```
