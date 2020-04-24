---
title: PebblePost拡張子
seo-title: PebblePost拡張子
description: PebblePost拡張は、Adobe Real-time Customer Data Platformの電子メールの送信先です。 拡張機能の詳細については、Adobe Exchangeの拡張機能のページを参照してください。
seo-description: PebblePost拡張は、Adobe Real-time Customer Data Platformの電子メールの送信先です。 拡張機能の詳細については、Adobe Exchangeの拡張機能のページを参照してください。
translation-type: tm+mt
source-git-commit: bfcbc56f05fa1c3b5fafd57b1166e50130b6007d

---


# PebblePost Extension {#pebblepost-extension}

## 概要 {#overview}

PebblePostのプログラムダイレクトメール®ソリューションは、デジタルマーケティング担当者がオンラインでの関心と意図を、コンバージョンを行うオフラインの有形メディアに結び付けるのに役立ちます。 マーケティング担当者は、アドビで作成したカスタムデータセグメントを活用して、関連性の高い、長期間持続する社内メディアのインプレッションをターゲット消費者に提供できます。 応答パスのアクティビティとオンサイトコンバージョンに基づいて、パフォーマンスをリアルタイムで分析します。

PebblePostは、Adobe Real-time Customer Data Platformの電子メール拡張機能です。 拡張機能の詳細については、 [Adobe Exchangeの拡張機能のページを参照してください](https://exchange.adobe.com/experiencecloud.details.100315.programmatic-direct-mail.html)。

この宛先は、エクスペリエンスプラットフォーム起動の拡張です。 Adobe Real-time CDPでの起動拡張機能の動作について詳しくは、「エクスペリエンスプラットフォーム起動拡張機能の概 [要」を参照してくださ](/help/rtcdp/destinations/experience-platform-launch-extensions.md)い。

![PebblePost Extension](assets/pebblepost-extension.png)

## 前提条件 {#prerequisites}

この拡張機能は、アドビのリアルタイムCDPを購入したすべてのお客様のDestinationsカタログで利用できます。

この拡張機能を使用するには、エクスペリエンスプラットフォームの起動にアクセスする必要があります。 エクスペリエンスプラットフォームの発売は、Adobe Experience Cloudのお客様に対して、付加価値機能として提供されます。 組織の管理者に連絡して、Launchへのアクセス権を取得し、拡張機能をインストールできるように **[!UICONTROL manage_properties]** 権限を付与するように依頼します。

## 拡張機能のインストール {#install-extension}

PebblePost拡張機能をインストールするには：

1. Adobe Real-time CDP [インターフェイスで](http://platform.adobe.com/)、宛先/カタロ **[!UICONTROL グに移動します]**。
2. カタログから拡張子を選択するか、検索バーを使用します。
3. リンク先をクリックしてハイライトし、右側のレールで「 **[!UICONTROL Install Extension]** 」を選択します。 「 **[!UICONTROL Install Extension]** 」コントロールが灰色表示になっている場合は、 **[!UICONTROL manage_properties]** 権限がありません。 前提条件を [参照してくださ](#prerequisites)い。
4. 「使用可能な **[!UICONTROL 起動プロパティを選択]** 」ウィンドウで、拡張機能をインストールする起動プロパティを選択します。 また、「起動」で新しいプロパティを作成するオプションもあります。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、起動ドキュメ [ントの「プロパティ](https://docs.adobe.com/content/help/en/launch/using/reference/admin/companies-and-properties.html#properties-page) 」ページの節を参照してください。
5. ワークフローが起動し、インストールが完了します。

拡張機能の設定オプションとインストールのサポートについて詳しくは、Adobe ExchangeのPebblePost [ページを参照してください](https://exchange.adobe.com/experiencecloud.details.100315.programmatic-direct-mail.html)。

拡張機能は、 [Experience Platform Launchインターフェイスに直接インストールすることもできます](https://launch.adobe.com/)。 Launchドキュ [メ追加ントの新しい拡張機能](https://docs.adobe.com/content/help/en/launch/using/reference/manage-resources/extensions/overview.html#add-a-new-extension) （英語のみ）を参照してください。

## How to use the extension {#how-to-use}

拡張機能をインストールすると、「起動」で開始がその拡張機能のルールを直接設定できます。

「起動」では、特定の状況でのみ拡張の宛先にイベントデータを送信するように、インストール済みの拡張のルールを設定できます。 拡張機能のルールの設定について詳しくは、ルールのドキュメントを参照 [してください](https://docs.adobe.com/help/ja-JP/launch/using/reference/manage-resources/rules.html)。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

起動インターフェイスで、拡張機能の設定、アップグレードおよび削除を行うことができます。

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合、Adobe Real-time CDP UIには、拡張機能の **[!UICONTROL Install]** が表示されます。 「拡張機能のインストール」の説明に従って [](#install-extension) 、インストールワークフローを開始し、拡張機能を設定または削除します。

拡張機能をアップグレードするには、Launchドキュメ [ントの](https://docs.adobe.com/content/help/en/launch/using/reference/manage-resources/extensions/extension-upgrade.html) Extensionアップグレードを参照してください。