---
keywords: Experience Platform；ホーム；人気のトピック；データフローの更新；スケジュールの編集
description: このチュートリアルでは、ソースワークスペースを使用して、取り込み頻度やインターバルレートなどのデータフロースケジュールを更新する手順を説明します。
solution: Experience Platform
title: UI でのSource接続データフローの更新
type: Tutorial
exl-id: 0499a2a3-5a22-47b1-ac0e-76a432bd26c0
source-git-commit: c2832821ea6f9f630e480c6412ca07af788efd66
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 20%

---

# UI でのデータフローの更新

このチュートリアルでは、ソースワークスペースを使用して、スケジュールやマッピングなど、既存のデータフローを更新する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## データフローの更新 {#update-dataflows}

>[!CONTEXTUALHELP]
>id="platform_sources_dataflows_daysRemaining"
>title="データセットの有効期限"
>abstract="この列は、ターゲットデータセットが自動的に期限切れになるまでの残り日数を示します。<br>ターゲットデータセットの有効期限が切れると、データフローは失敗します。データフローが失敗しないようにするには、ターゲットデータセットが正しい日付に期限切れになるように設定されていることを確認します。有効期限を更新する方法については、ドキュメントを参照してください。"

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して、[!UICONTROL ソース]ワークスペースにアクセスします。上部のヘッダーから **[!UICONTROL データフロー]** を選択して、既存のデータフローのリストを表示します。

![カタログ](../../images/tutorials/update-dataflows/catalog.png)

[!UICONTROL  データフロー ] ページには、対応するターゲットデータセット、ソース、アカウント名など、既存のすべてのデータフローのリストが含まれています。

リストを並べ替えるには、左上のフィルターアイコン ![ フィルター ](/help/images/icons/filter.png) を選択して、並べ替えパネルを使用します。

![filter-dataflows](../../images/tutorials/update-dataflows/filter-dataflows.png)

並べ替えパネルには、使用可能なすべてのソースのリストが表示されます。 リストから複数のソースを選択して、異なるソースに属するフィルタリングされたデータフローの選択にアクセスできます。

操作するソースを選択して、既存のデータフローのリストを表示します。 更新するデータフローを特定したら、データフロー名の横にある省略記号（`...`）を選択します。

![edit-source](../../images/tutorials/update-dataflows/edit-source.png)

ドロップダウンメニューが表示され、選択したデータフローを更新するオプションが表示されます。 ここから、データフローのマッピングセットと取り込みスケジュールを更新するように選択できます。 また、監視ダッシュボードでデータフローを検査したり、アラートを登録したり、データフローを無効または削除したりするためのオプションを選択できます。

データフローの情報を更新するには、「**[!UICONTROL データフローを更新]**」を選択します。

![update-dataflow](../../images/tutorials/update-dataflows/update-dataflow.png)

### データの追加

[!UICONTROL データを追加]手順が表示されます。適切なデータ形式を選択して、選択したデータの内容を確認してから、「**[!UICONTROL 次へ]**」を選択して続行します。

![add-data](../../images/tutorials/update-dataflows/add-data.png)

### データフローの詳細

[!UICONTROL  データフローの詳細 ] ページでは、データフローの名前と説明を更新したり、データフローのエラーしきい値を再設定したりできます。 この手順では、アラート配信登録の設定を構成または変更することもできます。

更新した値を指定したら、「**[!UICONTROL 次へ]**」を選択します。

![dataflow-detail](../../images/tutorials/update-dataflows/dataflow-detail.png)

### マッピング

>[!NOTE]
>
>マッピングを編集機能は、現在、Adobe Analytics、Adobe Audience Manager、HTTP API および [!DNL Marketo Engage] のソースではサポートされていません。

[!UICONTROL  マッピング ] ページには、データフローに関連付けられたマッピングセットを追加および削除できるインターフェイスが用意されています。

マッピングインターフェイスには、データフローの新しい推奨マッピングセットではなく、既存のマッピングセットが表示されます。 マッピングの更新は、今後予定されるデータフロー実行にのみ適用されます。 1 回限りの取り込み用にスケジュールされたデータフローでは、マッピングセットを更新できません。

ここから、マッピングインターフェイスを使用して、データフローに適用するマッピングセットを変更できます。 マッピングインターフェイスの使用方法に関する包括的な手順については、[ データ準備 UI ガイド ](../../../data-prep/ui/mapping.md) を参照してください。

![マッピング](../../images/tutorials/update-dataflows/mapping.png)

### スケジュール設定

[!UICONTROL  スケジュール ] 手順が表示され、データフローの取り込みスケジュールを更新し、選択したソースデータを更新されたマッピングで自動的に取り込むことができます。

>[!NOTE]
>
>1 回限りの取り込み用にスケジュールされたデータフローは、再スケジュールできません。

![new-schedule](../../images/tutorials/update-dataflows/new-schedule.png)

データフローページにある「インライン更新」オプションを使用して、データフローの取り込みスケジュールを更新することもできます。

データフローページで、データフロー名の横にある省略記号（`...`）を選択し、表示されるドロップダウンメニューから **[!UICONTROL スケジュールを編集]** を選択します。

![ スケジュールを編集 ](../../images/tutorials/update-dataflows/edit-schedule.png)

**[!UICONTROL スケジュールを編集]** ダイアログボックスには、データフローの取り込み頻度と間隔率を更新するオプションが用意されています。 更新した頻度と間隔の値を設定したら、「**[!UICONTROL 保存]**」を選択します。

![ スケジュールポップアップ ](../../images/tutorials/update-dataflows/schedule-pop-up.png)

### レビュー

**[!UICONTROL レビュー]** 手順が表示され、データフローを更新する前にレビューすることができます。

データフローをレビューしたら、「**[!UICONTROL 終了]**」を選択し、新しいマッピングセットが作成されるデータフローをしばらく待ちます。

![レビュー](../../images/tutorials/update-dataflows/review.png)

## 次の手順

このチュートリアルでは、[!UICONTROL  ソース ] ワークスペースを正常に使用して、データフローの取り込みスケジュールとマッピングセットを更新しました。

[!DNL Flow Service] API を使用してこれらの操作をプログラムで実行する手順については、[Flow Service API を使用したデータフローの更新 ](../../tutorials/api/update-dataflows.md) のチュートリアルを参照してください。
