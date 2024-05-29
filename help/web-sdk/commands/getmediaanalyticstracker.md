---
title: getMediaAnalyticsTracker
description: Media Analytics トラッカーオブジェクトを作成し、それを使用してメディアイベントをトラッキングする方法を説明します。
source-git-commit: 9384c1cc15441199e898d6cc0850e5422253f106
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 3%

---


# `getMediaAnalyticsTracker`

この Web SDK コマンドは、Media Analytics トラッカーを取得します。 このコマンドを使用してオブジェクトインスタンスを作成し、から提供されるのと同じ API を使用することができます。 [Media JS ライブラリ](https://adobe-marketing-cloud.github.io/media-sdks/reference/javascript_3x/APIReference.html)、メディアイベントを追跡

この `getMediaAnalyticsTracker` コマンドは、従来の Media Analytics API を返します。


| メソッド名 | 説明 | 構文 |
|-----------------|---|----------------|
| `getInstance` | 再生セッションをトラックするメディアのインスタンスを作成します。 | `Media.getInstance()` |
| `createMediaObject` | メディア情報を含むオブジェクトを作成します。 無効なパラメーターが渡された場合、空のオブジェクトを返します。 | `Media.createMediaObject(name, id, length, streamType, mediaType)` |
| `createAdBreakObject` | Adbreak 情報を含むオブジェクトを作成します。 無効なパラメーターが渡された場合、空のオブジェクトを返します。 | `Media.createAdBreakObject(name, position, startTime)` |
| `createAdObject` | 広告情報を含むオブジェクトを作成します。 無効なパラメーターが渡された場合、空のオブジェクトを返します。 | `Media.createAdObject(name, id, position, length)` |
| `createChapterObject` | チャプター情報を含むオブジェクトを作成します。 無効なパラメーターが渡された場合、空のオブジェクトを返します。 | `Media.createChapterObject(name, position, length, startTime)` |
| `createStateObject` | 状態情報を含むオブジェクトを作成します。 無効なパラメーターが渡された場合、空のオブジェクトを返します。 | `Media.createStateObject(name)` |
| `createQoEObject` | QoE 情報を含むオブジェクトを作成します。 無効なパラメーターが渡された場合、空のオブジェクトを返します。 | `Media.createQoEObject(bitrate, startupTime, fps, droppedFrames)` |

## インスタンスメソッド

| メソッド名 | 説明 | 構文 |
|---|---|----|
| `trackSessionStart` | 再生を開始する意図をトラッキングします。 これにより、メディアトラッカーインスタンスでトラッキングセッションが開始されます。 | `trackerInstance.trackSessionStart(mediaInfo, contextData)` |
| `trackPlay` | 前回の一時停止後にメディアの再生または再開をトラックする。 | `trackerInstance.trackPlay()` |
| `trackPause` | メディア一時停止のトラッキング。 | `trackerInstance.trackPause()` |
| `trackComplete` | メディアの追跡が完了しました。 メディアが完全に表示された場合にのみ、このメソッドを呼び出します。 | `trackerInstance.trackComplete()` |
| `trackSessionEnd` | 表示セッションの終了をトラッキングします。 ユーザーがメディアを最後まで表示しない場合でも、このメソッドを呼び出します。 | `trackerInstance.trackSessionEnd()` |
| `trackError` | メディア再生中に発生したエラーをトラッキングします。 | `trackerInstance.trackError("errorId")` |
| `trackEvent` | カスタムイベントのトラッキング。 | `trackerInstance.trackEvent(event, info, contextData)` |
| `updatePlayhead` | 再生ヘッドの位置を更新します。 | `trackerInstance.updatePlayhead(playhead)` |
| `updateQoEObject` | エクスペリエンスの品質を更新します。 | `trackerInstance.updateQoEObject(qoe)` |

## 定数

| 定数名 | 説明 | 値 |
|-----------------|--|-----------------|
| `MediaType` | メディアタイプ | `Video`、`Audio` |
| `StreamType` | ストリームタイプ | `VOD`, `Live`, `Linear`, `Podcast`, `Audiobook`, `AOD` |
| `VideoMetadataKeys` | ビデオストリームの標準メタデータキーを定義します | `Show`, `Season`, `Episode`, `AssetId`, `Genre`, `FirstAirDate`, `FirstDigitalDate`, `Rating`, `Originator`, `Network`, `ShowType`, `AdLoad`, `MVPD`, `Authorized`, `DayPart`, `Feed`, `StreamFormat` |
| `AudioMetadataKeys` | オーディオストリームの標準メタデータキーを定義します。 | `Artist`, `Album`, `Label`, `Author`, `Station`, `Publisher` |
| `AdMetadataKeys` | これは、広告の標準メタデータキーを定義します。 | `Advertiser`, `CampaignId`, `CreativeId`, `PlacementId`, `SiteId`, `CreativeUrl` |
| `Event` | これは、トラッキングイベントのタイプを定義します。 | `AdBreakStart`, `AdBreakComplete`, `AdStart`, `AdComplete`, `AdSkip`, `ChapterStart`, `ChapterComplete`, `ChapterSkip`, `SeekStart`, `SeekComplete`, `BufferStart`, `BufferComplete`, `BitrateChange`, `StateStart`, `StateEnd` |
| `PlayerState` | プレーヤーステートのトラッキングの標準値を定義します。 | `FullScreen`, `ClosedCaption`, `Mute`, `PictureInPicture`, `InFocus` |
