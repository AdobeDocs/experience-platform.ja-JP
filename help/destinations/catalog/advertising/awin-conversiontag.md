---
keywords: Awin Advertiserコンバージョンタグ拡張；コンバージョンタグ；Awin;awin;AWIN
title: Awin Advertiser コンバージョンタグの拡張
description: Awin Advertiserコンバージョンタグの拡張は、Adobe Experience Platformの広告先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: 99feb169-acf3-4e68-8785-3f4cf565e5a9
source-git-commit: c8d6c156b3351324fe1be11144afeae91f7a2a59
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 30%

---

# Awin Advertiser コンバージョンタグの拡張 {#awin-conversiontag-extension}

## 概要 {#overview}

コンバージョンタグは、AWIN.Tracking.Sale JavaScript オブジェクトの宣言です。この宣言は、確認ページで実行され、コンバージョンがおこなわれたことをマスタータグに指示します。その後、必要なトラッキングリクエストが実行されます。

Awin Advertiserコンバージョンタグは、Adobe Experience Platformの広告拡張機能です。 拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.103240.awin-conversion-tag.html) の拡張機能のページを参照してください。

この宛先はタグ拡張です。 Platformでのタグ拡張の動作について詳しくは、「[タグ拡張の概要](../launch-extensions/overview.md)」を参照してください。

![UI の Awin Advertiser コンバージョン拡張](../../assets/catalog/advertising/awin-conversion-tag/catalog.png)

## 前提条件 {#prerequisites}

この拡張機能は、Platformを購入したすべての顧客の宛先カタログで利用できます。

この拡張機能を使用するには、Platformのタグにアクセスする必要があります。 タグは、Adobe Experience Cloudのお客様に、付属の付加価値機能として提供されます。 組織の管理者に連絡してデータ収集UIへのアクセス権を取得し、拡張機能をインストールするための&#x200B;**[!UICONTROL manage_properties]**&#x200B;権限を付与するよう管理者に依頼します。

## 拡張機能のインストール {#install-extension}

[!DNL Awin Advertiser Conversion Tag]拡張機能をインストールするには：

[Platformインターフェイス](https://platform.adobe.com/)で、**[!UICONTROL 宛先]**/**[!UICONTROL カタログ]**&#x200B;に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトし、右側のレールで「**[!UICONTROL 設定]**」を選択します。 **[!UICONTROL Configure]**&#x200B;コントロールがグレー表示になっている場合は、**[!UICONTROL manage_properties]**&#x200B;権限がありません。 [前提条件](#prerequisites)を確認してください。

拡張機能をインストールするタグプロパティを選択します。 また、新しいプロパティを作成することもできます。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。[タグのドキュメント](../../../tags/ui/administration/companies-and-properties.md)のプロパティについて説明します。

ワークフローに従って、データ収集UIが表示され、インストールが完了します。

拡張機能の設定オプションとインストールのサポートについて詳しくは、「[Adobe Exchange の Awin Advertiser コンバージョンタグページ](https://exchange.adobe.com/experiencecloud.details.103240.awin-conversion-tag.html)」を参照してください。

拡張機能は、[データ収集UI](https://experience.adobe.com/#/data-collection/)に直接インストールすることもできます。 詳しくは、[新しい拡張機能](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension)の追加に関するガイドを参照してください。


## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、ルールの設定を開始できます。 データ収集UIでは、特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストール済みの拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、タグドキュメントの[rules](../../../tags/ui/managing-resources/rules.md)の概要を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

データ収集UIで、拡張機能の設定、アップグレード、削除をおこなうことができます。

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合、UIには拡張機能の&#x200B;**[!UICONTROL Install]**&#x200B;が表示されます。 「[拡張機能のインストール](#install-extension)」の説明に従って、拡張機能を設定または削除するインストールワークフローを開始します。

拡張機能をアップグレードするには、タグのドキュメントの[拡張機能のアップグレードプロセス](../../../tags/ui/managing-resources/extensions/extension-upgrade.md)のガイドを参照してください。
