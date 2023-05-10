---
keywords: Awin Advertiser mastertag 拡張機能；mastertag;Awin;awin;AWIN
title: Awin Advertiser Mastertag 拡張機能
description: Awin Advertiser Mastertag 拡張機能は、Adobe Experience Platformの広告先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: 99a9ea40-b89f-4503-91a7-60cc8e1cd6d3
source-git-commit: c8d6c156b3351324fe1be11144afeae91f7a2a59
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 64%

---

# [!DNL Awin Advertiser Mastertag] 拡張機能 {#awin-mastertag-extension}

## 概要 {#overview}

この [!DNL MasterTag] は、Awin トラッキングソリューションに必要なすべての機能を含む JavaScript ライブラリで、支払い情報を表示または処理するページを除き、確認ページを含むサイトのすべてのページに無条件に追加する必要があります。

[!DNL Awin Advertiser Mastertag] は、Adobe Experience Platformの広告拡張機能です。 拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.103176.awin-advertiser-mastertag.html) の拡張機能のページを参照してください。

この宛先はタグ拡張機能です。 Platform での拡張機能について詳しくは、 [タグ拡張機能の概要](../launch-extensions/overview.md).

![UI での Awin Advertiser Mastertag 拡張機能](../../assets/catalog/advertising/awin-mastertag/catalog.png)

## 前提条件 {#prerequisites}

プラットフォームを購入したすべての顧客は、この拡張機能を [!DNL Destinations] カタログから利用できます。

この拡張機能を使用するには、Adobe Experience Platform でタグにアクセスする必要があります。 タグは、標準装備の付加価値機能として Adobe Experience Cloud の顧客に提供されます。組織の管理者に問い合わせてタグへのアクセス権を取得し、拡張機能をインストールできるよう **[!UICONTROL manage_properties]** 権限の付与を依頼します。

## 拡張機能のインストール {#install-extension}

をインストールするには、以下を実行します。 [!DNL Awin Advertiser Mastertag] 拡張子：

[Platform インターフェイス](https://platform.adobe.com/)で、**[!UICONTROL 宛先]**／**[!UICONTROL カタログ]**&#x200B;に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトしてから、右側のパネルで「**[!UICONTROL 設定]**」を選択します。**[!UICONTROL 設定]**&#x200B;コントロールがグレー表示になっている場合は、**[!UICONTROL manage_properties]** 権限がありません。[前提条件](#prerequisites)を確認してください。

拡張機能をインストールするタグプロパティを選択します。 また、新しいプロパティを作成するオプションもあります。プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。のプロパティについて [タグドキュメント](../../../tags/ui/administration/companies-and-properties.md).

ワークフローに従ってデータ収集 UI が表示され、インストールが完了します。

この拡張機能の設定オプションとインストールのサポートについて詳しくは、[Adobe Exchange の Awin Advertiser Mastertag ページ](https://exchange.adobe.com/experiencecloud.details.103176.awin-advertiser-mastertag.html)を参照してください。

拡張機能は、[データ収集 UI](https://experience.adobe.com/#/data-collection/) で直接インストールできます。詳しくは、 [新しい拡張機能の追加](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension) を参照してください。


## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、ルールの設定を開始できます。データ収集 UI では、特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストール済みの拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、「 [ルール](../../../tags/ui/managing-resources/rules.md) （タグドキュメント）を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

データ収集 UI で、拡張機能の設定、アップグレードおよび削除を行うことができます。

>[!TIP]
>
>拡張機能がいずれかのプロパティに既にインストールされている場合、 UI ではその拡張機能に引き続き「**[!UICONTROL インストール]**」が表示されます。[拡張機能のインストール](#install-extension)の説明に従ってインストールワークフローを開始し、拡張機能を設定または削除します。

拡張機能をアップグレードするには、[拡張機能のアップグレードプロセス](../../../tags/ui/managing-resources/extensions/extension-upgrade.md) （タグドキュメント）のガイドを参照してください。
