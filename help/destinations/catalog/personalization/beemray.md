---
keywords: beemray, beemray拡張機能
title: Beemray 拡張機能
description: Beemray拡張機能は、Adobe Experience Platformのパーソナライゼーションの宛先です。 拡張機能について詳しくは、Adobe Exchange の拡張機能のページを参照してください。
exl-id: 5bb639f5-42b5-48ae-a3e9-7585595ab925
source-git-commit: 12c3f440319046491054b3ef3ec404798bb61f06
workflow-type: tm+mt
source-wordcount: '484'
ht-degree: 45%

---

# [!DNL Beemray] 拡張機能 {#beemray-extension}

## 概要 {#overview}

[!DNL Beemray] は、状況に合ったコンテキストを使用して製品を加速するお手伝いをします。インサイトを得たり、新しいエクスペリエンスを構築したり、インタラクションを促進したり、本当に重要な瞬間を捉えたりすることが可能になります。Beemray は、機械学習を使用してコンテキストインテリジェンスを自動化します。Beemray は、Adobe Experience Cloud および他のテクニカルパートナーに接続します。すべてがリアルタイムで実行されます。 この拡張機能は、[!DNL Beemray] SDKをサイトにインストールします。

Beemrayは、Adobe Experience Platformのパーソナライズ機能の拡張機能です。 拡張機能について詳しくは、[Adobe Exchange](https://exchange.adobe.com/experiencecloud.details.101063.beemray-human-context.html) の拡張機能のページを参照してください。

この宛先は[!DNL Adobe Experience Platform Launch]拡張です。 Platformでの[!DNL Platform Launch]Experience Platform Launchの動作について詳しくは、「[拡張機能の概要](../launch-extensions/overview.md)」を参照してください。

![Beemray 拡張機能](../../assets/catalog/personalization/beemray/catalog.png)

## 前提条件 {#prerequisites}

この拡張機能は、Platformを購入したすべての顧客の[!DNL Destinations]カタログで利用できます。

この拡張機能を使用するには、[!DNL Adobe Experience Platform Launch]にアクセスする必要があります。 [!DNL Platform Launch] は、Adobe Experience Cloud に付属の付加価値機能として提供されています。組織の管理者に問い合わせて[!DNL Platform Launch]へのアクセス権を取得し、拡張機能のインストールに必要な&#x200B;**[!UICONTROL manage_properties]**&#x200B;権限を付与するよう管理者に依頼します。

## 拡張機能のインストール {#install-extension}

[!DNL Beemray]拡張機能をインストールするには：

[Platformインターフェイス](http://platform.adobe.com/)で、**[!UICONTROL 宛先]**/**[!UICONTROL カタログ]**&#x200B;に移動します。

カタログから拡張機能を選択するか、検索バーを使用します。

宛先をクリックしてハイライトし、右側のレールで「**[!UICONTROL 設定]**」を選択します。 **[!UICONTROL Configure]**&#x200B;コントロールがグレー表示になっている場合は、**[!UICONTROL manage_properties]**&#x200B;権限がありません。 [前提条件](#prerequisites)を確認してください。

**[!UICONTROL 使用可能なPlatform launchプロパティを選択]**&#x200B;ウィンドウで、拡張機能をインストールする[!DNL Platform Launch]プロパティを選択します。 また、[!DNL Platform Launch]に新しいプロパティを作成することもできます。 プロパティは、ルール、データ要素、設定された拡張機能、環境およびライブラリの集まりです。プロパティについては、 ドキュメントの[「プロパティ」ページに関する節](../../../tags/ui/administration/companies-and-properties.md#properties-page)を参照してください。[!DNL Launch]

ワークフローで[!DNL Platform Launch]が開き、インストールが完了します。

拡張機能の設定オプションとインストールのサポートについて詳しくは、[Adobe Exchange の Beemray ページ](https://exchange.adobe.com/experiencecloud.details.101063.beemray-human-context.html)を参照してください。

拡張機能は、[Adobe Experience Platform Launchインターフェイス](https://launch.adobe.com/)に直接インストールすることもできます。  ドキュメントの[新しい拡張機能の追加](../../../tags/ui/managing-resources/extensions/overview.md#add-a-new-extension)に関するページを参照してください。[!DNL Platform Launch]

## 拡張機能の使用方法 {#how-to-use}

拡張機能をインストールしたら、[!DNL Platform Launch]で直接拡張機能のルールを設定できます。

[!DNL Platform Launch]では、特定の状況でのみ拡張機能の宛先にイベントデータを送信するように、インストールした拡張機能のルールを設定できます。 拡張機能のルールの設定について詳しくは、[ルールに関するドキュメント](../../../tags/ui/managing-resources/rules.md)を参照してください。

## 拡張機能の設定、アップグレード、削除 {#configure-upgrade-delete}

[!DNL Platform Launch]インターフェイスで、拡張機能の設定、アップグレード、削除を行うことができます。

>[!TIP]
>
>拡張機能が既にいずれかのプロパティにインストールされている場合、Platform UIには、拡張機能に対して&#x200B;**[!UICONTROL Install]**&#x200B;が表示されます。 「[拡張機能のインストール](#install-extension)」の説明にしたがってインストールワークフローを開始し、 に移動して拡張機能を設定または削除します。[!DNL Platform Launch]

拡張機能をアップグレードするには、 ドキュメントの「[拡張機能のアップグレード](../../../tags/ui/managing-resources/extensions/extension-upgrade.md)」を参照してください。[!DNL Platform Launch]
