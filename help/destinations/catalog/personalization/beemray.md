---
keywords: beemray, beemray 拡張機能
title: Beemray 拡張機能
description: Beemray 拡張機能は、Adobe Experience Platformのパーソナライゼーションの宛先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: 5bb639f5-42b5-48ae-a3e9-7585595ab925
source-git-commit: c8d6c156b3351324fe1be11144afeae91f7a2a59
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 30%

---

# [!DNL Beemray] 拡張機能 {#beemray-extension}

## 概要 {#overview}

[!DNL Beemray] は、状況に合ったコンテキストを使用して製品を加速するお手伝いをします。インサイトを得たり、新しいエクスペリエンスを構築したり、インタラクションを促進したり、本当に重要な瞬間を捉えたりすることが可能になります。Beemray は、機械学習を使用してコンテキストインテリジェンスを自動化します。Beemray は、Adobe Experience Cloud および他のテクニカルパートナーに接続します。すべてがリアルタイムで実行されます。 この拡張機能は、 [!DNL Beemray] サイトの SDK。

Beemray は、Adobe Experience Platformのパーソナライズ機能の拡張機能です。 拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.101063.beemray-human-context.html) の拡張機能のページを参照してください。

この宛先はタグ拡張です。 Platform でのタグ拡張機能について詳しくは、 [タグ拡張機能の概要](../launch-extensions/overview.md).

![Beemray 拡張機能](../../assets/catalog/personalization/beemray/catalog.png)

## 前提条件 {#prerequisites}

この拡張機能は、 [!DNL Destinations] Platform を購入したすべての顧客のカタログ。

この拡張機能を使用するには、Adobe Experience Platformのタグにアクセスする必要があります。 タグは、Adobe Experience Cloudのお客様に、付属の付加価値機能として提供されます。 組織の管理者に問い合わせてタグへのアクセス権を取得し、 **[!UICONTROL manage_properties]** 権限を持つユーザーが拡張機能をインストールできるようにする。

## 拡張機能のインストール {#install-extension}

をインストールするには、以下を実行します。 [!DNL Beemray] 拡張子：

内 [Platform インターフェイス](https://platform.adobe.com/)に移動します。 **[!UICONTROL 宛先]** > **[!UICONTROL カタログ]**.

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトし、「 」を選択します。 **[!UICONTROL 設定]** をクリックします。 この **[!UICONTROL 設定]** コントロールが灰色表示になっているので、次の項目はありません： **[!UICONTROL manage_properties]** 権限。 [前提条件](#prerequisites)を確認してください。

拡張機能をインストールするタグプロパティを選択します。 また、新しいプロパティを作成することもできます。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。のプロパティについて [タグドキュメント](../../../tags/ui/administration/companies-and-properties.md).

ワークフローに従ってデータ収集 UI が表示され、インストールが完了します。

拡張機能の設定オプションとインストールのサポートについて詳しくは、[Adobe Exchange の Beemray ページ](https://exchange.adobe.com/experiencecloud.details.101063.beemray-human-context.html)を参照してください。

拡張機能は、 [データ収集 UI](https://experience.adobe.com/#/data-collection/). 詳しくは、 [新しい拡張機能の追加](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension) を参照してください。

## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、ルールの設定を開始できます。 データ収集 UI では、特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストール済みの拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、「 [ルール](../../../tags/ui/managing-resources/rules.md) （タグドキュメント）を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

データ収集 UI で、拡張機能の設定、アップグレード、削除をおこなうことができます。

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合、UI は表示されます **[!UICONTROL インストール]** 拡張機能用。 インストールワークフローを開始します ( [拡張機能のインストール](#install-extension) 拡張機能を設定または削除するには、以下を実行します。

拡張機能をアップグレードするには、 [拡張機能のアップグレードプロセス](../../../tags/ui/managing-resources/extensions/extension-upgrade.md) （タグドキュメント）を参照してください。
