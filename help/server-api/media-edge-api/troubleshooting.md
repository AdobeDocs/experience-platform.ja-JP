---
keywords: Experience Platform；メディアエッジ；人気の高いトピック；日付範囲
solution: Experience Platform
title: Media Edge API の概要
description: Media Edge API のトラブルシューティングガイド
source-git-commit: f723114eebc9eb6bfa2512b927c5055daf97188b
workflow-type: tm+mt
source-wordcount: '678'
ht-degree: 1%

---


# Media Edge API トラブルシューティングガイド

このガイドでは、エラーの処理と正常な応答の取得に関するトラブルシューティング手順を説明します。

## エラー応答ツールの使用

失敗した応答のトラブルシューティングに役立つように、エラーには、エラーオブジェクトを含む応答本文が付随します。 この場合、応答本文には問題の詳細が含まれます。詳細は、 [RFC 7807 HTTP API の問題詳細](https://datatracker.ietf.org/doc/html/rfc7807). API のユーザーエクスペリエンスを向上させるために、問題の詳細は説明的です（配列キーの詳細は、見つからないフィールドまたは無効なフィールドに JsonPath を使用して表示されます）。 また、累積的な指標です（無効なフィールドはすべて同じリクエストでレポートされます）。


## セッション開始の検証

セッション開始リクエストの作成時に発生したほとんどの問題に対して、207 件のマルチステータス応答が発生します。
ペイロードは、Experience Edge Network Server API の致命的でないエラーに似ています。 すべての Media Analytics エラーのタイプは次のとおりです。  `https://ns.adobe.com/aep/errors/va-edge-0XXX-XXX`. 応答に表示される数値は、エラーステータスに対応します。

次の例は、セッション開始リクエストの応答本文で、必須フィールドがなく、無効なフィールドが含まれているものを示しています。

```
{
    "requestId": "d4be4f91-a535-41e7-80c6-bdd031d3a365",
    "handle": [
        ...
    ],
    "errors": [
        {
            "type": "https://ns.adobe.com/aep/errors/va-edge-0400-400",
            "status": 400,
            "title": "Invalid request",
            "report": {
                "eventIndex": 0,
                "details": [
                    {
                        "name": "$.xdm.mediaCollection.sessionDetails.name",
                        "reason": "Missing required field"
                    },
                    {
                        "name": "$.xdm.timestamp",
                        "reason": "Field should respect ISO 8601 standard for presenting date and time with offset to UTC (e.g. 2022-05-19T19:31:02Z, 2022-05-19T21:31:02+02:00, 2022-05-19T21:31:02.234+02:00)"
                    }
                ]
            }
        }
    ]
}
```

上記の例では、両方の問題が次のように指摘されています。 `name` および `reason` under `details`:最初の理由が表示されます `missing required field` 2 つ目は、ISO 8601 規格への準拠不可を示しています。


>[!NOTE]
>
> Media Analytics のアップストリームで発生したエラーについては、Adobeは、生成されたメディアセッションを引き続き処理することをお勧めします。

## イベントの検証

ほとんどの無効なイベントリクエストでは、400 Bad Request 応答が返されます。 この場合、ペイロードは、Experience Edge Network Server API の致命的なエラーに似ています。

イベントリクエストの場合、Media Edge API サービスには、XDM モデル自体に取り込まれていない追加のチェックが含まれます。 これには、パス `eventType` リクエストペイロードに一致 `eventType`.


次の例は、 `eventType` ～との不一致 `adBreakStart` に送信されたペイロード `ee/va/v1/chapterStart`:

```
{
    "type": "https://ns.adobe.com/aep/errors/va-edge-0400-400",
    "status": 400,
    "title": "Bad Request",
    "detail": "Invalid request. Please check your input and try again.",
    "report": {
        "details": "The payload eventType adBreakStart was different from the path eventType chapterStart"
    }
}
```

以下の例は、 `chapterStart` 呼び出しが見つかりません `chapterDetails` 情報：

```
{
    "type": "https://ns.adobe.com/aep/errors/va-edge-0400-400",
    "status": 400,
    "title": "Bad Request",
    "detail": "Invalid request. Please check your input and try again.",
    "report": {
        "details": [
            {
                "name": "$.events[0].xdm.mediaCollection.chapterDetails",
                "reason": "Missing required field"
            }
        ]
    }
}
```

## 400 レベルおよび 500 レベルのエラーの処理

次の表に、ステータス応答エラーの処理手順を示します。


| エラーコード | 説明 |
| ---------- | --------- |
| 4xx 無効なリクエスト | ほとんどの 4xx エラー ( 例： `400`, `403`, `404`) をユーザーが再試行しないでください。 リクエストを再試行しても、正常な応答は返されません。 ユーザーは、リクエストを再試行する前にエラーに対処する必要があります。 4xx ステータスコードに結び付くイベントは追跡されません。4xx 応答を受け取ったセッションのデータの正確性に影響を与える可能性があります。 |
| 410 消失 | 追跡対象のセッションがサーバー側で計算されなくなったことを示します。 この問題の最も一般的な原因は、セッションが 24 時間を超えていることです。 受信後 `410`、新しいセッションを開始して追跡します。 |
| 429 リクエストが多すぎます | この応答コードは、サーバーがリクエストをレート制限していることを示します。 フォロー： **再試行後** 応答ヘッダーの手順は慎重におこなってください。 戻される応答には、ドメイン固有のエラーコードが記述された HTTP 応答コードが含まれている必要があります。 |
| 500 内部サーバーエラー | `500` エラーは汎用の包括的エラーです。 `500` エラーは、次の場合を除き、再試行しないでください： `502`, `503` および `504`. |
| 502 無効なゲートウェイ | このエラーコードは、ゲートウェイとしての役割を果たしているサーバーが、アップストリームサーバーから無効な応答を受け取ったことを示します。 この問題は、サーバー間のネットワークの問題が原因で発生する可能性があります。 一時的なネットワークの問題は解決できるので、リクエストを再試行すると問題が解決する場合があります。 |
| 503 サービスを利用できません | このエラーコードは、サービスが一時的に利用できないことを示します。 この問題は、メンテナンスの間に発生する可能性があります。 受信者 `503` エラーはリクエストを再試行できますが、 **再試行後** ヘッダーの手順。 |


## セッションの応答が遅い場合のイベントのキューへの登録

メディアトラッキングセッションを開始した後、（セッション ID パラメーターを含む）セッション開始応答がバックエンドから返される前に、メディアプレーヤーが起動する場合があります。 これが発生すると、アプリは、セッション開始リクエストとその応答の間に届いたすべてのトラッキングイベントをキューに入れる必要があります。 Sessions 応答が届いたら、キューに入れられたイベントを最初に処理する必要があります。その後、ライブイベントの処理を開始できます。

最も良い結果を得るには、セッション ID を受け取る前にイベントを処理する方法について、配布物の「リファレンスプレーヤー」を確認してください。

次の例は、セッション ID を受け取る前にイベントを処理する方法を示しています。


```
// For event payload format, see "Request body" sections of "Session Start Request", "Event Requests" respectively.  *
 
VideoPlayer.prototype._collectEvent = function(event) {
    var sessionID = event.events[0].xdm.mediaCollection.sessionID
    var eventType = event.events[0].xdm.eventType.substring("media.".length);
    // If we don't have a Session ID yet,
    // queue the event and return...
    if (sessionId === undefined) {
        console.log("[Player] Queueing event")
        _pendingEvents.push(event)
        return;
    }
 
    // If we DO have a Session ID, process the
    // tracking event...
    apiClient.request({
        "baseUrl": `${endpoint}`,
        "path": `ee/va/v1/${eventType}`,
        "method": `POST`,
        "data": `${event}`
    }).then((response) => {
        if (eventType === "sessionStart") {
            var newSessionID = response.json().handle.filter((h) => h.type === "media-analytics:new-session")[0].payload[0].sessionId;
            _processPendingEvents(newSessionID);
        }
        […]
    }
}
 
VideoPlayer.prototype._processPendingEvents function(sessionID) {
    _pendingEvents.forEach((event) => {
        event.events[0].xdm.mediaCollection.sessionID = sessionID;
        _collectEvent(event);
    });
    _pendingEvents = [];
}
```


