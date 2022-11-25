---
title: Adobe Media Analytics for Audio and Video 拡張機能の概要
description: Adobe Experience Platform の Adobe Media Analytics for Audio and Video タグ拡張機能について説明します。
exl-id: 426cfd08-aead-4b35-824c-45494bca2fc8
source-git-commit: 88939d674c0002590939004e0235d3da8b072118
workflow-type: tm+mt
source-wordcount: '987'
ht-degree: 100%

---

# Adobe Media Analytics for Audio and Video 拡張機能の概要

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

このドキュメントには、Adobe Media Analytics for Audio and Video 拡張機能（Media Analytics 拡張機能）のインストール、設定および実装に関する情報が含まれています。この拡張機能を使用してルールを作成し、サンプルやサンプルへのリンクを作成する場合に使用できるオプションについても説明します。

Media Analytics（MA）拡張機能は、コアe JavaScript Media SDK（Media 2.x SDK）を追加します。この拡張機能では、タグのサイトまたはプロジェクトに `MediaHeartbeat` トラッカーインスタンスを追加する機能を提供します。MA 拡張機能には、次の 2 つの拡張拡張機能が必要です。

* [Analytics 拡張機能](../analytics/overview.md)
* [Experience Cloud ID 拡張機能](../id-service/overview.md)

>[!IMPORTANT]
>
> オーディオトラッキングには、Analytics 拡張機能 v1.6 以上が必要です。

タグプロジェクトに前述の 3 つの拡張機能すべてを含めたら、次の 2 つの方法のいずれかを実行できます。

* Web アプリケーションから `MediaHeartbeat` API を使用する
* 特定のメディアプレイヤーイベントを `MediaHeartbeat` トラッカーインスタンス上の API にマッピングする、プレーヤー固有の拡張機能を含めたりビルドしたりします。このインスタンスは、MA 拡張機能を通じて公開されます。

## MA 拡張機能のインストールと設定

* **インストール：** MA 拡張機能をインストールするには、拡張機能のプロパティを開き、 **[!UICONTROL エクステンション／カタログ]**&#x200B;を選択し、 **[!UICONTROL Analytics for Audio and Video]** 拡張機能にカーソルを置いて「**[!UICONTROL インストー ル]**」を選択します。

* **設定：** MA 拡張機能を設定するには、「[!UICONTROL エクステンション]」タブを開き、拡張機能にマウスポインターを置いて「**[!UICONTROL 設定]**」を選択します。

![MA 拡張機能の設定](../../../images/ext-va-config.jpg)

### 設定オプション：

| オプション | 説明 |
| :--- | :--- |
| Tracking Server | メディアハートビートを追跡するためのサーバーを定義します（分析トラッキングサーバーと同じサーバーではありません）。 |
| Application Version | メディアプレーヤーアプリケーション／SDK のバージョン。 |
| Player Name | 使用中のメディアプレーヤーの名前（例：「AVPlayer」、「HTML5 Player」、「My VideoPlayer」）。 |
| チャネル | チャネル名プロパティ |
| Online Video Provider | コンテンツの配布に使用するオンラインビデオプラットフォームの名前。 |
| Debug Logging | ログを有効または無効にします |
| Enable SSL | HTTPS 経由の ping 送信を有効または無効にします |
| Export APIs to Window Object | グローバルスコープへの Media Analytics API の書き出しを有効または無効にします |
| Variable Name | `window` オブジェクトの下の Media Analytics API を書き出すために使用する変数 |

**リマインダー：** MA 拡張機能には、[ Analytics](../analytics/overview.md) および [Experience Cloud ID](../id-service/overview.md) 拡張機能が必要です。また、拡張機能プロパティにこれらの拡張機能を追加して設定する必要があります。

## MA 拡張機能の使用

### webpage/JS-app からの使用

MA 拡張機能は、[!UICONTROL 設定] ページ内の「Windows オブジェクトに API を書き出す」設定を有効にして、MediaHeartbeat API をグローバルウィンドウオブジェクトに書き出します。設定された変数名の下に API をエクスポートします。例えば、変数名を `ADB` に設定している場合、`window.ADB.MediaHeartbeat` によって MediaHeartbeat にアクセスできます。

>[!IMPORTANT]
>
> MA 拡張機能は、`window["CONFIGURED_VARIABLE_NAME"]` が定義されておらず、既存の変数を上書きしない場合にのみ、API をエクスポートします。

