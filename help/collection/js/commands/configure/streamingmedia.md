---
title: streamingMedia
description: Web プロパティでのメディアの使用に関連するデータを収集するように Web SDKを設定します。
exl-id: f7733619-d35e-43eb-ac90-052717310c39
source-git-commit: 2e39a7809049c199d4778a0e17eb9e0f3b1d9775
workflow-type: tm+mt
source-wordcount: '300'
ht-degree: 8%

---

# `streamingMedia`

`streamingMedia` コンポーネントは、web サイト上のメディアセッションに関連するデータを収集するのに役立ちます。

収集されたデータには、メディアプレイバック、一時停止、完了およびその他の関連イベントに関する情報を含めることができます。 収集したら、このデータをAdobe Experience PlatformまたはAdobe Analyticsに送信して、レポートを生成できます。 この機能は、web サイトでのメディア消費行動を追跡および把握するための包括的なソリューションを提供します。

## 前提条件

Web SDKの `streamingMedia` コンポーネントを使用するには、次の前提条件を満たす必要があります。

* Adobe Experience PlatformまたはAdobe Analyticsへのアクセス権があることを確認します。
* Web SDK バージョン 2.20.0 以降を使用する必要があります。 最新バージョンのインストール方法については、[Web SDKのインストールの概要 ](../../install/overview.md) を参照してください。
* 使用しているデータストリームの「**[[!UICONTROL Media Analytics]](/help/datastreams/configure.md#advanced-options)**」オプションを有効にします。
* データストリームで使用するスキーマに、メディアコレクションのスキーマフィールドが含まれていることを確認してください。
* このページに示すように、Web SDKでストリーミングメディア機能を設定します。

`configure` コマンドを呼び出す場合は、`streamingMedia` オブジェクトを追加します。

```js
alloy("configure", {
    streamingMedia: {
        channel: "Video channel",
        playerName: "Example player",
        appVersion: "Media Analytics with Web SDK 2.20.0",
        mainPingInterval: 10,
        adPingInterval: 10
    }
});
```

| プロパティ | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| **`channel`** | 文字列 | ○ | ストリーミングメディアコレクションが発生するチャネルの名前。 例：`Video channel`。 |
| **`playerName`** | 文字列 | ○ | メディアプレーヤーの名前。 |
| **`appVersion`** | 文字列 | × | メディアプレーヤーアプリケーションのバージョン。 |
| **`mainPingInterval`** | 整数 | × | メインコンテンツに対する ping の頻度（秒）。 デフォルト値は `10` です。値の範囲は `10` ～ `50` 秒です。  値を指定しない場合、[ 自動的にトラッキングされるセッション ](../createmediasession.md#automatic) を使用するときにデフォルト値が使用されます。 |
| **`adPingInterval`** | 整数 | × | 広告コンテンツに対する ping の頻度（秒）。 デフォルト値は `10` です。値の範囲は `1` ～ `10` 秒です。 値を指定しない場合、[ 自動的にトラッキングされるセッション ](../createmediasession.md#automatic) を使用するときにデフォルト値が使用されます。 |

## Web SDK タグ拡張機能を使用したストリーミングメディア設定

これらの設定は、web SDK タグ拡張機能で [ ストリーミングメディア設定 ](/help/tags/extensions/client/web-sdk/configure/streaming-media.md) を使用して指定できます。
