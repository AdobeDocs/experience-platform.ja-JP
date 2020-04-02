---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: プレビュー
topic: developer guide
translation-type: tm+mt
source-git-commit: 45a196d13b50031d635ceb7c5c952e42c09bd893

---


# プレビュー開発ガイド

導入

- 新しいプレビュー
- 特定のプレビューの結果の取得
- 特定のプレビュー

## はじめに

このガイドで使用されるAPIエンドポイントは、セグメントAPIの一部です。 先に進む前に、セグメント化開発ガイ [ドを参照してください](./getting-started.md)。

特に、『セグメン [ト開発者](./getting-started.md#getting-started) 』ガイドの「はじめに」の節には、関連トピックへのリンク、ドキュメントでのサンプルAPI呼び出しの読み方のガイド、Experience Platform APIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報が含まれています。

## 新しいプレビュー

エンドポイントにPOSTリクエストをプレビューすることで、新しいエンドポイントを作成で `/preview` きます。

**API形式**

```http
POST /preview
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/core/ups/preview \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '
{
  "predicateExpression": "xEvent.metrics.commerce.abandons.value > 0",
  "predicateType": "pql/text",
  "predicateModel": "_xdm.context.profile",
  "graphType": "pdg"
}
 '
```

身体情報

**応答**

応答が成功すると、新しく作成された応答の詳細と共に、HTTPステータス201（作成済み）が返されます。プレビュー

```json
{
  "state": "NEW",
  "previewQueryId": "e890068b-f5ca-4a8f-a6b5-af87ff0caac3",
  "previewQueryStatus": "NEW",
  "previewId": "MDphcHAtMzJiZTAzMjgtM2YzMS00YjY0LThkODQtYWNkMGM0ZmJkYWQzOmU4OTAwNjhiLWY1Y2EtNGE4Zi1hNmI1LWFmODdmZjBjYWFjMzow",
  "previewExecutionId": 0
}
```

応答情報が存在しない場合、x-locationは存在しません。

## 特定のプレビューの結果の取得

特定のプレビューに関する詳細な情報を取得するには、エンドポイントにGETリクエストを送信し、 `/preview` リクエストパスにプレビューの値を `id` 指定します。

**API形式**

```http
GET /preview/{PREVIEW_ID}
```

- `{PREVIEW_ID}`:取得 `id` するプレビューの値。

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/preview/MDphcHAtMzJiZTAzMjgtM2YzMS00YjY0LThkODQtYWNkMGM0ZmJkYWQzOmU4OTAwNjhiLWY1Y2EtNGE4Zi1hNmI1LWFmODdmZjBjYWFjMzow \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、HTTPステータス200を返し、指定された応答に関する詳細な情報がプレビューされます。

```json
{
  "state": "RESULT_READY",
  "page": {
    "offset": 0,
    "size": 0
  },
  "results": [],
  "link": {
    "nextPage": ""
  },
  "previewSampledResultsCount": 0
}
```

## 特定のプレビュー

特定のプレビューを削除するには、エンドポイントにDELETEリクエストを送信し、 `/preview` リクエストパスにプレビューの値を `id` 指定します。この場合、

**API形式**

```http
DELETE /preview/{PREVIEW_ID}
```

- `{PREVIEW_ID}` 削除 `id` するプレビューの値。

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/preview/MDphcHAtMzJiZTAzMjgtM2YzMS00YjY0LThkODQtYWNkMGM0ZmJkYWQzOmU4OTAwNjhiLWY1Y2EtNGE4Zi1hNmI1LWFmODdmZjBjYWFjMzow \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、次のメッセージと共にHTTPステータス200を返します。

```json
{
  "status": true,
  "message": "KILLED"
}
```

## 次の手順
