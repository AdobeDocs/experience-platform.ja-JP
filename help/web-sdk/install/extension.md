---
title: タグ拡張機能を使用した Web SDK のインストール
description: Adobe Experience Cloud Data Collection を使用して、Web SDK ライブラリを参照します。
exl-id: ba8348c9-f642-4230-9f7f-4496d4154d83
source-git-commit: 58cd6300307881c3de7c52e07c401bf2ed908517
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 0%

---

# タグ拡張機能を使用した Web SDK のインストール

Adobeには、Web SDK を実装および設定するための専用のタグ拡張機能が用意されています。 この実装方法は、データ収集コードのデプロイと管理をAdobeがお勧めする主な方法です。

次に [前提条件](overview.md)を使用する場合、次の手順で Web SDK タグ拡張機能を導入できます。

## タグ内に拡張機能をインストールする

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択するか、タグプロパティを作成します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、 **[!UICONTROL カタログ]** タブをクリックします。
1. を探してインストールします。 **[!UICONTROL Adobe Experience Platform Web SDK]** 拡張子。
1. 各環境に適したサンドボックスとデータストリームを選択し、「 **[!UICONTROL 保存]**.

方法に関するドキュメントを参照してください。 [Web SDK タグ拡張機能の設定](../../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) を参照してください。

## タグコードを開発に公開する

これで、このタグ用の Web SDK 拡張機能がインストールされました。 開発環境で使用するタグコードを公開できるようになりました。

1. に移動します。 **[!UICONTROL 公開フロー]**&#x200B;を選択し、「 **[!UICONTROL ライブラリを追加]**.
1. 「Web SDK ライブラリの追加」など、必要な名前をこのライブラリに付けます。 を設定します。 [!UICONTROL 環境] ドロップダウンメニューから「開発」に移動します。
1. 選択 **[!UICONTROL 変更されたリソースをすべて追加]**&#x200B;を選択し、次に **[!UICONTROL 開発用に保存およびビルド]**.

## サイトにローダコードをインストールする

タグコードが公開されたので、タグローダーコードを Web サイトに追加できます。

1. に移動します。 **[!UICONTROL 環境]**&#x200B;をクリックし、「Development」の横にあるボックスアイコンをクリックして、この環境のインストール手順を含むモーダルウィンドウを開きます。
1. 埋め込みコードをコピーし、 `<head>` タグ内に貼り付けます。

## 実装を入力し、実稼動環境にパブリッシュします

詳しくは、 [Web SDK 拡張機能の概要](../../tags/extensions/client/web-sdk/overview.md) 拡張機能自体に関する詳細、および [タグの概要](../../tags/home.md) タグインターフェイスの操作について詳しくは、を参照してください。
