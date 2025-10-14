---
title: streamingMedia
description: Web SDK を設定して、web プロパティでのメディア使用に関連するデータを収集します。
exl-id: f7733619-d35e-43eb-ac90-052717310c39
source-git-commit: 57d42d88ec9a93744450a2a352590ab57d9e5bb7
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 6%

---

# `streamingMedia`

`streamingMedia` コンポーネントは、web サイト上のメディアセッションに関連するデータを収集するのに役立ちます。

収集されたデータには、メディアプレイバック、一時停止、完了およびその他の関連イベントに関する情報を含めることができます。 収集したら、このデータをAdobe Experience PlatformやAdobe Analyticsに送信して、レポートを生成できます。 この機能は、web サイトでのメディア消費行動を追跡および把握するための包括的なソリューションを提供します。

## 前提条件 {#prerequisites}

Web SDK の `streamingMedia` コンポーネントを使用するには、次の前提条件を満たす必要があります。

* Adobe Experience PlatformやAdobe Analyticsにアクセスできることを確認します。
* Web SDK バージョン 2.20.0 以降を使用する必要があります。 最新バージョンのインストール方法については、[Web SDK インストールの概要 &#x200B;](../../install/overview.md) を参照してください。
* 使用しているデータストリームの「**[[!UICONTROL Media Analytics]](../../../datastreams/configure.md#advanced-options)**」オプションを有効にします。
* データストリームで使用するスキーマに、メディアコレクションのスキーマフィールドが含まれていることを確認してください。
* [&#x200B; タグ拡張機能 &#x200B;](#tag-extension) または [JavaScript ライブラリ &#x200B;](#library) を使用して、このページで示すように Web SDK 設定でストリーミングメディア機能を設定します。

## Web SDK タグ拡張機能を使用したストリーミングメディアの設定 {#tag-extension}

Web SDK タグ拡張機能でストリーミングメディアを設定するには、次の手順に従います。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL 拡張機能]** に移動し、[!UICONTROL Adobe Experience Platform Web SDK **[!UICONTROL カードの]** 設定 &#x200B;] をクリックします。
1. **[!UICONTROL Web SDK タグ拡張機能設定ページ]** の説明に従って、[&#x200B; ストリーミングメディア &#x200B;](../../../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md#media-collection) 設定を行います。

## Web SDK JavaScript ライブラリを使用したストリーミングメディアの設定 {#library}

Web SDK でストリーミングメディアを設定するには、以下に説明するプロパティを使用します。

`configure` コマンドを呼び出す場合は、`streamingMedia` オブジェクトを追加します。

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
| `mainPingInterval` | 整数 | × | メインコンテンツに対する ping の頻度（秒）。 デフォルト値は `10` です。値の範囲は `10` ～ `50` 秒です。  値を指定しない場合、[&#x200B; 自動的にトラッキングされるセッション &#x200B;](../createmediasession.md#automatic) を使用するときにデフォルト値が使用されます。 |
| `adPingInterval` | 整数 | × | 広告コンテンツに対する ping の頻度（秒）。 デフォルト値は `10` です。値の範囲は `1` ～ `10` 秒です。 値を指定しない場合、[&#x200B; 自動的にトラッキングされるセッション &#x200B;](../createmediasession.md#automatic) を使用するときにデフォルト値が使用されます。 |
