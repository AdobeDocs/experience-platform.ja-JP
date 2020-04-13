---
title: Clicktale拡張機能
seo-title: Clicktale拡張機能
description: Clicktale拡張機能は、Adobe Real-time Customer Data Platformの解析の宛先です。 拡張機能の詳細については、Adobe Exchangeの拡張機能のページを参照してください。
seo-description: Clicktale拡張機能は、Adobe Real-time Customer Data Platformの解析の宛先です。 拡張機能の詳細については、Adobe Exchangeの拡張機能のページを参照してください。
translation-type: tm+mt
source-git-commit: ff91395844c239415123a33d65fa0deb2221ae25

---


# Clicktale拡張機能 {#clicktale-extension}

## 概要 {#overview}

Clicktaleとエクスペリエンスプラットフォーム起動の統合により、Clicktaleコードの導入と、Clicktaleのビジュアルエディターを使用した起動内のデータ要素のマッピング機能が可能になります。

Clicktaleは、Adobe Real-time Customer Data PlatformのAnalytics拡張機能です。 拡張機能の詳細については、 [Adobe Exchangeの拡張機能のページを参照してください](https://exchange.adobe.com/experiencecloud.details.100082.html)。

この宛先は、エクスペリエンスプラットフォーム起動の拡張です。 Adobe Real-time CDPでの起動拡張機能の動作について詳しくは、「エクスペリエンスプラットフォーム起動拡張機能の概 [要」を参照してくださ](/help/rtcdp/destinations/experience-platform-launch-extensions.md)い。

![Clicktale拡張機能](assets/clicktale-extension.png)


## 前提条件 {#prerequisites}

この拡張機能は、アドビのリアルタイムCDPを購入したすべてのお客様のDestinationsカタログで利用できます。

この拡張機能を使用するには、エクスペリエンスプラットフォームの起動にアクセスする必要があります。 エクスペリエンスプラットフォームの発売は、Adobe Experience Cloudのお客様に対して、付加価値機能として提供されます。 組織の管理者に連絡して、Launchへのアクセス権を取得し、拡張機能をインストールする権限を付与す **[!UICONTROL manage_properties]** るように依頼してください。

## 拡張機能のインストール {#install-extension}

Clicktale拡張機能をインストールするには：

1. Adobe Real-time [CDPインターフェイスで](http://platform.adobe.com/)、に進みます **[!UICONTROL Destinations > Catalog]**。
2. カタログから拡張子を選択するか、検索バーを使用します。
3. リンク先をクリックしてハイライトし、右側のレ **[!UICONTROL Install Extension]** ールでを選択します。 コントロール **[!UICONTROL Install Extension]** が灰色表示になっている場合は、権限があり **[!UICONTROL manage_properties]** ません。 前提条件を [参照してくださ](#prerequisites)い。
4. ウィンドウ **[!UICONTROL Select available Launch property]** で、拡張機能をインストールするLaunchプロパティを選択します。 また、「起動」で新しいプロパティを作成するオプションもあります。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、起動ドキュメ [ントの「プロパティ](https://docs.adobe.com/content/help/en/launch/using/reference/admin/companies-and-properties.html#properties-page) 」ページの節を参照してください。
5. ワークフローが起動し、インストールが完了します。

拡張機能の設定オプションについて詳しくは、Adobe Exchangeの [Clicktale extensionページを参照](https://exchange.adobe.com/experiencecloud.details.100082.html) してください。

拡張機能は、 [Experience Platform Launchインターフェイスに直接インストールすることもできます](https://launch.adobe.com/)。 Launchドキュ [メ追加ントの新しい拡張機能](https://docs.adobe.com/content/help/en/launch/using/reference/manage-resources/extensions/overview.html#add-a-new-extension) （英語のみ）を参照してください。


## How to use the extension {#how-to-use}

拡張機能をインストールすると、「起動」で開始がその拡張機能のルールを直接設定できます。

「起動」では、特定の状況でのみ拡張の宛先にイベントデータを送信するように、インストール済みの拡張のルールを設定できます。 拡張機能のルールの設定について詳しくは、ルールのドキュメントを参照 [してください](https://docs.adobe.com/help/ja-JP/launch/using/reference/manage-resources/rules.html)。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

起動インターフェイスで、拡張機能の設定、アップグレードおよび削除を行うことができます。

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合は、その拡張機能のAdobe Real-time CDP UIが引き続き **[!UICONTROL Install]** 表示されます。 「拡張機能のインストール」の説明に従って [](#install-extension) 、インストールワークフローを開始し、拡張機能を設定または削除します。

拡張機能をアップグレードするには、Launchドキュメ [ントの](https://docs.adobe.com/content/help/en/launch/using/reference/manage-resources/extensions/extension-upgrade.html) Extensionアップグレードを参照してください。



