---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: サンドボックスの更新
topic: developer guide
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '87'
ht-degree: 4%

---


# サンドボックスの更新

サンドボックス内の1つ以上のフィールドを更新するには、要求パスにサンドボックスを含むPATCH要求を作成し、要求ペイロード内 `name` で更新するプロパティを作成します。

>[!NOTE]
>
>現在、サンドボックスの `title` プロパティのみを更新できます。

**API形式**

```http
PATCH /sandboxes/{SANDBOX_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{SANDBOX_NAME}` | 更新するサンドボックスの `name` プロパティです。 |

**リクエスト**

次の要求は、&quot;dev-2&quot;という名前のサンドボックスの `title` プロパティを更新します。

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

正常に応答すると、HTTPステータス200 (OK)と共に、新たに更新されたサンドボックスの詳細が返されます。

```json
{
    "name": "dev-2",
    "title": "Development B",
    "state": "active",
    "type": "development",
    "region": "VA7"
}
```
