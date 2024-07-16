---
keywords: Experience Platform；ホーム；人気のトピック；query service;api ガイド；接続パラメーター；Query service;
solution: Experience Platform
title: 接続パラメーター API エンドポイント
description: /connection_parameters エンドポイントにGETリクエストを行うことで、インタラクティブサービスを使用するための接続パラメーターを取得できます。
role: Developer
exl-id: 1667f4a5-e6e5-41e9-8f9d-6d2c63c7d7d6
source-git-commit: c16ce1020670065ecc5415bc3e9ca428adbbd50c
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 29%

---

# 接続パラメーターエンドポイント

## サンプル API 呼び出し

次の節では、[!DNL Query Service] API を使用して作成できる API 呼び出しについて説明します。 この呼び出しには、一般的な API 形式、必要なヘッダーを示すサンプルリクエストおよびサンプル応答が含まれます。

### リクエスト接続パラメーター

`/connection_parameters` エンドポイントにGETリクエストを行うと、接続パラメーターを取得できます。 インタラクティブサービスを介した接続に接続パラメーターを使用するクライアントについては、[クエリサービスクライアント](../clients/overview.md)に関するドキュメントを参照してください。

**API 形式**

```http
GET /connection_parameters
```

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/connection_parameters
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合、HTTP ステータス 200 と接続パラメーターが返されます。

```json
{
    "username": "{USERNAME}",
    "dbName": "prod:all",
    "host": "{HOSTNAME}",
    "version": 1,
    "port": 80,
    "token": "{TOKEN}",
    "compressedToken": "{COMPRESSED_TOKEN}",
    "websocketHost": "{WEBSOCKET_HOSTNAME}"
}
```
