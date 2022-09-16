---
keywords: メダリア；メダリア
title: Medallia 拡張機能
description: Medalia 拡張機能は、Adobe Experience Platformの顧客の声の宛先になります。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: c28cf31a-890c-4dcd-b936-5a030dc6e83e
source-git-commit: b4e869f9bc29122db4fc66ccda752a50c7db729f
workflow-type: tm+mt
source-wordcount: '480'
ht-degree: 21%

---

# [!DNL Medallia] 拡張機能 {#medallia-extension}

## 概要 {#overview}

デプロイ [!DNL Medallia] Web プロパティをすばやくシームレスに操作できます。 また、拡張機能を使用すると、調査イベントを検出し、データ要素を通じてリアルタイムで顧客のフィードバックをキャプチャし、ルールで使用して顧客のエクスペリエンスをパーソナライズし、Adobe Analyticsとデータを共有できます。

[!DNL Medallia] は、Adobe Experience Platformの顧客の声拡張機能です。 拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.103279.medallia-for-adobe-launch.html) の拡張機能のページを参照してください。

この宛先はタグ拡張です。 Platform でのタグ拡張の仕組みについて詳しくは、 [タグ拡張機能の概要](../launch-extensions/overview.md).

![Medallia 拡張機能 ](../../assets/catalog/voice/medallia/catalog.png)

## 前提条件 {#prerequisites}

この拡張機能は、 [!DNL Destinations] Platform を購入したすべての顧客のカタログ。

この拡張機能を使用するには、Adobe Experience Platformのタグにアクセスする必要があります。 タグは、Adobe Experience Cloudのお客様に、付属の付加価値機能として提供されます。 組織の管理者に問い合わせてタグへのアクセス権を取得し、 **[!UICONTROL manage_properties]** 権限を持つユーザーが拡張機能をインストールできるようにする。

## 拡張機能のインストール {#install-extension}

をインストールするには、以下を実行します。 [!DNL Medallia] 拡張子：

内 [Platform インターフェイス](https://platform.adobe.com/)に移動します。 **[!UICONTROL 宛先]** > **[!UICONTROL カタログ]**.

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトし、「 」を選択します。 **[!UICONTROL 設定]** をクリックします。 この **[!UICONTROL 設定]** コントロールが灰色表示になっているので、次の項目はありません： **[!UICONTROL manage_properties]** 権限。 [前提条件](#prerequisites)を確認してください。

拡張機能をインストールするプロパティを選択します。 また、新しいプロパティを作成することもできます。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。のプロパティについて [プロパティページセクション](../../../tags/ui/administration/companies-and-properties.md#properties-page) 」の「 」を参照してください。

このワークフローでは、インストールを完了する手順について説明します。

この拡張機能の設定オプションとインストールのサポートについて詳しくは、[Adobe Exchange の Medallia ページ](https://exchange.adobe.com/experiencecloud.details.103279.medallia-for-adobe-launch.html)を参照してください。

拡張機能は、 [データ収集 UI](https://experience.adobe.com/#/data-collection/). 詳しくは、 [新しい拡張機能の追加](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension) （タグドキュメント）を参照してください。

## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、ルールの設定を開始できます。

特定の状況でのみ拡張機能の宛先にイベントデータを送信するよう、インストールした拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、 [タグドキュメント](../../../tags/ui/managing-resources/rules.md).

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

データ収集 UI で、拡張機能の設定、アップグレード、削除をおこなうことができます。

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合、Platform UI は表示されます **[!UICONTROL インストール]** 拡張機能用。 インストールワークフローを開始します ( [拡張機能のインストール](#install-extension) 拡張機能を設定または削除するには、以下を実行します。

拡張機能をアップグレードするには、 [拡張機能のアップグレードプロセス](../../../tags/ui/managing-resources/extensions/extension-upgrade.md) （タグドキュメント）を参照してください。
