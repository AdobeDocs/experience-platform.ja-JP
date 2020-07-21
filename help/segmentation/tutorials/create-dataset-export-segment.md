---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: オーディエンスセグメントの書き出し用データセットの作成
topic: tutorial
translation-type: tm+mt
source-git-commit: 6a0a9b020b0dc89a829c557bdf29b66508a10333
workflow-type: tm+mt
source-wordcount: '733'
ht-degree: 0%

---


# オーディエンスセグメントの書き出し用データセットの作成

[!DNL Adobe Experience Platform] 特定の属性に基づいて顧客プロファイルをオーディエンスに簡単にセグメント化できます。 セグメントを作成したら、そのオーディエンスをデータセットにエクスポートし、データセットにアクセスして処理することができます。 エクスポートが正常に完了するには、データセットを正しく設定する必要があります。

このチュートリアルでは、 [!DNL Experience Platform] UIを使用したオーディエンスセグメントの書き出しに使用できるデータセットを作成する手順について説明します。

このチュートリアルは、セグメント結果の [評価とアクセスのチュートリアルで概要を説明している手順に直接関連しています](./evaluate-a-segment.md)。 セグメントの評価のチュートリアルではAPIを使用してデータセットを作成する手順を説明します。このチュートリアルでは、 [!DNL Catalog Service][!DNL Experience Platform] UIを使用してデータセットを作成する手順について概説します。

## はじめに

セグメントをエクスポートするには、データセットがに基づいている必要があり [!DNL XDM Individual Profile Union Schema]ます。 和集合スキーマは、システム生成の読み取り専用スキーマで、同じクラス(この場合は [!DNL XDM Individual Profile] クラス)を共有するすべてのスキーマのフィールドを集計します。 和集合表示のスキーマの詳細については、『スキーマレジストリ開発ガイド』の「 [リアルタイム顧客プロファイル」の節を参照してください](../../xdm/schema/composition.md#union)。

UIで和集合スキーマを表示するには、左側のナビゲーションで **[!UICONTROL プロファイル]** (「 **** 」)をクリックし、次に示す和集合スキーマ(「」)タブをクリックします。

![Experience PlatformUIの「和集合スキーマ」タブ](../images/tutorials/segment-export-dataset/union-schema-ui.png)


## Datasetsワークスペース

The datasets workspace within the [!DNL Experience Platform] UI allows you to view and manage all of the datasets that your IMS organization has made, as well as create new ones.

データセットワークスペースを表示するには、左側のナビゲーションで **[!UICONTROL 「Datasets]** 」をクリックし、「 *[!UICONTROL Browse]* （参照）」タブをクリックします。 The datasets workspace contains a list of datasets, including columns showing *[!UICONTROL Name]*, *[!UICONTROL Created]* (date and time), *[!UICONTROL Source]*, *[!UICONTROL Schema]*, and *[!UICONTROL Last Batch Status]*, as well as the date and time the dataset was *[!UICONTROL Last Updated]*. 各列の幅に応じて、すべての列を表示するために左右にスクロールする必要がある場合があります。

>[!NOTE]
>
>検索バーの横にあるフィルターアイコンをクリックして、フィルタリング機能を使用し、有効になっているデータセットのみを表示 [!DNL Real-time Customer Profile]します。

![すべてのデータセットの表示](../images/tutorials/segment-export-dataset/datasets-workspace.png)

## データセットの作成

データセットを作成するには、 **[!UICONTROL Datasets]** ワークスペースの右上隅にある「データセットを [!UICONTROL 作成] 」をクリックします。

![「データセットを作成」をクリックします](../images/tutorials/segment-export-dataset/dataset-click-create.png)

データセットの *[!UICONTROL 作成画面で、「スキーマからデータセットを]* 作成 **[!UICONTROL 」をクリックして続行します]** 。

![データソースの選択](../images/tutorials/segment-export-dataset/create-dataset.png)

## 「XDM Individualプロファイル和集合」スキーマの選択

データセットで使用 [!DNL XDM Individual Profile Union Schema] するスキーマを選択するには、「[!UICONTROL XDM Individualプロファイル]」スキーマ(タイプが「[!UICONTROL 和集合]** 」)を選択画面で探します。

「 **[!UICONTROL XDM個々のプロファイル]**」の横のラジオボタンを選択し、右上隅の「 **[!UICONTROL 次へ]** 」をクリックします。

![スキーマの選択](../images/tutorials/segment-export-dataset/select-schema.png)

## データセットの設定

データセットを **[!UICONTROL 設定画面では、データセットに]** 名前を付ける必要があり *[!UICONTROL 、]* 説明 ** (Description)も表示されます。

**データセット名に関する注意事項：**
- データセット名は短くてわかりやすい名前にし、後でライブラリ内で簡単に見つけられるようにしてください。
- データセット名は一意にする必要があります。つまり、将来再利用できなくなるように固有の名前を付ける必要もあります。
- 説明フィールドを使用して、データセットに関する追加情報を提供することをお勧めします。これは、今後、他のユーザーがデータセットを区別する際に役立つ場合があるためです。

データセットに名前と説明が付いたら、「 **[!UICONTROL 完了]**」をクリックします。

![データセットの設定](../images/tutorials/segment-export-dataset/configure-dataset.png)

## データセットアクティビティ

空のデータセットが作成され、 *[!UICONTROL Datasets]* ワークスペースの「 [!UICONTROL データセットアクティビティ] 」タブに戻りました。 ワークスペースの左上隅に、データセットの名前と、「バッチが追加されていません」という通知が表示されます。 これは、まだこのデータセットにバッチを追加していないので、予想されることです。

Workspaceの右側には、新しいデータセットに関連する情報を含む「 **[!UICONTROL 情報]** 」 *[!UICONTROL タブが表示されます。例えば、名前名、名前、説明、説明、、]*&#x200B;スキーマ名、説明、ストリーミング、元データセットなど ************&#x200B;です。 「 [!UICONTROL 情報] 」タブには、データセットがいつ *[!UICONTROL 作成されたか]* 、および ** 最終変更日に関する情報も表示されます。

オーディエンスセグメントの書き出しワークフローを完了するにはこの値が必要なので、 **[!UICONTROL データセットID]**&#x200B;を控えておいてください。

![データセットアクティビティ](../images/tutorials/segment-export-dataset/dataset-activity.png)

## 次の手順

これで、に基づいてデータセットを作成したので、 [!DNL XDM Individual Profile Union Schema]データセットID **[!UICONTROL (データセットID]** )を使用して、セグメント結果の [評価とアクセスのチュートリアルを続行できます](./evaluate-a-segment.md) 。

この時点で、評価セグメントの結果のチュートリアルに戻り、セグメントのエクスポートのオーディエンスメンバーの [プロファイルの生成手順](./evaluate-a-segment.md#generate-profiles) （英語のみ）を参照してください。
