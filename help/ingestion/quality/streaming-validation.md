---
keywords: Experience Platform；ホーム；人気のあるトピック；ストリーミング；ストリーミング取り込み；ストリーミング取り込みの検証；検証；ストリーミング取り込みの検証；検証；同期検証；非同期検証；非同期検証；
solution: Experience Platform
title: ストリーミング取り込みの検証
topic: tutorial
type: Tutorial
description: ストリーミング取り込みを使用すると、ストリーミングエンドポイントをリアルタイムで使用して、データを Adobe Experience Platform にアップロードできます。ストリーミング取り込み API は、同期と非同期の 2 つの検証モードをサポートしています。
translation-type: tm+mt
source-git-commit: 089a4d517476b614521d1db4718966e3ebb13064
workflow-type: tm+mt
source-wordcount: '875'
ht-degree: 82%

---


# ストリーミング取り込みの検証

ストリーミング取り込みを使用すると、ストリーミングエンドポイントをリアルタイムで使用して、データを Adobe Experience Platform にアップロードできます。ストリーミング取り込み API は、同期と非同期の 2 つの検証モードをサポートしています。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

- [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 編成する際に使用される標準化されたフレームワーク。
- [[!DNL Streaming Ingestion]](../streaming-ingestion/overview.md):データの送信先のメソッドの1つ [!DNL Experience Platform]。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

[!DNL Platform] APIを呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。 次に示すように、すべての[!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソース（[!DNL Schema Registry]に属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform]のサンドボックスについて詳しくは、[サンドボックスの概要ドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

- Content-Type: `application/json`

### 検証の範囲

[!DNL Streaming Validation Service] は、次の領域の検証について説明します。
- 範囲
- 配置
- 列挙
- パターン
- タイプ
- 形式

## 同期検証

同期検証は、取り込みが失敗した理由に関するフィードバックを即座に提供する検証方法です。ただし、検証に失敗すると、検証に失敗したレコードは破棄され、ダウンストリームに送信されなくなります。そのため、同期検証は開発プロセス中にのみ使用する必要があります。同期検証をおこなうと、呼び出し元には、XDM 検証の結果が通知され、失敗した場合はさらに失敗の理由も通知されます。

デフォルトでは、同期検証はオンになっていません。有効にするには、API 呼び出しをおこなう際に、オプションのクエリパラメーター `synchronousValidation=true` を渡す必要があります。また、現時点では、同期検証は、ストリームエンドポイントが VA7 データセンターにある場合にのみ使用できます。

同期検証中にメッセージが失敗した場合、そのメッセージは出力キューに書き込まれず、ユーザーに対して即座にフィードバックが提供されます。

**API 形式**

```http
POST /collection/{CONNECTION_ID}?synchronousValidation=true
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 作成済みのストリーミング接続の `id` 値です。 |

**リクエスト**

同期検証を使用してデータをデータインレットに取り込むには、次のリクエストを送信します。

```shell
curl -X POST https://dcs.adobedc.net/collection/{CONNECTION_ID}?synchronousValidation=true \
  -H "Content-Type: application/json" \
  -d '{JSON_PAYLOAD}'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{JSON_PAYLOAD}` | 取り込むデータの JSON 本文です。 |

**応答**

同期検証が有効な場合、成功時の応答のペイロードには、検出された検証エラーが含まれています。

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

上記の応答では、スキーマ違反の検出数と内容がリスト表示されます。例えば、この応答では、キー `workEmail` と `person` がスキーマに定義されていないので使用できないということを述べています。また、`_id` の値が正しくないとも警告しています。スキーマが `string` を想定していたにもかかわらず、実際には `long` が挿入されたからです。5 つのエラーが発生すると、検証サービスはそのメッセージの処理を&#x200B;**停止**&#x200B;します。ただし、他のメッセージは引き続き解析されます。

## 非同期検証

非同期検証は、即座にフィードバックを提供しない検証方法です。代わりに、データの損失を防ぐために、[!DNL Data Lake]内の失敗したバッチにデータが送信されます。 この失敗したデータを、後で取得して、さらに分析と再現をおこなうことができます。この方法は、実稼働環境で使用する必要があります。特に要求されない限り、ストリーミング取り込みは非同期検証モードで動作します。

**API 形式**

```http
POST /collection/{CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 作成済みのストリーミング接続の `id` 値です。 |

**リクエスト**

非同期検証を使用してデータをデータインレットに取り込むには、次のリクエストを送信します。

```shell
curl -X POST https://dcs.adobedc.net/collection/{CONNECTION_ID} \
  -H "Content-Type: application/json" \
  -d '{JSON_PAYLOAD}'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{JSON_PAYLOAD}` | 取り込むデータの JSON 本文です。 |

>[!NOTE]
>
> 非同期検証はデフォルトで有効なので、追加のクエリパラメーターは不要です。

**応答**

非同期検証が有効な場合、成功時の応答は以下を返します。

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

明示的に要求されていないので、同期検証がスキップされたことが応答に示されている点に注意してください。

## 付録

この節では、様々なステータスコードと、データ取り込みの応答での意味について説明します。

### ステータスコード

| ステータスコード | 意味 |
| ----------- | ------------- |
| 200 | 成功です。同期検証の場合は、検証チェックに合格したことを意味します。非同期検証の場合は、メッセージが正常に受信されたことだけを意味します。最終的なメッセージのステータスは、データセットを観察することで確認できます。 |
| 400 | エラーです。リクエストに何か間違いがあります。ストリーミング検証サービスから、詳細を含んだエラーメッセージが表示されます。 |
| 401 | エラーです。リクエストは未承認です。ベアラトークンを使用してリクエストする必要があります。アクセスをリクエストする方法について詳しくは、[このチュートリアル](https://www.adobe.com/go/platform-api-authentication-en)または[ブログ投稿](https://medium.com/adobetech/using-postman-for-jwt-authentication-on-adobe-i-o-7573428ffe7f)を参照してください。 |
| 500 | エラーです。内部システムエラーが発生しました。 |
| 501 | エラーです。つまり、この場所では同期検証が&#x200B;**サポートされていません**。 |
| 503 | エラーです。サービスは現在利用できません。クライアントは、指数バックオフ戦略を使用して、少なくとも 3 回再試行する必要があります。 |