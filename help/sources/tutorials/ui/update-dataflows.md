---
keywords: Experience Platform；ホーム；人気の高いトピック；データフローの更新；スケジュールの編集
description: このチュートリアルでは、ソースワークスペースを使用して、取得頻度や間隔率など、データフロースケジュールを更新する手順を説明します。
solution: Experience Platform
title: UI でのソース接続データフローの更新
topic-legacy: overview
type: Tutorial
exl-id: 0499a2a3-5a22-47b1-ac0e-76a432bd26c0
source-git-commit: 6a9ad0ce5d664e3b32cab4183b54fabd5d9d19e3
workflow-type: tm+mt
source-wordcount: '724'
ht-degree: 14%

---

# UI でのデータフローの更新

このチュートリアルでは、ソースワークスペースを使用して、スケジュールやマッピングを含む既存のデータフローを更新する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## データフローの更新

Platform の UI で、左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。選択 **[!UICONTROL データフロー]** をクリックして、既存のデータフローのリストを表示します。

![カタログ](../../images/tutorials/update-dataflows/catalog.png)

この [!UICONTROL データフロー] ページには、既存のすべてのデータフローのリストが含まれています。これには、対応するターゲットデータセット、ソース、アカウント名に関する情報も含まれます。

リストで並べ替えるには、フィルターアイコンを選択します。 ![フィルター](../../images/tutorials/update/filter.png) を左上に表示して、並べ替えパネルを使用します。

![filter-dataflows](../../images/tutorials/update-dataflows/filter-dataflows.png)

並べ替えパネルには、使用可能なすべてのソースのリストが表示されます。 リストから複数のソースを選択して、異なるソースに属するデータフローのフィルタリングされた選択にアクセスできます。

既存のデータフローのリストを表示するソースを選択します。 更新するデータフローを特定したら、省略記号 (`...`) をクリックします。

![edit-source](../../images/tutorials/update-dataflows/edit-source.png)

ドロップダウンメニューが表示され、選択したデータフローを更新するためのオプションが提供されます。 ここから、データフローのマッピングセットおよび取り込みスケジュールを更新するように選択できます。 また、監視ダッシュボードでのデータフローの調査、アラートのサブスクライブ、データフローの無効化または削除のオプションを選択することもできます。

データフローの情報を更新するには、「 」を選択します。 **[!UICONTROL データフローを更新]**.

![update-dataflow](../../images/tutorials/update-dataflows/update-dataflow.png)

### データの追加

「[!UICONTROL データ追加]」手順が表示されます。適切なデータ形式を選択して、選択したデータの内容を確認し、「 」を選択します。 **[!UICONTROL 次へ]** をクリックして続行します。

![add-data](../../images/tutorials/update-dataflows/add-data.png)

### データフローの詳細

内 [!UICONTROL データフローの詳細] 」ページで、データフローの更新された名前と説明を指定したり、データフローのエラーしきい値を再設定したりできます。 この手順の間に、アラート配信登録の設定を構成または変更することもできます。

更新した値を指定したら、「 」を選択します。 **[!UICONTROL 次へ]**.

![dataflow-detail](../../images/tutorials/update-dataflows/dataflow-detail.png)

### マッピング

>[!NOTE]
>
>次のソースでは、マッピング編集機能は現在サポートされていません。Adobe Analytics、Adobe Audience Manager、HTTP API、 [!DNL Marketo Engage].

この [!UICONTROL マッピング] ページには、データフローに関連付けられたマッピングセットを追加および削除できるインターフェイスが用意されています。

マッピングインターフェイスには、新しい推奨マッピングセットではなく、データフローの既存のマッピングセットが表示されます。 マッピングの更新は、将来スケジュールされるデータフロー実行に対してのみ適用されます。 1 回限りの取り込み用にスケジュールされたデータフローでは、そのマッピングセットを更新することはできません。

ここから、マッピングインターフェイスを使用して、データフローに適用するマッピングセットを変更できます。 マッピングインターフェイスの使用方法に関する包括的な手順については、 [data prep UI ガイド](../../../data-prep/ui/mapping.md) を参照してください。

![マッピング](../../images/tutorials/update-dataflows/mapping.png)

### スケジュール設定

この [!UICONTROL スケジュール] 手順が表示され、データフローの取り込みスケジュールを更新し、選択したソースデータを更新されたマッピングで自動的に取り込むことができます。

>[!NOTE]
>
>1 回限りの取り込み用にスケジュールされたデータフローを再スケジュールすることはできません。

![新規スケジュール](../../images/tutorials/update-dataflows/new-schedule.png)

また、データフローページで提供されるインライン更新オプションを使用して、データフローの取り込みスケジュールを更新することもできます。

データフローページで、省略記号 (`...`) をクリックし、「 」を選択します。 **[!UICONTROL スケジュールを編集]** を選択します。

![編集スケジュール](../../images/tutorials/update-dataflows/edit-schedule.png)

この **[!UICONTROL スケジュールを編集]** ダイアログボックスに、データフローの取り込み頻度と間隔レートを更新するオプションが表示されます。 更新した頻度と間隔の値を設定したら、 **[!UICONTROL 保存]**.

![スケジュールポップアップ](../../images/tutorials/update-dataflows/schedule-pop-up.png)

### レビュー

この **[!UICONTROL レビュー]** 手順が表示され、更新前にデータフローを確認できます。

データフローをレビューしたら、「 」を選択します。 **[!UICONTROL 完了]** 新しいマッピングセットを作成してデータフローを作成するのにしばらく時間がかかります。

![レビュー](../../images/tutorials/update-dataflows/review.png)

## 次の手順

このチュートリアルでは、 [!UICONTROL ソース] workspace を使用して、データフローの取り込みスケジュールおよびマッピングセットを更新します。

を使用してこれらの操作をプログラムで実行する手順については、 [!DNL Flow Service] API( [フローサービス API を使用したデータフローの更新](../../tutorials/api/update-dataflows.md).
