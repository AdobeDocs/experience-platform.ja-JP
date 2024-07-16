---
keywords: アクティベーションを編集、宛先を編集、宛先を編集
title: アクティベーションデータフローを編集
type: Tutorial
description: Adobe Experience Platformの既存のアクティベーションデータフローを編集するには、この記事の手順に従います。
exl-id: 0d79fbff-bfde-4109-8353-c7530e9719fb
source-git-commit: 165793619437f403045b9301ca6fa5389d55db31
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 25%

---

# アクティベーションデータフローを編集 {#edit-activation-flows}

Adobe Experience Platformでは、書き出されたオーディエンスやプロファイル属性、書き出しの頻度、アクティベーションデータフローが有効か無効かなど、宛先に対する既存のアクティベーションデータフローの様々なコンポーネントを編集できます。

## データフローの編集 {#edit-dataflows}

既存のアクティベーションデータフローを編集するには、次の手順に従います。

1. [Experience Platform UI](https://platform.adobe.com/) にログインし、左側のナビゲーションバーから「**[!UICONTROL 宛先]**」を選択します。上部のヘッダーから「**[!UICONTROL 参照]**」を選択して、既存の宛先データフローを表示します。

   ![ 宛先の参照 ](../assets/ui/edit-activation/browse-destinations.png)

2. 左上のフィルターアイコン ![フィルターアイコン](../assets/ui/edit-activation/filter.png) を選択して、並べ替えパネルを開きます。並べ替えパネルには、すべての宛先のリストが表示されます。 リストから複数の宛先を選択して、選択した宛先に関連付けられた特定のデータフローを表示できます。

   ![ 宛先のフィルタリング ](../assets/ui/edit-activation/filter-destinations.png)

3. 編集する宛先データフローの名前を選択します。

   ![宛先を選択](../assets/ui/edit-activation/destination-select.png)

4. 宛先の **[!UICONTROL データフロー実行]** ページが表示され、使用可能なコントロールが表示されます。 この時点で、宛先データフローの複数のコンポーネントを編集できます。

   * 右側のパネルで **[!UICONTROL オーディエンスをアクティブ化]** を選択して、宛先に送信するオーディエンスまたはプロファイル属性を変更します。 このアクションを実行すると、宛先のタイプに応じて異なるアクティベーションワークフローが表示されます。 詳しくは、次のガイドを参照してください。
      * [ オーディエンスストリーミングの宛先に対するオーディエンスデータのアクティブ化 ](./activate-segment-streaming-destinations.md) （例：FacebookまたはTwitter）。
      * [ バッチプロファイルベースの宛先に対するオーディエンスデータのアクティブ化 ](./activate-batch-profile-destinations.md) （Amazon S3 やOracle Eloqua など）。
      * [ ストリーミングプロファイルベースの宛先へのオーディエンスデータのアクティブ化 ](./activate-streaming-profile-destinations.md) （HTTP API やAmazon Kinesisなど）。

   * さらに、宛先データフローの名前と説明を編集できます。
   * **[!UICONTROL 有効 ]/[!UICONTROL  無効]** 切替スイッチを使用して、宛先へのすべてのデータ書き出しを開始および一時停止できます。

   ![ 宛先の詳細 ](../assets/ui/edit-activation/destination-details.png)

## 次の手順 {#next-steps}

このチュートリアルでは、**[!UICONTROL destinations]** ワークスペースを使用して既存の宛先データフローを正常に更新しました。

宛先について詳しくは、[ 宛先の概要 ](../catalog/overview.md) を参照してください。
