---
keywords: target 拡張機能；target;target v2;target v2 拡張機能
title: Adobe Target v2 拡張機能
description: Adobe Target v2 拡張機能は、Adobe Experience Platformのパーソナライゼーションの宛先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: d1d5ebbc-9093-42b0-8d88-58779df3ec89
source-git-commit: 88939d674c0002590939004e0235d3da8b072118
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 76%

---

# Adobe Target v2 拡張機能 {#adobe-target-v2-extension}

## 概要 {#overview}

Adobe Target は、顧客のエクスペリエンスのカスタマイズやパーソナライズをおこない、Web サイト、モバイルサイト、アプリ、ソーシャルメディア、その他のデジタルチャネルでの収益性を最大化するのに必要なすべてのツールを備えた Adobe Experience Cloud ソリューションです。

Adobe Target v2 は、Adobe Experience Platformのパーソナライゼーション拡張機能です。 拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.102722.adobe-target-v2-launch-extension.html) の拡張機能のページを参照してください。

この宛先はタグ拡張機能です。 Platform でのタグ拡張の仕組みについて詳しくは、[タグ拡張機能の概要](../launch-extensions/overview.md)を参照してください。

![Adobe Target v2 拡張機能](../../assets/catalog/personalization/adobe-target-v2/catalog.png)

## 前提条件 {#prerequisites}

プラットフォームを購入したすべての顧客は、この拡張機能を [!DNL Destinations] カタログから利用できます。

この拡張機能を使用するには、Adobe Experience Platform でタグにアクセスする必要があります。 タグは、標準装備の付加価値機能として Adobe Experience Cloud の顧客に提供されます。組織の管理者に問い合わせてタグへのアクセス権を取得し、拡張機能をインストールできるよう **[!UICONTROL manage_properties]** 権限の付与を依頼します。

## 拡張機能のインストール {#install-extension}

Adobe Target v2 拡張機能をインストールするには：

[Platform インターフェイス](https://platform.adobe.com/)で、**[!UICONTROL 宛先]**／**[!UICONTROL カタログ]**&#x200B;に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトしてから、右側のパネルで「**[!UICONTROL 設定]**」を選択します。**[!UICONTROL 設定]**&#x200B;コントロールがグレー表示になっている場合は、**[!UICONTROL manage_properties]** 権限がありません。[前提条件](#prerequisites)を確認してください。

拡張機能をインストールするプロパティを選択します。 また、新しいプロパティを作成するオプションもあります。プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。次のドキュメントを参照してください： [プロパティ](../../../tags/ui/administration/companies-and-properties.md#properties-page) 詳しくは、タグのドキュメントを参照してください。

このワークフローでは、インストールを完了する手順について説明します。

拡張機能の設定オプションについて詳しくは、 [Adobe Target v2 拡張機能ページ](../../../tags/extensions/client/target-v2/overview.md) （タグドキュメント）を参照してください。

拡張機能は、[データ収集 UI](https://experience.adobe.com/#/data-collection/) で直接インストールできます。詳しくは、 [新しい拡張機能の追加](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension) を参照してください。


## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、ルールの設定を開始できます。データ収集 UI では、特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストール済みの拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、「 [ルール](../../../tags/ui/managing-resources/rules.md) （タグドキュメント）を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

データ収集 UI で、拡張機能の設定、アップグレードおよび削除を行うことができます。

>[!TIP]
>
>拡張機能がいずれかのプロパティに既にインストールされている場合、 UI ではその拡張機能に引き続き「**[!UICONTROL インストール]**」が表示されます。[拡張機能のインストール](#install-extension)の説明に従ってインストールワークフローを開始し、拡張機能を設定または削除します。

拡張機能をアップグレードするには、[拡張機能のアップグレードプロセス](../../../tags/ui/managing-resources/extensions/extension-upgrade.md) （タグドキュメント）のガイドを参照してください。
