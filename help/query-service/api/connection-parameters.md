---
keywords: Experience Platform;home;popular topics;query service;api guide;connection parameters;Query service;
solution: Experience Platform
title: クエリサービス開発者ガイド
topic: connection parameters
description: /connection_parametersエンドポイントにGET要求を行うことで、インタラクティブサービスを使用するための接続パラメーターを取得できます。
translation-type: tm+mt
source-git-commit: 648544bc60c0cee8ca8b167118391980b6c33d91
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 47%

---


# 接続パラメーター

## サンプル API 呼び出し

これで、使用するヘッダーがわかったので、[!DNL Query Service] APIを呼び出す準備が整いました。 以下の節では、[!DNL Query Service] APIを使用して実行できる様々なAPI呼び出しについて説明します。 各呼び出しでは一般的な API 形式、必須ヘッダーを示すリクエスト例および応答例が示されています。

### 接続パラメーターの要求

`/connection_parameters`エンドポイントにGETリクエストを行うことで、接続パラメーターを取得できます。 インタラクティブサービスを介した接続に接続パラメーターを使用するクライアントについては、[クエリサービスクライアント](../clients/overview.md)に関するドキュメントを参照してください。

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
