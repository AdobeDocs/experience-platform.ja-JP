---
keywords: Experience Platform;JupyterLab;notebooks;Data Science Workspace;popular topics
solution: Experience Platform
title: クエリサービス
topic: Tutorial
translation-type: tm+mt
source-git-commit: d0596dc3c744e192c4d2ad04d6365846a0115371

---


# クエリサービス

Adobe Experience Platformを使用すると、Data Science Workspaceで構造化クエリ言語(SQL)を使用できます。このため、クエリサービスを標準機能としてJupterLabに統合します。

このチュートリアルでは、Adobe Analyticsデータを調査、変換、分析する一般的な使用例の次のSQLクエリ例を示します。

- [JupyterLabとクエリサービス](#access-jupyterlab-and-query-service)
- [クエリデータ](#query-your-data)
   - [時間別訪問者数](#hourly-visitor-count)
   - [時間別アクティビティ数](#hourly-activity-count)
   - [1回のイベントセッションあたりの訪問者数](#number-of-events-per-visitor-session)
   - [特定の日の人気のあるページ](#popular-pages-for-a-given-day)
   - [特定の日のアクティブなユーザー](#active-users-for-a-given-day)
   - [ユーザ別のアクティブな市区町村アクティビティ](#active-cities-by-user-activity)

## はじめに

このチュートリアルを開始する前に、次の前提条件が必要です。

- Adobe Experience Platformへのアクセス。 Experience PlatformのIMS組織にアクセスできない場合は、次に進む前に、システム管理者にお問い合わせください。

- Adobe Analyticsデータセット

- このチュートリアルで使用する次の主要概念に関する実用的な理解
   - [エクスペリエンスデータモデル(XDM)とXDMシステム](../../xdm/home.md)
   - [クエリサービス](../../query-service/home.md)
   - [クエリサービスのSQL構文](../../query-service/sql/overview.md)
   - Adobe Analytics

## JupyterLabとクエリサービス

1. エクスペリエ [ンスプラットフォーム](https://platform.adobe.com)で、左のナビゲ **ーション列から** 「モデル」に移動します。 上部のヘ **ッダで** 「Notebooks」をクリックし、JupterLabを開きます。 JupyterLabが読み込まれるまで、しばらく待ちます。

   ![](../images/jupyterlab/query/notebook_ui.png)

   > [!NOTE] 新しい「ランチャー」タブが自動的に表示されなかった場合は、ファイル/新規ランチャーをクリックして、新し **い「ランチャー」タブを開きます**。

2. 「ランチャー」タブで、Python 3環境 **の空白** (Blank)アイコンをクリックして、空のノートブックを開きます。

   ![](../images/jupyterlab/query/blank_notebook.png)

   > [!NOTE] 現在、Python 3は、ノートブックのクエリサービスでサポートされている環境はPython 3のみです。

3. 左側の選択レールで、 **Data** （データ）アイコンをクリックし、重複で **Datasets** （データセット）ディレクトリをクリックして、すべてのデータセットをリストします。

   ![](../images/jupyterlab/query/dataset.png)

4. 調査するAdobe Analyticsデータセットを探し、リストを右クリックして、「 **Notebookのクエリデータ」をクリックします** 。空のノートブックにSQLクエリが生成されます。

5. 関数を含む最初の生成済みセルをクリックし、 `qs_connect()` 再生ボタンをクリックして実行します。 この関数は、ノートブックインスタンスとクエリサービスの間に接続を作成します。

   ![](../images/jupyterlab/query/execute.png)

6. 2番目に生成されたSQLクエリからAdobe Analyticsデータセット名をコピーします。この名前は、の後の値になりま `FROM`す。

   ![](../images/jupyterlab/query/dataset_name.png)

7. [ **+]ボタンをクリックして、新しいノートブックのセルを挿** 入します。

   ![](../images/jupyterlab/query/insert_cell.gif)

8. 次のインポート文を新しいセルにコピー、貼り付け、実行します。 次の文は、データを視覚化するために使用されます。

   ```python
   import plotly.plotly as py
   import plotly.graph_objs as go
   from plotly.offline import iplot
   ```

9. 次に、次の変数をコピーして新しいセルに貼り付けます。 必要に応じて値を変更し、実行します。

   ```python
   target_table = "your Adobe Analytics dataset name"
   target_year = "2019"
   target_month = "04"
   target_day = "01"
   ```

   - `target_table` :Adobe Analyticsデータセットの名前。
   - `target_year` :ターゲットデータの元の年。
   - `target_month` :ターゲット元の特定の月。
   - `target_day` :ターゲットデータの元の特定の日。
   >[!NOTE] これらの値はいつでも変更できます。 変更を適用する場合は、必ず変数のセルを実行して変更を適用してください。

## クエリデータ

個々のノートブック・セルに次のSQLクエリを入力します。 クエリを実行するには、セルをクリックし、再生ボタンをク **リック** します。 成功したクエリの結果またはエラーログは、実行されたセルの下に表示されます。

ノートブックが長期間非アクティブな場合、ノートブックとクエリサービスの接続が切断される場合があります。 その場合は、右上隅にある **Power** （パワー）ボタンをクリックしてJupyterLabを再起動します。

![](../images/jupyterlab/query/restart_button.png)

ノートブックのカーネルはリセットされますが、セルは残り、すべてのセ **ルを** 再実行して、中断した場所に移動します。

### 時間別訪問者数

次のクエリは、指定した日付の時間別訪問者数を返します。

#### クエリ

```sql
%%read_sql hourly_visitor -c QS_CONNECTION
SELECT Substring(timestamp, 1, 10)                               AS Day,
       Substring(timestamp, 12, 2)                               AS Hour, 
       Count(DISTINCT concat(enduserids._experience.aaid.id, 
                             _experience.analytics.session.num)) AS Visit_Count 
FROM   {target_table}
WHERE _acp_year = {target_year} 
      AND _acp_month = {target_month}  
      AND _acp_day = {target_day}
GROUP  BY Day, Hour
ORDER  BY Hour;
```

上記のクエリでは、句 `_acp_year` のターゲット `WHERE` がの値に設定されます `target_year`。 変数を中括弧(`{}`)で囲んで、SQLクエリに含めます。

オプションの変数は、クエリの最初の行に含まれま `hourly_visitor`す。 クエリの結果は、この変数にPandasデータフレームとして保存されます。 結果をデータフレームに保存すると、後で目的のPythonパッケージを使用してクエリ結果を視覚化できます。 次のPythonコードを新しいセルで実行し、棒グラフを生成します。

```python
trace = go.Bar(
    x = hourly_visitor['Hour'],
    y = hourly_visitor['Visit_Count'],
    name = "Visitor Count"
)
layout = go.Layout(
    title = 'Visit Count by Hour of Day',
    width = 1200,
    height = 600,
    xaxis = dict(title = 'Hour of Day'),
    yaxis = dict(title = 'Count')
)
fig = go.Figure(data = [trace], layout = layout)
iplot(fig)
```

### 時間別アクティビティ数

次のクエリは、指定した日付の時間別アクション数を返します。

#### クエリ <!-- omit in toc -->

```sql
%%read_sql hourly_actions -d -c QS_CONNECTION
SELECT Substring(timestamp, 1, 10)                        AS Day,
       Substring(timestamp, 12, 2)                        AS Hour, 
       Count(concat(enduserids._experience.aaid.id, 
                    _experience.analytics.session.num,
                    _experience.analytics.session.depth)) AS Count 
FROM   {target_table}
WHERE  _acp_year = {target_year} 
       AND _acp_month = {target_month}  
       AND _acp_day = {target_day}
GROUP  BY Day, Hour
ORDER  BY Hour;
```

上記のクエリを実行すると、結果がデータフレ `hourly_actions` ームとして保存されます。 新しいセルで次の関数を実行し、結果をプレビューします。

```python
hourly_actions.head()
```

上記のクエリを変更して、 **WHERE句の論理演算子を使用して、指定した日付範囲の時間別のアクション数を返すことができます** 。

#### クエリ <!-- omit in toc -->

```sql
%%read_sql hourly_actions_date_range -d -c QS_CONNECTION
SELECT Substring(timestamp, 1, 10)                        AS Day,
       Substring(timestamp, 12, 2)                        AS Hour, 
       Count(concat(enduserids._experience.aaid.id, 
                    _experience.analytics.session.num,
                    _experience.analytics.session.depth)) AS Count 
FROM   {target_table}
WHERE  timestamp >= TO_TIMESTAMP('2019-06-01 00', 'YYYY-MM-DD HH')
       AND timestamp <= TO_TIMESTAMP('2019-06-02 23', 'YYYY-MM-DD HH')
GROUP  BY Day, Hour
ORDER  BY Hour;
```

変更したクエリを実行すると、結果がデータフレ `hourly_actions_date_range` ームとして保存されます。 新しいセルで次の関数を実行し、結果をプレビューします。

```python
hourly_actions_date_rage.head()
```

### 1回のイベントセッションあたりの訪問者数

次のクエリは、指定した日付のイベントセッションあたりの訪問者数を返します。

#### クエリ <!-- omit in toc -->

```sql
%%read_sql events_per_session -c QS_CONNECTION
SELECT concat(enduserids._experience.aaid.id, 
              '-#', 
              _experience.analytics.session.num) AS aaid_sess_key, 
       Count(timestamp)                          AS Count 
FROM   {target_table}
WHERE  _acp_year = {target_year} 
       AND _acp_month = {target_month}  
       AND _acp_day = {target_day}
GROUP BY aaid_sess_key
ORDER BY Count DESC;
```

次のPythonコードを実行して、訪問セッションあたりのイベント数のヒストグラムを生成します。

```python
data = [go.Histogram(x = events_per_session['Count'])]

layout = go.Layout(
    title = 'Histogram of Number of Events per Visit Session',
    xaxis = dict(title = 'Number of Events'),
    yaxis = dict(title = 'Count')
)

fig = go.Figure(data = data, layout = layout)
iplot(fig)
```

### 特定の日の人気のあるページ

次のクエリは、指定した日付で最も人気の高い10ページを返します。

#### クエリ <!-- omit in toc -->

```sql
%%read_sql popular_pages -c QS_CONNECTION
SELECT web.webpagedetails.name                 AS Page_Name, 
       Sum(web.webpagedetails.pageviews.value) AS Page_Views 
FROM   {target_table}
WHERE  _acp_year = {target_year}
       AND _acp_month = {target_month}
       AND _acp_day = {target_day}
GROUP  BY web.webpagedetails.name 
ORDER  BY page_views DESC 
LIMIT  10;
```

### 特定の日のアクティブなユーザー

次のクエリは、指定した日付で最もアクティブな10人のユーザーを返します。

#### クエリ <!-- omit in toc -->

```sql
%%read_sql active_users -c QS_CONNECTION
SELECT enduserids._experience.aaid.id AS aaid, 
       Count(timestamp)               AS Count
FROM   {target_table}
WHERE  _acp_year = {target_year}
       AND _acp_month = {target_month}
       AND _acp_day = {target_day}
GROUP  BY aaid
ORDER  BY Count DESC
LIMIT  10;
```

### ユーザ別のアクティブな市区町村アクティビティ

次のクエリは、指定した日付のユーザーアクティビティの大部分を生成している10の市区町村を返します。

#### クエリ <!-- omit in toc -->

```sql
%%read_sql active_cities -c QS_CONNECTION
SELECT concat(placeContext.geo.stateProvince, ' - ', placeContext.geo.city) AS state_city, 
       Count(timestamp)                                                     AS Count
FROM   {target_table}
WHERE  _acp_year = {target_year}
       AND _acp_month = {target_month}
       AND _acp_day = {target_day}
GROUP  BY state_city
ORDER  BY Count DESC
LIMIT  10;
```

## 次の手順 <!-- omit in toc -->

このチュートリアルでは、Jupyterノートブックのクエリサービスを使用する場合の使用例をいくつか示しました。 「 [Analyze your data using Jupyter Notebooks](./analyze-your-data.md) 」チュートリアルに従って、データアクセスSDKを使用して同様の操作がどの程度実行されるかを確認します。