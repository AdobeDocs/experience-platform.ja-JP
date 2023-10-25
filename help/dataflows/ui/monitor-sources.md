---
keywords: Experience Platform；ホーム；人気の高いトピック；アカウントの監視；データフローの監視；データフロー；ソース
description: このチュートリアルでは、集計された監視ビューとクロスサービス監視の両方を使用してデータフローを監視する手順を説明します。
solution: Experience Platform
title: UI でのソースのデータフローの監視
type: Tutorial
exl-id: 53fa4338-c5f8-4e1a-8576-3fe13d930846
source-git-commit: 1a7ba52b48460d77d0b7695aa0ab2d5be127d921
workflow-type: tm+mt
source-wordcount: '1069'
ht-degree: 29%

---

# UI でのソースのデータフローの監視

>[!IMPORTANT]
>
>ストリーミングソース ( [HTTP API ソース](../../sources/connectors/streaming/http.md) は、現在、監視ダッシュボードではサポートされていません。 現時点では、ダッシュボードを使用してバッチソースを監視することのみできます。

Adobe Experience Platform では、様々なソースからデータを取り込み、Experience Platform 内で分析し、様々な目的でアクティブ化します。Platform では、データフローに透明性を提供することで、この非線形の可能性があるデータフローのトラッキングプロセスを容易にします。

監視ダッシュボードは、データフローのジャーニーを視覚的に表します。 集計された監視ビューを使用し、ソースレベルからデータフロー、およびデータフローの実行に垂直に移動して、データフローの成功または失敗に貢献する対応する指標を表示できます。 また、監視ダッシュボードのクロスサービス監視機能を使用して、ソースからへのデータフローのジャーニーを監視することもできます。 [!DNL Identity Service]、およびに [!DNL Profile].

このチュートリアルでは、集計された監視ビューとクロスサービス監視の両方を使用してデータフローを監視する手順を説明します。

## はじめに {#getting-started}

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [データフロー](../home.md)：データフローは、Platform 間でデータを移動するデータジョブを表します。データフローは異なるサービスをまたいで設定され、ソースコネクタからターゲットデータセット、[!DNL Identity] および [!DNL Profile]、[!DNL Destinations] へとデータを移動できます。
   * [データフロー実行](../../sources/notifications.md)：データフロー実行は、選択したデータフローの頻度設定に基づいて繰り返しスケジュールされたジョブです。
* [ソース](../../sources/home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ID サービス](../../identity-service/home.md)：デバイスやシステム間で ID を橋渡しすることで、個々の顧客とその行動をより確実に把握することができます。
* [リアルタイム顧客プロファイル](../../profile/home.md)：複数のソースから集計したデータに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
* [サンドボックス](../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## 集計された監視ビュー {#aggregated-monitoring-view}

>[!CONTEXTUALHELP]
>id="platform_monitoring_source_ingestion"
>title="ソース取り込み"
>abstract="ソース取り込みビューには、データレイクサービスのデータアクティビティステータスや指標に関する情報 (取り込まれたレコードや失敗したレコードなど) が表示されます。指標およびグラフについて詳しくは、指標定義ガイドを参照してください。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_monitoring_dataflow_run_details_ingestion"
>title="データフロー実行の詳細"
>abstract="ソース処理ビューには、データレイクサービスのデータアクティビティステータスや指標に関する情報 (取り込まれたレコードや失敗したレコードなど) が表示されます。指標およびグラフについて詳しくは、指標定義ガイドを参照してください。"
>text="Learn more in documentation"

Adobe Analytics の [Platform UI](https://platform.adobe.com)を選択します。 **[!UICONTROL 監視]** 左側のナビゲーションから、 [!UICONTROL 監視] ダッシュボード。 The [!UICONTROL 監視] ダッシュボードには、ソースからソースへのデータトラフィックの状態に関するインサイトを含む、すべてのソースデータフローに関する指標と情報が含まれます。 [!DNL Identity Service]、およびに [!DNL Profile].

ダッシュボードの中央には、 [!UICONTROL ソースの取り込み] 取得されたレコードと失敗したレコードに関するデータを表示する指標とグラフを含むパネル。

![monitoring-dashboard](../assets/ui/monitor-sources/monitoring-dashboard.png)

デフォルトでは、表示されるデータには、過去 24 時間のインジェスト率が含まれています。 「**[!UICONTROL 過去 24 時間]**」を選択すると、表示されるレコードの時間枠を調整できます。 

![change-date](../assets/ui/monitor-sources/change-date.png)

カレンダーポップアップウィンドウが開き、別の取り込み時間枠のオプションが表示されます。 選択 **[!UICONTROL 過去 30 日間]** 次に、「 **[!UICONTROL 適用]**

![adjust-time-frame](../assets/ui/monitor-sources/adjust-timeframe.png)

グラフはデフォルトで有効になっています。無効にすると、以下のソースのリストが展開されます。 グラフを無効にするには、「**[!UICONTROL 指標とグラフ]**」切替スイッチを選択します。

![指標とグラフ](../assets/ui/monitor-sources/metrics-graphs.png)

| ソース取り込み | 説明 |
| ---------------- | ----------- |
| [!UICONTROL 取り込まれたレコード] | 取り込まれたレコードの合計数。 |
| [!UICONTROL 失敗したレコード] | データのエラーが原因で取り込まれなかったレコードの合計数です。 |
| [!UICONTROL 失敗したデータフローの合計] | を含むデータフローの合計数 `failed` ステータス。 |

ソースの取り込みリストには、1 つ以上の既存のアカウントを含むすべてのソースが表示されます。 また、このリストには、適用した時間枠に基づく、各ソースの取り込み率、失敗したレコードの数、失敗したデータフローの合計数に関する情報も含まれます。

![source-ingestion](../assets/ui/monitor-sources/source-ingestion.png)

ソースのリストを並べ替えるには、「 **[!UICONTROL マイソース]** 次に、ドロップダウンメニューから選択したカテゴリを選択します。 例えば、クラウドストレージに焦点を当てるには、「  **[!UICONTROL クラウドストレージ]**

![カテゴリ別に並べ替え](../assets/ui/monitor-sources/sort-by-category.png)

すべてのソースの既存のデータフローをすべて表示するには、「 」を選択します。 **[!UICONTROL データフロー]**.

![view-all-dataflows](../assets/ui/monitor-sources/view-all-dataflows.png)

または、検索バーにソースを入力して、1 つのソースを分離することもできます。 ソースを特定したら、フィルターアイコンを選択します。 ![フィルター](../assets/ui/monitor-sources/filter.png) その横に、アクティブなデータフローのリストを表示します。

![検索](../assets/ui/monitor-sources/search.png)

データフローのリストが表示されます。 リストを絞り込み、エラーのあるデータフローに焦点を当てるには、「 」を選択します。 **[!UICONTROL 失敗のみを表示]**.

![show-failures-only](../assets/ui/monitor-sources/show-failures-only.png)

監視するデータフローを見つけ、フィルターアイコンを選択します。 ![フィルター](../assets/ui/monitor-sources/filter.png) の横に、実行ステータスの詳細を表示します。

![データフロー](../assets/ui/monitor-sources/dataflow.png)

データフローの実行ページには、データフローの実行開始日、データのサイズ、ステータス、および処理時間に関する情報が表示されます。 フィルターアイコンを選択します。 ![フィルター](../assets/ui/monitor-sources/filter.png) をクリックし、データフローの実行の詳細を確認します。

![dataflow-run-start](../assets/ui/monitor-sources/dataflow-run-start.png)

The [!UICONTROL データフローの実行の詳細] ページには、データフローのメタデータ、部分取り込みステータス、エラー概要に関する情報が表示されます。 エラー概要には、取り込みプロセスでエラーが発生した手順を示す特定の最上位エラーが含まれます。

下にスクロールして、発生したエラーに関する詳細情報を確認します。

![dataflow-run-details](../assets/ui/monitor-sources/dataflow-run-details.png)

The [!UICONTROL データフロー実行エラー] パネルには、データフローの取り込み失敗を引き起こした特定のエラーおよびエラーコードが表示されます。 このシナリオでは、マッパー変換エラーが発生し、結果として 24 件のレコードが失敗しました。

選択 **[!UICONTROL ファイル]** を参照してください。

![dataflow-run-errors](../assets/ui/monitor-sources/dataflow-run-errors.png)

The [!UICONTROL ファイル] パネルには、ファイルの名前とパスに関する情報が含まれます。

エラーの詳細な表現を得るには、 **[!UICONTROL エラー診断をプレビュー]**.

![files](../assets/ui/monitor-sources/files.png)

The [!UICONTROL エラー診断のプレビュー] ウィンドウが表示され、データフローで最大 100 個のエラーのプレビューが表示されます。 次の項目を選択できます。 **[!UICONTROL ダウンロード]** を使用して curl コマンドを取得します。このコマンドを使用すると、エラー診断をダウンロードできます。

完了したら、「 **[!UICONTROL 閉じる]**

![error-diagnostics](../assets/ui/monitor-sources/error-diagnostics.png)

上部のヘッダーにあるパンくずリストシステムを使用すると、 [!UICONTROL 監視] ダッシュボード。 選択 **[!UICONTROL 実行開始：2/14/2021、9:47 PM]** 前のページに戻り、「 **[!UICONTROL データフロー：ロイヤルティデータ取り込みのデモ — 失敗]** をクリックして、データフローページに戻ります。

![パンくず](../assets/ui/monitor-sources/breadcrumbs.png)

## 次の手順 {#next-steps}

このチュートリアルに従うことで、 **[!UICONTROL 監視]** ダッシュボード。 また、取り込みプロセス中にデータフローが失敗した原因となるエラーも正常に識別されました。 詳しくは、次のドキュメントを参照してください。

* [データフローの ID の監視](./monitor-identities.md)
* [データフローのプロファイルの監視](./monitor-profiles.md)
