---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ストリーミング取り込みの検証
topic: overview
translation-type: tm+mt
source-git-commit: 79466c78fd78c0f99f198b11a9117c946736f47a

---


# ストリーミング取り込みの検証

ストリーミングの取り込みを使用すると、リアルタイムでストリーミングエンドポイントを使用して、データをAdobe Experience Platformにアップロードできます。 ストリーミング取り込みAPIは、同期と非同期の2つの検証モードをサポートしています。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントに関する作業を理解している必要があります。

- [Experience Data Model(XDM)System](../../xdm/home.md):エクスペリエンスプラットフォームが顧客エクスペリエンスデータを整理するための標準化されたフレームワーク。
- [Streaming Ingest](../streaming-ingestion/overview.md):データをエクスペリエンスプラットフォームに送信する方法の1つ。

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

### 検証範囲

ストリーミング検証サービスでは、次の領域での検証が行われます。
- 範囲
- 配置
- 列挙
- パターン
- タイプ
- 形式

## 同期検証

同期検証は、インジェストが失敗した理由に関するフィードバックを即座に提供する検証方法です。 ただし、検証に失敗すると、検証に失敗したレコードは破棄され、ダウンストリームに送信されなくなります。 その結果、同期検証は開発プロセス中にのみ使用する必要があります。 同期検証を行うと、呼び出し元には、XDM検証の結果と、失敗した場合は失敗の理由の両方が通知されます。

デフォルトでは、同期検証はオンになっていません。 これを有効にするには、API呼び出しを行う際に、オプションのクエリ `synchronousValidation=true` パラメーターを渡す必要があります。 また、同期検証は、現在、ストリームエンドポイントがVA7データセンター上にある場合にのみ使用できます。

同期検証中にメッセージが失敗した場合、メッセージは出力キューに書き込まれず、ユーザーに対して即座にフィードバックが提供されます。

**API形式**

```http
POST /collection/{CONNECTION_ID}?synchronousValidation=true
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 以前に `id` 作成したストリーミング接続の値。 |

**リクエスト**

同期検証を使用して、データをデータインレットに取り込むために、次のリクエストを送信します。

```shell
curl -X POST https://dcs.adobedc.net/collection/{CONNECTION_ID}?synchronousValidation=true \
  -H "Content-Type: application/json" \
  -d '{JSON_PAYLOAD}'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{JSON_PAYLOAD}` | 取り込むデータのJSON本文。 |

**応答**

同期検証が有効な場合、成功した応答には、そのペイロードで検出された検証エラーが含まれます。

```json
{
    "type": "http://ns.adobe.com/adobecloud/problem/data-collection-service/inlet",
    "status": 400,
    "title": "Invalid XDM Message Format",
    "report": {
        "message": "inletId: [6aca7aa2d87ebd6b2780ca5724d94324a14475f140a2b69373dd5c714430dfd4] imsOrgId: [7BF122A65C5B3FE40A494026@AdobeOrg] Message is invalid",
        "cause": {
            "_streamingValidation": [
                {
                    "schemaLocation": "#",
                    "pointerToViolation": "#",
                    "causingExceptions": [
                        {
                            "schemaLocation": "#",
                            "pointerToViolation": "#",
                            "causingExceptions": [],
                            "keyword": "additionalProperties",
                            "message": "extraneous key [workEmail] is not permitted"
                        },
                        {
                            "schemaLocation": "#",
                            "pointerToViolation": "#",
                            "causingExceptions": [],
                            "keyword": "additionalProperties",
                            "message": "extraneous key [person] is not permitted"
                        },
                        {
                            "schemaLocation": "#/properties/_id",
                            "pointerToViolation": "#/_id",
                            "causingExceptions": [],
                            "keyword": "type",
                            "message": "expected type: String, found: Long"
                        }
                    ],
                    "message": "3 schema violations found"
                }
            ]
        }
    }
}
```

上記の応答では、リスト違反の検出数とスキーマ違反の検出内容が示されます。 例えば、この応答では、キーとがスキーマで定義さ `workEmail` れていな `person` いため、許可されていないと記述されています。 また、値に正しくないフラグが付けられ `_id` ます。スキーマがaを予期していたが、代 `string`わりにaが挿入され `long` たためです。 5つのエラーが発生すると、検証サービスはそのメッセージの **処理を** 停止します。 ただし、他のメッセージは引き続き解析されます。

## 非同期検証

非同期検証は、即座にフィードバックを提供しない検証方法です。 代わりに、データが失敗したバッチにData Lakeでデータが送信され、データが失われるのを防ぎます。 この失敗したデータは、後で取得して、さらに分析と再生を行うことができます。 この方法は、実稼働環境で使用する必要があります。 特に要求がない限り、ストリーミング取り込みは非同期検証モードで行われます。

**API形式**

```http
POST /collection/{CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 以前に `id` 作成したストリーミング接続の値。 |

**リクエスト**

非同期検証でデータを取り込むために、次のリクエストを送信します。

```shell
curl -X POST https://dcs.adobedc.net/collection/{CONNECTION_ID} \
  -H "Content-Type: application/json" \
  -d '{JSON_PAYLOAD}'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{JSON_PAYLOAD}` | 取り込むデータのJSON本文。 |

>[!NOTE] 非同期クエリはデフォルトで有効なので、追加の検証パラメーターは不要です。

**応答**

非同期検証が有効な場合、応答が成功すると次の値が返されます。

```json
{
    "inletId": "f6ca9706d61de3b78be69e2673ad68ab9fb2cece0c1e1afc071718a0033e6877",
    "xactionId": "1555445493896:8600:8",
    "receivedTimeMs": 1555445493932,
    "synchronousValidation": {
        "skipped": true
    }
}
```

同期検証が明示的に要求されていないので、応答で同期検証がスキップされたことが示されていることに注意してください。

## 付録

この節では、データを取り込む応答に対する様々なステータスコードの意味について説明します。

### ステータスコード

| ステータスコード | 意味 |
| ----------- | ------------- |
| 200 | 成功. 同期検証の場合は、検証チェックに合格したことを意味します。 非同期検証の場合、メッセージが正常に受信されたのみであることを意味します。 最終的なメッセージのステータスは、データセットを観察することで確認できます。 |
| 400 | エラー. お願いに何か間違いがある。 ストリーミング検証サービスから、詳細を含むエラーメッセージが表示されます。 |
| 401 | エラー. リクエストは未承認です。ベアラトークンを使用してリクエストする必要があります。 アクセスをリクエストする方法の詳細については、このチュートリアルまたは [この](../../tutorials/authentication.md) ブログ投稿を参照 [してください](https://medium.com/adobetech/using-postman-for-jwt-authentication-on-adobe-i-o-7573428ffe7f)。 |
| 500 | エラー. 内部システムエラーが発生しました。 |
| 501 | エラー. つまり、この場所では同期検証 **がサ** ポートされていません。 |
| 503 | エラー. サービスは現在使用できません。 クライアントは、指数バックオフ戦略を使用して、少なくとも3回再試行する必要があります。 |