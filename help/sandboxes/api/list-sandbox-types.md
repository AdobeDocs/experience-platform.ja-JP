---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リストがサポートするサンドボックスタイプ
topic: developer guide
translation-type: tm+mt
source-git-commit: b4741cdfd065bbaed7f2feeafe8619191e4b8f6c

---


# リストがサポートするサンドボックスタイプ

エンドポイントにGETリクエストを行うことで、組織でサポートされているリストのサンドボックスタイプを取得で `/sandboxTypes` きます。

**API形式**

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

成功した応答は、貴社でサポートされているリストタイプのサンドボックスを返します。

```json
{
    "sandboxTypes": [
        "production",
        "development"
    ]
}
```
