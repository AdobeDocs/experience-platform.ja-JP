---
keywords: Talk URL Inbox,Talk URL Inbox,Talk URL Inbox,Talk URL Inbox,Talk URL,Talk URL,Talk URL
title: Talk URL Inbox 拡張機能
description: Talk URL Inbox 拡張機能は、Adobe Experience Platformの顧客の声の宛先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: f3e962c6-82c0-4d2f-9a78-275f261f058d
source-git-commit: b4e869f9bc29122db4fc66ccda752a50c7db729f
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 27%

---

# [!DNL Talk URL Inbox] 拡張機能 {#talk-url-extension}

Web ページ訪問者から音声メッセージを受信します。ホスト UI とクラウドベースの機械学習機能を含み、音声メッセージをテキストインサイトへと即座に書き起こします。

[!DNL Talk URL Inbox] は、Adobe Experience Platformの顧客の声拡張機能です。拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.100583.html) の拡張機能のページを参照してください。

この宛先はタグ拡張です。 Platform でのタグ拡張の動作について詳しくは、「[ タグ拡張の概要 ](../launch-extensions/overview.md)」を参照してください。

![Talk URL Inbox 拡張機能](../../assets/catalog/voice/talkurl/catalog.png)

## 前提条件 {#prerequisites}

この拡張機能は、Platform を購入したすべてのお客様の [!DNL Destinations] カタログで利用できます。

この拡張機能を使用するには、Adobe Experience Platformのタグにアクセスする必要があります。 タグは、Adobe Experience Cloudのお客様に、付属の付加価値機能として提供されます。 組織の管理者に問い合わせてタグへのアクセス権を取得し、タグに拡張機能のインストールに必要な **[!UICONTROL manage_properties]** 権限を付与するよう依頼します。

## 拡張機能のインストール {#install-extension}

[!DNL Talk URL Inbox] 拡張機能をインストールするには：

[Platform インターフェイス ](https://platform.adobe.com/) で、**[!UICONTROL 宛先]** / **[!UICONTROL カタログ]** に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトし、右側のレールで「**[!UICONTROL 設定]**」を選択します。 **[!UICONTROL Configure]** コントロールがグレー表示になっている場合は、**[!UICONTROL manage_properties]** 権限がありません。 [前提条件](#prerequisites)を確認してください。

拡張機能をインストールするプロパティを選択します。 また、新しいプロパティを作成することもできます。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、タグのドキュメントの [ プロパティページの節 ](../../../tags/ui/administration/companies-and-properties.md#properties-page) を参照してください。

このワークフローでは、インストールを完了する手順を順を追って説明します。

拡張機能の設定オプションとインストールのサポートについて詳しくは、[Adobe Exchange の Talk URL Inbox ページ](https://exchange.adobe.com/experiencecloud.details.100583.html)を参照してください。

拡張機能は、[ データ収集 UI](https://experience.adobe.com/#/data-collection/) に直接インストールすることもできます。 詳しくは、タグのドキュメントの [ 新しい拡張子 ](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension) の追加に関する節を参照してください。

## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、ルールの設定を開始できます。

特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストールした拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、[ タグのドキュメント ](../../../tags/ui/managing-resources/rules.md) を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

データ収集 UI で、拡張機能の設定、アップグレード、削除をおこなうことができます。

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合、Platform UI には、拡張機能の **[!UICONTROL Install]** が表示されます。 「[ 拡張機能のインストール ](#install-extension)」の説明に従って、インストールワークフローを開始し、拡張機能を設定または削除します。

拡張機能をアップグレードするには、タグのドキュメントの [ 拡張機能のアップグレードプロセス ](../../../tags/ui/managing-resources/extensions/extension-upgrade.md) のガイドを参照してください。
