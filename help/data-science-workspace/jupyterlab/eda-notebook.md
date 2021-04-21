---
keywords: Experience Platform;JupyterLab;notebooks;Data Science Workspace；一般的なトピック；分析データノートブック；eda；調査データ分析；データサイエンス
solution: Experience Platform
title: 探索的データ分析(EDA)ノートブック
topic-legacy: overview
type: Tutorial
description: このガイドでは、探索的データ分析(EDA)ノートブックを使用して、Webデータのパターンを発見する方法、予測目標を持つ集計イベント、集計データを消去し、予測者と目標の関係を理解する方法について説明します。
exl-id: 48209326-0a07-4b5c-8b49-a2082a78fa47
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '2760'
ht-degree: 1%

---

# 調査データ分析(EDA)ノートブックを使用した予測モデルのためのWebベースのデータの調査

探索的データ分析(EDA)ノートブックは、データのパターンを発見し、データの整合性をチェックし、予測モデル用に関連データを要約するのに役立つように設計されています。

EDAノートブックの例は、Webベースのデータを考慮して最適化され、2つの部分で構成されています。 クエリサービスを使用して表示の傾向とデータのスナップショットを作成する開始の1つに分けてください。 次に、探索的なデータの分析を念頭に置いて、プロファイルと訪問者のレベルでデータを集計します。

2つ目の開始は、Pythonライブラリを使用して集計データの説明分析を実行することです。 このノートブックは、ヒストグラム、散布グラフ、ボックスプロット、相関行列などのビジュアライゼーションを示し、目標の予測に最も役立つと思われる機能を判断するために使用される実用的なインサイトを導き出します。

## はじめに

