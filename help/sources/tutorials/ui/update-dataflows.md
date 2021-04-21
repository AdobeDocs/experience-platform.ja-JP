---
keywords: Experience Platform；ホーム；人気のあるトピック；更新データフロー；編集スケジュール
description: このチュートリアルでは、ソース・ワークスペースを使用して、データ・フロー・スケジュール（取り込み頻度や間隔率など）を更新する手順を説明します。
solution: Experience Platform
title: UIでのソース接続データフローの更新
topic-legacy: overview
type: Tutorial
exl-id: 0499a2a3-5a22-47b1-ac0e-76a432bd26c0
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '744'
ht-degree: 6%

---

# UIのデータフローの更新

このチュートリアルでは、[!UICONTROL Sources]ワークスペースを使用したデータフロースケジュールおよびマッピングの編集に関する情報を含む、既存のソースのデータフローを更新する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [ソース](../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込むことができ、Platform Servicesを使用して、データの構造化、ラベル付け、および入力データの拡張を行うことができます。
- [サンドボックス](../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

## マッピングの編集

>[!NOTE]
>
>マッピングの編集機能は、現在、次のソースではサポートされていません。Adobe Analytics、Adobe Audience Manager、HTTP API、[!DNL Marketo Engage]。

プラットフォームUIで、左側のナビゲーションから「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。 上部のヘッダーから「**[!UICONTROL Dataflows]**」を選択し、既存のデータフローのリストを表示します。

![カタログ](../../images/tutorials/update-dataflows/catalog.png)

[!UICONTROL データフロー]ページには、実行ステータス、最終実行日、アカウント名など、既存のすべてのデータフローのリストが含まれます。

左上のフィルターアイコン![フィルター](../../images/tutorials/update/filter.png)を選択して、並べ替えパネルを起動します。

![filter-dataflows](../../images/tutorials/update-dataflows/filter-dataflows.png)

並べ替えパネルには、使用可能なすべてのソースのリストが表示されます。 リストから複数のソースを選択して、異なるソースに属するデータフローのフィルタ選択にアクセスできます。

既存のデータフローのリストを表示するために使用するソースを選択します。 更新するデータフローを特定したら、アカウント名の横の三点リーダー(`...`)を選択します。

![編集元](../../images/tutorials/update-dataflows/edit-source.png)

ドロップダウンメニューが表示され、選択したデータフローを更新するオプションが表示されます。 ここから、データフローのマッピングセットおよびインジェストスケジュールを更新できます。 監視ダッシュボードのデータフローを検査するオプションや、データフローを無効または削除するオプションを選択することもできます。

「**[!UICONTROL ソース]**&#x200B;を編集」を選択して、マッピングを更新します。

![edit-dataflow](../../images/tutorials/update-dataflows/edit-dataflow.png)

「[!UICONTROL データ追加]」手順が表示されます。適切なデータ形式を選択して、選択したデータの内容を確認し、**[!UICONTROL 次へ]**&#x200B;を選択して次に進みます。

![add-data](../../images/tutorials/update-dataflows/add-data.png)

[!UICONTROL マッピング]ページには、データセットに関連付けられたマッピングセットを追加および削除できるインターフェイスが用意されています。

>[!TIP]
>
>マッピングの更新は、将来スケジュールされるデータフローの実行にのみ適用されます。

新しいマッピングセットを追加するには、追加&#x200B;**[!UICONTROL 新しいマッピング]**&#x200B;を選択します。

![add-new-mapping](../../images/tutorials/update-dataflows/add-new-mapping.png)

次に、適切なソースフィールド属性とターゲットXDMフィールドの値を入力し、追加のマッピングセットを完了します。 「**[!UICONTROL 次へ]**」を選択して次に進みます。

![新しいマッピングが追加された](../../images/tutorials/update-dataflows/new-mapping-added.png)

[!UICONTROL スケジュール]ステップが表示され、データフローの取り込みスケジュールを更新し、選択したソースデータを更新したマッピングで自動的に取り込むことができます。

>[!NOTE]
>
>1回限りの取り込みと開始時間が過去のデータフローのマッピングセットは更新できません。

![スケジュール](../../images/tutorials/update-dataflows/scheduling.png)

[!UICONTROL Dataflow detail]ページでは、データフローの更新された名前と説明を指定でき、データフローのエラーしきい値を再構成できます。

更新した値を入力したら、「**[!UICONTROL 次へ]**」を選択します。

![データフロー詳細](../../images/tutorials/update-dataflows/dataflow-detail.png)

**[!UICONTROL レビュー]**&#x200B;ステップが表示され、データフローを更新前に確認できます。

データフローをレビューしたら、「**[!UICONTROL Finish]**」を選択し、新しいマッピングセットを含むデータフローを作成する時間を設定します。

![レビュー](../../images/tutorials/update-dataflows/review.png)

## スケジュールを編集

既存のデータフローのインジェストスケジュールを編集するには、データフロー名の横にある三点リーダー(`...`)を選択し、ドロップダウンメニューから[**[!UICONTROL スケジュール]**&#x200B;の編集]を選択します。

![スケジュールの編集](../../images/tutorials/update-dataflows/edit-schedule.png)

**[!UICONTROL スケジュールを編集]**&#x200B;ダイアログボックスには、データフローの取り込み頻度と間隔を更新するオプションが表示されます。 更新した頻度と間隔の値を設定したら、「**[!UICONTROL 保存]**」を選択します。

>[!NOTE]
>
>1回限りの取り込みにスケジュールされたデータフローを再スケジュールすることはできません。

![schedule-dialog-box](../../images/tutorials/update-dataflows/schedule-dialog-box.png)

| スケジュール設定 | 説明 |
| ---------- | ----------- |
| 頻度 | データフローがデータを収集する頻度。 既存のデータフローの頻度スケジュールを編集する場合に指定できる値は、次のとおりです。`minute`、`hour`、`day`、または`week`。 |
| 間隔 | この間隔は、連続する2つのフローの実行間隔を指定します。 間隔の値は、ゼロ以外の整数で、`15`以上である必要があります。 |

しばらくすると、画面下部に、更新が成功したことを確認する確認ボックスが表示されます。

![スケジュール確認](../../images/tutorials/update-dataflows/schedule-confirm.png)

## 次の手順

このチュートリアルに従うと、[!UICONTROL ソース]ワークスペースを使用して、データフローのインジェストスケジュールとマッピングセットを更新できます。

[!DNL Flow Service] APIを使用してプログラム的にこれらの操作を実行する手順については、[Flow Service API](../../tutorials/api/update-dataflows.md)を使用したデータフローの更新に関するチュートリアルを参照してください。
