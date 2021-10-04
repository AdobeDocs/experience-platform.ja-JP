---
keywords: 宛先のアクティブ化；データのアクティブ化
title: Activation の概要
type: Tutorial
seo-title: Activation overview
description: 様々なタイプの宛先に対して、Adobe Experience Platformでのオーディエンスデータをアクティブ化する方法を説明します。
seo-description: Learn how to activate the audience data you have in Adobe Experience Platform to various types of destinations.
exl-id: 987af401-2d93-45b4-a8f9-191e6058e4da
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 9%

---

# Activation の概要

Adobe Experience Platformは様々な宛先をサポートしています。 オーディエンスのアクティベーションワークフローは、サポートするオーディエンスデータのタイプと、データの書き出しの頻度に応じて、宛先によって異なります。

## アクティベーション方法 {#activation-methods}

[ 宛先を設定 ](connect-destination.md) した後、複数の方法でオーディエンスセグメントをアクティブ化できます。

### 宛先カタログからのオーディエンスのアクティブ化

宛先カタログから宛先に対してオーディエンスをアクティブ化する方法について詳しくは、次のガイドを参照してください。

* [ストリーミングセグメント書き出しの宛先に対するオーディエンスデータのアクティブ化](activate-segment-streaming-destinations.md)
* [ストリーミングプロファイルの書き出し先に対するオーディエンスデータのアクティブ化](activate-streaming-profile-destinations.md)
* [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](activate-batch-profile-destinations.md)

### [!UICONTROL  閲覧 ] ページからオーディエンスをアクティブ化する

**[!UICONTROL 参照]** ページから宛先へのデータをアクティブ化するには、次の手順に従います。

1. **[!UICONTROL 接続/宛先]** に移動し、「**[!UICONTROL 参照]**」タブを選択します。

   ![「参照」タブ](../assets/ui/activation-overview/browse-tab.png)

1. セグメントのアクティブ化に使用する宛先接続を見つけ、[!UICONTROL  名前 ] 列の 3 つのドットを選択して、**[!UICONTROL セグメントをアクティブ化]** を選択します。

   ![「セグメントをアクティブ化」ボタン](../assets/ui/activation-overview/activate-segments.png)

1. 選択した宛先に応じて、以下の記事で説明されている手順に従い、**[!UICONTROL セグメントの選択]** の手順から始めて、アクティベーションワークフローを終了します。

   * [ストリーミングセグメント書き出しの宛先に対するオーディエンスデータのアクティブ化](activate-segment-streaming-destinations.md)
   * [ストリーミングプロファイルの書き出し先に対するオーディエンスデータのアクティブ化](activate-streaming-profile-destinations.md)
   * [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](activate-batch-profile-destinations.md)

### セグメントの詳細ページからのオーディエンスのアクティブ化 {#activate-segment-details}

セグメントの詳細ページから、宛先に対してセグメントをアクティブ化できます。 詳しくは、[ セグメントの詳細 ](../../segmentation/ui/overview.md#segment-details) を参照してください。

選択した宛先に応じて、以下の記事で説明されている手順に従って、アクティベーションワークフローを終了します。

* [ストリーミングセグメント書き出しの宛先に対するオーディエンスデータのアクティブ化](activate-segment-streaming-destinations.md)
* [ストリーミングプロファイルの書き出し先に対するオーディエンスデータのアクティブ化](activate-streaming-profile-destinations.md)
* [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](activate-batch-profile-destinations.md)
