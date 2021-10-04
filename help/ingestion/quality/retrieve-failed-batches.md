---
keywords: Experience Platform；ホーム；人気のあるトピック；失敗したバッチの取得；失敗したバッチ；失敗したバッチ；バッチの取得；失敗したバッチ；失敗したバッチの取得；失敗したバッチの取得；失敗したバッチのダウンロード；失敗したバッチのダウンロード；
solution: Experience Platform
title: データアクセス API を使用した失敗したバッチの取得
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、データ取得 API を使用して、失敗したバッチに関する情報を取得する手順を説明します。
exl-id: 5fb9f28d-091e-4124-8d8e-b8a675938d3a
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 83%

---

# データアクセス API を使用した失敗したバッチの取得

Adobe Experience Platform でのデータのアップロードと取得には 2 つの方法があります。バッチ取り込みを使用すると、様々なファイルタイプ（CSV など）を使用してデータを挿入できます。また、ストリーミング取り込みを使用すると、ストリーミングエンドポイントをリアルタイムで使用して [!DNL Platform] にデータを挿入できます。

このチュートリアルでは、[!DNL Data Ingestion] API を使用して失敗したバッチに関する情報を取得する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

- [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Experience Platform] に使用される標準化されたフレームワーク。
- [[!DNL Data Ingestion]](../home.md)：[!DNL Experience Platform] にデータを送信するメソッド。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization： Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id： `{IMS_ORG}`

[!DNL Schema Registry] に属するリソースを含む [!DNL Experience Platform] 内のすべてのリソースは、特定の仮想サンドボックスに分離されます。 [!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name： `{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

- Content-Type: `application/json`

### 失敗したバッチの例

このチュートリアルでは、次のように、月の値を **00** に設定する、誤った形式のタイムスタンプを持つデータ例を使用します。

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

タイムスタンプの形式が正しくないため、上記のペイロードは XDM スキーマに対して正しく検証されません。

## 失敗したバッチの取得

**API 形式**

```http
GET /batches/{BATCH_ID}/failed
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | 検索するバッチの ID。 |

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

上記の応答で、成功したバッチと失敗したバッチのチャンクを確認できます。この応答から、失敗したバッチが `part-00000-44c7b669-5e38-43fb-b56c-a0686dabb982-c000.json` ファイルに含まれていることがわかります。

## 失敗したバッチのダウンロード

バッチで失敗したファイルがわかったら、失敗したファイルをダウンロードして、エラーメッセージを確認できます。

**API 形式**

```http
GET /batches/{BATCH_ID}/failed?path={FAILED_FILE}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | 失敗したファイルを含むバッチの ID。 |
| `{FAILED_FILE}` | 形式設定に失敗したファイルの名前。 |

**リクエスト**

次のリクエストを実行すると、取得エラーが発生したファイルをダウンロードできます。

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

以前に取得したバッチの日時が無効なので、次の検証エラーが表示されます。

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

このチュートリアルでは、失敗したバッチからエラーを取得する方法を説明しました。バッチ取得の詳細については、『[バッチ取得開発者ガイド](../batch-ingestion/overview.md)』を参照してください。ストリーミング取得の詳細については、「[ストリーミング接続の作成チュートリアル](../tutorials/create-streaming-connection.md)」を参照してください。

## 付録

この節では、発生する可能性のあるその他の取得エラータイプに関する情報を説明します。

### XDM の形式が正しくありません

前述の例のフローのタイムスタンプエラーと同様に、これらのエラーの原因は XDM の形式が正しくないことです。これらのエラーメッセージは、問題の性質によって異なります。その結果、特定のエラー例は表示されません。

### IMS 組織 ID が見つからないか、無効です

このエラーは、IMS 組織 ID がペイロードにないか、無効な場合に表示されます。

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

### XDM スキーマがありません

このエラーは、`xdmMeta` の `schemaRef` が見つからない場合に表示されます。

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

このエラーは、ヘッダー内の `source` に `name` がない場合に表示されます。

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

### XDM エンティティが見つかりません

このエラーは、`xdmEntity` が存在しない場合に表示されます。

```json
{
    "_validationErrors": [
        {
            "message": "Payload body is missing xdmEntity"
        }
    ]
}
```
