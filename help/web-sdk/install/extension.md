---
title: タグ拡張機能を使用した Web SDK のインストール
description: Adobe Experience Cloud Data Collection を使用した Web SDK ライブラリを参照します。
exl-id: ba8348c9-f642-4230-9f7f-4496d4154d83
source-git-commit: 58cd6300307881c3de7c52e07c401bf2ed908517
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 0%

---

# タグ拡張機能を使用した Web SDK のインストール

Adobeでは、Web SDK を実装および設定するための専用のタグ拡張機能を提供しています。 データ収集コードをデプロイおよび管理する場合にAdobeが推奨する主な手段は、この実装方法です。

[ 前提条件 ](overview.md) を満たしたら、次の手順を使用して Web SDK タグ拡張機能をデプロイできます。

## タグ内での拡張機能のインストール

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択するか、タグプロパティを作成します。
1. **[!UICONTROL 拡張機能]** に移動し、「**[!UICONTROL カタログ]**」タブを選択します。
1. **[!UICONTROL Adobe Experience Platform Web SDK]** 拡張機能を見つけてインストールします。
1. 各環境に適したサンドボックスとデータストリームを選択し、「**[!UICONTROL 保存]**」をクリックします。

詳しくは、[Web SDK タグ拡張機能の設定 ](../../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) に関するドキュメントを参照してください。

## タグコードを開発用にPublish

これで、このタグ用に Web SDK 拡張機能がインストールされました。 開発環境で使用するタグコードを公開できるようになりました。

1. **[!UICONTROL 公開フロー]** に移動し、「**[!UICONTROL ライブラリを追加]**」を選択します。
1. このライブラリに必要な名前を付けます（「Web SDK ライブラリを追加」など）。 [!UICONTROL &#x200B; 環境 &#x200B;] ドロップダウンメニューを「開発」に設定します。
1. 「**[!UICONTROL 変更されたリソースをすべて追加]**」を選択し、「**[!UICONTROL 開発用に保存およびビルド]**」をクリックします。

## サイトにローダーコードをインストールする

タグコードが公開されたので、タグローダーコードを web サイトに追加できます。

1. **[!UICONTROL 環境]** に移動し、「開発」の横にあるボックスアイコンをクリックして、この環境のインストール手順を含むモーダルウィンドウを開きます。
1. 埋め込みコードをコピーして、Web サイトの `<head>` タグに貼り付けます。

## 実装に入力して実稼動環境に公開します

拡張機能自体の詳細については [Web SDK 拡張機能の概要 ](../../tags/extensions/client/web-sdk/overview.md) を参照してください。タグインターフェイスの操作の詳細については [ タグの概要 ](../../tags/home.md) を参照してください。
