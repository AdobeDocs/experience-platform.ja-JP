---
keywords: Bing;Bing Ads イベントトラッキング；イベントトラッキング Bing;UET;UET 拡張機能
title: Bing Ads Universal Event Tracking（UET）拡張機能
description: Bing Ads ユニバーサルイベントトラッキング (UET) 拡張機能は、Adobe Experience Platformの広告先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: f2fc4d1f-01b0-4813-902c-9a3c30a8fa78
source-git-commit: c8d6c156b3351324fe1be11144afeae91f7a2a59
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 28%

---

# [!DNL Bing Ads Universal Event Tracking] (UET) 拡張 {#bing-ads-extension}

## 概要 {#overview}

この [!DNL Bing Ads Universal Event Tracking] (UET) タグ拡張は、誰かが検索広告をクリックした後の動作を追跡するのに便利です。 1 つの UET タグを使用して Web サイトでの顧客の行動を記録することによってそのデータを活用し、リマーケティングリストを使用してコンバージョンやターゲットオーディエンスを追跡できます。

[!DNL Bing Ads Universal Event Tracking] (UET) は、Adobe Experience Platformの広告拡張機能です。 拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.100154.html) の拡張機能のページを参照してください。

この宛先はタグ拡張です。 Platform でのタグ拡張機能について詳しくは、 [タグ拡張機能の概要](../launch-extensions/overview.md).

![Bing Ads 拡張](../../assets/catalog/advertising/bing-ads/catalog.png)

## 前提条件 {#prerequisites}

この拡張機能は、 [!DNL Destinations] Platform を購入したすべての顧客のカタログ。

この拡張機能を使用するには、Adobe Experience Platformのタグにアクセスする必要があります。 タグは、Adobe Experience Cloudのお客様に、付属の付加価値機能として提供されます。 組織の管理者に問い合わせてタグへのアクセス権を取得し、 **[!UICONTROL manage_properties]** 権限を持つユーザーが拡張機能をインストールできるようにする。

## 拡張機能のインストール {#install-extension}

をインストールするには、以下を実行します。 [!DNL Bing Ads Universal Event Tracking] (UET) 拡張機能：

内 [Platform インターフェイス](https://platform.adobe.com/)に移動します。 **[!UICONTROL 宛先]** > **[!UICONTROL カタログ]**.

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトし、「 」を選択します。 **[!UICONTROL 設定]** をクリックします。 この **[!UICONTROL 設定]** コントロールが灰色表示になっているので、次の項目はありません： **[!UICONTROL manage_properties]** 権限。 [前提条件](#prerequisites)を確認してください。

拡張機能をインストールするタグプロパティを選択します。 また、新しいプロパティを作成することもできます。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。のプロパティについて [タグドキュメント](../../../tags/ui/administration/companies-and-properties.md).

ワークフローに従ってデータ収集 UI が表示され、インストールが完了します。

拡張機能の設定オプションとインストールのサポートについて詳しくは、[Adobe Exchange の Bing Ads ユニバーサルイベントトラッキング（UET）のページ](https://exchange.adobe.com/experiencecloud.details.100154.html)を参照してください。

拡張機能は、 [データ収集 UI](https://experience.adobe.com/#/data-collection/). 詳しくは、 [新しい拡張機能の追加](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension) を参照してください。


## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、ルールの設定を開始できます。 データ収集 UI では、特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストール済みの拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、「 [ルール](../../../tags/ui/managing-resources/rules.md) （タグドキュメント）を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

データ収集 UI で、拡張機能の設定、アップグレード、削除をおこなうことができます。

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合、UI は表示されます **[!UICONTROL インストール]** 拡張機能用。 インストールワークフローを開始します ( [拡張機能のインストール](#install-extension) 拡張機能を設定または削除するには、以下を実行します。

拡張機能をアップグレードするには、 [拡張機能のアップグレードプロセス](../../../tags/ui/managing-resources/extensions/extension-upgrade.md) （タグドキュメント）を参照してください。
