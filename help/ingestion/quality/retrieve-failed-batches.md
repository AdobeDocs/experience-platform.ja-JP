---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 失敗したバッチの取得
topic: overview
translation-type: tm+mt
source-git-commit: 4817162fe2b7cbf4ae4c1ed325db2af31da5b5d3

---


# APIを使用した失敗したバッチの取得

Adobe Experience Platformには、データのアップロードと取り込みの2つの方法が用意されています。 バッチインジェストを使用すると、様々なファイルタイプ（CSVなど）を使用してデータを挿入できます。また、ストリーミングインジェストを使用すると、リアルタイムでストリーミングエンドポイントを使用してプラットフォームにデータを挿入できます。

このチュートリアルでは、Data Ingest APIを使用して、失敗したバッチに関する情報を取得する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントに関する作業を理解している必要があります。

- [Experience Data Model(XDM)System](../../xdm/home.md):エクスペリエンスプラットフォームが顧客エクスペリエンスデータを整理するための標準化されたフレームワーク。
- [データ取り込み](../home.md):データをエクスペリエンスプラットフォームに送信する方法です。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を示します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

- 認証：無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォーム内のすべてのリソース(スキーマレジストリに属するリソースを含む)は、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../../sandboxes/home.md)い。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次のヘッダーが追加で必要です。

- コンテンツタイプ： `application/json`

### 失敗したバッチのサンプル

このチュートリアルでは、次のように、サンプルデータを使用し、月の値を **00に設定する、誤った形式のタイムスタンプを使用します**。

```json
{
    "body": {
        "xdmEntity": {
            "id": "c8d11988-6b56-4571-a123-b6ce74236036",
            "timestamp": "2018-00-10T22:07:56Z",
            "environment": {
                "browserDetails": {
                    "userAgent": "Mozilla\/5.0 (Windows NT 5.1) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/29.0.1547.57 Safari\/537.36 OPR\/16.0.1196.62",
                    "acceptLanguage": "en-US",
                    "cookiesEnabled": true,
                    "javaScriptVersion": "1.6",
                    "javaEnabled": true
                },
                "colorDepth": 32,
                "viewportHeight": 799,
                "viewportWidth": 414
            }
        }
    }
}
```

タイムスタンプの形式が正しくないため、上記のペイロードはXDMスキーマに対して正しく検証されません。

## 失敗したバッチの取得

**API形式**

```http
GET /batches/{BATCH_ID}/failed
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | 検索するバッチのID。 |

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/export/batches/{BATCH_ID}/failed" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "Cache-Control: no-cache" \
  -H "Content-Type: application/json" \
  -H "x-api-key: {API_KEY}" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}
```

**応答**

```json
{
    "data": [
        {
            "name": "_SUCCESS",
            "length": "0",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io:443/data/foundation/export/batches/{BATCH_ID}/failed?path=_SUCCESS"
                }
            }
        },
        {
            "name": "part-00000-44c7b669-5e38-43fb-b56c-a0686dabb982-c000.json",
            "length": "1800",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io:443/data/foundation/export/batches/{BATCH_ID}/failed?path=part-00000-44c7b669-5e38-43fb-b56c-a0686dabb982-c000.json"
                }
            }
        }
    ],
    "_page": {
        "limit": 100,
        "count": 2
    }
}
```

上記の応答を使用すると、成功したバッチと失敗したバッチのチャンクを確認できます。 この応答から、失敗したバッチがファイルに含まれ `part-00000-44c7b669-5e38-43fb-b56c-a0686dabb982-c000.json` ていることがわかります。

## 失敗したバッチのダウンロード

バッチで失敗したファイルがわかったら、失敗したファイルをダウンロードして、エラーメッセージを確認できます。

**API形式**

```http
GET /batches/{BATCH_ID}/failed?path={FAILED_FILE}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | 失敗したファイルを含むバッチのID。 |
| `{FAILED_FILE}` | 形式設定に失敗したファイルの名前。 |

**リクエスト**

次のリクエストを実行すると、インジェストエラーが発生したファイルをダウンロードできます。

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/export/batches/{BATCH_ID}/failed?path={FAILED_FILE}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

以前に取り込んだバッチの日時が無効なので、次の検証エラーが表示されます。

```json
{
    "_validationErrors": [
        {
            "causingExceptions": [],
            "keyword": "format",
            "message": "[2018-00-23T22:07:01Z] is not a valid date-time. Expected [yyyy-MM-dd'T'HH:mm:ssZ, yyyy-MM-dd'T'HH:mm:ss.[0-9]{1-9}Z, yyyy-MM-dd'T'HH:mm:ss[+-]HH:mm, yyyy-MM-dd'T'HH:mm:ss.[0-9]{1,9}[+-]HH:mm]",
            "pointerToViolation": "#/timestamp",
            "schemaLocation": "#/properties/timestamp"
        }
    ]
}
```

## 次の手順

このチュートリアルを読んだ後、失敗したバッチからエラーを取得する方法を学びました。 バッチインジェストの詳細については、『バッチインジェスト開発者ガ [イド』を参照してくださ](../batch-ingestion/overview.md)い。 ストリーミング取り込みの詳細については、「ストリーミング接続の作成」チ [ュートリアルを参照してくださ](../tutorials/create-streaming-connection.md)い。

## 付録

この節では、発生する可能性のあるその他のインジェストエラータイプに関する情報を説明します。

### XDMの形式が正しくありません

前述の例のフローのタイムスタンプエラーと同様に、これらのエラーはXDMの形式が正しくないことが原因です。 これらのエラーメッセージは、問題の性質によって異なります。 その結果、特定のエラー例は表示されません。

### IMS組織IDが見つからないか、無効です

このエラーは、IMS組織IDがペイロードにないか、無効な場合に表示されます。

```json
{
    "type": "http://ns.adobe.com/adobecloud/problem/data-collection-service/inlet",
    "status": 400,
    "title": "Invalid XDM Message Format",
    "report": {
        "message": "inletId: [{INLET_ID}] imsOrgId: [{IMS_ORG}@AdobeOrg] Message has an absent or wrong ims org in the header"
    }
}
```

### XDMスキーマ

このエラーは、のが見つからな `schemaRef` い場合に表 `xdmMeta` 示されます。

```json
{
    "type": "http://ns.adobe.com/adobecloud/problem/data-collection-service/inlet",
    "status": 400,
    "title": "Invalid XDM Message Format",
    "report": {
        "message": "inletId: [{INLET_ID}] imsOrgId: [{IMS_ORG}@AdobeOrg] Message has unknown xdm format"
    }
}
```

### ソース名が見つかりません

このエラーは、ヘッダー内のが見つ `source` からない場合に表示されま `name`す。

```json
{
    "_errors":{
        "_streamingValidation": [
            {
                "message": "Payload header is missing Source Name"
            }
        ]
    }
}
```

### XDMエンティティが見つかりません

このエラーは、存在しない場合に表示され `xdmEntity` ます。

```json
{
    "_validationErrors": [
        {
            "message": "Payload body is missing xdmEntity"
        }
    ]
}
```
