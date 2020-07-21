---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ストリーミング取り込みの検証
topic: overview
translation-type: tm+mt
source-git-commit: 73a492ba887ddfe651e0a29aac376d82a7a1dcc4
workflow-type: tm+mt
source-wordcount: '815'
ht-degree: 3%

---


# ストリーミング取り込みの検証

ストリーミング取り込みを使用すると、リアルタイムでストリーミングエンドポイントを使用してAdobe Experience Platformにデータをアップロードできます。 ストリーミング取り込みAPIは、同期と非同期の2つの検証モードをサポートしています。

## はじめに

このガイドでは、次のAdobe Experience Platformのコンポーネントについて、十分に理解している必要があります。

- [!DNL Experience Data Model (XDM) System](../../xdm/home.md): 顧客体験データを [!DNL Experience Platform] 整理するための標準化されたフレームワーク。
- [!DNL Streaming Ingestion](../streaming-ingestion/overview.md): データの送信先のメソッドの1つ [!DNL Experience Platform]。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

### 必要なヘッダーの値の収集

APIを呼び出すには、まず [!DNL Platform] 認証チュートリアルを完了する必要があり [ます](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

に属するリソース [!DNL Experience Platform]を含む、のすべてのリソースは、特定の仮想サンドボックスに分離され [!DNL Schema Registry]ます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>のサンドボックスについて詳し [!DNL Platform]くは、 [Sandboxの概要ドキュメントを参照してください](../../sandboxes/home.md)。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

- Content-Type: `application/json`

### 検証範囲

[!DNL Streaming Validation Service] は、次の領域の検証について説明します。
- 範囲
- 配置
- 列挙
- パターン
- タイプ
- 書式

## 同期検証

同期検証は、インジェストが失敗した理由に関する直接のフィードバックを提供する検証方法です。 ただし、失敗すると、検証に失敗したレコードは破棄され、ダウンストリームに送信されることはありません。 そのため、同期検証は開発プロセスでのみ使用する必要があります。 同期検証を行うと、呼び出し元にはXDM検証の結果と、失敗した場合は失敗の理由の両方が通知されます。

デフォルトでは、同期検証はオンになっていません。 これを有効にするには、API呼び出しを行う際に、オプションのクエリパラメーター `synchronousValidation=true` を渡す必要があります。 また、同期検証は現在、ストリームエンドポイントがVA7データセンター上にある場合にのみ使用できます。

同期検証中にメッセージが失敗した場合、メッセージは出力キューに書き込まれず、ユーザーに即座にフィードバックが返されます。

**API形式**

```http
POST /collection/{CONNECTION_ID}?synchronousValidation=true
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 以前に作成したストリーミング接続の `id` 値。 |

**リクエスト**

同期検証を行って、データをデータインレットに取り込むために次のリクエストを送信します。

```shell
curl -X POST https://dcs.adobedc.net/collection/{CONNECTION_ID}?synchronousValidation=true \
  -H "Content-Type: application/json" \
  -d '{JSON_PAYLOAD}'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{JSON_PAYLOAD}` | 取り込むデータのJSON本文。 |

**応答**

同期検証が有効な場合、成功した応答には、そのペイロードで検証エラーが発生したものが含まれます。

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

上記の応答リストでは、スキーマ違反が検出された回数と違反の内容が示されます。 例えば、この応答では、キー `workEmail` とキーがスキーマで定義されていな `person` いため、許可されていないことを示しています。 また、スキーマがaを予期していたが、代わりにaが挿入さ `_id` れたため、値に不正なフラグが付けられ `string`て `long` います。 5つのエラーが発生すると、検証サービスはそのメッセージの **処理を停止します** 。 ただし、他のメッセージは引き続き解析されます。

## 非同期検証

非同期検証は、即時のフィードバックを提供しない検証方法です。 その代わりに、データの損失を防ぐために、失敗したバッチ [!DNL Data Lake] にデータが送信されます。 この失敗したデータは、後で取得して、さらに分析および再生することができます。 この方法は実稼働環境で使用する必要があります。 特に要求がない限り、ストリーミング取り込みは非同期検証モードで行われます。

**API形式**

```http
POST /collection/{CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 以前に作成したストリーミング接続の `id` 値。 |

**リクエスト**

非同期検証でデータを取り込むには、次のリクエストを送信します。

```shell
curl -X POST https://dcs.adobedc.net/collection/{CONNECTION_ID} \
  -H "Content-Type: application/json" \
  -d '{JSON_PAYLOAD}'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{JSON_PAYLOAD}` | 取り込むデータのJSON本文。 |

>[!NOTE]
>
>非同期検証はデフォルトで有効なので、追加のクエリパラメーターは必要ありません。

**応答**

非同期検証が有効な場合、成功した応答は次の値を返します。

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

同期検証が明示的に要求されていないので、応答では同期検証がスキップされたことを示しています。

## 付録

この節では、データを取り込むための応答に対する様々なステータスコードの意味について説明します。

### ステータスコード

| ステータスコード | 意味 |
| ----------- | ------------- |
| 200 | 成功. 同期検証の場合、検証チェックに合格したことを意味します。 非同期検証の場合、メッセージが正常に受信されたのみであることを意味します。 最終的なメッセージのステータスは、データセットを観察することで確認できます。 |
| 400 | エラー. お願いに何か間違いがある。 ストリーミング検証サービスから、詳細な情報を含むエラーメッセージが受信されます。 |
| 401 | エラー. リクエストは未承認です。ベアラートークンを使用してリクエストする必要があります。 アクセスをリクエストする方法の詳細については、この [チュートリアル](../../tutorials/authentication.md) または [ブログ投稿をご覧ください](https://medium.com/adobetech/using-postman-for-jwt-authentication-on-adobe-i-o-7573428ffe7f)。 |
| 500 | エラー. 内部システムエラーが発生しました。 |
| 501 | エラー. つまり、この場所では同期検証がサポートされ **ていません** 。 |
| 503 | エラー. サービスは現在使用できません。 クライアントは、指数的なバックオフ戦略を使用して、少なくとも3回再試行する必要があります。 |