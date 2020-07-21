---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: クエリサービス開発ガイド
topic: connection parameters
translation-type: tm+mt
source-git-commit: 3b710e7a20975880376f7e434ea4d79c01fa0ce5
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 1%

---


# 接続パラメータ

## サンプルAPI呼び出し

これで、使用するヘッダーが分かったので、 [!DNL Query Service] APIの呼び出しを開始する準備が整いました。 以下の節では、 [!DNL Query Service] APIを使用して実行できる様々なAPI呼び出しについて説明します。 各呼び出しには、一般的なAPI形式、必要なヘッダーを表示するサンプルリクエスト、サンプルレスポンスが含まれます。

### インタラクティブサービスの接続パラメーターを要求します

エンドポイントにGET要求を行うと、イン [タラクティブサービスを使用するための接続パラメーターを取得でき](../creating-queries/writing-queries.md)`/connection_parameters` ます。 接続パラメーターを使用してインタラクティブサービスを介して接続するクライアントの詳細については、 [クエリサービスクライアントに関するドキュメントを参照してください](../clients/overview.md)。

**API形式**

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

正常に応答すると、接続パラメーターと共にHTTPステータス200が返されます。

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
