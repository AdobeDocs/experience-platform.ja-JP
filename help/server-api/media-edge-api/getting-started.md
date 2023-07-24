---
solution: Experience Platform
title: Media Edge API の基本を学ぶ
description: Media Edge API の基本を学ぶ
source-git-commit: 3d0f2823dcf63f25c3136230af453118c83cdc7e
workflow-type: tm+mt
source-wordcount: '959'
ht-degree: 62%

---


# Media Edge API の基本を学ぶ

このガイドでは、Media Edge API サービスとの最初のインタラクションを成功させるための手順を説明します。これには、メディアセッションの開始と、Adobe Experience Platformソリューションに送信されるCustomer Journey Analytics(CJA) の追跡が含まれます。 Media Edge API サービスは、セッション開始エンドポイントで開始されます。セッションが開始されると、次の 1 つ以上のイベントをトラックできます。

* `play`
* `ping`
* `bitrateChange`
* `bufferStart`
* `pauseStart`
* `adBreakStart`
* `adStart`
* `adComplete`
* `adSkip`
* `adBreakComplete`
* `chapterStart`
* `chapterComplete`
* `chapterSkip`
* `error`
* `sessionEnd`
* `sessionComplete`
* `statesUpdate`

各イベントには、独自のエンドポイントがあります。 すべての Media Edge API エンドポイントは POST メソッドであり、イベントデータ用の JSON リクエスト本文が含まれます。Media Edge API エンドポイント、パラメーターおよび例について詳しくは、 [Media Edge Swagger ファイル](swagger.md).

このガイドでは、セッションの開始後に次のイベントを追跡する方法を説明します。

