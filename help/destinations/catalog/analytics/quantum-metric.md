---
keywords: Quantum Metric Extension;Quantum Metric;quantum metric;Quantum metric
title: Quantum Metric 拡張機能
seo-title: Quantum Metric 拡張機能
description: Quantum Metric拡張は、リアルタイム顧客データプラットフォームの分析先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
seo-description: Quantum Metric拡張は、リアルタイム顧客データプラットフォームの分析先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
translation-type: tm+mt
source-git-commit: 80db19822551883da272787affb6f7dc9dc3a745
workflow-type: tm+mt
source-wordcount: '563'
ht-degree: 26%

---


# [!DNL Quantum Metric] 拡張子 {#quantum-metric-extension}

## 概要 {#overview}

[!DNL Quantum Metric's] adobe開始との統合により、データ収集タグのコードレス導入が容易になり [!DNL Quantum Metric's] ます。 In addition, this extension offers the ability to capture Launch Data Elements containing useful information from the [!DNL Quantum Metric] API.

[!DNL Quantum Metric] は、Real-time Customer Data Platformのanalytics拡張です。 拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.101535.quantum-metric-extension-for-adobe-launch.html) の拡張機能のページを参照してください。

この目的地はAdobe Experience Platform Launchの延長線です。 For more information about how Platform Launch extensions work in Real-time CDP, see [Adobe Experience Platform Launch extensions overview](../launch-extensions/overview.md).

![Quantum Metric 拡張機能 ](../../assets/catalog/analytics/quantum-metric/catalog.png)

## 前提条件 {#prerequisites}

This extension is available in the [!DNL Destinations] catalog for all customers who have purchased Real-time CDP.

この拡張機能を使用するには、Adobe Experience Platform Launchにアクセスする必要があります。  Platform Launch は、Adobe Experience Cloud に付属の付加価値機能として提供されています。Contact your organization administrator to get access to Platform Launch and ask them to grant you the **[!UICONTROL manage_properties]** permission so you can install extensions.

## 拡張機能のインストール {#install-extension}

拡張機能をインストールするに [!DNL Quantum Metric] は：

In the [Real-time CDP interface](http://platform.adobe.com/), go to **[!UICONTROL Destinations]** > **[!UICONTROL Catalog]**.

カタログから拡張機能を選択するか、検索バーを使用します。

Click on the destination to highlight it, then select **[!UICONTROL Configure]** in the right rail. **[!UICONTROL Configure]** コントロールが灰色表示になっている場合は、 **[!UICONTROL manage_properties]** 権限がありません。 [前提条件](#prerequisites)を確認してください。

In the **[!UICONTROL Select available Platform Launch property]** window, select the Platform Launch property in which you want to install the extension. また、「プラットフォームの起動」で新しいプロパティを作成するオプションもあります。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。Learn about properties in the [Properties page section](https://experienceleague.adobe.com/docs/launch/using/reference/admin/companies-and-properties.html#properties-page) of the Platform Launch documentation.

このワークフローにより、プラットフォーム起動に移動してインストールを完了します。

この拡張機能の設定オプションとインストールのサポートについて詳しくは、[Adobe Exchange の Quantum Metric ページ](https://exchange.adobe.com/experiencecloud.details.101535.quantum-metric-extension-for-adobe-launch.html)を参照してください。

You can also install the extension directly in the [Adobe Experience Platform Launch interface](https://launch.adobe.com/). See [Add a new extension](https://experienceleague.adobe.com/docs/launch/using/reference/manage-resources/extensions/overview.html?lang=en#add-a-new-extension) in the Platform Launch documentation.

## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールすると、「プラットフォームの起動」で直接、拡張機能のルール設定を開始できます。

Platform Launchでは、特定の状況でのみイベントデータを拡張の宛先に送信するように、インストール済みの拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、[ルールに関するドキュメント](https://experienceleague.adobe.com/docs/launch/using/reference/manage-resources/rules.html)を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

プラットフォーム起動インターフェイスで、拡張機能の設定、アップグレードおよび削除ができます。

>[!TIP]
>
>If the extension is already installed on one of your properties, the Real-time CDP UI still displays **[!UICONTROL Install]** for the extension. Kick off the installation workflow as described in [Install extension](#install-extension) to get to Platform Launch and configure or delete your extension.

To upgrade your extension, see [Extension upgrade](https://experienceleague.adobe.com/docs/launch/using/reference/manage-resources/extensions/extension-upgrade.html) in the Platform Launch documentation.