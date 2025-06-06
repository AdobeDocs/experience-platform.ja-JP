---
keywords: Awin Advertiser mastertag 拡張機能；mastertag;Awin;awin;AWIN
title: Awin Advertiser Mastertag 拡張機能
description: Awin Advertiser Mastertag 拡張機能は、Adobe Experience Platformの広告先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: 99a9ea40-b89f-4503-91a7-60cc8e1cd6d3
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 53%

---

# [!DNL Awin Advertiser Mastertag] 拡張機能 {#awin-mastertag-extension}

## 概要 {#overview}

[!DNL MasterTag] は、Awin トラッキングソリューションに必要なすべての機能を含むJavaScript ライブラリであり、確認ページを含むサイト上のすべてのページに無条件で追加する必要があります。ただし、支払い情報を表示または処理するページは除きます。

[!DNL Awin Advertiser Mastertag] は、Adobe Experience Platformの広告拡張機能です。 拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.103176.awin-advertiser-mastertag.html) の拡張機能のページを参照してください。

この宛先はタグ拡張機能です。 Experience Platformでの拡張機能の仕組みについて詳しくは、[ タグ拡張機能の概要 ](../launch-extensions/overview.md) を参照してください。

![UI での Awin Advertiser Mastertag 拡張機能](../../assets/catalog/advertising/awin-mastertag/catalog.png)

## 前提条件 {#prerequisites}

Experience Platformを購入したすべての顧客は、この拡張機能を [!DNL Destinations] カタログから利用できます。

この拡張機能を使用するには、Adobe Experience Platform でタグにアクセスする必要があります。 タグは、標準装備の付加価値機能として Adobe Experience Cloud の顧客に提供されます。組織の管理者に問い合わせてタグへのアクセス権を取得し、拡張機能をインストールできるよう **[!UICONTROL manage_properties]** 権限の付与を依頼します。

## 拡張機能のインストール {#install-extension}

[!DNL Awin Advertiser Mastertag] 拡張機能をインストールします。

[Experience Platform インターフェイス ](https://platform.adobe.com/) で、**[!UICONTROL Destinations]**/**[!UICONTROL Catalog]** に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトしてから、右側のパネルで「**[!UICONTROL 設定]**」を選択します。**[!UICONTROL 設定]**&#x200B;コントロールがグレー表示になっている場合は、**[!UICONTROL manage_properties]** 権限がありません。[前提条件](#prerequisites)を確認してください。

拡張機能をインストールするタグプロパティを選択します。 また、新しいプロパティを作成するオプションもあります。プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについて詳しくは、[ タグのドキュメント ](../../../tags/ui/administration/companies-and-properties.md) を参照してください。

ワークフローにより、データ収集 UI に移動してインストールを完了します。

この拡張機能の設定オプションとインストールのサポートについて詳しくは、[Adobe Exchange の Awin Advertiser Mastertag ページ](https://exchange.adobe.com/experiencecloud.details.103176.awin-advertiser-mastertag.html)を参照してください。

拡張機能は、[データ収集 UI](https://experience.adobe.com/#/data-collection/) で直接インストールできます。詳しくは、[ 新しい拡張機能の追加 ](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension) のガイドを参照してください。


## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、ルールの設定を開始できます。 データ収集 UI では、特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストール済みの拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、タグドキュメントの [ ルール ](../../../tags/ui/managing-resources/rules.md) の概要を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

データ収集 UI で、拡張機能の設定、アップグレードおよび削除を行うことができます。

>[!TIP]
>
>拡張機能がいずれかのプロパティに既にインストールされている場合、UI ではその拡張機能に引き続き **[!UICONTROL インストール]** が表示されます。 [拡張機能のインストール](#install-extension)の説明に従ってインストールワークフローを開始し、拡張機能を設定または削除します。

拡張機能をアップグレードするには、[拡張機能のアップグレードプロセス](../../../tags/ui/managing-resources/extensions/extension-upgrade.md) （タグドキュメント）のガイドを参照してください。
