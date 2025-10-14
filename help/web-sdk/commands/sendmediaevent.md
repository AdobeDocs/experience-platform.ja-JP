---
title: sendMediaEvent
description: sendMediaEvent コマンドを使用して Web SDK でメディアセッションをトラッキングする方法を説明します。
exl-id: a38626fd-4810-40a0-8893-e98136634fac
source-git-commit: 877e12f1d53bb4a8d7c2564490d4e8f3e9e34e34
workflow-type: tm+mt
source-wordcount: '763'
ht-degree: 0%

---

# `sendMediaEvent`

`sendMediaEvent` コマンドは、Web SDK `streamingMedia` コンポーネントの一部です。 このコンポーネントを使用すると、web サイト上のメディアセッションに関連するデータを収集できます。 このコンポーネントの設定方法については、`streamingMedia` [&#x200B; ドキュメント &#x200B;](configure/streamingmedia.md) を参照してください。

`sendMediaEvent` コマンドを使用して、メディアのプレイバック、一時停止、完了、プレーヤーの状態の更新、およびその他の関連イベントをトラッキングします。

Web SDK は、メディアセッショントラッキングのタイプに基づいてメディアイベントを処理できます。

* **自動的にトラッキングされたセッションのイベント処理**。 このモードでは、`sessionID` をメディアイベントや再生ヘッド値に渡す必要はありません。 Web SDK は、提供されたプレーヤー ID と、メディアセッションの開始時に提供された `getPlayerDetails` コールバック関数に基づいて、これを処理します。
* **手動でトラッキングされたセッションのイベント処理**。 このモードでは、再生ヘッド値（整数値）と共に、`sessionID` をメディアイベントに渡す必要があります。 必要に応じて、エクスペリエンスの品質データの詳細を渡すこともできます。

## タイプ別にメディアイベントを処理 {#handle-by-type}

以下のタブを選択して、各イベントタイプのイベントタイプ処理とセッショントラッキング方法（自動または手動）の例を確認します。


### 再生 {#play}

`media.play` イベントタイプは、メディアの再生が開始されるタイミングを追跡するために使用されます。 このイベントは、プレーヤーが別の状態から「再生中」に状態を変更した際に送信する必要があります。 プレーヤーが「再生中」に移行するその他の状態には、「バッファリング」、ユーザーの「一時停止」からの再開、エラーからの回復、自動再生などがあります。

>[!BEGINTABS]

>[!TAB  自動セッショントラッキング ]

```javascript
alloy("sendMediaEvent", {
    playerId: "movie-test",
    xdm: {
        eventType: "media.play"
    }
});
```

>[!TAB  手動セッショントラッキング ]

```javascript
sessionPromise.then(sessionID => {
    alloy("sendMediaEvent", {
        xdm: {
            eventType: "media.play",
            mediaCollection: {
                playhead: parseInt(document.getElementById("movie-test").currentTime, 10),
                sessionID
            }
        }
    });
});
```

>[!ENDTABS]


### 一時停止 {#pause}

`media.pauseStart` イベントタイプは、メディアの再生が一時停止されたタイミングを追跡するために使用されます。 このイベントは、ユーザーが **[!UICONTROL 一時停止]** を押したときに送信されます。 再開イベントタイプはありません。 リク `media.pauseStart` ストの後に `media.play` イベントを送信すると、再開が推論されます。

>[!BEGINTABS]

>[!TAB  自動セッショントラッキング ]

```javascript
alloy("sendMediaEvent", {
    playerId: "movie-test",
    xdm: {
        eventType: "media.pauseStart"
    }
});
```

>[!TAB  手動セッショントラッキング ]

```javascript
sessionPromise.then(sessionID => {
    alloy("sendMediaEvent", {
        xdm: {
            eventType: "media.pauseStart",
            mediaCollection: {
                playhead: parseInt(document.getElementById("movie-test").currentTime, 10),
                sessionID
            }
        }
    });
});
```

>[!ENDTABS]


### エラー {#error}

`media.error` イベントタイプは、メディアの再生中にエラーが発生したタイミングを追跡するために使用されます。 このイベントは、エラーが発生した場合に送信する必要があります。

>[!BEGINTABS]

>[!TAB  自動セッショントラッキング ]

```javascript
alloy("sendMediaEvent", {
    playerId: "movie-test",
    xdm: {
        eventType: "media.error",
        mediaCollection: {
            errorDetails: {
                name: "network-error",
                source: "player"
            }
        }
    }
});
```

>[!TAB  手動セッショントラッキング ]

```javascript
sessionPromise.then(sessionID => {
    alloy("sendMediaEvent", {
        xdm: {
            eventType: "media.error",
            mediaCollection: {
                playhead: parseInt(document.getElementById("movie-test").currentTime, 10),
                sessionID,
                errorDetails: {
                    name: "network-error",
                    source: "player"
                }
            }
        }
    });
});
```

>[!ENDTABS]


### 広告ブレーク開始 {#ad-break-start}

`media.adBreakStart` イベントタイプは、広告ブレークが開始するタイミングを追跡するために使用されます。 このイベントは、広告ブレークが開始されたときに送信する必要があります。

>[!BEGINTABS]

>[!TAB  自動セッショントラッキング ]

```javascript
alloy("sendMediaEvent", {
    playerId: "movie-test",
    xdm: {
        eventType: "media.adBreakStart",
        mediaCollection: {
            advertisingPodDetails: {
                friendlyName: "Mid-roll",
                offset: 0,
                index: 1
            }
        }
    }
});
```

>[!TAB  手動セッショントラッキング ]

```javascript
sessionPromise.then(sessionID => {
    alloy("sendMediaEvent", {
        xdm: {
            eventType: "media.adBreakStart",
            mediaCollection: {
                playhead: parseInt(document.getElementById("movie-test").currentTime, 10),
                sessionID,
                advertisingPodDetails: {
                    friendlyName: "Mid-roll",
                    offset: 0,
                    index: 1
                }
            }
        }
    });
});
```

>[!ENDTABS]


### 広告ブレーク完了 {#ad-break-complete}

`media.adBreakComplete` イベントタイプは、広告ブレークが完了したタイミングを追跡するために使用されます。 このイベントは、広告ブレークが完了すると送信されます。

>[!BEGINTABS]

>[!TAB  自動セッショントラッキング ]

```javascript
alloy("sendMediaEvent", {
    playerId: "movie-test",
    xdm: {
        eventType: "media.adBreakComplete"
    }
});
```

>[!TAB  手動セッショントラッキング ]

```javascript
sessionPromise.then(sessionID => {
    alloy("sendMediaEvent", {
        xdm: {
            eventType: "media.adBreakComplete",
            mediaCollection: {
                playhead: parseInt(document.getElementById("movie-test").currentTime, 10),
                sessionID
            }
        }
    });
});
```

>[!ENDTABS]


### 広告開始 {#ad-start}

`media.adStart` イベントタイプは、広告の開始時に追跡するために使用されます。 このイベントは、広告の開始時に送信する必要があります。

>[!BEGINTABS]

>[!TAB  自動セッショントラッキング ]

```javascript
alloy("sendMediaEvent", {
    playerId: "movie-test",
    xdm: {
        eventType: "media.adStart",
        mediaCollection: {
            advertisingDetails: {
                friendlyName: "Ad 1",
                name: "/uri-reference/001",
                length: 10,
                advertiser: "Adobe Marketing",
                campaignID: "Adobe Analytics",
                creativeID: "creativeID",
                creativeURL: "https://creativeurl.com",
                placementID: "placementID",
                siteID: "siteID",
                podPosition: 11,
                playerName: "HTML5 player"
            },
            customMetadata: [{
                    name: "myCustomValue3",
                    value: "c3"
                },
                {
                    name: "myCustomValue2",
                    value: "c2"
                },
                {
                    name: "myCustomValue1",
                    value: "c1"
                }
            ]
        }
    }
});
```

>[!TAB  手動セッショントラッキング ]

```javascript
sessionPromise.then(sessionID => {
    alloy("sendMediaEvent", {
        xdm: {
        eventType: "media.adStart",
        mediaCollection: {
            playhead: parseInt(document.getElementById("movie-test").currentTime, 10),
            sessionID,
            advertisingDetails: {
              friendlyName: "Ad 1",
              name: "/uri-reference/001",
              length: 10,
              advertiser: "Adobe Marketing",
              campaignID: "Adobe Analytics",
              creativeID: "creativeID",
              creativeURL: "https://creativeurl.com",
              placementID: "placementID",
              siteID: "siteID",
              podPosition: 11,
              playerName: "HTML5 player"
            },
            customMetadata: [
              {
                name: "myCustomValue3",
                value: "c3"
              },
              {
                name: "myCustomValue2",
                value: "c2"
              },
              {
                name: "myCustomValue1",
                value: "c1"
              }]
        }
        }
    });
});
```

>[!ENDTABS]


### 広告完了 {#ad-complete}

`media.adComplete` イベントタイプは、広告が完了したタイミングを追跡するために使用されます。 このイベントは、広告が完了したときに送信する必要があります。

>[!BEGINTABS]

>[!TAB  自動セッショントラッキング ]

```javascript
alloy("sendMediaEvent", {
    playerId: "movie-test",
    xdm: {
        eventType: "media.adComplete"
    }
});
```

>[!TAB  手動セッショントラッキング ]

```javascript
sessionPromise.then(sessionID => {
    alloy("sendMediaEvent", {
        xdm: {
            eventType: "media.adComplete",
            mediaCollection: {
                playhead: parseInt(document.getElementById("movie-test").currentTime, 10),
                sessionID
            }
        }
    });
});
```

>[!ENDTABS]


### 広告スキップ {#ad-skip}

`media.adSkip` イベントタイプは、広告がスキップされたタイミングを追跡するために使用されます。 このイベントは、広告がスキップされた場合に送信する必要があります。

>[!BEGINTABS]

>[!TAB  自動セッショントラッキング ]

```javascript
alloy("sendMediaEvent", {
    playerId: "movie-test",
    xdm: {
        eventType: "media.adSkip"
    }
});
```

>[!TAB  手動セッショントラッキング ]

```javascript
sessionPromise.then(sessionID => {
    alloy("sendMediaEvent", {
        xdm: {
            eventType: "media.adSkip",
            mediaCollection: {
                playhead: parseInt(document.getElementById("movie-test").currentTime, 10),
                sessionID
            }
        }
    });
});
```

>[!ENDTABS]


### チャプター開始 {#chapter-start}

`media.chapterStart` イベントタイプは、チャプターが開始するタイミングを追跡するために使用されます。 このイベントは、チャプターの開始時に送信される必要があります。

>[!BEGINTABS]

>[!TAB  自動セッショントラッキング ]

```javascript
alloy("sendMediaEvent", {
    playerId: "movie-test",
    xdm: {
        eventType: "media.chapterStart",
        mediaCollection: {
            chapterDetails: {
                friendlyName: "Chapter 1",
                position: 1,
                length: 10,
                index: 1,
                offset: 0
            },
            customMetadata: [{
                    name: "myCustomValue3",
                    value: "c3"
                },
                {
                    name: "myCustomValue2",
                    value: "c2"
                },
                {
                    name: "myCustomValue1",
                    value: "c1"
                }
            ]
        }
    }
});
```

>[!TAB  手動セッショントラッキング ]

```javascript
sessionPromise.then(sessionID => {
    alloy("sendMediaEvent", {
        xdm: {
            eventType: "media.chapterStart",
            mediaCollection: {
                playhead: parseInt(document.getElementById("movie-test").currentTime, 10),
                sessionID,
                chapterDetails: {
                    friendlyName: "Chapter 1",
                    position: 1,
                    length: 10,
                    index: 1,
                    offset: 0
                },
                customMetadata: [{
                        name: "myCustomValue3",
                        value: "c3"
                    },
                    {
                        name: "myCustomValue2",
                        value: "c2"
                    },
                    {
                        name: "myCustomValue1",
                        value: "c1"
                    }
                ]
            }
        }
    });
});
```

>[!ENDTABS]


### チャプター完了 {#chapter-complete}

`media.chapterComplete` イベントタイプは、チャプターが完了したタイミングを追跡するために使用されます。 このイベントは、チャプターが完了したときに送信する必要があります。

>[!BEGINTABS]

>[!TAB  自動セッショントラッキング ]

```javascript
alloy("sendMediaEvent", {
    playerId: "movie-test",
    xdm: {
        eventType: "media.chapterComplete"
    }
});
```

>[!TAB  手動セッショントラッキング ]

```javascript
sessionPromise.then(sessionID => {
    alloy("sendMediaEvent", {
        xdm: {
            eventType: "media.chapterComplete",
            mediaCollection: {
                playhead: parseInt(document.getElementById("movie-test").currentTime, 10),
                sessionID
            }
        }
    });
});
```

>[!ENDTABS]


### チャプタースキップ {#chapter-skip}

`media.chapterSkip` イベントタイプは、チャプターがスキップされた際の追跡に使用されます。 このイベントは、チャプターがスキップされた場合に送信する必要があります。

>[!BEGINTABS]

>[!TAB  自動セッショントラッキング ]

```javascript
alloy("sendMediaEvent", {
    playerId: "movie-test",
    xdm: {
        eventType: "media.chapterSkip"
    }
});
```

>[!TAB  手動セッショントラッキング ]

```javascript
sessionPromise.then(sessionID => {
    alloy("sendMediaEvent", {
        xdm: {
            eventType: "media.chapterSkip",
            mediaCollection: {
                playhead: parseInt(document.getElementById("movie-test").currentTime, 10),
                sessionID
            }
        }
    });
});
```

>[!ENDTABS]


### バッファー開始 {#buffer-start}

`media.bufferStart` イベントタイプは、バッファリングが開始するタイミングを追跡するために使用されます。 このイベントは、バッファー処理の開始時に送信する必要があります。 `bufferResume` のイベントタイプはありません。 `bufferStart` 後に再生イベントを送信すると、`bufferResume` が推測されます。

>[!BEGINTABS]

>[!TAB  自動セッショントラッキング ]

```javascript
alloy("sendMediaEvent", {
    playerId: "movie-test",
    xdm: {
        eventType: "media.bufferStart"
    }
});
```

>[!TAB  手動セッショントラッキング ]

```javascript
sessionPromise.then(sessionID => {
    alloy("sendMediaEvent", {
        xdm: {
            eventType: "media.bufferStart",
            mediaCollection: {
                playhead: parseInt(document.getElementById("movie-test").currentTime, 10),
                sessionID
            }
        }
    });
});
```

>[!ENDTABS]


### ビットレート変更 {#bitrate-change}

ビットレートが変更された際には、`media.bitrateChange` のイベントタイプを使用してトラッキングが行われます。 このイベントは、ビットレートが変更された場合に送信する必要があります。

>[!BEGINTABS]

>[!TAB  自動セッショントラッキング ]

```javascript
alloy("sendMediaEvent", {
    playerId: "movie-test",
    xdm: {
        eventType: "media.bitrateChange",
        mediaCollection: {
            qoeDataDetails: {
                framesPerSecond: 1,
                bitrate: 35000,
                droppedFrames: 30,
                timeToStart: 1364
            }
        }
    }
});
```

>[!TAB  手動セッショントラッキング ]

```javascript
sessionPromise.then(sessionID => {
    alloy("sendMediaEvent", {
        xdm: {
            eventType: "media.bitrateChange",
            mediaCollection: {
                playhead: parseInt(document.getElementById("movie-test").currentTime, 10),
                sessionID,
                qoeDataDetails: {
                    bitrate: 35000,
                    droppedFrames: 30,
                    timeToStart: 1364
                }
            }
        }
    });
});
```

>[!ENDTABS]


### 状態の更新 {#state-updates}

`media.statesUpdate` イベントタイプは、プレーヤーの状態が変更されたタイミングを追跡するために使用されます。 このイベントは、プレーヤーの状態が変更されたときに送信する必要があります。

>[!BEGINTABS]

>[!TAB  自動セッショントラッキング ]

```javascript
alloy("sendMediaEvent", {
    playerId: "movie-test",
    xdm: {
        eventType: "media.statesUpdate",
        mediaCollection: {
            statesStart: [{
                    name: "mute"
                },
                {
                    name: "pictureInPicture"
                }
            ],
            statesEnd: [{
                name: "fullScreen"
            }]
        }
    }
});
```

>[!TAB  手動セッショントラッキング ]

```javascript
sessionPromise.then(sessionID => {
    alloy("sendMediaEvent", {
        xdm: {
            eventType: "media.stateUpdate",
            mediaCollection: {
                playhead: parseInt(document.getElementById("movie-test").currentTime, 10),
                sessionID,
                statesStart: [{
                        name: "mute"
                    },
                    {
                        name: "pictureInPicture"
                    }
                ],
                statesEnd: [{
                    name: "fullScreen"
                }]
            }
        }
    });
});
```

>[!ENDTABS]


### セッション終了 {#session-end}

`media.sessionEnd` イベントタイプは、ユーザーがコンテンツの表示を中止し、戻る可能性が低い場合に、セッションを直ちに閉じるように Media Analytics バックエンドに通知するために使用されます。

`sessionEnd` イベントを送信しない場合、放棄されたセッションは、イベントの受信が 10 分間行われなかった後、または再生ヘッドの移動が 30 分間行われなかった場合にタイムアウトします。 セッションは自動的に削除されます。

>[!BEGINTABS]

>[!TAB  自動セッショントラッキング ]

```javascript
alloy("sendMediaEvent", {
    playerId: "movie-test",
    xdm: {
        eventType: "media.sessionEnd"
    }
});
```

>[!TAB  手動セッショントラッキング ]

```javascript
sessionPromise.then(sessionID => {
    alloy("sendMediaEvent", {
        xdm: {
            eventType: "media.sessionEnd",
            mediaCollection: {
                playhead: parseInt(document.getElementById("movie-test").currentTime, 10),
                sessionID
            }
        }
    });
});
```

>[!ENDTABS]


### セッション完了 {#session-complete}

`media.sessionComplete` イベントタイプは、メディアセッションが完了したタイミングを追跡するために使用されます。 このイベントは、メインコンテンツの終わりに達したときに送信する必要があります。

>[!BEGINTABS]

>[!TAB  自動セッショントラッキング ]

```javascript
alloy("sendMediaEvent", {
    playerId: "movie-test",
    xdm: {
        eventType: "media.sessionComplete"
    }
});
```

>[!TAB  手動セッショントラッキング ]

```javascript
sessionPromise.then(sessionID => {
    alloy("sendMediaEvent", {
        xdm: {
            eventType: "media.sessionComplete",
            mediaCollection: {
                playhead: parseInt(document.getElementById("movie-test").currentTime, 10),
                sessionID
            }
        }
    });
});
```

>[!ENDTABS]
