---
keywords: Experience Platform；ホーム；人気のあるトピック；クエリサービス；api ガイド；接続パラメーター；クエリサービス；
solution: Experience Platform
title: 接続パラメーター API エンドポイント
topic-legacy: connection parameters
description: インタラクティブサービスを使用するための接続パラメーターを取得するには、/connection_parameters エンドポイントにGETリクエストを送信します。
exl-id: 1667f4a5-e6e5-41e9-8f9d-6d2c63c7d7d6
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 38%

---

# 接続パラメーターエンドポイント

## サンプル API 呼び出し

これで、使用するヘッダーを理解できたので、[!DNL Query Service] API の呼び出しを開始できます。 以下の節では、[!DNL Query Service] API を使用しておこなえる様々な API 呼び出しについて説明します。 各呼び出しでは一般的な API 形式、必須ヘッダーを示すリクエスト例および応答例が示されています。

### 接続パラメーターのリクエスト

`/connection_parameters` エンドポイントにGETリクエストを送信することで、接続パラメーターを取得できます。 インタラクティブサービスを介した接続に接続パラメーターを使用するクライアントについては、[クエリサービスクライアント](../clients/overview.md)に関するドキュメントを参照してください。

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
