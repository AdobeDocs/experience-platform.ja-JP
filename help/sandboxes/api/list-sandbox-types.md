---
keywords: Experience Platform；ホーム；人気のあるトピック；リストサンドボックス
solution: Experience Platform
title: APIでサポートされるリストサンドボックスの種類
topic: 開発ガイド
description: /sandboxTypesエンドポイントにGETリクエストを行うことで、組織でサポートされているSandboxタイプのリストを取得できます。
translation-type: tm+mt
source-git-commit: ca3de18c093d7b692b582045afea4401d7133b9b
workflow-type: tm+mt
source-wordcount: '83'
ht-degree: 46%

---


# APIでリストがサポートするサンドボックスの種類

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
