---
keywords: Experience Platform;JupyterLab；ノートブック；Data Science Workspace；人気のあるトピック；データノートブックの分析；eda；調査データ分析；データサイエンス
solution: Experience Platform
title: Exploratory Data Analysis(EDA) ノートブック
topic-legacy: overview
type: Tutorial
description: このガイドでは、調査データ分析 (EDA) ノートブックを使用して、Web データのパターンを検出する方法、予測目標を持つ集計イベント、集計データを消去する方法、および述語と目標の関係を理解する方法に焦点を当てています。
exl-id: 48209326-0a07-4b5c-8b49-a2082a78fa47
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '2760'
ht-degree: 1%

---

# 調査データ分析 (EDA) ノートブックを使用した予測モデルの Web ベースのデータの調査

調査データ分析 (EDA) ノートブックは、データのパターンの検出、データの正常性の確認、および予測モデルに関連するデータの要約を支援するように設計されています。

EDA ノートブックの例は、Web ベースのデータを考慮して最適化され、2 つの部分で構成されています。 第 1 部では、クエリサービスを使用して、トレンドとデータのスナップショットを表示します。 次に、調査データの分析を念頭に置いて、データをプロファイルと訪問者のレベルで集計します。

第 2 部は、Python ライブラリを使用して集計データに対して記述的分析を実行することから始まります。 このノートブックは、ヒストグラム、散布グラフ、ボックスプロット、相関行列などのビジュアライゼーションを示し、目標の予測に役立つと思われる機能を判断するために使用できる実用的なインサイトを導き出します。

## はじめに

