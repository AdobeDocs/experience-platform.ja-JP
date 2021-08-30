---
keywords: Experience Platform；ホーム；人気のあるトピック；リストサンドボックス
solution: Experience Platform
title: サンドボックスタイプAPIエンドポイント
topic-legacy: developer guide
description: /sandboxTypesエンドポイントに対してGETリクエストを実行することで、組織でサポートされているサンドボックスタイプのリストを取得できます。
exl-id: eb5e1b44-37f5-4ed5-98f5-ac8db8792c7d
source-git-commit: f5ce7b7f09c624c53065757bb8a9b09f989dce0a
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 45%

---

# サンドボックスタイプエンドポイント

`/sandboxTypes` エンドポイントに対して GET リクエストを実行すると、組織でサポートされているサンドボックスタイプのリストを取得できます。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Sandbox] API](https://www.adobe.io/experience-platform-apis/references/sandbox) の一部です。続行する前に、[はじめにのガイド](./getting-started.md)を参照して、関連ドキュメントへのリンク、このドキュメントのAPI呼び出し例の読み方、およびExperience PlatformAPIを正しく呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## サポートされているサンドボックスタイプのリストの取得

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
