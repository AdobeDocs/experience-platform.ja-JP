---
keywords: DoubleClick Floodlight;doubleclick floodlight 拡張機能；doubleclick;floodlight
title: DoubleClick Floodlight（ベータ版）拡張機能
description: DoubleClick Floodlight（ベータ版）拡張機能は、Adobe Experience Platformの広告先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: 64e1f964-a58e-43d2-8b1a-3baa6104ab3a
source-git-commit: b4e869f9bc29122db4fc66ccda752a50c7db729f
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 18%

---

# [!DNL DoubleClick Floodlight] （ベータ版）拡張機能

## 概要 {#overview}

この拡張機能を使用すると、の迅速かつ容易なデプロイメントが可能になります。 [!DNL DoubleClick Floodlight] （グローバルサイトタグではなく）従来の floodlight 形式を使用するタグ。 注意：この拡張機能はベータ版です。

[!DNL DoubleClick Floodlight] （ベータ版）は、Adobe Experience Platformの広告拡張機能です。 拡張機能について詳しくは、 [!DNL Google] のサポートドキュメント [DoubleClick Floodlight](https://support.google.com/dcm/answer/2823388?hl=ja).

この宛先はタグ拡張です。 Platform でのタグ拡張の仕組みについて詳しくは、 [タグ拡張機能の概要](../launch-extensions/overview.md).

![Doubleclick Floodlight 拡張機能](../../assets/catalog/advertising/doubleclick-floodlight/catalog.png)

## 前提条件 {#prerequisites}

この拡張機能は、 [!DNL Destinations] Platform を購入したすべての顧客のカタログ。

この拡張機能を使用するには、Adobe Experience Platformのタグにアクセスする必要があります。 タグは、Adobe Experience Cloudのお客様に、付属の付加価値機能として提供されます。 組織の管理者に問い合わせてタグへのアクセス権を取得し、 **[!UICONTROL manage_properties]** 権限を持つユーザーが拡張機能をインストールできるようにする。

## 拡張機能のインストール {#install-extension}

DoubleClick Floodlight（ベータ版）拡張機能をインストールするには、次の手順に従います。

内 [Platform インターフェイス](https://platform.adobe.com/)に移動します。 **[!UICONTROL 宛先]** > **[!UICONTROL カタログ]**.

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトし、「 」を選択します。 **[!UICONTROL 設定]** をクリックします。 この **[!UICONTROL 設定]** コントロールが灰色表示になっているので、次の項目はありません： **[!UICONTROL manage_properties]** 権限。 [前提条件](#prerequisites)を確認してください。

拡張機能をインストールするプロパティを選択します。 また、新しいプロパティを作成することもできます。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。のプロパティについて [プロパティページセクション](../../../tags/ui/administration/companies-and-properties.md#properties-page) 」の「 」を参照してください。

このワークフローでは、インストールを完了する手順について説明します。

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
