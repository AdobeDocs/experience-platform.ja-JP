---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 失敗したバッチの取得
topic: overview
translation-type: tm+mt
source-git-commit: 4817162fe2b7cbf4ae4c1ed325db2af31da5b5d3
workflow-type: tm+mt
source-wordcount: '623'
ht-degree: 1%

---


# APIを使用した失敗したバッチの取得

Adobe Experience Platformには、データのアップロードと取り込みに2つの方法が用意されています。 バッチ取り込みを使用すると、様々なファイルタイプ（CSVなど）を使用してデータを挿入できます。また、ストリーミングエンドポイントを使用してリアルタイムにデータをプラットフォームに挿入できます。

このチュートリアルでは、Data Ingest APIを使用して、失敗したバッチに関する情報を取得する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

- [Experience Data Model(XDM)System](../../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
- [データ取り込み](../home.md): データをエクスペリエンスプラットフォームに送信する方法。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](../../landing/troubleshooting.md#how-do-i-format-an-api-request) 。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

スキーマレジストリに属するリソースを含む、エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、「 [サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)」を参照してください。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

- Content-Type: `application/json`

### 失敗したバッチのサンプル

このチュートリアルでは、サンプルデータを使用し、タイムスタンプの形式が不適切で、月の値を **00に設定します**。以下に例を示します。

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

## 失敗したバッチを取得します

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

上記の応答を使用すると、成功したバッチと失敗したバッチのチャンクを確認できます。 この応答から、失敗したバッチがファイルに含まれているこ `part-00000-44c7b669-5e38-43fb-b56c-a0686dabb982-c000.json` とがわかります。

## 失敗したバッチのダウンロード

失敗したバッチ内のファイルを特定したら、失敗したファイルをダウンロードして、エラーメッセージを確認できます。

**API形式**

```http
GET /batches/{BATCH_ID}/failed?path={FAILED_FILE}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | 失敗したファイルを含むバッチのID。 |
| `{FAILED_FILE}` | フォーマットに失敗したファイルの名前。 |

**リクエスト**

次の要求を実行すると、インジェストエラーが発生したファイルをダウンロードできます。

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

以前に取り込んだバッチに無効な日時が含まれていたので、次の検証エラーが表示されます。

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

このチュートリアルを読み終えた後、失敗したバッチからエラーを取得する方法を学びました。 バッチインジェストの詳細については、『 [バッチインジェスト開発者ガイド](../batch-ingestion/overview.md)』を参照してください。 ストリーミング取り込みの詳細については、「ストリーミング接続の [作成」のチュートリアルを参照してください](../tutorials/create-streaming-connection.md)。

## 付録

このセクションでは、発生する可能性がある他のインジェストエラータイプに関する情報を説明します。

### XDMが正しくフォーマットされていません

前の例のフローのタイムスタンプエラーと同様、これらのエラーは、XDMが正しくフォーマットされていないことが原因です。 これらのエラーメッセージは、問題の性質に応じて異なります。 その結果、特定のエラー例は表示されません。

### IMS組織IDがないか、無効です

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

### XDMスキーマがありません

このエラーは、のがない場合 `schemaRef` に表示 `xdmMeta` されます。

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

### ソース名がありません

このエラーは、ヘッダー `source` 内のが見つからない場合に表示され `name`ます。

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

### XDMエンティティがありません

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
