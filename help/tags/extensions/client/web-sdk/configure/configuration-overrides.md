---
title: データストリーム設定の上書き設定
description: 特定の条件が満たされた場合に設定を変更します。
source-git-commit: 46e5d007b27eaa67c9ee49e35a711424de383d68
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 3%

---

# データストリーム設定の上書き設定

データストリームの上書きにより、データストリームの追加設定を定義できます。この設定は、web SDKを介してEdge Networkに渡されます。 この機能を使用すると、新しいデータストリームを作成したり、既存のトリガーを変更したりすることなく、様々なデータストリームの動作を条件付きで設定できます。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Extensions]** に移動し、**[!UICONTROL Configure]** カードで [!UICONTROL Adobe Experience Platform Web SDK] を選択します。
1. **[!UICONTROL Datastream configuration overrides]** セクションまで下にスクロールします。

データストリーム設定の上書きは、次の 2 つの手順で構成されます。

1. まず、データストリーム UI で [ データストリームの設定 ](/help/datastreams/configure.md) を行う際に、データストリーム設定の上書きを定義する必要があります。 上書きの設定方法の手順については、データストリームに関するドキュメントの [ データストリーム設定の上書き ](/help/datastreams/overrides.md) を参照してください。
1. データストリーム UI でデータストリームの上書きを設定したら、タグ拡張機能を設定できます。

データストリームの上書きは、環境ごとに設定する必要があります。 開発環境、ステージング環境および実稼動環境では、すべて別々のオーバーライドが行われます。 上書き設定は、任意の環境間でコピーできます。

![Web SDK タグ拡張機能ページを使用したデータストリーム設定の上書きを示す画像。](../assets/datastream-overrides.png)

デフォルトでは、データストリーム設定の上書きは無効になっています。 デフォルトでは、「**[!UICONTROL Match datastream configuration]**」オプションが選択されています。

![ データストリーム設定がデフォルト設定を上書きすることを示す web SDK タグ拡張機能ユーザーインターフェイス ](../assets/datastream-override-default.png)

タグ拡張機能でデータストリームの上書きを有効にするには、ドロップダウンメニューから「**[!UICONTROL Enabled]**」を選択します。

![ データストリーム設定の上書きの有効設定を示す web SDK タグ拡張機能ユーザーインターフェイス ](../assets/datastream-override-enabled.png)

データストリーム設定の上書きを有効にすると、以下に説明する各サービスの上書きを設定できます。 これらのデータストリーム上書き設定は、選択した環境のサーバーサイドのデータストリーム設定とルールを上書きします。

## Adobe Analytics

Adobe Analytics サービスへのデータルーティングを上書きします。

![Adobe Analytics データストリーム上書き設定を示す web SDK タグ拡張機能の UI 画像。](../assets/datastream-override-analytics.png)

* **[!UICONTROL Enabled]** / **[!UICONTROL Disabled]**:Adobe Analyticsへのデータルーティングを有効または無効にします。
* **[!UICONTROL Report suites]**:Adobe Analyticsの宛先レポートスイートの ID。 値は、データストリーム設定から事前設定された上書きレポートスイート（またはレポートスイートのコンマ区切りリスト）である必要があります。 この設定は、プライマリレポートスイートを上書きします。
* **[!UICONTROL Add Report Suite]**：追加のレポートスイートを追加するには、このオプションを選択します。

## Adobe Audience Manager

Adobe Audience Managerへのデータルーティングを上書きします。

![Adobe Audience Manager データストリーム上書き設定を示す web SDK タグ拡張機能の UI 画像。](../assets/datastream-override-audience-manager.png)

* **[!UICONTROL Enabled]** / **[!UICONTROL Disabled]**:Adobe Audience Managerへのデータルーティングを有効または無効にします。
* **[!UICONTROL Third-party ID sync container]**:Audience Managerの宛先サードパーティ ID 同期コンテナの ID。 この値は、データストリーム設定から事前設定されたセカンダリコンテナである必要があり、プライマリコンテナを上書きします。

## Adobe Experience Platform

Adobe Experience Platformへのデータルーティングを上書きします。

![Adobe Experience Platform データストリーム上書き設定を示す web SDK タグ拡張機能の UI 画像。](../assets/datastream-override-experience-platform.png)

* **[!UICONTROL Enabled]** / **[!UICONTROL Disabled]**:Adobe Experience Platformへのデータルーティングを有効または無効にします。
* **[!UICONTROL Event dataset]**:Adobe Experience Platformの宛先イベントデータセットの ID。 値は、データストリーム設定の事前設定済みセカンダリデータセットである必要があります。
* **[!UICONTROL Offer Decisioning]**:[!DNL Offer Decisioning] サービスへのデータルーティングを有効または無効にします。
* **[!UICONTROL Edge Segmentation]**:[!DNL Edge Segmentation] サービスへのデータルーティングを有効または無効にします。
* **[!UICONTROL Personalization Destinations]**：パーソナライゼーションの宛先へのデータルーティングを有効または無効にします。
* **[!UICONTROL Adobe Journey Optimizer]**: [!DNL Adobe Journey Optimizer] へのデータルーティングを有効または無効にします。

## Adobe サーバーサイドイベント転送

Adobe サーバーサイドイベント転送サービスへのデータルーティングを上書きします。

![Adobe サーバーサイドイベント転送データストリームの上書き設定を示す Web SDK タグ拡張機能の UI 画像。](../assets/datastream-override-ssf.png)

* **[!UICONTROL Enabled]** / **[!UICONTROL Disabled]**:Adobe サーバーサイドイベント転送サービスへのデータルーティングを有効または無効にします。

## Adobe Target {#target}

Adobe Targetへのデータルーティングを上書きします。

![Adobe Target データストリーム上書き設定を示す web SDK タグ拡張機能の UI 画像。](../assets/datastream-override-target.png)

* **[!UICONTROL Enabled]** / **[!UICONTROL Disabled]**:Adobe Targetへのデータルーティングを有効または無効にします。
