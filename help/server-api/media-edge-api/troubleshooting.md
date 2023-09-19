---
solution: Experience Platform
title: Media Edge API の基本を学ぶ
description: Media Edge API トラブルシューティングガイド
source-git-commit: 3272db15283d427eb4741708dffeb8141f61d5ff
workflow-type: tm+mt
source-wordcount: '664'
ht-degree: 98%

---


# Media Edge API トラブルシューティングガイド

このガイドでは、エラーの処理と正常な応答の取得に関するトラブルシューティング手順を説明します。

## エラー応答支援の使用

失敗した応答のトラブルシューティングを支援するために、エラーにはエラーオブジェクトを含む応答本文が伴います。この場合、応答本文には、[RFC 7807 HTTP API に関する問題の詳細](https://datatracker.ietf.org/doc/html/rfc7807)で定義されている問題の詳細が含まれます。API ユーザーエクスペリエンスを向上させるために、問題の詳細が説明的になっています（配列キーの詳細は、欠落しているフィールドまたは無効なフィールドへの JsonPath を使用して表示されます）。また、これらは累積的でもあります（無効なフィールドはすべて同じリクエストで報告されます）。


## セッション開始の検証

セッション開始リクエストに関する問題のほとんどは、207 マルチステータス応答という結果になります。
ペイロードは、 [サーバー API](../error-handling.md)は致命的でないエラーです。 すべての
Media Analytics エラーのタイプは次のとおりです：`https://ns.adobe.com/aep/errors/va-edge-0XXX-XXX`。応答に表示される数値はエラーステータスに対応します。

次の例は、必須フィールドが欠落しているか、無効なフィールドが含まれているセッション開始リクエストの応答本文を示しています。

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

上記の例では、両方の問題が `details` の下の `name` と `reason` によって示されています。最初の理由は `missing required field` を表示し、2 番目の理由は ISO 8601 標準への非準拠を示しています。


>[!NOTE]
>
> Media Analytics からのアップストリームで発生したエラーの場合は、生成されたメディアセッションの処理を続行することをお勧めします。

## イベントの検証

ほとんどの無効なイベントリクエストでは、400 無効なリクエスト応答が返されます。このような場合、ペイロードは Server API の致命的なエラーに似ています。

イベントリクエストの場合、Media Edge API サービスには、XDM モデル自体ではキャプチャされない追加のチェックが含まれています。これには、パス `eventType` がリクエストペイロード `eventType` と一致するかどうかのチェックが含まれます。


次の例は、`ee/va/v1/chapterStart` に送信された `adBreakStart` ペイロードと `eventType` の不一致を示しています。

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

次の例は、`chapterDetails` 情報が欠落している `chapterStart` 呼び出しを検出する追加の Media Edge API チェックを示しています。

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

次の表に、ステータス応答エラーを処理する手順を示します。


| エラーコード | 説明 |
| ---------- | --------- |
| 4xx 無効なリクエスト | ほとんどの 4xx エラー（`400`、`403`、`404` など）は、ユーザーが再試行しないでください。リクエストを再試行しても、応答は成功しません。ユーザーは、リクエストを再試行する前にエラーに対処する必要があります。4xx ステータスコードを生成するイベントは追跡されないので、4xx 応答を受信したセッションのデータの精度に影響を与える可能性があります。 |
| 410 削除 | トラッキング対象のセッションが、サーバーサイドで計算されなくなったことを示します。この最も一般的な理由は、セッションが 24 時間を超えていることです。`410` を受信したら、新しいセッションを開始してトラックします。 |
| 429 リクエストが多すぎます | この応答コードは、サーバーがリクエストをレート制限していることを示します。応答ヘッダーの **Retry-After** 手順に慎重に従います。戻ってくる応答には、ドメイン固有のエラーコードなど、HTTP 応答コードが含まれている必要があります。 |
| 500 内部サーバーエラー | `500` エラーは一般的かつ包括的なエラーです。`500` エラーは再試行しないでください（`502`、`503`、`504` を除く）。 |
| 502 無効なゲートウェイ | このエラーコードは、サーバーがゲートウェイとして機能している際に、アップストリームサーバーから無効な応答を受信したことを示します。これは、サーバー間のネットワークの問題によって発生する可能性があります。一時的なネットワークの問題は自動的に解決されるので、リクエストを再試行すると問題が解決する場合があります。 |
| 503 サービスを利用できません | このエラーコードは、サービスが一時的に利用できないことを示します。これはメンテナンス期間中に発生する場合があります。`503` エラーの受信者はリクエストを再試行できますが、**Retry-After** ヘッダーの指示にも従う必要があります。 |


## セッションの応答が遅い場合のイベントのキューへの登録

メディアトラッキングセッションを開始した後、バックエンドから（セッション ID パラメーターと共に）セッション開始応答が返される前に、メディアプレーヤーが起動する場合があります。このエラーが発生すると、アプリは、セッション開始リクエストとその応答の間に届いたすべてのトラッキングイベントをキューに登録する必要があります。セッション応答が届いたら、キューに登録されたイベントを最初に処理する必要があります。それから、ライブイベントの処理を開始できます。

最良の結果を得るには、セッション ID 受信する前にイベントを処理する方法を、配布内のリファレンスプレーヤーで確認します。

次の例は、セッション ID を受信する前にイベントを処理するメソッドを示しています。


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


