---
keywords: 宛先のアクティブ化；データのアクティブ化
title: 有効化の概要
type: Tutorial
description: 様々なタイプの宛先に対して、Adobe Experience Platformのオーディエンスをアクティブ化する方法を説明します。
exl-id: 987af401-2d93-45b4-a8f9-191e6058e4da
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 25%

---

# 有効化の概要

>[!IMPORTANT]
> 
>* データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* エクスポートする *id*、が必要です **[!UICONTROL ID グラフの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png "宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。"){width="100" zoomable="yes"}

Adobe Experience Platformは、幅広い宛先をサポートします。 Audience Activation のワークフローは、サポートするオーディエンスデータのタイプと、データ書き出しの頻度に基づいて、宛先によって異なります。

## アクティブ化メソッド {#activation-methods}

お先に [宛先の設定](connect-destination.md)オーディエンスは、次のような複数の方法でアクティブ化できます。

### 宛先カタログからオーディエンスをアクティブ化

宛先カタログから宛先に対するオーディエンスのアクティブ化について詳しくは、次のガイドを参照してください。

* [ストリーミングオーディエンスの書き出し先に対するオーディエンスデータの有効化](activate-segment-streaming-destinations.md)
* [ストリーミングプロファイル書き出し宛先に対するオーディエンスデータの有効化](activate-streaming-profile-destinations.md)
* [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](activate-batch-profile-destinations.md)

### からのオーディエンスのアクティブ化 [!UICONTROL 参照] ページ

以下の手順に従って、から宛先へのデータを有効化します **[!UICONTROL 参照]** ページ。

1. に移動 **[!UICONTROL 接続/宛先]**&#x200B;を選択し、 **[!UICONTROL 参照]** タブ。

   ![「参照」タブ](../assets/ui/activation-overview/browse-tab.png)

1. セグメントをアクティベートするために使用する宛先接続を見つけ、で 3 つのドットを選択します [!UICONTROL 名前] 列を選択してから、を選択します **[!UICONTROL オーディエンスをアクティベート]**.

   ![「オーディエンスをアクティベート」ボタン](../assets/ui/activation-overview/activate-segments.png)

1. 選択した宛先に応じて、以下の記事で説明する手順（から始まる）に従います **[!UICONTROL セグメントを選択]** アクティブ化ワークフローを完了するには、次の手順に従います。

   * [ストリーミングオーディエンスの書き出し先に対するオーディエンスデータの有効化](activate-segment-streaming-destinations.md)
   * [ストリーミングプロファイル書き出し宛先に対するオーディエンスデータの有効化](activate-streaming-profile-destinations.md)
   * [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](activate-batch-profile-destinations.md)

### オーディエンスの詳細ページからオーディエンスをアクティベート {#activate-audience-details}

オーディエンスの詳細ページから、宛先に対するオーディエンスをアクティブ化できます。 参照： [オーディエンスの詳細](../../segmentation/ui/audience-portal.md#audience-details) を参照してください。

選択した宛先に応じて、以下の記事で説明する手順に従って、アクティベーションワークフローを完了します。

* [ストリーミングオーディエンスの書き出し先に対するオーディエンスデータの有効化](activate-segment-streaming-destinations.md)
* [ストリーミングプロファイル書き出し宛先に対するオーディエンスデータの有効化](activate-streaming-profile-destinations.md)
* [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](activate-batch-profile-destinations.md)
