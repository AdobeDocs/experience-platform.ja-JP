---
keywords: Experience Platform;home;popular topics;query service;api guide;connection parameters;Query service;
solution: Experience Platform
title: クエリサービス開発者ガイド
topic: connection parameters
description: /connection_parametersエンドポイントにGET要求を行うことで、インタラクティブサービスを使用するための接続パラメーターを取得できます。
translation-type: tm+mt
source-git-commit: 4b2df39b84b2874cbfda9ef2d68c4b50d00596ac
workflow-type: tm+mt
source-wordcount: '143'
ht-degree: 62%

---


# 接続パラメーター

## サンプル API 呼び出し

Now that you understand what headers to use, you are ready to begin making calls to the [!DNL Query Service] API. The following sections walk through the various API calls you can make using the [!DNL Query Service] API. 各呼び出しでは一般的な API 形式、必須ヘッダーを示すリクエスト例および応答例が示されています。

### インタラクティブサービスの接続パラメーターの要求

`/connection_parameters` エンドポイントに GET リクエストを実行することで、[インタラクティブサービス](../creating-queries/writing-queries.md)を使用するための接続パラメーターを取得できます。インタラクティブサービスを介した接続に接続パラメーターを使用するクライアントについては、[クエリサービスクライアント](../clients/overview.md)に関するドキュメントを参照してください。

**API 形式**

```http
GET /connection_parameters
```

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/query/connection_parameters
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
