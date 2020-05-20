---
keywords: Experience Platform;insights; customer ai;popular topics
solution: Experience Platform
title: 予測スコアを使用した顧客セグメントの作成
topic: Create a segment
translation-type: tm+mt
source-git-commit: 66ccea896846c1da4310c1077e2dc7066a258063
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 52%

---


# 予測スコアを使用した顧客セグメントの作成

予測の実行が完了すると、予測された傾向スコアはプロファイルによって自動的に使用されます。顧客AIスコアを使用してプロファイルを強化することで、顧客セグメントを作成し、その傾向スコアに基づいてオーディエンスを見つけることができます。 ここでは、セグメントビルダーを使用してセグメントを作成する手順を説明します。セグメントの作成に関するより詳細なチュートリアルについては、『[セグメントビルダーユーザーガイド](../../../segmentation/tutorials/create-a-segment.md)』を参照してください。

>[!IMPORTANT] この方法を利用するには、データセットに対してリアルタイム顧客プロファイルを有効にする必要があります。

プラットフォーム UI で、左側のナビゲーションの「**[!UICONTROL セグメント]**」をクリックし、「**[!UICONTROL セグメント作成]**」をクリックします。

![](../images/user-guide/segments.png)

「*セグメントビルダー*」が表示されます。左側の「*フィールド*」列の「*属性*」タブで、「**[!UICONTROL XDM Individual Profile]**」という名前のフォルダーをクリックし、組織の名前空間を持つフォルダーをクリックします。「**[!UICONTROL Customer AI]**」という名前のフォルダーには、予測実行の結果が格納され、スコアが属するインスタンスに基づいて名前が付けられます。インスタンスフォルダーをクリックして、目的のインスタンスの結果にアクセスします。

![](../images/user-guide/results.png)

セグメントビルダーの中央にある「**[!UICONTROL スコア]**」属性を&#x200B;*ルールビルダーキャンバス*&#x200B;にドラッグ&amp;ドロップし、ルールを定義します。

右側の *セグメントプロパティ* 列で、セグメントの名前を指定します。

![](../images/user-guide/properties.png)

左側の「 *フィールド* 」列の上で、 **歯車** アイコンをクリックし、ドロップダウンから「 *マージポリシー* 」を選択します。 Click **[!UICONTROL Save]** to create the segment.

![](../images/user-guide/merge_policy.png)

## 次の手順

このチュートリアルに従うと、セグメントビルダーを使用して、その傾向スコアに基づくオーディエンスを正しく見つけることができました。 目的のオーディエンスをアクティブ化して、ターゲットできるようになりました。 See the [destinations overview](https://docs.adobe.com/content/help/en/experience-platform/rtcdp/destinations/destinations-overview.html) for more information.