---
title: mediaCollection
description: Web SDK を設定して、web プロパティでのメディア使用に関連するデータを収集します。
source-git-commit: 1d1bb754769defd122faaa2160e06671bf02c974
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 6%

---


# `streamingMedia`

この `streamingMedia` コンポーネントは、web サイト上のメディアセッションに関連するデータを収集するのに役立ちます。

収集されたデータには、メディアプレイバック、一時停止、完了およびその他の関連イベントに関する情報を含めることができます。 収集したら、このデータをAdobe Experience PlatformやAdobe Analyticsに送信して、レポートを生成できます。 この機能は、web サイトでのメディア消費行動を追跡および把握するための包括的なソリューションを提供します。

## 前提条件 {#prerequisites}

を使用するには `streamingMedia` web SDK のコンポーネントです。次の前提条件を満たす必要があります。

* Adobe Experience PlatformやAdobe Analyticsにアクセスできることを確認します。
* Web SDK バージョン 2.20.0 以降を使用する必要があります。 を参照してください。 [Web SDK インストールの概要](../../install/overview.md) を参照して、最新バージョンのインストール方法を確認してください。
* を有効にする **[[!UICONTROL Media Analytics]](../../../datastreams/configure.md#advanced-options)** 使用しているデータストリームのオプション。
* データストリームで使用するスキーマに、メディアコレクションのスキーマフィールドが含まれていることを確認してください。
* このページで示されているように、次のいずれかを使用して、Web SDK 設定でストリーミングメディア機能を設定します。 [タグ拡張機能](#tag-extension) または [JavaScript ライブラリ](#library).

## Web SDK タグ拡張機能を使用したストリーミングメディアの設定 {#tag-extension}

Web SDK タグ拡張機能でストリーミングメディアを設定するには、次の手順に従います。

1. へのログイン [experience.adobe.com](https://experience.adobe.com) Adobe IDの資格情報を使用します。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、 **[!UICONTROL 設定]** 日 [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. の設定 **[!UICONTROL ストリーミングメディア]** の説明に従って設定 [Web SDK タグ拡張機能の設定ページ](../../../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md#media-collection).

## Web SDK JavaScript ライブラリを使用したストリーミングメディアの設定 {#library}

Web SDK でストリーミングメディアを設定するには、以下に説明するプロパティを使用します。

呼び出し時 `configure` コマンド、を追加 `streamingMedia` オブジェクト。

```js
alloy("configure", {
    streamingMedia: {
        channel: "video channel",
        playerName: "test player",
        appVersion: "Media Analytics with Web SDK 2.20.0",
        mainPingInterval: 10,
        adPingInterval: 10
    }
});
```

| プロパティ | タイプ | 必須 | 説明 |
|---------|----------|---------|---------|
| `channel` | 文字列 | ○ | ストリーミングメディアコレクションが発生するチャネルの名前。 例：`Video channel`。 |
| `playerName` | 文字列 | ○ | メディアプレーヤーの名前。 |
| `appVersion` | 文字列 | × | メディアプレーヤーアプリケーションのバージョン。 |
| `mainPingInterval` | 整数 | × | メインコンテンツに対する ping の頻度（秒）。 デフォルト値は `10` です。値の範囲は次のとおりです `10` 対象： `50` 秒。  値を指定しない場合、を使用するとデフォルト値が使用されます。 [自動的に追跡されたセッション](../createmediasession.md#automatic). |
| `adPingInterval` | 整数 | × | 広告コンテンツに対する ping の頻度（秒）。 デフォルト値は `10` です。値の範囲は次のとおりです `1` 対象： `10` 秒。 値を指定しない場合、を使用するとデフォルト値が使用されます。 [自動的に追跡されたセッション](../createmediasession.md#automatic). |
