---
keywords: ターゲット拡張機能；ターゲット；ターゲット v2；ターゲット v2拡張機能
title: Adobe Target v2 拡張機能
description: Adobe Target v2拡張機能は、Adobe Experience Platformのパーソナライゼーションの宛先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: d1d5ebbc-9093-42b0-8d88-58779df3ec89
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '455'
ht-degree: 38%

---

# [!DNL Adobe Target] v2拡張機能 {#adobe-target-v2-extension}

## 概要 {#overview}

[!DNL Adobe Target]は、web サイト、モバイルサイト、アプリ、ソーシャルメディア、その他のデジタルチャネルでの売上を最大化するために、顧客体験をカスタマイズおよびパーソナライズするために必要なすべてを提供する[!DNL Adobe Experience Cloud] ソリューションです。

[!DNL Adobe Target] v2は[!DNL Adobe Experience Platform]のパーソナライゼーション拡張機能です。 拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.102722.adobe-target-v2-launch-extension.html) の拡張機能のページを参照してください。

この宛先はタグ拡張機能です。 Experience Platformでのタグ拡張機能の仕組みについて詳しくは、[ タグ拡張機能の概要](../launch-extensions/overview.md)を参照してください。

![Adobe Target v2 拡張機能](../../assets/catalog/personalization/adobe-target-v2/catalog.png)

## 前提条件 {#prerequisites}

この拡張機能は、[!DNL Destinations] カタログで、Experience Platformを購入したすべてのお客様が利用できます。

この拡張機能を使用するには、[!DNL Adobe Experience Platform]のタグにアクセスする必要があります。 タグは、含まれている付加価値機能として[!DNL Adobe Experience Cloud]のお客様に提供されます。 タグへのアクセス権を取得するには、組織の管理者に連絡し、拡張機能をインストールできるように&#x200B;**[!UICONTROL manage_properties]**&#x200B;権限を付与するように依頼してください。

## 拡張機能のインストール {#install-extension}

[!DNL Adobe Target] v2拡張機能をインストールするには：

[Experience Platform インターフェイス ](https://platform.adobe.com/)で、**[!UICONTROL Destinations]** > **[!UICONTROL Catalog]**&#x200B;に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

宛先を選択し、右側のパネルで「**[!UICONTROL Configure]**」を選択します。 **[!UICONTROL Configure]** コントロールがグレー表示されている場合、**[!UICONTROL manage_properties]**&#x200B;権限がありません。 [前提条件](#prerequisites)を確認してください。

拡張機能をインストールするプロパティを選択します。 また、新しいプロパティを作成するオプションもあります。プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。詳しくは、タグ ドキュメントの[properties](../../../tags/ui/administration/companies-and-properties.md#properties-page)のドキュメントを参照してください。

このワークフローでは、インストールを完了する手順について説明します。

拡張機能の設定オプションについて詳しくは、タグドキュメントの[Adobe Target v2拡張機能ページ ](../../../tags/extensions/client/target-v2/overview.md)を参照してください。

拡張機能は、[データ収集 UI](https://experience.adobe.com/#/data-collection/) で直接インストールできます。詳しくは、[新しい拡張機能の追加](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension)に関するガイドを参照してください。


## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、ルールの設定を開始できます。 データ収集UIでは、特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストール済みの拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、タグのドキュメントの[ ルール ](../../../tags/ui/managing-resources/rules.md)の概要を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

データ収集 UI で、拡張機能の設定、アップグレードおよび削除を行うことができます。

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合でも、UIには拡張機能の&#x200B;**[!UICONTROL Install]**&#x200B;が表示されます。 [拡張機能のインストール](#install-extension)の説明に従ってインストールワークフローを開始し、拡張機能を設定または削除します。

拡張機能をアップグレードするには、[拡張機能のアップグレードプロセス](../../../tags/ui/managing-resources/extensions/extension-upgrade.md) （タグドキュメント）のガイドを参照してください。
