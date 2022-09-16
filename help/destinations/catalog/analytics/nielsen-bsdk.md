---
keywords: Nielsen BSDK;nielsen bsdk;nielsen BSDK
title: Nielsen BSDK 拡張機能
description: Nielsen BSDK 拡張機能は、Adobe Experience Platformの分析の宛先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: e1c10673-e3f4-474d-98d7-960124b2bfc7
source-git-commit: b4e869f9bc29122db4fc66ccda752a50c7db729f
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 34%

---

# [!DNL Nielsen BSDK] 拡張機能 {#nielsen-bsdk-extension}

## 概要 {#overview}

[!DNL Nielsen Digital SDK] tag 拡張機能は、次のデジタル測定製品を介して audience measurement を提供します。

DCR：広告を含む非線形デジタルコンテンツの日々の測定を提供する測定ソリューションは、デスクトップ、モバイル、タブレット、および接続されたデバイスにわたるデジタルコンテンツオーディエンスの消費を包括的に表示します。

DTVR：これは、参加しているプログラミングソースのデスクトップおよびモバイルデバイスで発生する線形 TV 視聴を考慮したものです。これは、MRC から認定を受けた最初のソリューションで、コンピューターや携帯端末での視聴プログラミングに対する TV オーディエンス測定への貢献度を示します。

[!DNL Nielsen BSDK] は、Adobe Experience Platformの analytics 拡張機能です。 拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.101361.html) の拡張機能のページを参照してください。

この宛先はタグ拡張です。 Platform でのタグ拡張の仕組みについて詳しくは、 [タグ拡張機能の概要](../launch-extensions/overview.md).

![Nielsen BSDK 拡張機能](../../assets/catalog/analytics/nielsen-bsdk/catalog.png)

## 前提条件 {#prerequisites}

この拡張機能は、 [!DNL Destinations] Platform を購入したすべての顧客のカタログ。

この拡張機能を使用するには、Adobe Experience Platformのタグにアクセスする必要があります。 タグは、Adobe Experience Cloudのお客様に、付属の付加価値機能として提供されます。 組織の管理者に問い合わせてタグへのアクセス権を取得し、 **[!UICONTROL manage_properties]** 権限を持つユーザーが拡張機能をインストールできるようにする。

## 拡張機能のインストール {#install-extension}

をインストールするには、以下を実行します。 [!DNL Nielsen BSDK] 拡張子：

内 [Platform インターフェイス](https://platform.adobe.com/)に移動します。 **[!UICONTROL 宛先]** > **[!UICONTROL カタログ]**.

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトし、「 」を選択します。 **[!UICONTROL 設定]** をクリックします。 この **[!UICONTROL 設定]** コントロールが灰色表示になっているので、次の項目はありません： **[!UICONTROL manage_properties]** 権限。 [前提条件](#prerequisites)を確認してください。

拡張機能をインストールするプロパティを選択します。 また、新しいプロパティを作成することもできます。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。のプロパティについて [プロパティページセクション](../../../tags/ui/administration/companies-and-properties.md#properties-page) 」の「 」を参照してください。

このワークフローでは、インストールを完了する手順について説明します。

拡張機能の設定オプションとインストールのサポートについて詳しくは、[Adobe Exchange の Nielsen BSDK のページ](https://exchange.adobe.com/experiencecloud.details.101361.html)を参照してください。

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
