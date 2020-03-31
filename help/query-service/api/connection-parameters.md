---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: クエリサービス開発者ガイド
topic: connection parameters
translation-type: tm+mt
source-git-commit: 7d5d98d8e32607abf399fdc523d2b3bc99555507

---


# 接続パラメータ

## サンプルAPI呼び出し

これで、使用するヘッダーを理解できたので、クエリサービスAPIの呼び出しを開始できます。 以下の節では、クエリサービスAPIを使用して行える様々なAPI呼び出しについて説明します。 各呼び出しには、一般的なAPI形式、必要なヘッダーを示すサンプルリクエスト、およびサンプル応答が含まれます。

### インタラクティブサービスの接続パラメーターを要求します

エンドポイントにGET要求を行うことで、インタラクテ [ィブサービスを使用する](../creating-queries/writing-queries.md) ための接続パラメーターを取得で `/connection_parameters` きます。 インタラクティブサービスを介して接続する接続パラメーターを使用するクライアントの詳細については、 [クエリサービスクライアントのドキュメントを参照してくださ](../clients/overview.md)い。

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

成功した応答は、接続パラメーターと共にHTTPステータス200を返します。

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
