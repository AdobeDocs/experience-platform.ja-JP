---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: オーディエンスセグメントをエクスポートするためのデータセットの作成
topic: tutorial
translation-type: tm+mt
source-git-commit: c6c5ada52321b11543254f80399c38365f0fb9d7
workflow-type: tm+mt
source-wordcount: '733'
ht-degree: 68%

---


# オーディエンスセグメントをエクスポートするためのデータセットの作成

[!DNL Adobe Experience Platform] では、特定の属性に基づいて顧客プロファイルをオーディエンスに簡単にセグメント化できます。セグメントを作成したら、そのオーディエンスをデータセットにエクスポートし、そこでオーディエンスにアクセスしたりて操作したりできます。エクスポートを正常におこなうには、データセットを正しく設定する必要があります。

This tutorial walks through the steps required to create a dataset that can be used for exporting an audience segment using the [!DNL Experience Platform] UI.

このチュートリアルは、[セグメント結果の評価とアクセス](./evaluate-a-segment.md)に関するチュートリアルで説明している手順に直接関連しています。The evaluating a segment tutorial provides steps for creating a dataset using the [!DNL Catalog Service] API, whereas this tutorial outlines steps to create a dataset using the [!DNL Experience Platform] UI.

## はじめに

セグメントをエクスポートするには、データセットがに基づいている必要があり [!DNL XDM Individual Profile Union Schema]ます。 A union schema is a system-generated, read-only schema that aggregates the fields of all schemas that share the same class, in this case that is the [!DNL XDM Individual Profile] class. 和集合表示スキーマについて詳しくは、[スキーマレジストリ開発者ガイドのリアルタイム顧客プロファイルの節](../../xdm/schema/composition.md#union)を参照してください。

UI で和集合スキーマを表示するには、左側のナビゲーションで「**[!UICONTROL Profiles]**」をクリックし、「**[!UICONTROL Union schema]**」タブをクリックします（下図を参照）。

![Experience Platform UI の和集合スキーマタブ](../images/tutorials/segment-export-dataset/union-schema-ui.png)


## データセットワークスペース

The datasets workspace within the [!DNL Experience Platform] UI allows you to view and manage all of the datasets that your IMS organization has made, as well as create new ones.

データセットワークスペースを表示するには、左側のナビゲーションで「**[!UICONTROL Datasets]**」をクリックし、「**[!UICONTROL Browse]**」タブをクリックします。データセットワークスペースには、**[!UICONTROL 名前]**、**[!UICONTROL 作成日時]**（日付と時刻）、**[!UICONTROL ソース]**、**[!UICONTROL スキーマ]**、**[!UICONTROL 最終バッチステータス]**&#x200B;を示す列、および&#x200B;**[!UICONTROL 最終更新日時]**&#x200B;を含むデータセットのリストが含まれています。各列の幅によっては、すべての列を表示するには、左または右にスクロールする必要があります。

>[!NOTE]
>
>Click on the filter icon next to the search bar to use filtering capabilities to view only those datasets enabled for [!DNL Real-time Customer Profile].

![すべてのデータセットの表示](../images/tutorials/segment-export-dataset/datasets-workspace.png)

## データセットの作成

データセットを作成するには、「データセット」ワークスペースの右上隅にある「**[!UICONTROL データセットを作成]**」をクリックします。

![「データセットを作成」のクリック](../images/tutorials/segment-export-dataset/dataset-click-create.png)

**[!UICONTROL Create Dataset]** 画面で、「**[!UICONTROL Create Dataset from Schema]**」をクリックして続行します。

![データソースの選択](../images/tutorials/segment-export-dataset/create-dataset.png)

## XDM 個別プロファイル和集合スキーマの選択

To select the [!DNL XDM Individual Profile Union Schema] for use in your dataset, find the &quot;[!UICONTROL XDM Individual Profile]&quot; schema with a type of &quot;[!UICONTROL Union]&quot; on the **[!UICONTROL Select Schema]** screen.

「**[!UICONTROL XDM Individual Profile]**」の横のラジオボタンを選択し、右上隅の「**[!UICONTROL Next]**」をクリックします。

![スキーマの選択](../images/tutorials/segment-export-dataset/select-schema.png)

## データセットの設定

**[!UICONTROL データセットの設定]**&#x200B;画面で、データセットの&#x200B;**[!UICONTROL 名前]**&#x200B;を指定し、データセットの&#x200B;**[!UICONTROL 説明]**&#x200B;も入力できます。

**データセット名に関する注意事項：**
- 後でライブラリ内で簡単に見つけられるように、データセット名は短く、わかりやすい名前にする必要があります。
- データセット名は一意である必要があります。つまり、今後再利用されないように十分な固有の名前を付ける必要があります。
- ベストプラクティスは、説明フィールドを使用して、データセットに関する追加情報を提供することです。これは、今後、他のユーザーがデータセットを区別する際に役立つ可能性があるためです。

データセットに名前と説明が付いたら、「**[!UICONTROL 完了]**」をクリックします。

![データセットの設定](../images/tutorials/segment-export-dataset/configure-dataset.png)

## データセットアクティビティ

空のデータセットが作成され、データセットワークスペースの「**[!UICONTROL データセットアクティビティ]**」タブに戻りました。ワークスペースの左上隅にデータセットの名前と、「バッチが追加されていません」という通知が表示されます。このデータセットにバッチをまだ追加していないので、これは期待通りです。

「データセット」ワークスペースの右側に「**[!UICONTROL 情報]**」タブがあり、新しいデータセットに関した「**[!UICONTROL データセット ID]**」、「**[!UICONTROL 名前]**」、「**[!UICONTROL 説明]**」、「**[!UICONTROL テーブル名]**」、「**[!UICONTROL スキーマ]**」、「**[!UICONTROL ストリーミング]**」「**[!UICONTROL ソース]**」などの情報が含まれます。The [!UICONTROL Info] tab also includes information about when the dataset was **[!UICONTROL Created]** and its **[!UICONTROL Last Modified]** date.

**[!UICONTROL データセット ID]** は、オーディエンスセグメントのエクスポートワークフローを完了するために必要になるので、メモしておいてください。

![データセットアクティビティ](../images/tutorials/segment-export-dataset/dataset-activity.png)

## 次の手順

Now that you have created a dataset based on the [!DNL XDM Individual Profile Union Schema], you can use the **[!UICONTROL Dataset ID]** to continue the [evaluating and accessing segment results](./evaluate-a-segment.md) tutorial.

At this time, please return to the evaluating segment results tutorial and pick up from the [generating profiles for audience members](./evaluate-a-segment.md#generate-profiles) step of the exporting a segment workflow.
