---
keywords: Experience Platform;home;popular topics;update sandbox
solution: Experience Platform
title: サンドボックスの更新
topic: developer guide
description: 要求パスにサンドボックスの名前を含むPATCH要求を作成し、要求ペイロードに更新するプロパティを含めることで、サンドボックス内の1つ以上のフィールドを更新できます。
translation-type: tm+mt
source-git-commit: 0af537e965605e6c3e02963889acd85b9d780654
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 72%

---


# サンドボックスの更新

リクエストパスにサンドボックスの`name`を含む PATCH リクエストを作成し、リクエストペイロードで更新するプロパティを作成することで、サンドボックス内の 1 つ以上のフィールドを更新できます。

>[!NOTE]
>
>現在、サンドボックスの`title`プロパティのみを更新できます。

**API 形式**

```http
PATCH /sandboxes/{SANDBOX_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{SANDBOX_NAME}` | 更新するサンドボックスの`name`プロパティ。 |

**リクエスト**

次のリクエストは、「dev-2」という名前のサンドボックスの`title`プロパティを更新します。

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

成功した応答は、新しく更新されたサンドボックスの詳細を含む HTTP ステータス 200（OK）を返します。

```json
{
    "name": "dev-2",
    "title": "Development B",
    "state": "active",
    "type": "development",
    "region": "VA7"
}
```
