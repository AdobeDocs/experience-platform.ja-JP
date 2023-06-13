---
keywords: Experience Platform；メディアエッジ；人気の高いトピック；日付範囲
solution: Experience Platform
title: Media Edge API の概要
description: Media Edge API の概要
exl-id: null
source-git-commit: 696ddd93d87601f9f6dedfd651ee12573dc4990a
workflow-type: tm+mt
source-wordcount: '963'
ht-degree: 7%

---


# Media Edge API の概要

このガイドでは、Media Edge API サービスとの最初のインタラクションを成功させる手順を説明します。 これには、メディアセッションの開始と、Adobe Experience Platform(AEP) ソリューションに送信されたCustomer Journey Analytics(CJA) の追跡が含まれます。 Media Edge API サービスは、セッション開始エンドポイントで開始されます。 セッションが開始されると、次のイベントを 1 つ以上追跡できます。

* play
* ping
* bitrateChange
* bufferStart
* pauseStart
* adBreakStart
* adStart
* adComplete
* adSkip
* adBreakComplete
* chapterStart
* chapterComplete
* chapterSkip
* error
* sessionEnd
* sessionComplete
* statesUpdate

各イベントには、独自のエンドポイントがあります。 すべての Media Edge API エンドポイントはPOSTメソッドで、イベントデータ用の JSON リクエスト本文が含まれます。 Media Edge API のエンドポイント、パラメーターおよび例について詳しくは、 Media Edge Swagger ファイルを参照してください。

このガイドでは、セッションの開始後に次のイベントを追跡する方法について説明します。

* バッファー開始
* Play
* セッション完了

## API の実装

呼び出されるモデルとパスの小さな違いに加えて、Media Edge API はメディアコレクション API と同じです。 メディアコレクションの実装の詳細は、次のドキュメントに示すように、引き続き Media Edge API に対して有効です。

* [プレーヤーでの HTTP リクエストタイプの設定](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/streaming-media-apis/mc-api-impl/mc-api-sed-pings.html?lang=en)
* [ping イベントの送信](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/streaming-media-apis/mc-api-impl/mc-api-sed-pings.html?lang=en)
* [タイムアウト条件](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/streaming-media-apis/mc-api-impl/mc-api-timeout.html?lang=en)
* [イベントの順序の制御](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/streaming-media-apis/mc-api-impl/mc-api-ctrl-order.html?lang=en)

## Authorization

現在、Media Edge API は、リクエストに認証ヘッダーを必要としません。


## セッションの開始

サーバーでメディアセッションを開始するには、セッション開始エンドポイントを使用します。 成功した応答には、 `sessionId`：後続のイベントリクエストの必須パラメーター。

セッション開始リクエストを実行する前に、次の手順を実行する必要があります。

* この `datastreamId` は、セッションセッション開始リクエストの必須POSTです。 を取得するには、以下を実行します。 `datastreamId`を参照してください。 [データストリームの設定](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/configure.html?lang=ja).

* 必要最小限のデータ（以下のリクエストの例で示すように）を含む、リクエストペイロードの JSON オブジェクト。

この情報を入手したら、 `datastreamId` を次の呼び出しで使用します。

**POST**  `https://edge.adobedc.net/ee-pre-prd/va/v1/sessionStart?configId={datastream ID} \`

### リクエストの例

次の例は、セッション開始 cURL リクエストを示しています。

```
curl -i --request POST '{uri}/ee/va/v1/sessionStart?configId={dataStreamId}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "events": [
    {
      "xdm": {
        "eventType": "media.sessionStart",
        "mediaCollection": {
          "sessionDetails": {
            "name": "Media Analytics API Sample",
            "playerName": "sample-html5-api-player",
            "contentType": "VOD",
            "length": 60,
            "channel": "sample-channel",
            "appVersion": "va-api-1.0.0"
          },
          "playhead": 0
        }
      }
    }
  ]
}'
```

上記の例では、 `eventType` 値にプレフィックスが含まれる `media` 次によると [エクスペリエンスデータモデル (XDM)](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=ja) ドメインを指定する場合。

また、のデータ型マッピング `eventType` 上記の例では、次のようになります。

| eventType | データタイプ |
| -------- | ------ |
| mediaSessionStart | [sessionDetails](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/sessiondetails.schema.md) |
| media.chapterStart | [chapterDetails](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/chapterdetails.schema.md) |
| media.adBreakStart | [advertisingPodDetails](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/advertisingpoddetails.schema.md) |
| media.adStart | [advertisingDetails](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/advertisingdetails.schema.md) |
| media.error | [errorDetails](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/errordetails.schema.md) |
| media.statesUpdate | [statesStart](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/mediadetails.schema.md#xdmstatesstart):配列[playerStateData], [statesEnd](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/mediadetails.schema.md#xdmstatesend):配列[playerStateData] |
| media.sessionStart, media.chapterStart, media.adStart | [customMetadata](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/mediadetails.schema.md#xdmcustommetadata) |
| すべて | [qoeDataDetails](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/qoedatadetails.schema.md) |

### 応答の例

次の例は、セッション開始リクエストの成功応答を示しています。

```
HTTP/2 200
x-request-id: 99603f5c-95cf-49ad-9afb-0ba6c5867fd7
x-rate-limit-remaining: 599
vary: Origin
date: Tue, 07 Mar 2023 14:37:58 GMT
x-konductor: 23.3.2:367bc7dc
x-adobe-edge: IRL1;6
server: jag
content-type: application/json;charset=utf-8
strict-transport-security: max-age=31536000; includeSubDomains
cache-control: no-cache, no-store, max-age=0, no-transform
x-xss-protection: 1; mode=block
x-content-type-options: nosniff

