---
keywords: Advertising Cloud;Advertising Cloud 拡張機能；Advertising Cloud の宛先
title: Adobe Advertising Cloud 拡張機能
description: Adobe Advertising Cloud 拡張機能は、Adobe Experience Platformの広告の宛先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: 3415a85f-5678-4f5b-b7cf-e185a66d084f
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '441'
ht-degree: 42%

---

# Adobe Advertising Cloud 拡張機能 {#adobe-advertising-cloud-extension}

## 概要 {#overview}

これは、DSPと検索の両方に [!DNL Advertising Cloud] コンバージョンタグとオーディエンスタグを実装するための [!DNL Advertising Cloud] しい拡張機能です（現在、DCO はサポートされていません）。

Adobe Advertising Cloud は、Adobe Experience Platformの広告拡張機能です。

この宛先はタグ拡張機能です。 Experience Platformでのタグ拡張機能の仕組みについて詳しくは、[&#x200B; タグ拡張機能の概要 &#x200B;](../launch-extensions/overview.md) を参照してください。

![Adobe Advertising Cloud 拡張機能](../../assets/catalog/advertising/adobe-advertising-cloud/catalog.png)

## 前提条件 {#prerequisites}

Experience Platformを購入したすべての顧客は、この拡張機能を宛先カタログから使用できます。

この拡張機能を使用するには、Experience Platformのタグにアクセスする必要があります。 タグは、標準装備の付加価値機能として Adobe Experience Cloud の顧客に提供されます。組織の管理者に問い合わせて、UI のデータ収集機能へのアクセス権を取得し、拡張機能をインストールできるよう **[!UICONTROL manage_properties]** 権限の付与を依頼します。

## 拡張機能のインストール {#install-extension}

Adobe Advertising Cloud 拡張機能をインストールするには、以下を実行します。

[Experience Platform インターフェイス &#x200B;](https://platform.adobe.com/) で、**[!UICONTROL Destinations]**/**[!UICONTROL Catalog]** に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトしてから、右側のパネルで「**[!UICONTROL 設定]**」を選択します。**[!UICONTROL 設定]**&#x200B;コントロールがグレー表示になっている場合は、**[!UICONTROL manage_properties]** 権限がありません。[前提条件](#prerequisites)を確認してください。

拡張機能をインストールするタグプロパティを選択します。 また、新しいプロパティを作成するオプションもあります。プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについて詳しくは、[&#x200B; タグのドキュメント &#x200B;](../../../tags/ui/administration/companies-and-properties.md) を参照してください。

ワークフローにより、データ収集 UI に移動してインストールを完了します。

拡張機能は、[データ収集 UI](https://experience.adobe.com/#/data-collection/) で直接インストールできます。詳しくは、[&#x200B; 新しい拡張機能の追加 &#x200B;](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension) のガイドを参照してください。

## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、ルールの設定を開始できます。 データ収集 UI では、特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストール済みの拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、タグドキュメントの [&#x200B; ルール &#x200B;](../../../tags/ui/managing-resources/rules.md) の概要を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

データ収集 UI で、拡張機能の設定、アップグレードおよび削除を行うことができます。

>[!TIP]
>
>拡張機能がいずれかのプロパティに既にインストールされている場合、UI ではその拡張機能に引き続き **[!UICONTROL インストール]** が表示されます。 [拡張機能のインストール](#install-extension)の説明に従ってインストールワークフローを開始し、拡張機能を設定または削除します。

拡張機能をアップグレードするには、[拡張機能のアップグレードプロセス](../../../tags/ui/managing-resources/extensions/extension-upgrade.md) （タグドキュメント）のガイドを参照してください。