このガイドを読む前に、[[!DNL JupyterLab] ユーザーガイド](./overview.md)を参照し、[!DNL JupyterLab]とData Science Workspace内でのその役割の概要を確認してください。 また、独自のデータを使用している場合は、 [!DNL Jupyterlab] ノートブック](./access-notebook-data.md)の[データアクセスに関するドキュメントを参照してください。 このガイドでは、ノートブックデータの制限に関する重要な情報を説明します。

このノートブックは、AnalyticsAnalysis WorkspaceにあるAdobe Analyticsエクスペリエンスイベントデータの形式の中間値データセットを使用します。 EDAノートブックを使用するには、次の値`target_table`と`target_table_id`を使用してデータテーブルを定義する必要があります。 任意の中間値データセットを使用できます。

これらの値を見つけるには、JupyterLabデータアクセスガイドのpython](./access-notebook-data.md#write-python)の節の[データセットへの書き込みで説明されている手順に従ってください。 データセット名(`target_table`)は、datasetディレクトリ内にあります。 データセットを右クリックしてノートブックにデータを検索または書き込むと、実行可能なコードエントリにデータセットID(`target_table_id`)が格納されます。

## データ検出

この節では、「ユーザーアクティビティ別上位10都市」や「閲覧商品数上位10都市」など、表示の傾向を分析するために使用する設定手順と例のクエリについて説明します。

### ライブラリの設定

JupterLabは複数のライブラリをサポートしています。 次のコードをコードセルに貼り付けて実行し、この例で使用する必要なすべてのパッケージを収集してインストールできます。 独自のデータ分析には、この例以外の追加または代替パッケージを使用できます。 サポートされているパッケージのリストを確認するには、`!pip list --format=columns`を新しいセルにコピー&amp;ペーストします。

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

### Adobe Experience Platformに接続[!DNL Query Service]

[!DNL JupyterLab] プラットフォームでは、 [!DNL Python] ノートブックでSQLを使用して、 [クエリサービスを通じてデータにアクセスできます](https://docs.adobe.com/content/help/ja-JP/experience-platform/query/home.html)。[!DNL Query Service]経由でデータにアクセスすると、実行時間が長いため、大きなデータセットを扱うのに便利です。 [!DNL Query Service]を使用したデータのクエリには、処理時間制限が10分あることをお勧めします。

[!DNL JupyterLab]で[!DNL Query Service]を使用する前に、[[!DNL Query Service] SQL構文](https://docs.adobe.com/content/help/ja-JP/experience-platform/query/home.html#!api-specification/markdown/narrative/technical_overview/query-service/sql/syntax.md)に関する十分な理解を得ておく必要があります。

JupyterLab のクエリサービスを利用するには、まず作業中の Python ノートブックとクエリサービスの間の接続を作成する必要があります。これは、次のセルを実行することで達成できます。

```python
qs_connect()
```

### 調査用のmidvaluesデータセットの定義

データのクエリーと調査を開始するには、midvaluesデータセットテーブルを指定する必要があります。 `table_name`と`table_id`の値をコピーして、独自のデータテーブルの値に置き換えます。

```python
target_table = "table_name"
target_table_id = "table_id"
```

完了すると、このセルは次の例のようになります。

```python
target_table = "cross_industry_demo_midvalues"
target_table_id = "5f7c40ef488de5194ba0157a"
```

### 利用可能な日付のデータセットを調べます。

以下に示すセルを使用すると、表の対象となる日付範囲を表示できます。 日数、最初の日付、最後の日付を調べるのは、より詳細な分析が必要な日付範囲の選択に役立ちます。

```python
%%read_sql -c QS_CONNECTION
SELECT distinct Year(timestamp) as Year, Month(timestamp) as Month, count(distinct DAY(timestamp)) as Count_days, min(DAY(timestamp)) as First_date, max(DAY(timestamp)) as Last_date, count(timestamp) as Count_hits
from {target_table}
group by Month(timestamp), Year(timestamp)
order by Year, Month;
```

セルを実行すると、次の出力が生成されます。

![クエリ日の出力](../images/jupyterlab/eda/query-date-output.PNG)

### データセット検出の日付の設定

データセット検出に使用できる日付を決定した後、以下のパラメーターを更新する必要があります。 このセルに設定される日付は、クエリの形式でのデータ検出にのみ使用されます。 このガイドで後述する調査データの分析に適した範囲に日付が再度更新されます。

```python
target_year = "2020" ## The target year
target_month = "02" ## The target month
target_day = "(01,02,03)" ## The target days
```

### データセットの検出

すべてのパラメーターを設定し、[!DNL Query Service]を起動し、日付範囲を設定したら、データ行の読み取りを開始する準備が整います。 読み取る行数は制限する必要があります。

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

データセットの行を表示するには、次のセルを使用します。 この例では、行数は5行に制限されています。

```python
Table.head(5)
```

![テーブル行の出力](../images/jupyterlab/eda/data-table-overview.PNG)

データセットに含まれるデータを把握したら、データセットをさらに分類すると役立ちます。 この例では、各列の列名とデータ型が一覧表示され、出力を使用してデータ型が正しいかどうかを確認します。

```python
ColumnNames_Types = pd.DataFrame(Table.dtypes)
ColumnNames_Types = ColumnNames_Types.reset_index()
ColumnNames_Types.columns = ["Column_Name", "Data_Type"]
ColumnNames_Types
```

![列名とデータ型のリスト](../images/jupyterlab/eda/data-columns.PNG)

### データセットのトレンド調査

次の節では、データのトレンドとパターンを調査するために使用される4つのクエリの例を示します。 次の例は完全なものではありませんが、よく見られる機能の一部を示しています。

**特定の日の時間別アクティビティ数**

このクエリでは、1日の中でのアクションおよびクリック数を分析します。 出力は、1日の1時間あたりのアクティビティ数に関する指標を含む表の形式で表されます。

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

![クエリ1の出力](../images/jupyterlab/eda/hour-count-raw.PNG)

クエリの動作を確認した後、データを一変量のプロットヒストグラムに表示し、視覚的に明確にする。

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

![クエリ1の棒グラフの出力](../images/jupyterlab/eda/activity-count-by-hour-of-day.png)

**特定の日に閲覧された上位 10 ページ**

このクエリは、特定の日に最も多く閲覧されたページを分析します。 出力は、ページ名とページ表示数に関する指標を含む表の形式で表されます。

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

クエリの動作を確認した後、データを一変量のプロットヒストグラムに表示し、視覚的に明確にする。

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

![トップ10閲覧ページ](../images/jupyterlab/eda/top-ten-viewed-pages-for-a-given-day.png)

**ユーザーアクティビティ別上位10都市**

このクエリは、データの送信元の市区町村を分析します。

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

クエリの動作を確認した後、データを一変量のプロットヒストグラムに表示し、視覚的に明確にする。

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

![10都市](../images/jupyterlab/eda/top-ten-cities-by-user-activity.png)

**閲覧された商品の上位10件**

このクエリは、閲覧された上位10製品のリストを提供します。 次の例では、`Explode()`関数を使用して、`productlistitems`オブジェクト内の各製品を独自の行に返します。 これにより、異なるSKUに対して、集計製品の表示に対してネストされたクエリを行うことができます。

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

クエリの動作を確認した後、データを一変量のプロットヒストグラムに表示し、視覚的に明確にする。

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

![トップ10の商品表示](../images/jupyterlab/eda/top-ten-viewed-products.png)

データの傾向とパターンを調べた後、目標の予測に使用する機能について適切なアイデアを得る必要があります。 テーブルをスキム処理すると、値と開始内の各データ属性の形式、明らかな誤った表現、大きな外れ値をすばやく強調表示して、属性間で調べる候補の関係を示すことができます。

## 探索的データ分析

調査データの分析は、データに対する理解を深め、モデリングの基盤として使用できる説得力のある質問の直感を構築するために使用します。

データ検出手順を終了した後、イベントレベルのデータを調べ、イベント、市区町村、またはユーザーIDのレベルで集約を行って1日のトレンドを確認します。 このデータは重要ですが、全体像を示すわけではありません。 Webサイトで購入を引き起こす要因がまだわかりません。

これを理解するには、プロファイル/訪問者レベルでデータを集計し、購入目標を定義し、相関、ボックスプロット、散布グラフなどの統計的概念を適用する必要があります。 これらの方法は、定義した予測ウィンドウで、購入者のアクティビティと購入者以外の購入者のパターンを比較するために使用されます。

この節では、次の機能を作成し、検討します。

- `COUNT_UNIQUE_PRODUCTS_PURCHASED`:購入した個別製品の数。
- `COUNT_CHECK_OUTS`:チェックアウト数。
- `COUNT_PURCHASES`:購入数。
- `COUNT_INSTANCE_PRODUCTADDS`:製品追加インスタンスの数。
- `NUMBER_VISITS` :訪問回数。
- `COUNT_PAID_SEARCHES`:有料検索の数。
- `DAYS_SINCE_VISIT`:最後の訪問からの日数。
- `TOTAL_ORDER_REVENUE`:注文総売上高。
- `DAYS_SINCE_PURCHASE`:前回の購入からの日数。
- `AVG_GAP_BETWEEN_ORDERS_DAYS`:購入間の平均差（日数）。
- `STATE_CITY`:州と市区町村を含みます。

データの集計を続行する前に、探索的データ分析で使用される予測変数のパラメーターを定義する必要があります。 言い換えれば、データサイエンスのモデルに何を求めているのですか？ 共通のパラメーターには、目標、予測期間および分析期間が含まれます。

EDAノートブックを使用する場合は、次の値を置き換えてから先に進む必要があります。

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

### 機能と目標の作成のためのデータ集約

調査的な分析を開始するには、プロファイルレベルで目標を作成し、その後データセットを集計する必要があります。 この例では、2つのクエリが提供されています。 最初のクエリには、目標の作成が含まれます。 2つ目のクエリを更新して、1つ目のクエリ内の変数以外の変数を含める必要があります。 クエリの`limit`を更新する必要がある場合があります。 次のクエリを実行した後、集計データを調査できるようになりました。

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

次の3つのセル例を使用して、結合が成功したことを確認します。

`Data.shape` 列数の後に行数が続く値を返します。例：(11913, 12)

```python
Data.shape
```

`Data.head(5)` 5行のデータを含むテーブルを返します。返される表には、プロファイルIDにマップされた集計データの12列すべてが含まれます。

```python
Data.head(5)
```

![例表](../images/jupyterlab/eda/raw-aggregate-data.PNG)

このセルは、一意のプロファイル数を印刷します。

```python
print("Count of unique profiles :", (len(Data)))
```

### 欠落した値と外れ値を検出する

データの集計を完了し、目標と結合したら、データの正常性チェックと呼ばれる場合があるデータを確認する必要があります。

この手順では、欠落した値と外れ値を識別します。 問題が特定された場合、次のタスクは問題を処理するための具体的な戦略を策定することです。

>[!NOTE]
>
>この手順では、データログ処理で障害を示す可能性のある値の破損を特定できます。

```python
Missing = pd.DataFrame(round(Data.isnull().sum()*100/len(Data),2))
Missing.columns =['Percentage_missing_values'] 
Missing['Features'] = Missing.index
```

次のセルは、見つからない値を視覚化するために使用します。

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

欠落した値を検出した後は、異常値を識別することが重要です。 平均、標準偏差、および相関関係などのパラメトリック統計は、外れ値に対して非常に敏感です。 また、線形回帰などの一般的な統計的手順の前提も、これらの統計に基づいています。 外れ値人は分析をめちゃくちゃにする。

外れ値を識別するために、四分位数間の範囲を使用します。 四分位数間の範囲(IQR)は、第1四分位数と第3四分位数（25番目と第75番目のパーセンタイル）の間の範囲です。 この例では、IQRの25番目のパーセンタイル値の1.5倍、またはIQRの75番目のパーセンタイル値の1.5倍に該当するすべてのデータポイントを収集します。 これらのいずれかに該当する値は、次のセルで外れ値として定義されます。

>[!TIP]
>
>異常値を修正するには、取り組んでいるビジネスや業界について理解しておく必要があります。 時々、外れ値だからといって、観察を止められない。 異常値は正当な観察結果であり、多くの場合最も興味深い観察結果です。 外れ値の削除について詳しくは、[オプションのデータクリーニング手順](#optional-data-clean)を参照してください。

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

結果を視覚化することは、常に重要です。

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

欠落した値や外れ値に関するデータを修正すると、分析の開始を行うことができます。 分析には次の3つのタイプがあります。一変量、二変量、多変量分析の分析。 単一変数分析は、単一の変数の関係を使用して、データの取得、要約、データ内のパターンの検索を行います。 二変量分析分析では一度に複数の変数を調べ、多変量分析分析では一度に3つ以上の変数を調べます。

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

![特徴の分布](../images/jupyterlab/eda/distribution-of-features.PNG)

機能の分布を確認したら、配列を使用して視覚化したデータグラフを作成できます。 次のセルを使用して、上記の表を数値データで視覚化します。

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

カテゴリデータのグループ化は、集計されたデータの各列に含まれる値とその分布を理解するために使用されます。 次の例では、上位10カテゴリを使用して、分布の描画を支援します。 1つの列に含まれる一意の値が何千個もある可能性があることに注意してください。 散乱したプロットをレンダリングして読みにくくしたくない。 ビジネス目標を念頭に置き、データをグループ化すると、より有意義な結果を得られます。

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

![カタゴリック柱](../images/jupyterlab/eda/graph-category.PNG)

### 単一の明確な値のみを持つ列の削除

値が1のみの列は分析に情報を追加せず、削除できます。

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

1つの値の列を削除したら、新しいセルの`Data.columns`コマンドを使用して、残りの列でエラーを確認します。

### 欠落した値に対する正しい対応

次の節では、欠落した値の修正方法のサンプルを示します。 上記のデータでは、1つの列のみに値が欠落していましたが、下の例のセルでは、すべてのデータ型に対して正しい値が設定されています。 これには、以下が含まれます。

- 数値データ型：適用可能な場合は、0またはmaxを入力します。
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

一旦完了すると、クリーンデータは二変量分析の準備ができます。

### 二変量分析

二変量分析は、フィーチャとターゲット変数など、2組の値の関係を理解するのに役立ちます。 異なるプロットは、カテゴリデータタイプと数値データタイプに応じて異なるので、この分析はデータタイプごとに個別に行う必要があります。 2変量分析の場合は、次のグラフを使用することをお勧めします。

- **相関**:相関係数は、2つのフィーチャ間の関係の強さの測定値です。相関関係には —1 ～ 1の値があります。ここで、次の値を指定します。1は強いポジティブな関係を示し、-1は強いネガティブな関係を示し、0の結果は全く関係がないことを示します。
- **ペアプロット**:ペアプロットは、各変数間の関係を視覚化する簡単な方法です。データ内の各変数間の関係のマトリックスを生成します。
- **ヒートマップ**:ヒートマップは、データセット内のすべての変数の相関係数です。
- **ボックスプロット**:ボックスプロットは、5つの数値の概要(最小、第1四分位数(Q1)、中央値、第3四分位数(Q3)、最大値)に基づいてデータの分布を表示する標準化された方法です。
- **カウント・プロット**:カウントプロットは、一部の分類特徴のヒストグラムや棒グラフに似ています。特定の種類のカテゴリに基づいて、項目のオカレンス数を表示します。

「goal」変数と述語/機能の関係を理解するために、グラフはデータ型に基づいて使用されます。 数値フィーチャの場合、&#39;goal&#39;変数がカテゴリの場合はボックスプロットを使用し、&#39;goal&#39;変数が数値の場合はペアプロットとヒートマップを使用する必要があります。

分類特徴の場合、「目標」変数がカテゴリの場合は可算プロットを、「目標」変数が数値の場合はボックスプロットを使用する必要があります。 これらの方法を使用すると、関係を理解するのに役立ちます。 これらの関係は、機能、つまり予測者と目標の形式にすることができます。

**数値予測**

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

![熱地図](../images/jupyterlab/eda/bi-graph10.PNG)

**分類予測**

次の例は、各カテゴリ変数の上位10カテゴリに対して周波数プロットをプロットし、表示するために使用します。

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

相関分析を使用すると、数値に関する重要な上位10個の機能のリストを作成できます。 これらの機能はすべて、「目標」機能を予測するために使用できます。 このリストは、モデルの構築を開始する際のフィーチャリストとして使用できます。

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

データの分析中に、インサイトを見つけるのは珍しくありません。 次の例は、ターゲットイベントの最新性と金額の値をマッピングするインサイトです。

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

## オプションのデータクリーニング手順{#optional-data-clean}

異常値を修正するには、取り組んでいるビジネスや業界について理解しておく必要があります。 時々、外れ値だからといって、観察を止められない。 異常値は正当な観察結果であり、多くの場合最も興味深い観察結果です。

外れ値の詳細と、それらを削除するかどうかについては、[分析因子](https://www.theanalysisfactor.com/outliers-to-drop-or-not-to-drop/)の項目を参照してください。

次の例では、[四分位数範囲](https://www.thoughtco.com/what-is-the-interquartile-range-rule-3126244)を使用した外れ値のセルキャップとフロアのデータポイントを示します。

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

調査データの分析が完了したら、モデルの作成を開始する準備が整います。 または、派生したデータやインサイトを使用して、Power BIなどのツールを使用してダッシュボードを作成できます。

Adobe Experience Platformでは、モデル作成プロセスを「レシピ（モデルインスタンス）」と「モデル」の2つの異なるステージに分けています。 レシピ作成プロセスを開始するには、[JupyerLab Notebooks](./create-a-recipe.md)でレシピを作成するドキュメントを参照してください。 このドキュメントには、[!DNL JupyterLab]ノートブック内のレシピを作成、トレーニング、スコアリングするための情報と例が含まれています。