このガイドを読む前に、[[!DNL JupyterLab]  ユーザーガイド ](./overview.md) を参照し、[!DNL JupyterLab] の概要と Data Science Workspace 内での役割について確認してください。 また、独自のデータを使用している場合は、 [!DNL Jupyterlab]  ノートブック ](./access-notebook-data.md) の [ データアクセスに関するドキュメントを参照してください。 このガイドでは、ノートブックデータの制限に関する重要な情報を説明します。

このノートブックでは、Analytics Analysis WorkspaceにあるAdobe Analytics Experience Events データの形式の中間値データセットを使用します。 EDA ノートブックを使用するには、次の値 `target_table` と `target_table_id` を使用してデータテーブルを定義する必要があります。 任意の中間値データセットを使用できます。

これらの値を見つけるには、JupyterLab データアクセスガイドの python](./access-notebook-data.md#write-python) でのデータセットへの書き込みの手順に従います。 [データセット名 (`target_table`) は、データセットディレクトリ内にあります。 データセットを右クリックしてノートブックにデータを調査または書き込むと、実行可能コードエントリにデータセット ID(`target_table_id`) が提供されます。

## データ検出

この節では、「ユーザーアクティビティ別の上位 10 都市」や「閲覧された製品の上位 10 都市」などのトレンドの表示に使用される設定手順とクエリ例を示します。

### ライブラリの設定

JupyterLab は複数のライブラリをサポートします。 次のコードは、コードセルに貼り付けて実行し、この例で使用する必要なパッケージをすべて収集してインストールできます。 独自のデータ分析では、この例以外の追加または代替パッケージを使用できます。 サポートされているパッケージの一覧を表示するには、`!pip list --format=columns` をコピーして新しいセルに貼り付けます。

```python
!pip install colorama
import chart_studio.plotly as py
import plotly.graph_objs as go
from plotly.offline import iplot
from scipy import stats
import numpy as np
import warnings
warnings.filterwarnings('ignore')
from scipy.stats import pearsonr
import matplotlib.pyplot as plt
from scipy.stats import pearsonr
import pandas as pd
import math
import re
import seaborn as sns
from datetime import datetime
import colorama
from colorama import Fore, Style
pd.set_option('display.max_columns', None)
pd.set_option('display.max_rows', None)
pd.set_option('display.width', 1000)
pd.set_option('display.expand_frame_repr', False)
pd.set_option('display.max_colwidth', -1)
```

### Adobe Experience Platformに接続 [!DNL Query Service]

[!DNL JupyterLab] Platform では、ノートブックで SQL を使用して、ク [!DNL Python] エリサービスを通じてデータにア [クセスできます](https://docs.adobe.com/content/help/ja-JP/experience-platform/query/home.html)。[!DNL Query Service] を通じたデータへのアクセスは、実行時間が優れているので、大規模なデータセットを扱う場合に便利です。 [!DNL Query Service] を使用したデータのクエリには、処理時間の制限は 10 分です。

[!DNL JupyterLab] で [!DNL Query Service] を使用する前に、[[!DNL Query Service] SQL 構文 ](https://docs.adobe.com/content/help/ja-JP/experience-platform/query/home.html#!api-specification/markdown/narrative/technical_overview/query-service/sql/syntax.md) に関する十分な知識があることを確認してください。

JupyterLab のクエリサービスを利用するには、まず作業中の Python ノートブックとクエリサービスの間の接続を作成する必要があります。これは、次のセルを実行することで実現できます。

```python
qs_connect()
```

### 調査用の中間値データセットの定義

データのクエリと調査を開始するには、中間値のデータセットテーブルを指定する必要があります。 `table_name` と `table_id` の値をコピーして、独自のデータテーブル値に置き換えます。

```python
target_table = "table_name"
target_table_id = "table_id"
```

完了すると、このセルは次の例のようになります。

```python
target_table = "cross_industry_demo_midvalues"
target_table_id = "5f7c40ef488de5194ba0157a"
```

### 使用可能な日付のデータセットの調査

以下に示すセルを使用して、表の日付範囲を表示できます。 日数、最初の日付、最終日付を調べる目的は、さらに分析する日付範囲の選択を支援することです。

```python
%%read_sql -c QS_CONNECTION
SELECT distinct Year(timestamp) as Year, Month(timestamp) as Month, count(distinct DAY(timestamp)) as Count_days, min(DAY(timestamp)) as First_date, max(DAY(timestamp)) as Last_date, count(timestamp) as Count_hits
from {target_table}
group by Month(timestamp), Year(timestamp)
order by Year, Month;
```

セルを実行すると、次の出力が生成されます。

![クエリの日付出力](../images/jupyterlab/eda/query-date-output.PNG)

### データセット検出の日付の設定

データセット検出に使用可能な日付を決定したら、以下のパラメーターを更新する必要があります。 このセルで設定された日付は、クエリの形式でのデータ検出にのみ使用されます。 日付が再び更新され、このガイドの後半の調査データ分析に適した範囲になります。

```python
target_year = "2020" ## The target year
target_month = "02" ## The target month
target_day = "(01,02,03)" ## The target days
```

### データセットの検出

[!DNL Query Service] を開始し、日付範囲を設定したら、データ行の読み取りを開始する準備が整います。 読み取る行数は制限する必要があります。

```python
from platform_sdk.dataset_reader import DatasetReader
from datetime import date
dataset_reader = DatasetReader(PLATFORM_SDK_CLIENT_CONTEXT, dataset_id=target_table_id)
# If you do not see any data or would like to expand the default date range, change the following query
Table = dataset_reader.limit(5).read()
```

データセット内の列数を表示するには、次のセルを使用します。

```python
print("\nNumber of columns:",len(Table.columns))
```

データセットの行を表示するには、次のセルを使用します。 この例では、行数は 5 行に制限されています。

```python
Table.head(5)
```

![表の行の出力](../images/jupyterlab/eda/data-table-overview.PNG)

データセットに含まれるデータを把握したら、データセットをさらに分類すると役立ちます。 この例では、各列の列名とデータ型が表示され、出力はデータ型が正しいかどうかを確認するために使用されます。

```python
ColumnNames_Types = pd.DataFrame(Table.dtypes)
ColumnNames_Types = ColumnNames_Types.reset_index()
ColumnNames_Types.columns = ["Column_Name", "Data_Type"]
ColumnNames_Types
```

![列名とデータ型リスト](../images/jupyterlab/eda/data-columns.PNG)

### データセットのトレンド調査

次の節では、データのトレンドとパターンを調べるためのクエリの例を 4 つ示します。 以下の例は完全なものではありませんが、よく参照される機能の一部を示しています。

**指定した日の 1 時間ごとのアクティビティ数**

このクエリは、1 日を通してのアクション数とクリック数を分析します。 出力は、1 日の各時間のアクティビティ数に関する指標を含むテーブルの形式で表されます。

```sql
%%read_sql query_2_df -c QS_CONNECTION

SELECT Substring(timestamp, 12, 2)                        AS Hour, 
       Count(enduserids._experience.aaid.id) AS Count 
FROM   {target_table}
WHERE  Year(timestamp) = {target_year} 
       AND Month(timestamp) = {target_month}  
       AND Day(timestamp) in {target_day}
GROUP  BY Hour
ORDER  BY Hour;
```

![クエリ 1 出力](../images/jupyterlab/eda/hour-count-raw.PNG)

クエリの動作を確認した後、データを一変量プロットヒストグラムに表示して、視覚的にわかりやすくすることができます。

```python
trace = go.Bar(
    x = query_2_df['Hour'],
    y = query_2_df['Count'],
    name = "Activity Count"
)

layout = go.Layout(
    title = 'Activity Count by Hour of Day',
    width = 1200,
    height = 600,
    xaxis = dict(title = 'Hour of Day'),
    yaxis = dict(title = 'Count')
)

fig = go.Figure(data = [trace], layout = layout)
iplot(fig)
```

![クエリ 1 の棒グラフ出力](../images/jupyterlab/eda/activity-count-by-hour-of-day.png)

**特定の日に閲覧された上位 10 ページ**

このクエリは、特定の日に最も多く閲覧されたページを分析します。 出力は、ページ名とページビュー数の指標を含む表の形式で表されます。

```sql
%%read_sql query_4_df -c QS_CONNECTION

SELECT web.webpagedetails.name                 AS Page_Name, 
       Sum(web.webpagedetails.pageviews.value) AS Page_Views 
FROM   {target_table}
WHERE  Year(timestamp) = {target_year}
       AND Month(timestamp) = {target_month}
       AND Day(timestamp) in {target_day}
GROUP  BY web.webpagedetails.name 
ORDER  BY page_views DESC 
LIMIT  10;
```

クエリの動作を確認した後、データを一変量プロットヒストグラムに表示して、視覚的にわかりやすくすることができます。

```python
trace = go.Bar(
    x = query_4_df['Page_Name'],
    y = query_4_df['Page_Views'],
    name = "Page Views"
)

layout = go.Layout(
    title = 'Top Ten Viewed Pages For a Given Day',
    width = 1000,
    height = 600,
    xaxis = dict(title = 'Page_Name'),
    yaxis = dict(title = 'Page_Views')
)

fig = go.Figure(data = [trace], layout = layout)
iplot(fig)
```

![上位 10 件の閲覧ページ](../images/jupyterlab/eda/top-ten-viewed-pages-for-a-given-day.png)

**ユーザーアクティビティ別にグループ化された上位 10 都市**

このクエリは、データの送信元の都市を分析します。

```sql
%%read_sql query_6_df -c QS_CONNECTION

SELECT concat(placeContext.geo.stateProvince, ' - ', placeContext.geo.city) AS state_city, 
       Count(timestamp)                                                     AS Count
FROM   {target_table}
WHERE  Year(timestamp) = {target_year}
       AND Month(timestamp) = {target_month}
       AND Day(timestamp) in {target_day}
GROUP  BY state_city
ORDER  BY Count DESC
LIMIT  10;
```

クエリの動作を確認した後、データを一変量プロットヒストグラムに表示して、視覚的にわかりやすくすることができます。

```python
trace = go.Bar(
    x = query_6_df['state_city'],
    y = query_6_df['Count'],
    name = "Activity by City"
)

layout = go.Layout(
    title = 'Top Ten Cities by User Activity',
    width = 1200,
    height = 600,
    xaxis = dict(title = 'City'),
    yaxis = dict(title = 'Count')
)

fig = go.Figure(data = [trace], layout = layout)
iplot(fig)
```

![上位 10 都市](../images/jupyterlab/eda/top-ten-cities-by-user-activity.png)

**閲覧された製品の上位 10 位**

このクエリは、閲覧された上位 10 件の製品のリストを表示します。 次の例では、`Explode()` 関数を使用して、`productlistitems` オブジェクト内の各製品を独自の行に返します。 これにより、ネストされたクエリを実行して、様々な SKU の製品表示を集計できます。

```sql
%%read_sql query_7_df -c QS_CONNECTION

SELECT Product_List_Items.sku AS Product_SKU,
       Sum(Product_Views) AS Total_Product_Views
FROM  (SELECT Explode(productlistitems) AS Product_List_Items, 
              commerce.productviews.value   AS Product_Views 
       FROM   {target_table}
       WHERE  Year(timestamp) = {target_year}
              AND Month(timestamp) = {target_month}
              AND Day(timestamp) in {target_day}
              AND commerce.productviews.value IS NOT NULL) 
GROUP BY Product_SKU 
ORDER BY Total_Product_Views DESC
LIMIT  10;
```

クエリの動作を確認した後、データを一変量プロットヒストグラムに表示して、視覚的にわかりやすくすることができます。

```python
trace = go.Bar(
    x = "SKU-" + query_7_df['Product_SKU'],
    y = query_7_df['Total_Product_Views'],
    name = "Product View"
)

layout = go.Layout(
    title = 'Top Ten Viewed Products',
    width = 1200,
    height = 600,
    xaxis = dict(title = 'SKU'),
    yaxis = dict(title = 'Product View Count')
)

fig = go.Figure(data = [trace], layout = layout)
iplot(fig)
```

![トップ 10 の製品表示](../images/jupyterlab/eda/top-ten-viewed-products.png)

データの傾向やパターンを調べた後、目標を予測するために、どの機能を構築したいかを適切に把握する必要があります。 テーブルをスキミングすると、各データ属性の形式、明らかな誤った表現、値内の大きな外れ値をすばやく強調表示し、属性間を調べるための候補となる関係を提案できます。

## 探索的データ分析

調査データ分析は、データの理解を深め、モデリングの基礎として使用できる、説得力のある質問の直感を構築するために使用します。

データ検出手順が完了したら、イベント、市区町村またはユーザー ID レベルの集計を使用して、イベントレベルのデータを調べ、1 日のトレンドを確認します。 このデータは重要ですが、全体像は示しません。 Web サイトでの購入を促す要因がまだわかっていない。

これを理解するには、プロファイル/訪問者レベルでデータを集計し、購入目標を定義して、相関、ボックスグラフ、散布グラフなどの統計的概念を適用する必要があります。 これらの方法は、定義した予測ウィンドウで、購入者と非購入者のアクティビティのパターンを比較するために使用されます。

この節では、次の機能を作成および調査します。

- `COUNT_UNIQUE_PRODUCTS_PURCHASED`:購入した個別製品の数。
- `COUNT_CHECK_OUTS`:チェックアウト数。
- `COUNT_PURCHASES`:購入の数。
- `COUNT_INSTANCE_PRODUCTADDS`:製品追加インスタンスの数。
- `NUMBER_VISITS` :訪問の数。
- `COUNT_PAID_SEARCHES`:有料検索の数。
- `DAYS_SINCE_VISIT`:最後の訪問からの経過日数。
- `TOTAL_ORDER_REVENUE`:注文売上高の合計。
- `DAYS_SINCE_PURCHASE`:前回の購入からの日数。
- `AVG_GAP_BETWEEN_ORDERS_DAYS`:購入間の平均ギャップ（日数）。
- `STATE_CITY`:州と市区町村を含みます。

データの集計を続ける前に、調査データ分析で使用する予測変数のパラメーターを定義する必要があります。 つまり、データサイエンスモデルから何を求めているのですか？ 一般的なパラメーターには、目標、予測期間、分析期間が含まれます。

EDA ノートブックを使用している場合は、先に進む前に以下の値を置き換える必要があります。

```python
goal = "commerce.`order`.purchaseID" #### prediction variable
goal_column_type = "numerical" #### choose either "categorical" or "numerical"
prediction_window_day_start = "2020-01-01" #### YYYY-MM-DD
prediction_window_day_end = "2020-01-31" #### YYYY-MM-DD
analysis_period_day_start = "2020-02-01" #### YYYY-MM-DD
analysis_period_day_end = "2020-02-28" #### YYYY-MM-DD

### If the goal is a categorical goal then select threshold for the defining category and creating bins. 0 is no order placed, and 1 is at least one order placed:
threshold = 1
```

### 機能と目標の作成のためのデータ集計

調査分析を開始するには、プロファイルレベルで目標を作成し、データセットを集計する必要があります。 この例では、2 つのクエリが指定されます。 最初のクエリには、目標の作成が含まれます。 2 つ目のクエリを更新して、1 つ目のクエリの変数以外の変数を含める必要があります。 クエリの `limit` を更新することもできます。 次のクエリを実行した後、集計データを調査に使用できるようになりました。

```sql
%%read_sql target_df -d -c QS_CONNECTION

SELECT DISTINCT endUserIDs._experience.aaid.id                  AS ID,
       Count({goal})                                            AS TARGET
FROM   {target_table}
WHERE DATE(TIMESTAMP) BETWEEN '{prediction_window_day_start}' AND '{prediction_window_day_end}'
GROUP BY endUserIDs._experience.aaid.id;
```

```sql
%%read_sql agg_data -d -c QS_CONNECTION

SELECT z.*, z1.state_city as STATE_CITY
from
((SELECT y.*,a2.AVG_GAP_BETWEEN_ORDERS_DAYS as AVG_GAP_BETWEEN_ORDERS_DAYS
from
(select a1.*, f.DAYS_SINCE_PURCHASE as DAYS_SINCE_PURCHASE
from
(SELECT DISTINCT a.ID  AS ID,
COUNT(DISTINCT Product_Items.SKU) as COUNT_UNIQUE_PRODUCTS_PURCHASED,
COUNT(a.check_out) as COUNT_CHECK_OUTS,
COUNT(a.purchases) as COUNT_PURCHASES, 
COUNT(a.product_list_adds) as COUNT_INSTANCE_PRODUCTADDS,
sum(CASE WHEN a.search_paid = 'TRUE' THEN 1 ELSE 0 END) as COUNT_PAID_SEARCHES,
DATEDIFF('{analysis_period_day_end}', MAX(a.date_a)) as DAYS_SINCE_VISIT,
ROUND(SUM(Product_Items.priceTotal * Product_Items.quantity), 2) AS TOTAL_ORDER_REVENUE
from 
(SELECT endUserIDs._experience.aaid.id as ID,
commerce.`checkouts`.value as check_out,
commerce.`order`.purchaseID as purchases, 
commerce.`productListAdds`.value as product_list_adds,
search.isPaid as search_paid,
DATE(TIMESTAMP) as date_a,
Explode(productlistitems) AS Product_Items
from {target_table}
Where DATE(TIMESTAMP) BETWEEN '{analysis_period_day_start}' AND '{analysis_period_day_end}') as a
group by a.ID) as a1
left join 
(SELECT DISTINCT endUserIDs._experience.aaid.id as ID,
DATEDIFF('{analysis_period_day_end}', max(DATE(TIMESTAMP))) as DAYS_SINCE_PURCHASE
from {target_table}
where DATE(TIMESTAMP) BETWEEN '{analysis_period_day_start}' AND '{analysis_period_day_end}'
and commerce.`order`.purchaseid is not null
GROUP BY endUserIDs._experience.aaid.id) as f
on f.ID = a1.ID
where a1.COUNT_PURCHASES>0) as y
left join
(select ab.ID, avg(DATEDIFF(ab.ORDER_DATES, ab.PriorDate)) as AVG_GAP_BETWEEN_ORDERS_DAYS
from
(SELECT distinct endUserIDs._experience.aaid.id as ID, TO_DATE(DATE(TIMESTAMP)) as ORDER_DATES, 
TO_DATE(LAG(DATE(TIMESTAMP),1) OVER (PARTITION BY endUserIDs._experience.aaid.id ORDER BY DATE(TIMESTAMP))) as PriorDate
FROM {target_table}
where DATE(TIMESTAMP) BETWEEN '{analysis_period_day_start}' AND '{analysis_period_day_end}'
AND commerce.`order`.purchaseid is not null) AS ab
where ab.PriorDate is not null
GROUP BY ab.ID) as a2
on a2.ID = y.ID) z    
left join
(select t.ID, t.state_city from
(
SELECT DISTINCT endUserIDs._experience.aaid.id as ID,
concat(placeContext.geo.stateProvince, ' - ', placeContext.geo.city) as state_city, 
ROW_NUMBER() OVER(PARTITION BY endUserIDs._experience.aaid.id ORDER BY DATE(TIMESTAMP) DESC) AS ROWNUMBER
FROM   {target_table}
WHERE  DATE(TIMESTAMP) BETWEEN '{analysis_period_day_start}' AND '{analysis_period_day_end}') as t
where t.ROWNUMBER = 1) z1
on z.ID = z1.ID)
limit 500000;
```

### 集計データセットの機能と目標の結合

次のセルは、前の例で概要を説明した集計データセットの機能と予測目標を結合するために使用します。

```python
Data = pd.merge(agg_data,target_df, on='ID',how='left')
Data['TARGET'].fillna(0, inplace=True)
```

次の 3 つのセルの例を使用して、結合が成功したことを確認します。

`Data.shape` 列数の後に行数が続く例を返します。(11913、12)。

```python
Data.shape
```

`Data.head(5)` は、5 行のデータを含むテーブルを返します。返されるテーブルには、プロファイル ID にマッピングされた 12 列の集計データがすべて含まれています。

```python
Data.head(5)
```

![例の表](../images/jupyterlab/eda/raw-aggregate-data.PNG)

このセルは、一意のプロファイルの数を印刷します。

```python
print("Count of unique profiles :", (len(Data)))
```

### 欠落した値および外れ値の検出

データの集計を完了し、目標と統合したら、データヘルスチェックと呼ばれる場合もあるデータを確認する必要があります。

このプロセスでは、欠落した値と外れ値を識別します。 問題が特定されたら、次のタスクは、問題を処理するための特定の戦略を考え出すことです。

>[!NOTE]
>
>この手順の間、データ・ログ・プロセスで障害を示す可能性のある値の破損を見つけることができます。

```python
Missing = pd.DataFrame(round(Data.isnull().sum()*100/len(Data),2))
Missing.columns =['Percentage_missing_values'] 
Missing['Features'] = Missing.index
```

次のセルは、欠落している値を視覚化するために使用されます。

```python
trace = go.Bar(
    x = Missing['Features'],
    y = Missing['Percentage_missing_values'],
    name = "Percentage_missing_values")

layout = go.Layout(
    title = 'Missing values',
    width = 1200,
    height = 600,
    xaxis = dict(title = 'Features'),
    yaxis = dict(title = 'Percentage of missing values')
)

fig = go.Figure(data = [trace], layout = layout)
iplot(fig)
```

![値がありません](../images/jupyterlab/eda/missing-values.png)

欠落した値を検出した後は、異常値を識別することが重要です。 平均、標準偏差、相関などのパラメトリック統計は、外れ値に対して非常に敏感です。 また、線形回帰などの一般的な統計手順の前提も、これらの統計に基づいています。 これは異常値が分析を台無しにする可能性があることを意味します。

この例では、四分位数間の範囲を使用して外れ値を識別します。 四分位数間の範囲 (IQR) は、第 1 四分位数と第 3 四分位数（25 番目と第 75 番目のパーセンタイル）の間の範囲です。 次の例では、IQR が 25 番目のパーセンタイルより下の 1.5 倍、または IQR が 75 番目のパーセンタイルより上の 1.5 倍に該当するすべてのデータポイントを収集します。 次のセルでは、これらのいずれかに該当する値が外れ値として定義されます。

>[!TIP]
>
>異常値を修正するには、作業中のビジネスと業界に関する理解が必要です。 外れ値だけでは観察を落とせないこともあります。 異常値は正当な観測値であり、多くの場合、最も興味深い観測値です。 異常値の削除について詳しくは、[ オプションのデータクリーニング手順 ](#optional-data-clean) を参照してください。

```python
TARGET = Data.TARGET

Data_numerical = Data.select_dtypes(include=['float64', 'int64'])
Data_numerical.drop(['TARGET'],axis = 1,inplace = True)
Data_numerical1 = Data_numerical

for i in range(0,len(Data_numerical1.columns)):
    Q1 = Data_numerical1.iloc[:,i].quantile(0.25)
    Q3 = Data_numerical1.iloc[:,i].quantile(0.75)
    IQR = Q3 - Q1
    Data_numerical1.iloc[:,i] = np.where(Data_numerical1.iloc[:,i]<(Q1 - 1.5 * IQR),np.nan, np.where(Data_numerical1.iloc[:,i]>(Q3 + 1.5 * IQR),
                                                                                                    np.nan,Data_numerical1.iloc[:,i]))
    
Outlier = pd.DataFrame(round(Data_numerical1.isnull().sum()*100/len(Data),2))
Outlier.columns =['Percentage_outliers'] 
Outlier['Features'] = Outlier.index   
```

これまでと同様に、結果を視覚化することが重要です。

```python
trace = go.Bar(
    x = Outlier['Features'],
    y = Outlier['Percentage_outliers'],
    name = "Percentage_outlier")

layout = go.Layout(
    title = 'Outliers',
    width = 1200,
    height = 600,
    xaxis = dict(title = 'Features'),
    yaxis = dict(title = 'Percentage of outliers')
)

fig = go.Figure(data = [trace], layout = layout)
iplot(fig)
```

![異常値グラフ](../images/jupyterlab/eda/outliers.png)

### 単変量分析

欠落した値や外れ値のデータを修正したら、分析を開始できます。 分析には次の 3 つのタイプがあります。単変量、二変量、多変量分析。 単一変量分析は、単一の変数の関係を使用して、データを取得し、要約し、データ内のパターンを見つけます。 二変量分析では一度に複数の変数が調べられ、多変量分析では一度に 3 つ以上の変数が調べられます。

次の例では、機能の分布を視覚化するテーブルを作成します。

```python
Data_numerical = Data.select_dtypes(include=['float64', 'int64'])
distribution = pd.DataFrame([Data_numerical.count(),Data_numerical.mean(),Data_numerical.quantile(0), Data_numerical.quantile(0.01),
                             Data_numerical.quantile(0.05),Data_numerical.quantile(0.25), Data_numerical.quantile(0.5),
                        Data_numerical.quantile(0.75),  Data_numerical.quantile(0.95),Data_numerical.quantile(0.99), Data_numerical.max()])
distribution = distribution.T
distribution.columns = ['Count', 'Mean', 'Min', '1st_perc','5th_perc','25th_perc', '50th_perc','75th_perc','95th_perc','99th_perc','Max']
distribution
```

![機能の配布](../images/jupyterlab/eda/distribution-of-features.PNG)

機能の分布を得たら、配列を使用して視覚化されたデータグラフを作成できます。 次のセルは、上記の表を数値データで視覚化するために使用します。

```python
A = sns.palplot(sns.color_palette("Blues"))
```

```python
for column in Data_numerical.columns[0:]:
    plt.figure(figsize=(5, 4))
    plt.ticklabel_format(style='plain', axis='y')
    sns.distplot(Data_numerical[column], color = A, kde=False, bins=6, hist_kws={'alpha': 0.4});
```

![数値データグラフ](../images/jupyterlab/eda/univaiate-graphs.png)

### 分類データ

グループ化カテゴリデータは、集計データの各列とその分布に含まれる値を理解するために使用されます。 この例では、上位 10 のカテゴリを使用して、分布の印刷を支援します。 列には何千もの一意の値が含まれる場合があることに注意してください。 乱雑なプロットをレンダリングして読みにくくしたくない場合。 ビジネス目標を念頭に置いて、データをグループ化すると、より意味のある結果が得られます。

```python
Data_categorical = Data.select_dtypes(include='object')
Data_categorical.drop(['ID'], axis = 1, inplace = True, errors = 'ignore')
```

```python
for column in Data_categorical.columns[0:]:
    if (len(Data_categorical[column].value_counts())>10):
        plt.figure(figsize=(12, 8))
        sns.countplot(x=column, data = Data_categorical, order = Data_categorical[column].value_counts().iloc[:10].index, palette="Set2");
    else:
        plt.figure(figsize=(12, 8))
        sns.countplot(x=column, data = Data_categorical, palette="Set2");
```

![異形柱](../images/jupyterlab/eda/graph-category.PNG)

### 単一のユニーク値のみを含む列の削除

値が 1 のみの列は、分析に情報を追加せず、削除できます。

```python
for col in Data.columns:
    if len(Data[col].unique()) == 1:
        if col == 'TARGET':
            print(Fore.RED + '\033[1m' + 'WARNING : TARGET HAS A SINGLE UNIQUE VALUE, ANY BIVARIATE ANALYSIS (NEXT STEP IN THIS NOTEBOOK) OR PREDICTION WILL BE MEANINGLESS' + Fore.RESET + '\x1b[21m')
        elif col == 'ID':
            print(Fore.RED + '\033[1m' + 'WARNING : THERE IS ONLY ONE PROFILE IN THE DATA, ANY BIVARIATE ANALYSIS (NEXT STEP IN THIS NOTEBOOK) OR PREDICTION WILL BE MEANINGLESS' + Fore.RESET + '\x1b[21m')
        else:
            print('Dropped column :',col)
            Data.drop(col,inplace=True,axis=1)
```

1 つの値の列を削除したら、新しいセルで `Data.columns` コマンドを使用して、残りの列でエラーがないか確認します。

### 欠落した値の正しさ

次の節では、欠落値の修正に関するサンプルアプローチを示します。 イベントは、上記のデータでは 1 つの列に値がありませんでしたが、以下の例のセルではすべてのデータ型の値が正しくなっています。 これには、以下が含まれます。

- 数値データ型：該当する場合は、0 または max を入力します。
- 分類データ型：入力モーダル値

```python
#### Select only numerical data
Data_numerical = Data.select_dtypes(include=['float64', 'int64'])

#### For columns that contain days we impute max days of history for null values, for rest all we impute 0

# Imputing days with max days of history
Days_cols = [col for col in Data_numerical.columns if 'DAYS_' in col]
d1 = datetime.strptime(analysis_period_day_start, "%Y-%m-%d")
d2 = datetime.strptime(analysis_period_day_end, "%Y-%m-%d")
A = abs((d2 - d1).days)

for column in Days_cols:
    Data[column].fillna(A, inplace=True)

# Imputing 0
Data_numerical = Data.select_dtypes(include=['float64', 'int64'])
Missing_numerical = Data_numerical.columns[Data_numerical.isnull().any()].tolist()

for column in Missing_numerical:
    Data[column].fillna(0, inplace=True)
```

```python
#### Correct for missing values in categorical columns (Replace with mode)
Data_categorical = Data.select_dtypes(include='object')
Missing_cat = Data_categorical.columns[Data_categorical.isnull().any()].tolist() 
for column in Missing_cat:
    Data[column].fillna(Data[column].mode()[0], inplace=True)
```

完了すると、クリーンデータは二変量分析の準備が整います。

### 二変量分析

二変量分析は、特徴とターゲット変数など、2 組の値の関係を理解するのに役立ちます。 異なるプロットは、カテゴリデータ型と数値データ型に対応するので、この分析はデータ型ごとに別々に行う必要があります。 二変量分析では、次のグラフをお勧めします。

- **相関**:相関係数は、2 つのフィーチャ間の関係の強さの尺度です。相関関係には —1 ～ 1 の値があります。ここで、1 は強い正の関係を示し、-1 は強い負の関係を示し、0 の結果は全く関係を示しません。
- **ペアプロット**:ペアプロットは、各変数間の関係を視覚化する簡単な方法です。データ内の各変数間の関係のマトリックスを生成します。
- **ヒートマップ**:ヒートマップは、データセット内のすべての変数の相関係数です。
- **ボックスプロット**:ボックスプロットは、5 つの数の概要 ( 最小、第 1 四分位数 (Q1)、中央値、第 3 四分位数 (Q3)、最大 ) に基づいてデータ分布を表示する標準化された方法です。
- **カウントプロット**:カウントプロットは、一部の分類特徴のヒストグラムや棒グラフに似ています。特定のタイプのカテゴリに基づいて、ある項目の発生回数を表示します。

「goal」変数と述語/機能との関係を理解するために、グラフはデータ型に基づいて使用されます。 数値フィーチャーの場合、&#39;goal&#39;変数がカテゴリの場合はボックスプロットを使用し、&#39;goal&#39;変数が数値の場合はペアプロットとヒートマップを使用する必要があります。

分類特性の場合、「ゴール」変数がカテゴリの場合はカウントプロットを、「ゴール」変数が数値の場合はボックスプロットを使用する必要があります。 これらの方法を使用すると、関係を理解するのに役立ちます。 これらの関係は、機能、つまり述語と目標の形式で設定できます。

**数値の予測**

```python
if len(Data) == 1:
    print(Fore.RED + '\033[1m' + 'THERE IS ONLY ONE PROFILE IN THE DATA, BIVARIATE ANALYSIS IS NOT APPLICABLE, PLEASE INCLUDE AT LEAST ONE MORE PROFILE TO DO BIVARIATE ANALYSIS')
elif len(Data['TARGET'].unique()) == 1:
    print(Fore.RED + '\033[1m' + 'TARGET HAS A SINGLE UNIQUE VALUE, BIVARIATE ANALYSIS IS NOT APPLICABLE, PLEASE INCLUDE PROFILES WITH ATLEAST ONE DIFFERENT VALUE OF TARGET TO DO BIVARIATE ANALYSIS')
else:
    if (goal_column_type == "categorical"):
        TARGET_categorical = pd.DataFrame(np.where(TARGET>=threshold,"1","0"))
        TARGET_categorical.rename(columns={TARGET_categorical.columns[0]: "TARGET_categorical" }, inplace = True)
        Data_numerical = Data.select_dtypes(include=['float64', 'int64'])
        Data_numerical.drop(['TARGET'],inplace=True,axis=1)
        Data_numerical = pd.concat([Data_numerical, TARGET_categorical.astype(int)], axis = 1)
        ncols_for_charts = len(Data_numerical.columns)-1
        nrows_for_charts = math.ceil(ncols_for_charts/4)
        fig, axes = plt.subplots(nrows=nrows_for_charts, ncols=4, figsize=(18, 15))
        for idx, feat in enumerate(Data_numerical.columns[:-1]):
            ax = axes[int(idx // 4), idx % 4]
            sns.boxplot(x='TARGET_categorical', y=feat, data=Data_numerical, ax=ax)
            ax.set_xlabel('')
            ax.set_ylabel(feat)
            fig.tight_layout();
    else:
        Data_numerical = Data.select_dtypes(include=['float64', 'int64'])
        TARGET = pd.DataFrame(Data_numerical.TARGET)
        Data_numerical = Data.select_dtypes(include=['float64', 'int64'])
        Data_numerical.drop(['TARGET'],inplace=True,axis=1)
        Data_numerical = pd.concat([Data_numerical, TARGET.astype(int)], axis = 1)
        for i in Data_numerical.columns[:-1]:
            sns.pairplot(x_vars=i, y_vars=['TARGET'], data=Data_numerical, height = 4)
        f, ax = plt.subplots(figsize = (10,8))
        corr = Data_numerical.corr()
```

セルを実行すると、次の出力が生成されます。

![プロット](../images/jupyterlab/eda/bivariant-graphs.png)

![ヒートマップ](../images/jupyterlab/eda/bi-graph10.PNG)

**分類の述語**

次の例は、各カテゴリ変数の上位 10 カテゴリの周波数プロットをプロットおよび表示する場合に使用します。

```python
if len(Data) == 1:
    print(Fore.RED + '\033[1m' + 'THERE IS ONLY ONE PROFILE IN THE DATA, BIVARIATE ANALYSIS IS NOT APPLICABLE, PLEASE INCLUDE AT LEAST ONE MORE PROFILE TO DO BIVARIATE ANALYSIS')
elif len(Data['TARGET'].unique()) == 1:
    print(Fore.RED + '\033[1m' + 'TARGET HAS A SINGLE UNIQUE VALUE, BIVARIATE ANALYSIS IS NOT APPLICABLE, PLEASE INCLUDE PROFILES WITH ATLEAST ONE DIFFERENT VALUE OF TARGET TO DO BIVARIATE ANALYSIS')
else:
    if (goal_column_type == "categorical"):
        TARGET_categorical = pd.DataFrame(np.where(TARGET>=threshold,"1","0"))
        TARGET_categorical.rename(columns={TARGET_categorical.columns[0]: "TARGET_categorical" }, inplace = True)
        Data_categorical = Data.select_dtypes(include='object')
        Data_categorical.drop(["ID"], axis =1, inplace = True)
        Cat_columns = Data_categorical
        Data_categorical = pd.concat([TARGET_categorical,Data_categorical], axis =1)
        for column in Cat_columns.columns:
            A = Data_categorical[column].value_counts().iloc[:10].index
            Data_categorical1 = Data_categorical[Data_categorical[column].isin(A)]
            plt.figure(figsize=(12, 8))
            sns.countplot(x="TARGET_categorical",hue=column, data = Data_categorical1, palette = 'Blues')
            plt.xlabel("GOAL")
            plt.ylabel("COUNT")
            plt.show();
    else:
        Data_categorical = Data.select_dtypes(include='object')
        Data_categorical.drop(["ID"], axis =1, inplace = True)
        Target = Data.TARGET
        Data_categorical = pd.concat([Data_categorical,Target], axis =1)
        for column in Data_categorical.columns[:-1]:
            A = Data_categorical[column].value_counts().iloc[:10].index
            Data_categorical1 = Data_categorical[Data_categorical[column].isin(A)]
            sns.catplot(x=column, y="TARGET", kind = "boxen", data =Data_categorical1, height=5, aspect=13/5);
```

セルを実行すると、次の出力が生成されます。

![カテゴリ関係](../images/jupyterlab/eda/categorical-predictor.PNG)

### 重要な数値特徴

相関解析を使用して、上位 10 の重要な数値フィーチャのリストを作成できます。 これらの機能はすべて、「目標」機能を予測するために使用できます。 このリストは、モデルの構築を開始する際のフィーチャリストとして使用できます。

```python
if len(Data) == 1:
    print(Fore.RED + '\033[1m' + 'THERE IS ONLY ONE PROFILE IN THE DATA, BIVARIATE ANALYSIS IS NOT APPLICABLE, PLEASE INCLUDE AT LEAST ONE MORE PROFILE TO FIND IMPORTANT VARIABLES')
elif len(Data['TARGET'].unique()) == 1:
    print(Fore.RED + '\033[1m' + 'TARGET HAS A SINGLE UNIQUE VALUE, BIVARIATE ANALYSIS IS NOT APPLICABLE, PLEASE INCLUDE PROFILES WITH ATLEAST ONE DIFFERENT VALUE OF TARGET TO FIND IMPORTANT VARIABLES')
else:
    Data_numerical = Data.select_dtypes(include=['float64', 'int64'])
    Correlation = pd.DataFrame(Data_numerical.drop("TARGET", axis=1).apply(lambda x: x.corr(Data_numerical.TARGET)))
    Correlation['Corr_abs'] = abs(Correlation)
    Correlation = Correlation.sort_values(by = 'Corr_abs', ascending = False)
    Imp_features = pd.DataFrame(Correlation.index[0:10])
    Imp_features.rename(columns={0:'Important Feature'}, inplace=True)
    print(Imp_features)
```

![重要な機能](../images/jupyterlab/eda/important-feature-model.PNG)

### インサイトの例

データの分析中に、インサイトを明らかにすることは珍しくありません。 次の例は、ターゲットイベントの最新性と金額の値をマッピングするインサイトです。

```python
# Proxy for monetary value is TOTAL_ORDER_REVENUE and proxy for frequency is NUMBER_VISITS
if len(Data) == 1:
    print(Fore.RED + '\033[1m' + 'THERE IS ONLY ONE PROFILE IN THE DATA, INSIGHTS ANALYSIS IS NOT APPLICABLE, PLEASE INCLUDE AT LEAST ONE MORE PROFILE TO FIND IMPORTANT VARIABLES')
elif len(Data['TARGET'].unique()) == 1:
    print(Fore.RED + '\033[1m' + 'TARGET HAS A SINGLE UNIQUE VALUE, INSIGHTS ANALYSIS IS NOT APPLICABLE, PLEASE INCLUDE PROFILES WITH ATLEAST ONE DIFFERENT VALUE OF TARGET TO FIND IMPORTANT VARIABLES')
else:
    sns.lmplot("DAYS_SINCE_VISIT", "TOTAL_ORDER_REVENUE", Data, hue="TARGET", fit_reg=False);
```

![インサイトの例](../images/jupyterlab/eda/insight.PNG)

## オプションのデータクリーニング手順 {#optional-data-clean}

異常値を修正するには、作業中のビジネスと業界に関する理解が必要です。 外れ値だけでは観察を落とせないこともあります。 異常値は正当な観測値であり、多くの場合、最も興味深い観測値です。

異常値の詳細と、それらをドロップするかどうかについては、[ 分析係数 ](https://www.theanalysisfactor.com/outliers-to-drop-or-not-to-drop/) からこのエントリを読んでください。

次の例では、[ 四分位数範囲 ](https://www.thoughtco.com/what-is-the-interquartile-range-rule-3126244) を使用して外れ値を設定するセルキャップと床のデータポイントを示します。

```python
TARGET = Data.TARGET

Data_numerical = Data.select_dtypes(include=['float64', 'int64'])
Data_numerical.drop(['TARGET'],axis = 1,inplace = True)

for i in range(0,len(Data_numerical.columns)):
    Q1 = Data_numerical.iloc[:,i].quantile(0.25)
    Q3 = Data_numerical.iloc[:,i].quantile(0.75)
    IQR = Q3 - Q1
    Data_numerical.iloc[:,i] = np.where(Data_numerical.iloc[:,i]<(Q1 - 1.5 * IQR), (Q1 - 1.5 * IQR), np.where(Data_numerical.iloc[:,i]>(Q3 + 1.5 * IQR),
                                                                                                     (Q3 + 1.5 * IQR),Data_numerical.iloc[:,i]))
Data_categorical = Data.select_dtypes(include='object')
Data = pd.concat([Data_categorical, Data_numerical, TARGET], axis = 1)
```

## 次の手順

調査データの解析が完了したら、モデルの作成を開始する準備が整います。 または、導き出したデータやインサイトを使用して、Power BIなどのツールを使用してダッシュボードを作成できます。

Adobe Experience Platformは、モデル作成プロセスを、レシピ（モデルインスタンス）とモデルの 2 つの異なるステージに分けます。 レシピの作成プロセスを開始するには、JupyerLab Notebooks](./create-a-recipe.md) での [ レシピの作成に関するドキュメントを参照してください。 このドキュメントには、[!DNL JupyterLab] ノートブック内のレシピを作成、トレーニング、スコアリングするための情報と例が含まれています。
