---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 予測
topic: developer guide
translation-type: tm+mt
source-git-commit: 16ebff522c5b08e4c100f5d2f972ef4db64656a7
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 3%

---


# 予測

導入

- 特定の見積ジョブの結果を取得します

## はじめに

このガイドで使用されるAPIエンドポイントは、セグメント化APIの一部です。 先に進む前に、 [セグメント化開発ガイドを参照してください](./getting-started.md)。

特に、セグメント化開発ガイドの「 [はじめに](./getting-started.md#getting-started) 」の節には、関連トピックへのリンク、ドキュメント内のサンプルAPI呼び出しを読むためのガイド、Experience Platform APIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報が含まれています。

## 特定の見積ジョブの結果を取得します

特定の予測ジョブの詳細を取得するには、エンドポイントにGETリクエストを作成し、その予測ジョブの `/estimate``id` 値をリクエストパスに指定します。

**API形式**

```http
GET /estimate/{PREVIEW_ID}
```

- `{PREVIEW_ID}`: 取得する見積ジョブの `id` 値。

**リクエスト**

次のリクエストは、特定の見積ジョブの結果を取得します。

//プレビューIDを取得する必要があります

```shell
curl -X GET https://platform.adobe.io/data/core/ups/estimate/{PREVIEW_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、予測ジョブの詳細と共にHTTPステータス200を返します。

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