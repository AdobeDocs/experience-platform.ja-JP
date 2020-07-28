---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: サポートされるサンドボックスタイプのリスト
topic: developer guide
translation-type: tm+mt
source-git-commit: b4741cdfd065bbaed7f2feeafe8619191e4b8f6c
workflow-type: tm+mt
source-wordcount: '47'
ht-degree: 100%

---


# サポートされるサンドボックスタイプのリスト

`/sandboxTypes` エンドポイントに対して GET リクエストを実行すると、組織でサポートされているサンドボックスタイプのリストを取得できます。

**API 形式**

```http
GET /sandboxTypes
```

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxTypes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、組織でサポートされているサンドボックスタイプのリストを返します。

```json
{
    "sandboxTypes": [
        "production",
        "development"
    ]
}
```