1. **MediaHeartbeat インスタンスの作成：** `window["CONFIGURED_VARIABLE_NAME"].MediaHeartbeat.getInstance`

   **パラメーター：**&#x200B;これらの関数を公開する有効なデリゲートオブジェクト。

   | メソッド |  説明 |
   | :--- | :--- |
   | `getQoSObject()` | 現在の QoS 情報を含む `theMediaObject` インスタンスを返します。このメソッドは、再生セッション中に複数回呼び出されます。プレーヤー実装は、常に、利用可能な最新の QoS データを返す必要があります。 |
   | `getCurrentPlaybackTime()` | 再生ヘッドの現在の位置を返します。VOD 追跡の場合は、メディアアイテムの開始時からの時間（秒）を返します。ライブ追跡の場合は、プログラムの開始時からの時間（秒）を返します。 |

   **戻り値：** `MediaHeartbeat` インスタンスで解決されるか、エラーメッセージが表示されて拒否されるプロミス。

1. **MediaHeartbeat 定数へアクセス：** `window["CONFIGURED_VARIABLE_NAME"].MediaHeartbeat`

   これにより、 [`MediaHeartbeat`](https://adobe-marketing-cloud.github.io/media-sdks/reference/javascript/MediaHeartbeat.html) クラスのすべての定数および静的メソッドが表示されます。

   サンプルプレイヤーは、[MA サンプルプレイヤー](https://github.com/Adobe-Marketing-Cloud/media-sdks/tree/master/samples/launch/js/2.x)で入手できます。サンプルプレイヤーは、Web アプリケーションから直接 Media Analytics をサポートするために、MA 拡張機能を使用する方法を紹介しています。

1. 次のように、MediaHeartbeat トラッカーインスタンスを作成します。

   ```javascript
   var MediaHeartbeat = window["CONFIGURED_VARIABLE_NAME"].MediaHeartbeat;
   
   var delegate = {
       getCurrentPlaybackTime: this._getCurrentPlaybackTime.bind(this),
       getQoSObject: this._getQoSObject.bind(this),
   };
   
   var config = {
       playerName: "Custom Player",
       ovp: "Custom OVP",
       channel: "Custom Channel"
   };
   
   var self = this;
   MediaHeartbeat.getInstance(delegate, config).then(function(instance) {
       self._mediaHeartbeat = instance;
       // Do Tracking using the MediaHeartbeat instance.
   }).catch(function(err){
       // Getting MediaHeartbeat instance failed.
   });
   ```

### 他の拡張機能からの使用

MA 拡張機能は、`get-instance` および `media-heartbeat` 共有モジュールを他の拡張機能に公開します。（共有モジュールについて詳しくは、 [共有モジュールのドキュメント](../../../extension-dev/web/shared.md) /を参照してください。）

>[!IMPORTANT]
>
> 共有モジュールは、他の拡張機能からのみアクセスできます。つまり、Web ページや JS アプリケーションは共有モジュールにアクセスすることも、拡張機能以外の `turbine`（後述のサンプルコードを参照）を使用することもできません。

1. **MediaHeartbeat インスタンスの作成：** `get-instance` 共有モジュール

   **パラメーター：**

   * 次の関数を公開する有効な delegate オブジェクト：

      | メソッド |  説明 |
      | :--- | :--- |
      | `getQoSObject()` | 現在の QoS 情報を含む `MediaObject` インスタンスを返します。このメソッドは、再生セッション中に複数回呼び出されます。プレイヤー実装は、常に、利用可能な最新の QoS データを返す必要があります。 |
      | `getCurrentPlaybackTime()` | 再生ヘッドの現在の位置を返します。VOD 追跡の場合は、メディアアイテムの開始時からの時間（秒）を返します。ライブ追跡の場合は、プログラムの開始時からの時間（秒）を返します。 |

   * これらのプロパティを公開するオプションの config オブジェクト：

      | プロパティ | 説明 | 必須 |
      | :--- | :--- | :--- |
      | オンラインビデオプロバイダ | コンテンツの配布に使用するオンラインビデオプラットフォームの名前。 | いいえ。存在する場合、拡張機能の設定時に定義された値を上書きします。 |
      | プレーヤー名 | 使用中のメディアプレーヤーの名前（例：「AVPlayer」、「HTML5 Player」、「My VideoPlayer」）。 | いいえ。存在する場合、拡張機能の設定時に定義された値を上書きします。 |
      | チャネル | チャネル名プロパティ | いいえ。存在する場合、拡張機能の設定時に定義された値を上書きします。 |
   **戻り値：** `MediaHeartbeat` インスタンスで解決されるか、エラーメッセージが表示されて拒否されるプロミス。

1. **MediaHeartbeat 定数へのアクセス：** `media-heartbeat` 共有モジュール

   このモジュールでは、次のクラスからすべての定数および静的メソッドを公開します：[https://adobe-marketing-cloud.github.io/media-sdks/reference/javascript/MediaHeartbeat.html](https://adobe-marketing-cloud.github.io/media-sdks/reference/javascript/MediaHeartbeat.html)。

1. 次のように、MediaHeartbeat トラッカーインスタンスを作成します。

   ```javascript
   var getMediaHeartbeatInstance =
     turbine.getSharedModule('adobe-video-analytics', 'get-instance');
   
   var MediaHeartbeat =
     turbine.getSharedModule('adobe-video-analytics', 'media-heartbeat');
     ...
   
   var delegate = {
       getCurrentPlaybackTime: this._getCurrentPlaybackTime.bind(this),
       getQoSObject: this._getQoSObject.bind(this),
   }
   
   var config = {
       playerName: "Custom Player",
       ovp: "Custom OVP",
       channel: "Custom Channel"
   }
   ...
   
   var self = this;
   getMediaHeartbeatInstance(delegate, config).then(function(instance) {
       self._mediaHeartbeat = instance;
       ...
       // Do Tracking using the MediaHeartbeat instance.
   }).catch(function(err){
       // Getting MediaHeartbeat instance failed.
   });
   
   ...
   ```

1. Media Heartbeatインスタンスを使用し、[Media SDK JS のドキュメント](https://experienceleague.adobe.com/docs/media-analytics/using/sdk-implement/setup/setup-javascript/set-up-js-2.html?lang=ja)および[JS API のドキュメント](https://adobe-marketing-cloud.github.io/media-sdks/reference/javascript/index.html)に従ってメディアトラッキングを実装します。

>[!NOTE]
>
>**テスト：**&#x200B;このリリースでは、拡張機能をテストするには、すべての独立した拡張機能にアクセスできる [Platform](../../../extension-dev/submit/upload-and-test.md) に拡張機能をアップロードする必要があります。


<!--
## Leveraging the sample HTML5 player

You can obtain the MA extension sample HTML5 player here: [HTML5 Sample Player](https://github.com/adobe/reactor-adobe-va-sample-player). The sample player acts as a reference to create video player extensions and to showcase using the MA extension to support media tracking.

### Sample player extension action types

This section describes the action types available in the Sample Player extension.

#### Open Video

The _Open Video_ action provides various configurations for creating and customizing an HTML5 player, providing a video to play and enabling/disabling Adobe Video Analytics tracking.

**Action Configuration / Player Settings:** Note the CSS Selector setting which specifics the `<div>` in the web page where the player is added. Note also that the _Enable Adobe Analytics_ checkbox is checked in the Analytics Settings pane.

![Player Extension Action Configuration](../../../images/ext-va-sp-action.png)

![Player Extension Action Configuration](../../../images/ext-va-sp-action1.png)

* [\[...\]/openVideo/openVideo.jsx](https://github.com/adobe/reactor-adobe-va-sample-player/blob/master/src/view/actions/openVideo/openVideo.jsx) -

  UI Code to configure the Action is defined here.

* [\[...\]/actions/openVideo.js](https://github.com/adobe/reactor-adobe-va-sample-player/blob/master/src/lib/actions/openVideo.js) - This file exports a function that gets executed when the Action is triggered as part of the tag rule.

  This is a code snippet from `openVideo.js` where the `openVideo` Action is executed:

  ```javascript
    function openVideo(settings) {
        let player;
        try {
            Logger.info(LOG_TAG, `Executing action with ${JSON.stringify(settings)}`);

            player = new PlayerFacade(settings);
            PlayerStore.registerPlayer(player);
            player.load(settings.media);
        } catch (ex) {
            Logger.error(LOG_TAG, `Creating player for action openVideo failed with error ${ex.message}`);

            // Cleanup from DOM.
            if (player) {
                player.destroy();
            }
        }
    }
    ...
  ```

* [\[...\]/analytics/adobeAnalyticsProvider.js](https://github.com/adobe/reactor-adobe-va-sample-player/blob/master/src/lib/helpers/analytics/adobeAnalyticsProvider.js) - This file implements Video Analytics tracking by using Shared Modules exposed by the VA extension.

### Sample player extension basic deployment

Once the Sample Player extension is installed, you'll need to create at least one rule to properly deploy it. The Image below shows a sample rule that opens the specified video when the core extension fires the `DOMLoaded` event.

![Player Extension Sample Rule](../../../images/ext-va-sp-rule.png)

After you have saved this rule, you will need to add it to a Library, and then build and deploy so that you can test the behavior.

-->
