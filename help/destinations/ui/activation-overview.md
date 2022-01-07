---
keywords: 宛先のアクティブ化；データのアクティブ化
title: Activation の概要
type: Tutorial
seo-title: Activation overview
description: Adobe Experience Platformで様々なタイプの宛先に対してオーディエンスデータをアクティブ化する方法を説明します。
seo-description: Learn how to activate the audience data you have in Adobe Experience Platform to various types of destinations.
exl-id: 987af401-2d93-45b4-a8f9-191e6058e4da
source-git-commit: f4ae6831569e8a5b458c42f76810212174f04811
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 9%

---

# Activation の概要

Adobe Experience Platformは様々な宛先をサポートしています。 オーディエンスのアクティベーションワークフローは、サポートするオーディエンスデータのタイプとデータの書き出し頻度に基づいて、宛先によって異なります。

## アクティベーション方法 {#activation-methods}

後 [宛先の設定](connect-destination.md)では、以下の複数の方法でオーディエンスセグメントをアクティブ化できます。

### 宛先カタログからのオーディエンスのアクティブ化

宛先カタログから宛先に対してオーディエンスをアクティブ化する方法について詳しくは、次のガイドを参照してください。

* [ストリーミングセグメント書き出しの宛先に対するオーディエンスデータのアクティブ化](activate-segment-streaming-destinations.md)
* [ストリーミングプロファイルの書き出し先に対するオーディエンスデータのアクティブ化](activate-streaming-profile-destinations.md)
* [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](activate-batch-profile-destinations.md)

### 以下からオーディエンスをアクティブ化 [!UICONTROL 参照] ページ

次の手順に従って、から宛先に対してデータをアクティブ化します。 **[!UICONTROL 参照]** ページ。

1. に移動します。 **[!UICONTROL 接続/宛先]**&#x200B;をクリックし、 **[!UICONTROL 参照]** タブをクリックします。

   ![「参照」タブ](../assets/ui/activation-overview/browse-tab.png)

1. セグメントのアクティブ化に使用する宛先接続を見つけ、 [!UICONTROL 名前] 列、「 **[!UICONTROL セグメントのアクティブ化]**.

   ![「セグメントをアクティブ化」ボタン](../assets/ui/activation-overview/activate-segments.png)

1. 選択した宛先に応じて、以下の記事で説明されている手順に従い、最初に **[!UICONTROL セグメントを選択]** ステップ（アクティベーションワークフローを終了するには： ）:

   * [ストリーミングセグメント書き出しの宛先に対するオーディエンスデータのアクティブ化](activate-segment-streaming-destinations.md)
   * [ストリーミングプロファイルの書き出し先に対するオーディエンスデータのアクティブ化](activate-streaming-profile-destinations.md)
   * [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](activate-batch-profile-destinations.md)

### セグメントの詳細ページからのオーディエンスのアクティブ化 {#activate-segment-details}

セグメントの詳細ページから、宛先に対してセグメントをアクティブ化できます。 詳しくは、 [セグメントの詳細](../../segmentation/ui/overview.md#segment-details) を参照してください。

選択した宛先に応じて、以下の記事で説明されている手順に従って、アクティベーションワークフローを完了します。

* [ストリーミングセグメント書き出しの宛先に対するオーディエンスデータのアクティブ化](activate-segment-streaming-destinations.md)
* [ストリーミングプロファイルの書き出し先に対するオーディエンスデータのアクティブ化](activate-streaming-profile-destinations.md)
* [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](activate-batch-profile-destinations.md)
