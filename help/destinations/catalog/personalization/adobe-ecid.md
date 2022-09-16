---
Keywords: ECID;ecid
title: Experience Cloud ID サービス拡張機能
description: Experience CloudID サービス拡張は、Adobe Experience Platformのパーソナライゼーションの宛先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: 4cc49c14-66ec-43e0-a106-70d9c3646d87
source-git-commit: c8d6c156b3351324fe1be11144afeae91f7a2a59
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 14%

---

# [!DNL Experience Cloud] ID サービス拡張機能 {#adobe-ecid-extension}

## 概要 {#overview}

この拡張機能は、 [!DNL Experience Cloud] ID サービス：すべての [!DNL Experience Cloud] ソリューション

[!DNL Experience Cloud] ID サービスは、Adobe Experience Platformのパーソナライズ機能の拡張機能です。 拡張機能について詳しくは、 [Experience CloudID サービス拡張ページ](../../../tags/extensions/web/id-service/overview.md) （タグドキュメント）を参照してください。

この宛先はタグ拡張です。 Platform でのタグ拡張機能について詳しくは、 [タグ拡張機能の概要](../launch-extensions/overview.md).

![Adobe ECID 拡張機能](../../assets/catalog/personalization/adobe-ecid/catalog.png)

## 前提条件 {#prerequisites}

この拡張機能は、Platform を購入したすべての顧客が、宛先カタログで使用できます。

この拡張機能を使用するには、Platform のタグにアクセスする必要があります。 タグは、Adobe Experience Cloudのお客様に、付属の付加価値機能として提供されます。 組織の管理者に問い合わせてデータ収集 UI へのアクセス権を取得し、 **[!UICONTROL manage_properties]** 権限を持つユーザーが拡張機能をインストールできるようにする。

## 拡張機能のインストール {#install-extension}

をインストールするには、以下を実行します。 [!DNL Experience Cloud] ID サービス拡張機能：

内 [Platform インターフェイス](https://platform.adobe.com/)に移動します。 **[!UICONTROL 宛先]** > **[!UICONTROL カタログ]**.

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトし、「 」を選択します。 **[!UICONTROL 設定]** をクリックします。 この **[!UICONTROL 設定]** コントロールが灰色表示になっているので、次の項目はありません： **[!UICONTROL manage_properties]** 権限。 [前提条件](#prerequisites)を確認してください。

拡張機能をインストールするタグプロパティを選択します。 また、新しいプロパティを作成することもできます。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。のプロパティについて [タグドキュメント](../../../tags/ui/administration/companies-and-properties.md).

ワークフローに従ってデータ収集 UI が表示され、インストールが完了します。

拡張機能の設定オプションとインストールのサポートについて詳しくは、 [Experience CloudID サービス拡張ページ](../../../tags/extensions/web/id-service/overview.md) （タグドキュメント）を参照してください。

拡張機能は、 [データ収集 UI](https://experience.adobe.com/#/data-collection/). 詳しくは、 [新しい拡張機能の追加](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension) を参照してください。

## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、ルールの設定を開始できます。 データ収集 UI では、特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストール済みの拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、「 [ルール](../../../tags/ui/managing-resources/rules.md) （タグドキュメント）を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

データ収集 UI で、拡張機能の設定、アップグレード、削除をおこなうことができます。

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合、UI は表示されます **[!UICONTROL インストール]** 拡張機能用。 インストールワークフローを開始します ( [拡張機能のインストール](#install-extension) 拡張機能を設定または削除するには、以下を実行します。

拡張機能をアップグレードするには、 [拡張機能のアップグレードプロセス](../../../tags/ui/managing-resources/extensions/extension-upgrade.md) （タグドキュメント）を参照してください。
