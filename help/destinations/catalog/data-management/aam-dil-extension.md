---
keywords: Audience Manager DIL拡張機能；destination audience manager;dil 拡張機能
title: Audience Manager DIL 拡張機能
description: Audience Manager DIL拡張機能は、Adobe Experience Platformの Data Management Platform （DMP）の宛先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: 7e1099de-0650-4ee2-b746-721afe194097
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '514'
ht-degree: 53%

---

# Audience Manager DIL 拡張機能 {#aam-dil-extension}

## 概要 {#overview}

これは、Adobe Audience Manager データ統合ライブラリ拡張機能（クライアントサイド実装）です。メモ：この拡張機能は、Adobe Analytics データのサーバーサイド転送（SSF）で使用するためのものではありません。SSF の場合は、Adobe Analytics 拡張機能を使用します。重要：バージョン 8.0 以降、DILは [!DNL Experience Cloud] ID サービス バージョン 3.3 以降に強く依存します。 すべての [!DNL Audience Manager] データ統合機能を使用するには、[!DNL Experience Cloud] ID サービスとDILの両方を実装してください。

DIL[!DNL Audience Manager]、Adobe Experience Platformの Data Management Platform （DMP）拡張機能です。 拡張機能について詳しくは、タグドキュメントの [Audience Manager拡張機能のページ &#x200B;](../../../tags/extensions/client/audience-manager/overview.md) を参照してください。

この宛先はタグ拡張機能です。 Experience Platformでの拡張機能の仕組みについて詳しくは、[&#x200B; タグ拡張機能の概要 &#x200B;](../launch-extensions/overview.md) を参照してください。

![Audience Manager DIL 拡張機能](../../assets/catalog/data-management-platform/aam-dil-extension/configure.png)

## 前提条件 {#prerequisites}

Experience Platformを購入したすべての顧客は、この拡張機能を [!DNL Destinations] カタログから利用できます。

この拡張機能を使用するには、Adobe Experience Platform でタグにアクセスする必要があります。 タグは、標準装備の付加価値機能として Adobe Experience Cloud の顧客に提供されます。組織の管理者に問い合わせてタグへのアクセス権を取得し、拡張機能をインストールできるよう **[!UICONTROL manage_properties]** 権限の付与を依頼します。

## 拡張機能のインストール {#install-extension}

[!DNL Audience Manager] DIL拡張機能をインストールするには：

[Experience Platform インターフェイス &#x200B;](https://platform.adobe.com/) で、**[!UICONTROL Destinations]**/**[!UICONTROL Catalog]** に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトしてから、右側のパネルで「**[!UICONTROL 設定]**」を選択します。**[!UICONTROL 設定]**&#x200B;コントロールがグレー表示になっている場合は、**[!UICONTROL manage_properties]** 権限がありません。[前提条件](#prerequisites)を確認してください。

拡張機能をインストールするプロパティを選択します。 また、新しいプロパティを作成するオプションもあります。プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについて詳しくは、[&#x200B; タグのドキュメント &#x200B;](../../../tags/ui/administration/companies-and-properties.md#properties-page) を参照してください。

このワークフローでは、インストールを完了する手順について説明します。

拡張機能の設定オプションについて詳しくは、タグドキュメントの [Audience Manager拡張機能ページ &#x200B;](../../../tags/extensions/client/audience-manager/overview.md) を参照してください。

拡張機能は、[データ収集 UI](https://experience.adobe.com/#/data-collection/) で直接インストールできます。詳しくは、[&#x200B; 新しい拡張機能の追加 &#x200B;](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension) のガイドを参照してください。

## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、ルールの設定を開始できます。 データ収集 UI では、特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストール済みの拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、タグドキュメントの [&#x200B; ルール &#x200B;](../../../tags/ui/managing-resources/rules.md) の概要を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

データ収集 UI で、拡張機能の設定、アップグレードおよび削除を行うことができます。

>[!TIP]
>
>拡張機能がいずれかのプロパティに既にインストールされている場合、UI ではその拡張機能に引き続き **[!UICONTROL インストール]** が表示されます。 [拡張機能のインストール](#install-extension)の説明に従ってインストールワークフローを開始し、拡張機能を設定または削除します。

拡張機能をアップグレードするには、[拡張機能のアップグレードプロセス](../../../tags/ui/managing-resources/extensions/extension-upgrade.md) （タグドキュメント）のガイドを参照してください。
