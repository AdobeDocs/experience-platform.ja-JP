---
keywords: Experience Platform；ホーム；人気の高いトピック；Segmentation Service;Segmentation;Segmentation；データセットの作成；オーディエンスセグメントのエクスポート；セグメントのエクスポート；
solution: Experience Platform
title: オーディエンスセグメントエクスポート用のデータセットの作成
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、Experience Platform UI で、オーディエンスセグメントのエクスポートに使用できるデータセットを作成する手順を説明します。
exl-id: 1cd16e43-b050-42ba-a894-d7ea477b65f3
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '773'
ht-degree: 54%

---

# オーディエンスセグメントをエクスポートするためのデータセットの作成

[!DNL Adobe Experience Platform] では、特定の属性に基づいて顧客プロファイルをオーディエンスに簡単にセグメント化できます。セグメントを作成したら、そのオーディエンスをデータセットにエクスポートし、そこでオーディエンスにアクセスしたりて操作したりできます。エクスポートを正常におこなうには、データセットを正しく設定する必要があります。

[!DNL Experience Platform] UIを使用したオーディエンスセグメントの書き出しに使用できるデータセットを作成する手順を説明します。

このチュートリアルは、[セグメント結果の評価とアクセス](./evaluate-a-segment.md)に関するチュートリアルで説明している手順に直接関連しています。セグメントの評価のチュートリアルでは[!DNL Catalog Service] APIを使用してデータセットを作成する手順を紹介します。このチュートリアルでは、[!DNL Experience Platform] UIを使用してデータセットを作成する手順について概説します。

## はじめに

セグメントをエクスポートするには、データセットが[!DNL XDM Individual Profile Union Schema]に基づいている必要があります。 和集合スキーマは、システム生成の読み取り専用スキーマで、同じクラス（この場合は[!DNL XDM Individual Profile]クラス）を共有するすべてのスキーマのフィールドを集計します。 和集合表示スキーマについて詳しくは、[スキーマレジストリ開発者ガイドのリアルタイム顧客プロファイルの節](../../xdm/schema/composition.md#union)を参照してください。

UI で和集合スキーマを表示するには、左側のナビゲーションで「**[!UICONTROL Profiles]**」をクリックし、「**[!UICONTROL Union schema]**」タブをクリックします（下図を参照）。

![Experience Platform UI の和集合スキーマタブ](../images/tutorials/segment-export-dataset/union-schema-ui.png)


## データセットワークスペース

[!DNL Experience Platform] UI内のデータセットワークスペースを使用すると、IMS組織が作成したすべてのデータセットの表示と管理を行えるほか、新しいデータセットを作成できます。

データセットワークスペースを表示するには、左側のナビゲーションで「**[!UICONTROL Datasets]**」をクリックし、「**[!UICONTROL Browse]**」タブをクリックします。データセットワークスペースには、名前、作成日時、ソース、スキーマ、最終バッチステータスを示す列、およびデータセットが最後に更新された日時など、データセットのリストが含まれます。 各列の幅によっては、すべての列を表示するには、左または右にスクロールする必要があります。

>[!NOTE]
>
>検索バーの横にあるフィルターアイコンをクリックして、フィルター機能を使用し、[!DNL Real-time Customer Profile]に対して有効になっているデータセットのみを表示します。

![すべてのデータセットの表示](../images/tutorials/segment-export-dataset/datasets-workspace.png)

## データセットの作成

データセットを作成するには、「データセット」ワークスペースの右上隅にある「**[!UICONTROL データセットを作成]**」をクリックします。****

![「データセットを作成」のクリック](../images/tutorials/segment-export-dataset/dataset-click-create.png)

**[!UICONTROL Create Dataset]** 画面で、「**[!UICONTROL Create Dataset from Schema]**」をクリックして続行します。

![データソースの選択](../images/tutorials/segment-export-dataset/create-dataset.png)

## XDM 個別プロファイル和集合スキーマの選択

データセットに使用する[!DNL XDM Individual Profile Union Schema]を選択するには、**[!UICONTROL スキーマを選択]**&#x200B;画面で、「[!UICONTROL 和集合]」のタイプを持つ「[!UICONTROL XDM Indivitualプロファイル]」スキーマを探します。

「**[!UICONTROL XDM Individual Profile]**」の横のラジオボタンを選択し、右上隅の「**[!UICONTROL Next]**」をクリックします。

![スキーマの選択](../images/tutorials/segment-export-dataset/select-schema.png)

## データセットの設定

**[!UICONTROL データセットを設定]**&#x200B;画面で、データセットに名前を付け、データセットの説明を入力する必要があります。

**データセット名に関する注意事項：**
- 後でライブラリ内で簡単に見つけられるように、データセット名は短く、わかりやすい名前にする必要があります。
- データセット名は一意である必要があります。つまり、今後再利用されないように十分な固有の名前を付ける必要があります。
- ベストプラクティスは、説明フィールドを使用して、データセットに関する追加情報を提供することです。これは、今後、他のユーザーがデータセットを区別する際に役立つ可能性があるためです。

データセットに名前と説明が付いたら、「**[!UICONTROL 完了]**」をクリックします。

![データセットの設定](../images/tutorials/segment-export-dataset/configure-dataset.png)

## データセットアクティビティ

空のデータセットが作成され、データセットワークスペースの「**[!UICONTROL データセットアクティビティ]**」タブに戻りました。****&#x200B;ワークスペースの左上隅にデータセットの名前と、「バッチが追加されていません」という通知が表示されます。このデータセットにバッチをまだ追加していないので、これは期待通りです。

データセットワークスペースの右側には、「**[!UICONTROL 情報]**」タブが表示され、データセットID、名前、説明、テーブル名、スキーマ、ストリーミング、ソースなど、新しいデータセットに関連する情報が示されます。 「**[!UICONTROL 情報]**」タブには、データセットの作成日時と最終変更日に関する情報も含まれています。

**[!UICONTROL データセット ID]** は、オーディエンスセグメントのエクスポートワークフローを完了するために必要になるので、メモしておいてください。

![データセットアクティビティ](../images/tutorials/segment-export-dataset/dataset-activity.png)

## 次の手順

[!DNL XDM Individual Profile Union Schema]に基づいてデータセットを作成したら、データセットIDを使用して[セグメント結果の評価とアクセスのチュートリアル](./evaluate-a-segment.md)に進むことができます。

現時点では、評価セグメントの結果のチュートリアルに戻り、セグメントのエクスポートの[オーディエンスメンバ](./evaluate-a-segment.md#generate-profiles)のプロファイルの生成手順から進んでください。
