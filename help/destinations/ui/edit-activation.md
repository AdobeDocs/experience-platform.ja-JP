---
keywords: アクティベーションを編集、宛先を編集、宛先を編集
title: アクティベーションデータフローを編集
type: Tutorial
description: Adobe Experience Platformの既存のアクティベーションデータフローを編集するには、この記事の手順に従います。
exl-id: 0d79fbff-bfde-4109-8353-c7530e9719fb
source-git-commit: ca33131c505803b74075f6d8331095b016a301a0
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# アクティベーションデータフローを編集 {#edit-activation-flows}

Adobe Experience Platformでは、宛先に対する既存のアクティベーションデータフローの様々なコンポーネントを設定できます。以下に例を示します。

* [ 有効化または無効化 ](#enable-disable-dataflows) アクティベーションデータフロー
* [ 追加のオーディエンスとプロファイル属性の追加 ](#add-audiences) アクティベーションデータフローに追加する。
* [ 追加のデータセット ](#add-datasets) アクティベーションワークフローへの追加
* アクティベーションデータフローの [ 名前と説明を編集 ](#edit-names-descriptions)

<!-- * [Apply access labels](#apply-access-labels) to exported data; -->

## アクティベーションデータフローを参照 {#browse-activation-dataflows}

次の手順に従って、既存のアクティベーションデータフローを参照し、編集するデータフローを特定します。

1. [Experience Platform UI](https://platform.adobe.com/) にログインし、左側のナビゲーションバーから「**[!UICONTROL 宛先]**」を選択します。上部のヘッダーから「**[!UICONTROL 参照]**」を選択して、既存の宛先データフローを表示します。

   ![ 宛先の参照 ](../assets/ui/edit-activation/browse-destinations.png)

2. 左上のフィルターアイコン ![フィルターアイコン](../../images/icons/filter.png) を選択して、並べ替えパネルを開きます。並べ替えパネルには、すべての宛先のリストが表示されます。 リストから複数の宛先を選択して、選択した宛先に関連付けられた特定のデータフローを表示できます。

   ![ 宛先のフィルタリング ](../assets/ui/edit-activation/filter-destinations.png)

3. 編集する宛先データフローの名前を選択します。

   ![宛先を選択](../assets/ui/edit-activation/destination-select.png)

4. 宛先の **[!UICONTROL データフロー実行]** ページが表示され、使用可能なコントロールが表示されます。 宛先タイプに応じて、様々なデータフロー操作を実行できます。 サポートされる各データフロー操作については、次の節を参照してください。

## アクティベーションデータフローを有効または無効にする {#enable-disable-dataflows}

**[!UICONTROL 有効 ]/[!UICONTROL  無効]** 切替スイッチを使用して、宛先へのすべてのデータ書き出しを開始または一時停止します。

![ データフロー実行の有効/無効の切り替えを示すExperience Platform UI 画像。](../assets/ui/edit-activation/enable-toggle.png)

## アクティベーションデータフローへのオーディエンスの追加 {#add-audiences}

右側のパネルで **[!UICONTROL オーディエンスをアクティブ化]** を選択して、宛先に送信するオーディエンスまたはプロファイル属性を変更します。 このアクションを実行すると、宛先のタイプに応じて異なるアクティベーションワークフローが表示されます。

![ 「オーディエンスのデータフロー実行をアクティブ化」オプションを示すExperience Platform UI 画像。](../assets/ui/edit-activation/activate-audiences.png)

各宛先タイプのアクティベーションワークフローについて詳しくは、次のガイドを参照してください。

* [ ストリーミング宛先に対するオーディエンスのアクティブ化 ](./activate-segment-streaming-destinations.md) （例：FacebookまたはTwitter）。
* [ プロファイル書き出しのバッチ宛先に対するオーディエンスのアクティブ化 ](./activate-batch-profile-destinations.md) （例：Amazon S3 またはOracle Eloqua）。
* [ ストリーミングプロファイル書き出し宛先に対するオーディエンスのアクティブ化 ](./activate-streaming-profile-destinations.md) （HTTP API やAmazon Kinesisなど）。

## アクティベーションデータフローへのデータセットの追加 {#add-datasets}

右側のパネルで **[!UICONTROL データセットを書き出し]** を選択して、宛先に書き出す追加のデータセットを選択します。 このオプションを選択すると、[ データセット書き出しワークフロー ](export-datasets.md) が表示されます。

>[!NOTE]
>
>このオプションは、[ データセットの書き出しをサポートする宛先 ](export-datasets.md#supported-destinations) に対してのみ表示されます。

![ 「データセットを書き出し」データフロー実行オプションを示すExperience Platform UI 画像。](../assets/ui/edit-activation/export-datasets.png)

<!-- ## Apply access labels {#apply-access-labels}

Select **[!UICONTROL Apply access labels]** to edit the data usage labels for the exported data. See the [data usage labels documentation](../../data-governance/labels/overview.md) to learn more.

![Experience Platform UI image showing the Export datasets dataflow run option.](../assets/ui/edit-activation/apply-access-labels.png) -->

## アクティベーションデータフローの名前と説明の編集 {#edit-names-descriptions}

アクティベーションデータフローの名前と説明を編集するには、「**[!UICONTROL 宛先名]**」フィールドと「**[!UICONTROL 説明]**」フィールドを使用します。

![ 宛先の詳細 ](../assets/ui/edit-activation/edit-destination-name-description.png)

## 次の手順 {#next-steps}

このチュートリアルでは、**[!UICONTROL destinations]** ワークスペースを使用して既存の宛先データフローを正常に更新しました。

宛先について詳しくは、[ 宛先の概要 ](../catalog/overview.md) を参照してください。
