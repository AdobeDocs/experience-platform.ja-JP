---
keywords: Experience Platform；ホーム；人気のあるトピック；リストサンドボックス
solution: Experience Platform
title: サポートされるサンドボックスタイプのリスト
topic: developer guide
description: /sandboxTypesエンドポイントにGETリクエストを行うことで、組織でサポートされているSandboxタイプのリストを取得できます。
translation-type: tm+mt
source-git-commit: 0af537e965605e6c3e02963889acd85b9d780654
workflow-type: tm+mt
source-wordcount: '75'
ht-degree: 62%

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
