---
title: createMediaSession
description: メディアセッションを自動的に管理するように Web SDK を設定する方法について説明します
source-git-commit: 83d3de67e7680369dc890f58b16d9668058e221c
workflow-type: tm+mt
source-wordcount: '356'
ht-degree: 7%

---


# `createMediaSession`

この `createMediaSession` コマンドは Web SDK の一部 `streamingMedia` コンポーネント。 このコンポーネントを使用すると、web サイト上のメディアセッションに関連するデータを収集できます。 を参照してください。 `streamingMedia` [詳細を見る](configure/streamingmedia.md) を参照して、このコンポーネントの設定方法を確認してください。

収集されたデータには、メディアプレイバック、一時停止、完了およびその他の関連イベントに関する情報を含めることができます。 収集したら、このデータをに送信できます。 [ストリーミングメディア用 Adobe Analytics](https://experienceleague.adobe.com/ja/docs/media-analytics/using/media-overview)：指標を集計します。 この機能は、web サイトでのメディア消費行動を追跡および把握するための包括的なソリューションを提供します。

Web SDK でメディアセッションを作成するには、次の 2 つの方法があります。

* [自動的にトラッキングされるメディアセッション](#automatic) web SDK で、へのメディア ping イベントのディスパッチを管理できるようにします [ストリーミングメディア用 Adobe Analytics](https://experienceleague.adobe.com/ja/docs/media-analytics/using/media-overview). これらの ping の頻度は、の設定によって決まります [streamingMedia](configure/streamingmedia.md) コンポーネント。
* [手動でトラッキングされるメディアセッション](#manual) へのセッション ping イベントのディスパッチをより詳細に制御できます。 [ストリーミングメディア用 Adobe Analytics](https://experienceleague.adobe.com/ja/docs/media-analytics/using/media-overview). さらに、を保存できます `sessionID` （メディアセッション用）。

## 自動的にトラッキングされるメディアセッションの作成 {#automatic}

メディアセッションのトラッキングを自動的に開始するには、 `createMediaSession` メソッドと以下に説明するオプション。

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
| `getPlayerDetails` | 関数 | ○ | プレーヤーの詳細を返す関数。 このコールバック関数は、のすべてのメディアイベントの前に Web SDK によって呼び出されます。 `playerId` 提供されます。 |
| `xdm.eventType ` | オブジェクト | × | メディアイベントタイプ。 指定しない場合、自動的に次のように設定されます。 `media.sessionStart`. |
| `xdm.mediaCollection.sessionDetails` | オブジェクト | ○ | セッションの詳細オブジェクト。 この `sessionDetails` オブジェクトには、セッションの詳細プロパティを含める必要があります。 を参照してください。 [メディアコレクションスキーマ](../../xdm/data-types/media-collection-details.md) 詳しくは、ドキュメントを参照してください。 |


## 手動でトラッキングされるメディアセッションの作成 {#manual}

メディアセッションのトラッキングを手動で開始するには、 `createMediaSession` メソッドと以下に説明するオプション。

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
| `xdm.eventType` | オブジェクト | × | メディアイベントタイプ。 指定しない場合、自動的に次のように設定されます。 `media.sessionStart`. |
| `xdm.mediaCollection.sessionDetails` | オブジェクト | ○ | セッションの詳細オブジェクト。 この `sessionDetails` オブジェクトには、セッションの詳細プロパティを含める必要があります。 を参照してください。 [メディアコレクションスキーマ](../../xdm/data-types/media-collection-details.md) 詳しくは、ドキュメントを参照してください。 |
| `xdm.mediaCollection.playhead` | 整数 | ○ | 現在の再生ヘッド。 |
| `xdm.mediaCollection.qoeDataDetails` | オブジェクト | × | エクスペリエンスデータの品質の詳細。 を参照してください。 [メディアコレクションスキーマ](../../xdm/data-types/media-collection-details.md) 詳しくは、ドキュメントを参照してください。 |
