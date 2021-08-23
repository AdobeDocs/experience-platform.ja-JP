---
title: Adobe MediumAnalytics (3.x SDK) for Audio and Video拡張機能の概要
description: Adobe Experience Platform の Adobe Media Analytics (3.x SDK) for Audio and Video タグ拡張機能について説明します。
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '764'
ht-degree: 96%

---

# Adobe Media Analytics (3.x SDK) for Audio and Video 拡張機能の概要

>[!NOTE]
>
>Adobe Experience Platform Launchは、Adobe Experience Platformのデータ収集テクノロジーのスイートとしてリブランドされました。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

このドキュメントには、Adobe Media Analytics (3.x SDK) for Audio and Video 拡張機能（Media Analytics 拡張機能）のインストール、設定および実装に関する情報が含まれています。この拡張機能を使用してルールを作成し、サンプルやサンプルへのリンクを作成する場合に使用できるオプションについても説明します。

Media Analytics（MA）拡張機能は、コアe JavaScript Media SDK（Media 3.x SDK）を追加します。この拡張機能では、タグが有効なサイトまたはプロジェクトに `Media` トラッカーインスタンスを追加する機能を提供します。MA 拡張機能には、次の 2 つの拡張拡張機能が必要です。

* [Analytics 拡張機能](../analytics/overview.md)
* [Experience Cloud ID 拡張機能](../id-service/overview.md)

>[!IMPORTANT]
>
>この拡張機能は、Media 3.x SDK でデプロイされ、Media 2.x SDK との下位互換性はありません。ページで既に Media 2.x SDK を使用している場合は、この拡張機能の代わりに [Adobe Media Analytics for Audio and Video 拡張機能](../media-analytics/overview.md)を使用します。

タグが有効なプロジェクトに前述の 3 つの拡張機能すべてを含めたら、次の 2 つの方法のいずれかを実行できます。

* Web アプリケーションから `Media` API を使用する
* 特定のメディアプレイヤーイベントを `Media` トラッカーインスタンス上の API にマッピングする、プレーヤー固有の拡張機能を含めたりビルドしたりします。このインスタンスは、MA 拡張機能を通じて公開されます。

## MA 拡張機能のインストールと設定

* **インストール：** MA 拡張機能をインストールするには、拡張機能プロパティを開き、**[!UICONTROL エクステンション／カタログ]**&#x200B;を選択し、 **[!UICONTROL オーディオおよびビデオ向け Adobe Media Analytics（3.x SDK）]**&#x200B;拡張機能にカーソルを置いて「**[!UICONTROL インストール]**」を選択します。

* **設定：** MA 拡張機能を設定するには、「[!UICONTROL 拡張機能]」タブを開き、拡張機能にカーソルを置いて「**[!UICONTROL 設定]**」をクリックします。

![MA 拡張機能の設定](../../../images/ext-ma-config.png)

### 設定オプション：

| オプション | 説明 |
| :--- | :--- |
| Collection API Server | Media Collection API Server を定義します（このサーバーを取得するには、アドビの担当者にお問い合わせください） |
| Application Version | メディアプレーヤーアプリケーション／SDK のバージョン。 |
| Player Name | 使用中のメディアプレーヤーの名前（例：「AVPlayer」、「HTML5 Player」、「My VideoPlayer」）。 |
| チャネル | チャネル名プロパティ |
| Debug Logging | ログを有効または無効にします |
| Enable SSL | HTTPS 経由の ping 送信を有効または無効にします |
| Export APIs to Window Object | グローバルスコープへの Media Analytics API の書き出しを有効または無効にします |
| Variable Name | `window` オブジェクトの下の Media Analytics API を書き出すために使用する変数 |

**リマインダー：** MA 拡張機能には、[ Analytics](../analytics/overview.md) および [Experience Cloud ID](../id-service/overview.md) 拡張機能が必要です。また、拡張機能プロパティにこれらの拡張機能を追加して設定する必要があります。

## MA 拡張機能の使用

### webpage/JS-app からの使用

MA 拡張機能は、[!UICONTROL 設定]ページ内の「Window オブジェクトに API を書き出し」設定を有効にして、メディア API をグローバルウィンドウオブジェクトに書き出します。設定された変数名の下に API をエクスポートします。例えば、変数名を `ADB` に設定している場合、`window.ADB.Media` によって Media API にアクセスできます。

>[!IMPORTANT]
>
> MA 拡張機能は、`window["CONFIGURED_VARIABLE_NAME"]` が定義されておらず、既存の変数を上書きしない場合にのみ、API をエクスポートします。

1. **Media API：** `window["CONFIGURED_VARIABLE_NAME"].Media`

   これにより、Media SDK のすべての API と定数が公開されます：[https://adobe-marketing-cloud.github.io/media-sdks/reference/javascript_3x/APIReference.html](https://adobe-marketing-cloud.github.io/media-sdks/reference/javascript_3x/APIReference.html)

1. **Media トラッカーインスタンスを作成：** `window["CONFIGURED_VARIABLE_NAME"].Media.getInstance`

   **戻り値：**&#x200B;メディアセッションを追跡するための `Media` トラッカーインスタンス。

   ```javascript
   var Media = window["CONFIGURED_VARIABLE_NAME"].Media;
   
   var tracker = Media.getInstance();
   ```

1. Media トラッカーインスタンスを使用し、[JS API ドキュメント](https://adobe-marketing-cloud.github.io/media-sdks/reference/javascript_3x/index.html)に従ってメディアトラッキングを実装します。

サンプルプレイヤーは、[MA サンプルプレイヤー](https://github.com/Adobe-Marketing-Cloud/media-sdks/tree/master/samples/launch/js/3.x)で入手できます。サンプルプレイヤーは、Web アプリケーションから直接 Media Analytics をサポートするために、MA 拡張機能を使用する方法を紹介しています。


### 他の拡張機能からの使用

MA 拡張機能は、`media` を共有モジュールとして他の拡張機能に公開します。（共有モジュールについて詳しくは、[共有モジュールのドキュメント](../../../extension-dev/web/shared.md)/を参照してください。）

>[!IMPORTANT]
>
> 共有モジュールは、他の拡張機能からのみアクセスできます。つまり、Web ページや JavaScript アプリケーションは共有モジュールにアクセスすることも、拡張機能以外の `turbine`（後述のサンプルコードを参照）を使用することもできません。

1. **Media API：** `media` 共有モジュール

   これにより、Media SDK のすべての API と定数が公開されます：[https://adobe-marketing-cloud.github.io/media-sdks/reference/javascript_3x/APIReference.html](https://adobe-marketing-cloud.github.io/media-sdks/reference/javascript_3x/APIReference.html)

1. 次のように、Media トラッカーインスタンスを作成します。

   **戻り値：**&#x200B;メディアセッションを追跡するための `Media` トラッカーインスタンス。

   ```javascript
   var Media =
     turbine.getSharedModule('adobe-media-analytics', 'media');
   
   var tracker = Media.getInstance();
   ```

1. Media トラッカーインスタンスを使用し、[JS API ドキュメント](https://adobe-marketing-cloud.github.io/media-sdks/reference/javascript_3x/index.html)に従ってメディアトラッキングを実装します。

>[!NOTE]
>
>**テスト：**&#x200B;このリリースでは、拡張機能をテストするには、すべての独立した拡張機能にアクセスできる [ Platform ](../../../extension-dev/submit/upload-and-test.md) に拡張機能をアップロードする必要があります。
