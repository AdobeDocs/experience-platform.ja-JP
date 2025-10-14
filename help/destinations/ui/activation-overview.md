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
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

Adobe Experience Platformは、幅広い宛先をサポートします。 Audience Activation のワークフローは、サポートするオーディエンスデータのタイプと、データ書き出しの頻度に基づいて、宛先によって異なります。

## アクティブ化メソッド {#activation-methods}

[&#x200B; 宛先を設定 &#x200B;](connect-destination.md) した後、複数の方法でオーディエンスをアクティブ化できます。

### 宛先カタログからオーディエンスをアクティブ化

宛先カタログから宛先に対するオーディエンスのアクティブ化について詳しくは、次のガイドを参照してください。

* [ストリーミングオーディエンスの書き出し先に対するオーディエンスデータの有効化](activate-segment-streaming-destinations.md)
* [ストリーミングプロファイル書き出し宛先に対するオーディエンスデータの有効化](activate-streaming-profile-destinations.md)
* [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](activate-batch-profile-destinations.md)

### [!UICONTROL &#x200B; 参照 &#x200B;] ページからオーディエンスをアクティベート

**[!UICONTROL 参照]** ページから宛先へのデータをアクティブ化するには、次の手順に従います。

1. **[!UICONTROL 接続/宛先]** に移動し、「**[!UICONTROL 参照]** タブを選択します。

   ![&#x200B; 「参照」タブ &#x200B;](../assets/ui/activation-overview/browse-tab.png)

1. セグメントをアクティベートするために使用する宛先接続を見つけ、「名前 [!UICONTROL &#x200B; 列の 3 つのドットを選択してから、「**[!UICONTROL オーディエンスをアクティベート &#x200B;] を選択します]**。

   ![&#x200B; 「オーディエンスをアクティベート」ボタン &#x200B;](../assets/ui/activation-overview/activate-segments.png)

1. 選択した宛先に応じて、以下の記事で説明する手順に従います。まず、**[!UICONTROL セグメントを選択]** 手順に従って、アクティベーションワークフローを完了します。

   * [ストリーミングオーディエンスの書き出し先に対するオーディエンスデータの有効化](activate-segment-streaming-destinations.md)
   * [ストリーミングプロファイル書き出し宛先に対するオーディエンスデータの有効化](activate-streaming-profile-destinations.md)
   * [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](activate-batch-profile-destinations.md)

### オーディエンスの詳細ページからオーディエンスをアクティベート {#activate-audience-details}

オーディエンスの詳細ページから、宛先に対するオーディエンスをアクティブ化できます。 詳しくは、[&#x200B; オーディエンスの詳細 &#x200B;](../../segmentation/ui/audience-portal.md#audience-details) を参照してください。

選択した宛先に応じて、以下の記事で説明する手順に従って、アクティベーションワークフローを完了します。

* [ストリーミングオーディエンスの書き出し先に対するオーディエンスデータの有効化](activate-segment-streaming-destinations.md)
* [ストリーミングプロファイル書き出し宛先に対するオーディエンスデータの有効化](activate-streaming-profile-destinations.md)
* [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](activate-batch-profile-destinations.md)
