---
keywords: livefyre;livefyre 拡張機能
title: Adobe Livefyre 拡張機能
description: Adobe Livefyre 拡張機能は、Adobe Experience Platformのソーシャル宛先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: a134c144-e7b8-4d48-8c90-5999e5ceb8a0
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 55%

---

# Adobe Livefyre 拡張機能 {#adobe-livefyre-extension}

## 概要 {#overview}

Adobe Livefyre を使用すれば、一定量のユーザー作成コンテンツを発見、整理し、Webサイトに公開して、高度にパーソナライズされた本物のエクスペリエンスを作成することができます。

Adobe Livefyre は、Adobe Experience Platformのソーシャル拡張機能です。 Adobe Livefyre について詳しくは、[Livefyre 実装ガイド ](https://experienceleague.adobe.com/docs/livefyre/implementation/home.html?lang=ja) を参照してください。

この宛先はタグ拡張機能です。 Experience Platformでのタグ拡張機能の仕組みについて詳しくは、[ タグ拡張機能の概要 ](../launch-extensions/overview.md) を参照してください。

![Adobe Livefyre 拡張機能](../../assets/catalog/social/adobe-livefyre/catalog.png)

## 前提条件 {#prerequisites}

Experience Platformを購入したすべての顧客は、この拡張機能を [!DNL Destinations] カタログから利用できます。

この拡張機能を使用するには、Adobe Experience Platform でタグにアクセスする必要があります。 タグは、標準装備の付加価値機能として Adobe Experience Cloud の顧客に提供されます。組織の管理者に問い合わせてタグへのアクセス権を取得し、拡張機能をインストールできるよう **[!UICONTROL manage_properties]** 権限の付与を依頼します。

## 拡張機能のインストール {#install-extension}

Adobe Livefyre 拡張機能をインストールするには：

[Experience Platform インターフェイス ](https://platform.adobe.com/) で、**[!UICONTROL Destinations]**/**[!UICONTROL Catalog]** に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトしてから、右側のパネルで「**[!UICONTROL 設定]**」を選択します。**[!UICONTROL 設定]**&#x200B;コントロールがグレー表示になっている場合は、**[!UICONTROL manage_properties]** 権限がありません。[前提条件](#prerequisites)を確認してください。

拡張機能をインストールするタグプロパティを選択します。 また、新しいプロパティを作成するオプションもあります。プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについて詳しくは、[ タグのドキュメント ](../../../tags/ui/administration/companies-and-properties.md) を参照してください。

ワークフローにより、データ収集 UI に移動してインストールを完了します。

拡張機能は、[データ収集 UI](https://experience.adobe.com/#/data-collection/) で直接インストールできます。詳しくは、[ 新しい拡張機能の追加 ](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension) のガイドを参照してください。

## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、ルールの設定を開始できます。 データ収集 UI では、特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストール済みの拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、タグドキュメントの [ ルール ](../../../tags/ui/managing-resources/rules.md) の概要を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

データ収集 UI で、拡張機能の設定、アップグレードおよび削除を行うことができます。

>[!TIP]
>
>拡張機能がいずれかのプロパティに既にインストールされている場合、UI ではその拡張機能に引き続き **[!UICONTROL インストール]** が表示されます。 [拡張機能のインストール](#install-extension)の説明に従ってインストールワークフローを開始し、拡張機能を設定または削除します。

拡張機能をアップグレードするには、[拡張機能のアップグレードプロセス](../../../tags/ui/managing-resources/extensions/extension-upgrade.md) （タグドキュメント）のガイドを参照してください。
