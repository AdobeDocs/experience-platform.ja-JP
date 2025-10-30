---
title: createMediaSession
description: メディアセッションを自動的に管理するように web SDKを設定する方法について説明します
exl-id: abcb26f6-7249-4235-99eb-e4b9aeecff3e
source-git-commit: be2ad7a02d4bdf5a26a0847c8ee7a9a93746c2ad
workflow-type: tm+mt
source-wordcount: '356'
ht-degree: 7%

---

# `createMediaSession`

`createMediaSession` コマンドは、Web SDK `streamingMedia` コンポーネントの一部です。 このコンポーネントを使用すると、web サイト上のメディアセッションに関連するデータを収集できます。 このコンポーネントの設定方法については、`streamingMedia` [ ドキュメント ](configure/streamingmedia.md) を参照してください。

収集されたデータには、メディアプレイバック、一時停止、完了およびその他の関連イベントに関する情報を含めることができます。 収集したら、このデータを [Adobe Analytics for Streaming Media](https://experienceleague.adobe.com/ja/docs/media-analytics/using/media-overview) に送信して、指標を集計できます。 この機能は、web サイトでのメディア消費行動を追跡および把握するための包括的なソリューションを提供します。

Web SDKでは、次の 2 つの方法でメディアセッションを作成できます。

* [ 自動トラッキングされるメディアセッション ](#automatic) により、Web SDKで [Adobe Analytics for Streaming Media](https://experienceleague.adobe.com/ja/docs/media-analytics/using/media-overview) へのメディア ping イベントのディスパッチを管理できます。 これらの ping の頻度は、[streamingMedia](configure/streamingmedia.md) コンポーネントの設定によって決まります。
* [ 手動でトラッキングされるメディアセッション ](#manual) を使用すると、[Adobe Analytics for Streaming Media](https://experienceleague.adobe.com/ja/docs/media-analytics/using/media-overview) へのセッション Ping イベントのディスパッチをより詳細に制御できます。 さらに、メディアセッション用の `sessionID` を保存することもできます。

## 自動的にトラッキングされるメディアセッションの作成 {#automatic}

メディアセッションのトラッキングを自動的に開始するには、以下に説明するオプションで `createMediaSession` メソッドを呼び出します。

```javascript
    alloy("createMediaSession", {
        playerId: "movie-test",
        getPlayerDetails: () => {
            return {
                playhead: document.getElementById("movie-test").currentTime,
                qoeDataDetails: {
                    bitrate: 1000,
                    startupTime: 1000,
                    fps: 30,
                    droppedFrames: 10
                }
            };
        },
        xdm: {
            eventType: "media.sessionStart",
            mediaCollection: {
                sessionDetails: {
                    ...
                }
            }
        }
    });
```

| プロパティ | タイプ | 必須 | 説明 |
|---------|----------|---------|---------|
| `playerId` | 文字列 | ○ | プレーヤー ID。メディアセッションを表す一意の識別子です。 |
| `getPlayerDetails` | 関数 | ○ | プレーヤーの詳細を返す関数。 このコールバック関数は、指定された `playerId` の各メディアイベントの前に、Web SDKによって呼び出されます。 |
| `xdm.eventType` | オブジェクト | × | メディアイベントタイプ。 指定しない場合、自動的に `media.sessionStart` に設定されます。 |
| `xdm.mediaCollection.sessionDetails` | オブジェクト | ○ | セッションの詳細オブジェクト。 `sessionDetails` オブジェクトには、セッションの詳細プロパティを含める必要があります。 詳しくは、[ メディアコレクションのスキーマ ](../../xdm/data-types/media-collection-details.md) ドキュメントを参照してください。 |


## 手動でトラッキングされるメディアセッションの作成 {#manual}

メディアセッションのトラッキングを手動で開始するには、以下に説明するオプションで `createMediaSession` メソッドを呼び出します。

```javascript
const sessionPromise = alloy("createMediaSession", {
    xdm: {
        eventType: "media.sessionStart",
        mediaCollection: {
            playhead: 0,
            sessionDetails: {
                ...
            },
            qoeDataDetails: {
                bitrate: 1000,
                startupTime: 1000,
                fps: 30,
                droppedFrames: 10
            }
        }
    }
});
```

| プロパティ | タイプ | 必須 | 説明 |
|---------|----------|---------|---------|
| `xdm.eventType` | オブジェクト | × | メディアイベントタイプ。 指定しない場合、自動的に `media.sessionStart` に設定されます。 |
| `xdm.mediaCollection.sessionDetails` | オブジェクト | ○ | セッションの詳細オブジェクト。 `sessionDetails` オブジェクトには、セッションの詳細プロパティを含める必要があります。 詳しくは、[ メディアコレクションのスキーマ ](../../xdm/data-types/media-collection-details.md) ドキュメントを参照してください。 |
| `xdm.mediaCollection.playhead` | 整数 | ○ | 現在の再生ヘッド。 |
| `xdm.mediaCollection.qoeDataDetails` | オブジェクト | × | エクスペリエンスデータの品質の詳細。 詳しくは、[ メディアコレクションのスキーマ ](../../xdm/data-types/media-collection-details.md) ドキュメントを参照してください。 |