{
    "requestId": "df14bca1-ba0f-4574-ae80-a4e24a960c00",
    "handle": [
        {
            "payload": [
                {
                    "sessionId": "af8bb22766e458fa0eef98c48ea42c9e351c463318230e851a19946862020333"
                }
            ],
            "type": "media-analytics:new-session",
            "eventIndex": 0
        },
        {
            "payload": [
                {
                    "key": "kndctr_6D9FE18C5536A5E90A4C98A6_AdobeOrg_cluster",
                    "value": "irl1",
                    "maxAge": 1800
                },
                {
                    "key": "kndctr_6D9FE18C5536A5E90A4C98A6_AdobeOrg_identity",
                    "value": "CiY1MTkxMDM4OTc1MzkwMTY4NTQ1NjAxNDg4OTgzODU5MTAzMDcyMVIPCKKt8KnsMBgBKgRJUkwx8AGirfCp7DA=",
                    "maxAge": 34128000
                }
            ],
            "type": "state:store"
        }
    ]
}
```

上記の例の応答では、 `sessionId` は次のように表示されます。 `af8bb22766e458fa0eef98c48ea42c9e351c463318230e851a19946862020333`. この ID は、後続のイベントリクエストで必須パラメーターとして使用します。

セッション開始エンドポイントのパラメーターと例について詳しくは、 Media Edge Swagger ファイルを参照してください。

XDM メディアデータパラメーターについて詳しくは、 [メディア詳細情報スキーマ](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/mediadetails.schema.md#xdmplayhead).


## バッファー開始イベントリクエスト

メディアプレーヤーでバッファリングが開始する際に、バッファー開始イベントシグナルが発生します。 バッファー再開は API サービスのイベントではありません。代わりに、バッファー開始の後に play イベントが送信されると推論されます。 バッファー開始イベントリクエストを作成するには、 `sessionId` を次のエンドポイントへの呼び出しのペイロードに追加します。

**POST**  `https://edge.adobedc.net/ee-pre-prd/va/v1/bufferStart \`

### リクエストの例

次の例は、バッファー開始 cURL リクエストを示しています。

```
curl -X 'POST' \
  'https://edge.adobedc.net/ee-pre-prd/va/v1/bufferStart' \
  -H 'accept: */*' \
  -H 'Content-Type: application/json' \
  -d '{
  "events": [
    {
      "xdm": {
        "eventType": "media.bufferStart",
        "mediaCollection": {
          "sessionID": "af8bb22766e458fa0eef98c48ea42c9e351c463318230e851a19946862020333",
          "playhead": 25
        },
        "timestamp": "2022-03-04T13:39:00+00:00"
      }
    }
  ]
}'
```

上記の例では、同じ `sessionId` 前の呼び出しで返された値は、Buffer Start リクエストの必須パラメーターとして使用されます。

バッファー開始エンドポイントのパラメーターと例について詳しくは、 Media Edge Swagger ファイルを参照してください。

成功時の応答はステータスが 200 で、コンテンツは含まれていません。

## イベントリクエストを再生

Play イベントは、メディアプレーヤーがその状態を「再生中」から「バッファリング」、「一時停止」、「エラー」などの別の状態に変更したときに送信されます。 Play イベントをリクエストするには、 `sessionId` を次のエンドポイントへの呼び出しのペイロードに追加します。

**POST**  `https://edge.adobedc.net/ee-pre-prd/va/v1/play \`

### リクエストの例

次の例は、Play cURL リクエストを示しています。

```
curl -X 'POST' \
  'https://edge.adobedc.net/ee-pre-prd/va/v1/play' \
  -H 'accept: */*' \
  -H 'Content-Type: application/json' \
  -d '{
  "events": [
    {
      "xdm": {
        "eventType": "media.play",
        "mediaCollection": {
          "sessionID": "af8bb22766e458fa0eef98c48ea42c9e351c463318230e851a19946862020333",
          "playhead": 25
        },
        "timestamp": "2022-03-04T13:39:00+00:00"
      }
    }
  ]
}'
```

成功時の応答はステータスが 200 で、コンテンツは含まれていません。

Play エンドポイントのパラメーターと例について詳しくは、 Media Edge Swagger ファイルを参照してください。

## Session Complete イベントリクエスト

セッション完了イベントは、メインコンテンツの終わりに達したときに送信されます。 Session Complete イベントリクエストを作成するには、 `sessionId` を次のエンドポイントへの呼び出しのペイロードに追加します。

**POST**  `https://edge.adobedc.net/ee-pre-prd/va/v1/sessionComplete \`

### リクエストの例

次の例は、Session Complete cURL リクエストを示しています。

```
curl -X 'POST' \
  'https://edge.adobedc.net/ee-pre-prd/va/v1/sessionComplete' \
  -H 'accept: */*' \
  -H 'Content-Type: application/json' \
  -d '{
  "events": [
    {
      "xdm": {
        "eventType": "media.sessionComplete",
        "mediaCollection": {
          "sessionID": "af8bb22766e458fa0eef98c48ea42c9e351c463318230e851a19946862020333",
          "playhead": 25
        },
        "timestamp": "2022-03-04T13:39:00+00:00"
      }
    }
  ]
}'
```

成功時の応答はステータスが 200 で、コンテンツは含まれていません。

## 応答コード

次の表に、Media Edge API リクエストによって生じる可能性のある応答コードを示します。

| ステータス | 説明 |
| ---------- | --------- |
| 200 | セッションが正常に作成されました |
| 207 | Experience Edge Network に接続するサービスの 1 つに関する問題（トラブルシューティングガイドの詳細を参照） |
| 400 レベル | Bad request |
| 500 レベル | サーバーエラー |

エラーと失敗した応答コードの処理について詳しくは、 Media Edge のトラブルシューティングガイドを参照してください。


