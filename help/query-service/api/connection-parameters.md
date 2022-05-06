---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；API ガイド；接続パラメーター；クエリサービス；
solution: Experience Platform
title: 接続パラメーター API エンドポイント
topic-legacy: connection parameters
description: /connection_parameters エンドポイントに対してGETリクエストを実行することで、インタラクティブサービスを使用するための接続パラメーターを取得できます。
exl-id: 1667f4a5-e6e5-41e9-8f9d-6d2c63c7d7d6
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 28%

---

# 接続パラメーターエンドポイント

## サンプル API 呼び出し

以下の節では、 [!DNL Query Service] API この呼び出しには、一般的な API 形式、必要なヘッダーを示すリクエスト例および応答例が含まれています。

### 接続パラメーターをリクエスト

接続パラメーターを取得するには、 `/connection_parameters` endpoint. インタラクティブサービスを介した接続に接続パラメーターを使用するクライアントについては、[クエリサービスクライアント](../clients/overview.md)に関するドキュメントを参照してください。

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
