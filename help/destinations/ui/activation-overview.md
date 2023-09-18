---
keywords: 宛先のアクティブ化；データのアクティブ化
title: 有効化の概要
type: Tutorial
description: 様々なタイプの宛先に対してAdobe Experience Platformでオーディエンスをアクティブ化する方法を説明します。
exl-id: 987af401-2d93-45b4-a8f9-191e6058e4da
source-git-commit: afcb5f80edaa4d68ba167123feb2ba9060469243
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 30%

---

# 有効化の概要

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。
>* 書き出す *id*、 **[!UICONTROL ID グラフを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png "ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。"){width="100" zoomable="yes"}

Adobe Experience Platformは様々な宛先をサポートしています。 オーディエンスのアクティベーションワークフローは、サポートするオーディエンスデータのタイプとデータの書き出し頻度に応じて、宛先によって異なります。

## アクティベーション方法 {#activation-methods}

後で [宛先の設定](connect-destination.md)オーディエンスをアクティブ化するには、次の複数の方法があります。

### 宛先カタログからのオーディエンスのアクティブ化

宛先カタログから宛先に対してオーディエンスをアクティブ化する方法について詳しくは、次のガイドを参照してください。

* [ストリーミングオーディエンスの書き出し先に対するオーディエンスデータの有効化](activate-segment-streaming-destinations.md)
* [ストリーミングプロファイル書き出し宛先に対するオーディエンスデータの有効化](activate-streaming-profile-destinations.md)
* [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](activate-batch-profile-destinations.md)

### 以下からオーディエンスをアクティブ化 [!UICONTROL 参照] ページ

次の手順に従って、から宛先に対してデータをアクティブ化します。 **[!UICONTROL 参照]** ページに貼り付けます。

1. に移動します。 **[!UICONTROL 接続/宛先]**&#x200B;をクリックし、 **[!UICONTROL 参照]** タブをクリックします。

   ![「参照」タブ](../assets/ui/activation-overview/browse-tab.png)

1. セグメントのアクティブ化に使用する宛先接続を見つけ、 [!UICONTROL 名前] 列、「 **[!UICONTROL オーディエンスをアクティブ化]**.

   ![オーディエンスのアクティブ化ボタン](../assets/ui/activation-overview/activate-segments.png)

1. 選択した宛先に応じて、以下の記事で説明されている手順に従い、最初に **[!UICONTROL セグメントを選択]** ステップ（アクティベーションワークフローを終了するには： ）:

   * [ストリーミングオーディエンスの書き出し先に対するオーディエンスデータの有効化](activate-segment-streaming-destinations.md)
   * [ストリーミングプロファイル書き出し宛先に対するオーディエンスデータの有効化](activate-streaming-profile-destinations.md)
   * [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](activate-batch-profile-destinations.md)

### オーディエンスの詳細ページからのオーディエンスのアクティブ化 {#activate-audience-details}

オーディエンスの詳細ページから、オーディエンスを宛先に対してアクティブ化できます。 詳しくは、 [オーディエンスの詳細](../../segmentation/ui/overview.md#audience-details) を参照してください。

選択した宛先に応じて、以下の記事で説明されている手順に従って、アクティベーションワークフローを完了します。

* [ストリーミングオーディエンスの書き出し先に対するオーディエンスデータの有効化](activate-segment-streaming-destinations.md)
* [ストリーミングプロファイル書き出し宛先に対するオーディエンスデータの有効化](activate-streaming-profile-destinations.md)
* [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](activate-batch-profile-destinations.md)
