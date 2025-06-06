---
title: さらに表示
description: SQL 分析されたデータに対する様々な表示オプションについて説明します。 カスタムダッシュボードから、分析の表形式の結果を表示したり、処理されたデータを CSV 形式でダウンロードしたりできます。
exl-id: f57d85cf-dbd2-415c-bf01-8faa49871377
source-git-commit: ddf886052aedc025ff125c03ab63877cb049583d
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 1%

---

# さらに表示 {#view-more}

[&#128279;](./overview.md#query-pro-mode)query pro モード [ で ](./overview.md) カスタムインサイト  を作成したら、様々な形式でグラフデータを表示できます。 結果の表形式を表示するか、データを CSV ファイルとしてダウンロードしてスプレッドシートに表示できます。

## 表形式の結果 {#tabulated-results}

SQL から Query pro モードを使用して作成したすべてのグラフについて、Experience PlatformUI で表形式の分析結果を表示できます。

カスタムダッシュボードから、任意のウィジェットの省略記号（`...`）を選択して、「[!UICONTROL &#x200B; さらに表示 &#x200B;]」および「[!UICONTROL SQL を表示 &#x200B;]」オプションにアクセスします。

![ インサイトの省略記号ドロップダウンメニューと、「さらに表示」オプションおよび「SQL を表示」オプションがハイライト表示されたカスタムダッシュボード。](../images/sql-insights-query-pro-mode/ellipses-dropdown.png)

## CSV をダウンロード {#download-csv}

[!UICONTROL &#x200B; さらに表示 &#x200B;] 機能は、グラフの特定のデータポイントを表形式で表示します。 データの共有と操作のプロセスを簡素化するために、このダイアログから処理済みのデータを CSV 形式でダウンロードできます。 「**[!UICONTROL CSV をダウンロード]**」を選択して、データをダウンロードします。

>[!NOTE]
>
>CSV のダウンロードは、最初の 500 レコードに制限されます。

![ インサイトのプレビューと、インサイトを生成した SQL の結果を表形式で表示するダイアログ。](../images/sql-insights-query-pro-mode/view-more-download-csv.png)

## 列で並べ替え {#sort-column}

表形式の結果を表示する場合、並べ替え機能を使用して、列を昇順または降順で並べ替えることができます。 カスタムダッシュボードから、任意のテーブルの省略記号（`...`）を選択して、「[!UICONTROL &#x200B; さらに表示 &#x200B;] オプションにアクセスします。

![ テーブルの省略記号ドロップダウンメニューと「さらに表示」オプションがハイライト表示されたカスタムダッシュボード。](../images/sql-insights-query-pro-mode/advanced-ellipses-dropdown.png)

列名の横にあるドロップダウンメニューを選択し、「昇順で並べ替え **[!UICONTROL または「降順で並べ替え]** を選択すると、列を並べ替えるこ **[!UICONTROL ができ]** す。

>[!NOTE]
>
>[!UICONTROL &#x200B; 昇順で並べ替え &#x200B;] および [!UICONTROL &#x200B; 降順で並べ替え &#x200B;] オプションは、[ 並べ替え機能 ](./overview.md#advanced-attributes) で設定された列にのみ表示されます。

![ 「昇順で並べ替え」オプションと「降順で並べ替え」オプションがハイライト表示されたテーブル列のドロップダウン。](../images/sql-insights-query-pro-mode/advanced-sort-dropdown.png)

## 列のサイズ変更 {#resize-column}

表形式の結果の列のサイズを変更して、データを読みやすくすることができます。 カスタムダッシュボードから、テーブルの省略記号（`...`）を選択して、「[!UICONTROL &#x200B; 詳細を表示 &#x200B;]」オプションにアクセスします。 列名の横にあるドロップダウンメニューを使用して列名のサイズを変更し、「**[!UICONTROL 列のサイズを変更]**」を選択します。

![ 「列をサイズ変更」オプションがハイライト表示されたテーブル列ドロップダウン。](../images/sql-insights-query-pro-mode/advanced-resize-dropdown.png)

スライダーを選択して左右にドラッグし、必要に応じて列サイズを調整します。

![ 列のリサイズバーがハイライト表示されているテーブル。](../images/sql-insights-query-pro-mode/advanced-resize-column.png)

## テーブルのページネーション {#table-pagination}

[!UICONTROL &#x200B; 詳細を表示 &#x200B;] 機能で、ページネーションがテーブルに自動的に適用されるので、SQL クエリを手動で変更する必要はありません。 この機能により、より管理しやすい形式でデータが表示され、大きなデータセット間を簡単に移動できるようになります。

1 ページにつき最大 500 件のレコードを表示できます。 レコード間を移動するには、ページ下部の **[!UICONTROL >]** を使用します。

![ 結果とページネーションがハイライト表示された表形式の結果。](../images/sql-insights-query-pro-mode/advanced-table-pagination.png)

## 次の手順

このドキュメントでは、カスタムグラフの SQL 分析の表形式の結果を表示し、データを CSV ファイルとしてダウンロードする方法を確認しました。 [ カスタムインサイトの背後にある SQL を表示 ](./view-sql.md) する方法については、SQL を表示ドキュメントを参照してください。

また、[ ガイド付きデザインモードガイド ](../standard-dashboards.md) を使用して、Adobe Experience Platform UI で既存のデータモデルからグラフを生成する方法についても説明します。
