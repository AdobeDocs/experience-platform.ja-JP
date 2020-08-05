---
keywords: Experience Platform;insights; customer ai;popular topics
solution: Experience Platform
title: 予測スコアを使用した顧客セグメントの作成
topic: Create a segment
translation-type: tm+mt
source-git-commit: e351a2d489730c1f1bd5f87be8d85612090bc009
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 87%

---


# 予測スコアを使用した顧客セグメントの作成

予測の実行が完了すると、予測された傾向スコアはプロファイルによって自動的に使用されます。顧客 AI スコアによるプロファイルの強化により、傾向スコアに基づくオーディエンスを発見するための顧客セグメントを作成できます。ここでは、セグメントビルダーを使用してセグメントを作成する手順を説明します。セグメントの作成に関するより詳細なチュートリアルについては、『[セグメントビルダーユーザーガイド](../../../segmentation/ui/segment-builder.md)』を参照してください。

>[!IMPORTANT]
>
> この方法を利用するには、データセットに対してリアルタイムの顧客プロファイルを有効にする必要があります。

プラットフォーム UI で、左側のナビゲーションの「**[!UICONTROL セグメント]**」をクリックし、「**[!UICONTROL セグメント作成]**」をクリックします。

![](../images/user-guide/segments.png)

「*セグメントビルダー*」が表示されます。左側の「*フィールド*」列の「*属性*」タブで、「**[!UICONTROL XDM Individual Profile]**」という名前のフォルダーをクリックし、組織の名前空間を持つフォルダーをクリックします。「**[!UICONTROL Customer AI]**」という名前のフォルダーには、予測実行の結果が格納され、スコアが属するインスタンスに基づいて名前が付けられます。インスタンスフォルダーをクリックして、目的のインスタンスの結果にアクセスします。

![](../images/user-guide/results.png)

セグメントビルダーの中央にある「**[!UICONTROL スコア]**」属性を&#x200B;*ルールビルダーキャンバス*&#x200B;にドラッグ&amp;ドロップし、ルールを定義します。

右側の&#x200B;*セグメントのプロパティ*&#x200B;列で、セグメントの名前を指定します。

![](../images/user-guide/properties.png)

Above the left-hand *Fields* column, click the **gear** icon and select a *Merge policy* from the drop-down. 「**[!UICONTROL 保存]**」をクリックしてセグメントを作成します。

![](../images/user-guide/merge_policy.png)

## 次の手順

このチュートリアルに従うと、セグメントビルダーを使用して、その傾向スコアに基づくオーディエンスを正しく見つけることができました。 これで、オーディエンスを宛先にアクティブ化することでターゲットに設定できます。詳しくは、「[宛先の概要](https://docs.adobe.com/content/help/ja-JP/experience-platform/rtcdp/destinations/destinations-overview.html)」を参照してください。