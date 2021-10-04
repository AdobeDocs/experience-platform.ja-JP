---
keywords: Audience ManagerDIL拡張機能；宛先 audience manager;dil 拡張機能
title: Audience Manager DIL 拡張機能
description: Audience ManagerDIL拡張機能は、Adobe Experience Platformのデータ管理プラットフォーム (DMP) の宛先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: 7e1099de-0650-4ee2-b746-721afe194097
source-git-commit: c8d6c156b3351324fe1be11144afeae91f7a2a59
workflow-type: tm+mt
source-wordcount: '514'
ht-degree: 21%

---

# Audience Manager DIL 拡張機能 {#aam-dil-extension}

## 概要 {#overview}

これは、Adobe Audience Manager データ統合ライブラリ拡張機能（クライアントサイド実装）です。メモ：この拡張機能は、Adobe Analytics データのサーバーサイド転送（SSF）で使用するためのものではありません。SSF の場合は、Adobe Analytics 拡張機能を使用します。重要：バージョン 8.0 以降、DILは [!DNL Experience Cloud] ID サービスのバージョン 3.3 以降に強く依存します。 [!DNL Audience Manager] データ統合機能を完全に利用するには、[!DNL Experience Cloud] ID サービスとDILの両方を実装してください。

[!DNL Audience Manager] DILは、Adobe Experience Platformのデータ管理プラットフォーム (DMP) 拡張機能です。拡張機能について詳しくは、タグのドキュメントの [Audience Manager拡張のページ ](../../../tags/extensions/web/audience-manager/overview.md) を参照してください。

この宛先はタグ拡張です。 Platform での拡張機能の仕組みについて詳しくは、「[ タグ拡張機能の概要 ](../launch-extensions/overview.md)」を参照してください。

![Audience Manager DIL 拡張機能](../../assets/catalog/data-management-platform/aam-dil-extension/configure.png)

## 前提条件 {#prerequisites}

この拡張機能は、Platform を購入したすべてのお客様の [!DNL Destinations] カタログで利用できます。

この拡張機能を使用するには、Adobe Experience Platformのタグにアクセスする必要があります。 タグは、Adobe Experience Cloudのお客様に、付属の付加価値機能として提供されます。 組織の管理者に問い合わせてタグへのアクセス権を取得し、タグに拡張機能のインストールに必要な **[!UICONTROL manage_properties]** 権限を付与するよう依頼します。

## 拡張機能のインストール {#install-extension}

[!DNL Audience Manager] 拡張機能をインストールするには、次のDILを使用します。

[Platform インターフェイス ](https://platform.adobe.com/) で、**[!UICONTROL 宛先]** / **[!UICONTROL カタログ]** に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトし、右側のレールで「**[!UICONTROL 設定]**」を選択します。 **[!UICONTROL Configure]** コントロールがグレー表示になっている場合は、**[!UICONTROL manage_properties]** 権限がありません。 [前提条件](#prerequisites)を確認してください。

拡張機能をインストールするプロパティを選択します。 また、新しいプロパティを作成することもできます。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。[ タグのドキュメント ](../../../tags/ui/administration/companies-and-properties.md#properties-page) でプロパティについて説明します。

このワークフローでは、インストールを完了する手順を順を追って説明します。

拡張機能の設定オプションについて詳しくは、タグドキュメントの [Audience Manager拡張のページ ](../../../tags/extensions/web/audience-manager/overview.md) を参照してください。

拡張機能は、[ データ収集 UI](https://experience.adobe.com/#/data-collection/) に直接インストールすることもできます。 詳しくは、[ 新しい拡張機能の追加 ](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension) のガイドを参照してください。

## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、ルールの設定を開始できます。 データ収集 UI では、特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストール済みの拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、タグのドキュメントの [rules](../../../tags/ui/managing-resources/rules.md) の概要を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

データ収集 UI で、拡張機能の設定、アップグレード、削除をおこなうことができます。

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合、UI には拡張機能の **[!UICONTROL Install]** が表示されます。 「[ 拡張機能のインストール ](#install-extension)」の説明に従って、インストールワークフローを開始し、拡張機能を設定または削除します。

拡張機能をアップグレードするには、タグのドキュメントの [ 拡張機能のアップグレードプロセス ](../../../tags/ui/managing-resources/extensions/extension-upgrade.md) のガイドを参照してください。
