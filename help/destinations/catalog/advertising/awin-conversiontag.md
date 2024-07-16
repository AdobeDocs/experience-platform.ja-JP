---
keywords: Awin Advertiser Conversion Tag 拡張機能；コンバージョンタグ；Awin;awin;AWIN
title: Awin Advertiser Conversion Tag 拡張機能
description: Awin Advertiser Conversion Tag 拡張機能は、Adobe Experience Platformの広告先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: 99feb169-acf3-4e68-8785-3f4cf565e5a9
source-git-commit: 8ded2aed32dffa4f0923fedac7baf798e68a9ec9
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 54%

---

# Awin Advertiser Conversion Tag 拡張機能 {#awin-conversiontag-extension}

## 概要 {#overview}

コンバージョンタグは、AWIN.Tracking.Sale JavaScript オブジェクトの宣言です。この宣言は、確認ページで実行され、コンバージョンがおこなわれたことをマスタータグに指示します。その後、必要なトラッキングリクエストが実行されます。

Awin Advertiser Conversion Tag は、Adobe Experience Platformの広告拡張機能です。 拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.103240.awin-conversion-tag.html) の拡張機能のページを参照してください。

この宛先はタグ拡張機能です。 Platform でのタグ拡張機能の仕組みについて詳しくは、[ タグ拡張機能の概要 ](../launch-extensions/overview.md) を参照してください。

![UI の Awin Advertiser コンバージョン拡張](../../assets/catalog/advertising/awin-conversion-tag/catalog.png)

## 前提条件 {#prerequisites}

プラットフォームを購入したすべての顧客は、この拡張機能を宛先カタログから利用できます。

この拡張機能を使用するには、Platform のタグにアクセスする必要があります。 タグは、標準装備の付加価値機能として Adobe Experience Cloud の顧客に提供されます。組織の管理者に問い合わせて、UI のデータ収集機能へのアクセス権を取得し、拡張機能をインストールできるよう **[!UICONTROL manage_properties]** 権限の付与を依頼します。

## 拡張機能のインストール {#install-extension}

[!DNL Awin Advertiser Conversion Tag] 拡張機能をインストールします。

[Platform インターフェイス](https://platform.adobe.com/)で、**[!UICONTROL 宛先]**／**[!UICONTROL カタログ]**&#x200B;に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトしてから、右側のパネルで「**[!UICONTROL 設定]**」を選択します。**[!UICONTROL 設定]**&#x200B;コントロールがグレー表示になっている場合は、**[!UICONTROL manage_properties]** 権限がありません。[前提条件](#prerequisites)を確認してください。

拡張機能をインストールするタグプロパティを選択します。 また、新しいプロパティを作成するオプションもあります。プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについて詳しくは、[ タグのドキュメント ](../../../tags/ui/administration/companies-and-properties.md) を参照してください。

ワークフローにより、データ収集 UI に移動してインストールを完了します。

拡張機能の設定オプションとインストールのサポートについて詳しくは、「[Adobe Exchange の Awin Advertiser コンバージョンタグページ](https://exchange.adobe.com/experiencecloud.details.103240.awin-conversion-tag.html)」を参照してください。

拡張機能は、[データ収集 UI](https://experience.adobe.com/#/data-collection/) で直接インストールできます。詳しくは、[ 新しい拡張機能の追加 ](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension) のガイドを参照してください。


## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、ルールの設定を開始できます。 データ収集 UI では、特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストール済みの拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、タグドキュメントの [ ルール ](../../../tags/ui/managing-resources/rules.md) の概要を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

データ収集 UI で、拡張機能の設定、アップグレードおよび削除を行うことができます。

>[!TIP]
>
>拡張機能がいずれかのプロパティに既にインストールされている場合、UI ではその拡張機能に引き続き **[!UICONTROL インストール]** が表示されます。 [拡張機能のインストール](#install-extension)の説明に従ってインストールワークフローを開始し、拡張機能を設定または削除します。

拡張機能をアップグレードするには、[拡張機能のアップグレードプロセス](../../../tags/ui/managing-resources/extensions/extension-upgrade.md) （タグドキュメント）のガイドを参照してください。
