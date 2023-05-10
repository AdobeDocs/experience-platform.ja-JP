---
keywords: Analytics 拡張機能， Analytics 拡張機能，宛先 Analytics
title: Adobe Analytics 拡張機能
description: Adobe Analytics拡張機能は、Adobe Experience Platformの分析の宛先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: 95b6e079-09a6-4262-bd01-11f155286aa9
source-git-commit: 88939d674c0002590939004e0235d3da8b072118
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 54%

---

# Adobe Analytics 拡張機能

## 概要 {#overview}

Adobe Analytics は、顧客像を把握し、顧客インテリジェンスを活用してビジネスを導く力をユーザーに提供する、業界最先端のソリューションです。

Adobe Analyticsは、Adobe Experience Platformの analytics 拡張機能です。 拡張機能について詳しくは、 [Adobe Analytics拡張機能の概要](/help/tags/extensions/client/analytics/overview.md) （タグドキュメント）を参照してください。

この宛先はタグ拡張機能です。 Platform でのタグ拡張機能について詳しくは、 [タグ拡張機能の概要](../launch-extensions/overview.md).

![Adobe Analytics 拡張機能](../../assets/catalog/analytics/adobe-analytics/catalog.png)

## 前提条件 {#prerequisites}

この拡張機能は、Platform を購入したすべての顧客が、宛先カタログで使用できます。

この拡張機能を使用するには、 Platform でタグにアクセスする必要があります。 タグは、標準装備の付加価値機能として Adobe Experience Cloud の顧客に提供されます。組織の管理者に問い合わせてデータ収集 UI へのアクセス権を取得し、 **[!UICONTROL manage_properties]** 権限を持つユーザーが拡張機能をインストールできるようにする。

## 拡張機能のインストール {#install-extension}

Adobe Analytics 拡張機能をインストールするには、以下をおこないます。

[Platform インターフェイス](https://platform.adobe.com/)で、**[!UICONTROL 宛先]**／**[!UICONTROL カタログ]**&#x200B;に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトしてから、右側のパネルで「**[!UICONTROL 設定]**」を選択します。**[!UICONTROL 設定]**&#x200B;コントロールがグレー表示になっている場合は、**[!UICONTROL manage_properties]** 権限がありません。[前提条件](#prerequisites)を確認してください。

拡張機能をインストールするタグプロパティを選択します。 また、新しいプロパティを作成するオプションもあります。プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。のプロパティについて [タグドキュメント](../../../tags/ui/administration/companies-and-properties.md).

ワークフローに従ってデータ収集 UI が表示され、インストールが完了します。

拡張機能の設定オプションについて詳しくは、 [Adobe Analytics拡張機能ページ](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/implement-solutions/analytics.html) タグに関するドキュメント

拡張機能は、[データ収集 UI](https://experience.adobe.com/#/data-collection/) で直接インストールできます。詳しくは、 [新しい拡張機能の追加](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension) を参照してください。

## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、ルールの設定を開始できます。データ収集 UI では、特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストール済みの拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、「 [ルール](../../../tags/ui/managing-resources/rules.md) （タグドキュメント）を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

データ収集 UI で、拡張機能の設定、アップグレードおよび削除を行うことができます。

>[!TIP]
>
>拡張機能がいずれかのプロパティに既にインストールされている場合、 UI ではその拡張機能に引き続き「**[!UICONTROL インストール]**」が表示されます。[拡張機能のインストール](#install-extension)の説明に従ってインストールワークフローを開始し、拡張機能を設定または削除します。

拡張機能をアップグレードするには、[拡張機能のアップグレードプロセス](../../../tags/ui/managing-resources/extensions/extension-upgrade.md) （タグドキュメント）のガイドを参照してください。
