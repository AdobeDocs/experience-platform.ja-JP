---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 予測
topic: developer guide
translation-type: tm+mt
source-git-commit: 16ebff522c5b08e4c100f5d2f972ef4db64656a7

---


# 予測

導入

- 特定の見積ジョブの結果の取得

## はじめに

このガイドで使用されるAPIエンドポイントは、セグメントAPIの一部です。 先に進む前に、セグメント化開発ガイ [ドを参照してください](./getting-started.md)。

特に、『セグメン [ト開発者](./getting-started.md#getting-started) 』ガイドの「はじめに」の節には、関連トピックへのリンク、ドキュメントでのサンプルAPI呼び出しの読み方のガイド、Experience Platform APIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報が含まれています。

## 特定の見積ジョブの結果の取得

特定の見積ジョブの詳細を取得するには、エンドポイントにGETリクエストを作成し、 `/estimate` 見積ジョブの値をリクエストパス `id` に指定します。

**API形式**

```http
GET /estimate/{PREVIEW_ID}
```

- `{PREVIEW_ID}`:取得 `id` する見積ジョブの値。

**リクエスト**

次のリクエストは、特定の見積ジョブの結果を取得します。

//プレビューIDを取得

```shell
curl -X GET https://platform.adobe.io/data/core/ups/estimate/{PREVIEW_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、推定ジョブの詳細を含むHTTPステータス200を返します。

```json
{
  "estimatedSize": 0,
  "numRowsToRead": 1,
  "state": "RESULT_READY",
  "profilesReadSoFar": 1,
  "standardError": 0,
  "error": {
    "description": "",
    "traceback": ""
  },
  "profilesMatchedSoFar": 0,
  "totalRows": 1,
  "confidenceInterval": "95%",
  "_links": {
    "preview": "https://platform.adobe.io/data/core/ups/preview/app-32be0328-3f31-4b64-8d84-acd0c4fbdad3/execution/0?previewQueryId=e890068b-f5ca-4a8f-a6b5-af87ff0caac3"
  }
}
```

## 次の手順

見積もりジョブの結果を取得する方法がわかったので、