* [バッファー開始](#buffer-start-event-request)
* [再生](#play-event-request)
* [セッション完了](#session-complete-event-request)

## API の実装 {#implement-api}

呼び出されるモデルとパスの小さな違いに加えて、Media Edge API は、 [メディアコレクション API](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/streaming-media-apis/mc-api-overview.html?lang=ja). 次のドキュメントで説明しているように、Media Collection の実装の詳細は Media Edge API に対して引き続き有効です。

* [プレーヤーでの HTTP リクエストタイプの設定](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/streaming-media-apis/mc-api-impl/mc-api-sed-pings.html?lang=ja)
* [ping イベントの送信](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/streaming-media-apis/mc-api-impl/mc-api-sed-pings.html?lang=ja)
* [タイムアウト条件](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/streaming-media-apis/mc-api-impl/mc-api-timeout.html?lang=ja)
* [イベントの順序の制御](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/streaming-media-apis/mc-api-impl/mc-api-ctrl-order.html?lang=ja)

## 認証 {#authorization}

現在、Media Edge API のリクエストには認証ヘッダーは必要ありません。


## セッションの開始 {#start-session}

サーバー上でメディアセッションを開始するには、セッション開始エンドポイントを使用します。正常な応答には、後続のイベントリクエストの必須パラメーターである `sessionId` が含まれます。

セッション開始リクエストを行う前に、次の情報が必要です。

* この `datastreamId`—POSTSession Start リクエストの必須パラメータ。 `datastreamId` を取得するには、[データストリームの設定](https://experienceleague.adobe.com/docs/experience-platform/datastreams/configure.html?lang=ja)を参照してください。

* 必要最小限のデータを含むリクエストペイロードの JSON オブジェクト（以下のリクエストの例を参照）。

この情報を取得したら、次の呼び出しで `datastreamId` を指定します。

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

上記のリクエストの例では、`eventType` 値には、ドメインを指定するための[エクスペリエンスデータモデル（XDM）](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=ja)に従ったプレフィックス `media.` が含まれています。

また、のデータ型マッピング `eventType` 上記の例では、次のようになります。

| eventType | データタイプ |
| -------- | ------ |
| media.SessionStart | [sessionDetails](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/sessiondetails.schema.md) |
| media.chapterStart | [chapterDetails](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/chapterdetails.schema.md) |
| media.adBreakStart | [advertisingPodDetails](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/advertisingpoddetails.schema.md) |
| media.adStart | [advertisingDetails](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/advertisingdetails.schema.md) |
| media.error | [errorDetails](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/errordetails.schema.md) |
| media.statesUpdate | [statesStart](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/mediadetails.schema.md#xdmstatesstart):配列[playerStateData], [statesEnd](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/mediadetails.schema.md#xdmstatesend):配列[playerStateData] |
| media.sessionStart、media.chapterStart、media.adStart | [customMetadata](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/mediadetails.schema.md#xdmcustommetadata) |
| すべて | [qoeDataDetails](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/qoedatadetails.schema.md) |

### 応答の例

次の例は、セッション開始リクエストに対する正常な応答を示しています。

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

上記の応答例では、`sessionId` は `af8bb22766e458fa0eef98c48ea42c9e351c463318230e851a19946862020333` として示されています。この ID は、後続のイベントリクエストで必須パラメーターとして使用します。

セッション開始エンドポイントのパラメーターと例について詳しくは、 [Media Edge Swagger](swagger.md) ファイル。

XDM メディアデータパラメーターについて詳しくは、[メディア詳細情報スキーマ](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/mediadetails.schema.md#xdmplayhead)を参照してください。


## バッファー開始イベントリクエスト {#buffer-start}

バッファー開始イベントは、メディアプレーヤーでバッファリングが開始された際にシグナルで通知します。バッファー再開は API サービスのイベントではありません。代わりに、バッファー開始の後に play イベントが送信されると推論されます。 バッファー開始イベントリクエストを行うには、次のエンドポイントへの呼び出しのペイロードで `sessionId` を使用します。

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

上記の例のリクエストでは、同じ `sessionId` 前の呼び出しで返された値は、Buffer Start リクエストの必須パラメーターとして使用されます。

成功時の応答はステータスが 200 で、コンテンツは含まれていません。

Buffer Start エンドポイントのパラメータと例について詳しくは、 [Media Edge Swagger](swagger.md) ファイル。


## 再生イベントリクエスト {#play-event}

再生イベントは、メディアプレーヤーが「バッファリング」、「一時停止」、「エラー」などの別の状態から「再生中」に状態を変更した際に送信されます。再生イベントリクエストを行うには、次のエンドポイントへの呼び出しのペイロードで `sessionId` を使用します。

**POST**  `https://edge.adobedc.net/ee-pre-prd/va/v1/play \`

### リクエストの例

次の例は、再生 cURL リクエストを示しています。

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

Play エンドポイントのパラメーターと例について詳しくは、 [Media Edge Swagger](swagger.md) ファイル。

## セッション完了イベントリクエスト {#session-complete}

セッション完了イベントは、メインコンテンツの終了に達すると送信されます。セッション完了イベントリクエストを行うには、次のエンドポイントへの呼び出しのペイロードで `sessionId` を使用します。

**POST**  `https://edge.adobedc.net/ee-pre-prd/va/v1/sessionComplete \`

### リクエストの例

次の例は、セッション完了 cURL リクエストを示しています。

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

セッション完了エンドポイントのパラメーターと例について詳しくは、 [Media Edge Swagger](swagger.md) ファイル。

## 応答コード

次の表は、Media Edge API リクエストから生成される可能性のある応答コードを示しています。

| ステータス | 説明 |
| ---------- | --------- |
| 200 | セッションが正常に作成されました |
| 207 | Experience Edge ネットワークに接続するサービスの 1 つに関する問題 ( 詳しくは、 [トラブルシューティングガイド](troubleshooting.md)) |
| 400 レベル | リクエストが正しくありません |
| 500 レベル | サーバーエラー |

## この機能に関するその他のヘルプ

* [メディアエッジトラブルシューティングガイド](troubleshooting.md)
* [Media Edge API の概要](overview.md)


