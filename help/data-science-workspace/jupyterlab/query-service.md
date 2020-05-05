---
keywords: Experience Platform;JupyterLab;notebooks;Data Science Workspace;popular topics
solution: Experience Platform
title: ジュピターノートのクエリサービス
topic: Tutorial
translation-type: tm+mt
source-git-commit: 1447196da7dbf59c1f498de40f12ed74c328c0e6

---


# ジュピターノートのクエリサービス

Adobe Experience Platformでは、Structured Language(SQL)をData Science Workspaceで使用できます。これにより、クエリサービスが標準機能としてJupterLabに統合されます。

このチュートリアルでは、Adobe Analyticsデータを調査、変換、分析する一般的な使用例のサンプルSQLクエリを示します。

## はじめに

このチュートリアルを開始する前に、次の前提条件を満たす必要があります。

- Adobe Experience Platformへのアクセス。 Experience PlatformのIMS組織にアクセスできない場合は、次に進む前に、システム管理者にお問い合わせください。

- Adobe Analyticsデータセット

- このチュートリアルで使用する次の主要概念の実際の理解
   - [Experience Data Model(XDM)およびXDMシステム](../../xdm/home.md)
   - [クエリサービス](../../query-service/home.md)
   - [クエリサービスのSQL構文](../../query-service/sql/overview.md)
   - Adobe Analytics

## JupyterLabおよびクエリサービスへのアクセス {#access-jupyterlab-and-query-service}

1. 「 [エクスペリエンスプラットフォーム](https://platform.adobe.com)」で、左のナビゲーション列 **[!UICONTROL Notebooks]** に移動します。 JupyterLabが読み込まれるまで、少し時間をお待ちください。

   ![](../images/jupyterlab/query/jupyterlab_launcher.png)

   > [!NOTE] 新しい「ランチャー」タブが自動的に表示されなかった場合は、をクリックして新しい「ランチャー」タブを開き、を選択 **[!UICONTROL File]** し **[!UICONTROL New Launcher]**&#x200B;ます。

2. 「ランチャー」タブで、Python 3環境の **[!UICONTROL Blank]** アイコンをクリックして、空のノートブックを開きます。

   ![](../images/jupyterlab/query/blank_notebook.png)

   > [!NOTE] Python 3は、現在、ノートブックのクエリサービスでサポートされている唯一の環境です。

3. 左側の選択レールで、 **[!UICONTROL Data]** アイコンをクリックし、重複をクリックして、すべてのデータセットをリストする **[!UICONTROL Datasets]** ディレクトリをクリックします。

   ![](../images/jupyterlab/query/dataset.png)

4. 調査するAdobe Analyticsデータセットを探し、リストを右クリックして、をクリックし、空のノートブック **[!UICONTROL Query Data in Notebook]** にSQLクエリを生成します。

5. 関数を含む最初に生成されたセルをクリックし `qs_connect()` 、再生ボタンをクリックして実行します。 この関数は、ノートブックインスタンスとクエリサービスとの間の接続を作成します。

   ![](../images/jupyterlab/query/execute.png)

6. 2番目に生成されたSQLクエリからAdobe Analyticsデータセット名をコピーします。この名前は、の後の値になり `FROM`ます。

   ![](../images/jupyterlab/query/dataset_name.png)

7. [ **+]** ボタンをクリックして、新しいノートブックのセルを挿入します。

   ![](../images/jupyterlab/query/insert_cell.gif)

8. 次のインポート・ステートメントを新しいセルにコピー、貼り付け、実行します。 以下の文は、データを視覚化するために使用されます。

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

   - `target_table` : Adobe Analyticsデータセットの名前。
   - `target_year` : ターゲットデータの元となる特定の年。
   - `target_month` : ターゲットの開始月を指定します。
   - `target_day` : ターゲットデータの元となる特定の日。
   >[!NOTE] これらの値はいつでも変更できます。 変更を適用する場合は、必ず変数セルを実行し、変更を適用してください。

## データのクエリ {#query-your-data}

個々のノートブック・セルに次のSQLクエリを入力します。 クエリを実行するには、セルをクリックし、次に **[!UICONTROL play]** ボタンをクリックします。 正常なクエリ結果またはエラーログが、実行されたセルの下に表示されます。

ノートブックが長時間非アクティブな場合、ノートブックとクエリサービス間の接続が切断される場合があります。 このような場合は、右上隅にある **[!UICONTROL Power]** ボタンをクリックしてJupterLabを再起動します。

![](../images/jupyterlab/query/restart_button.png)

ノートブックのカーネルはリセットされますが、セルは残ります。セルを再実行して、中断した場所 **[!UICONTROL all]** に移動してください。

### 時間別訪問者数 {#hourly-visitor-count}

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

上記のクエリでは、 `_acp_year` 節のターゲットがの値に設定され `WHERE``target_year`ます。 変数を波括弧(`{}`)で囲んで、SQLクエリに含めます。

クエリの最初の行には、オプションの変数が含まれ `hourly_visitor`ます。 クエリの結果は、この変数にPandasのデータフレームとして保存されます。 結果をデータフレームに格納すると、目的のPythonパッケージを使用して、後でクエリ結果を視覚化できます。 次のPythonコードを新しいセルで実行して、棒グラフを生成します。

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

### 時間別アクティビティ数 {#hourly-activity-count}

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

上記のクエリを実行すると、結果がデータフレーム `hourly_actions` として保存されます。 新しいセルで次の関数を実行して、結果をプレビューします。

```python
hourly_actions.head()
```

上記のクエリを変更して、 **WHERE** 節の論理演算子を使用して、指定した日付範囲に対して時間別のアクション数を返すことができます。

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

変更したクエリを実行すると、結果がデータフレーム `hourly_actions_date_range` として保存されます。 新しいセルで次の関数を実行して、結果をプレビューします。

```python
hourly_actions_date_rage.head()
```

### 訪問者セッションあたりのイベント数 {#number-of-events-per-visitor-session}

次のクエリは、指定した日付に対する訪問者セッションあたりのイベント数を返します。

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

### 特定の日の人気のあるページ {#popular-pages-for-a-given-day}

次のクエリは、指定した日付に対して、最頻訪問ページ10ページを返します。

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

### 特定の日のアクティブなユーザー {#active-users-for-a-given-day}

次のクエリは、指定した日付に対して最もアクティブなユーザー10人を返します。

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

### ユーザーアクティビティ別のアクティブな市区町村 {#active-cities-by-user-activity}

次のクエリは、指定した日付に対して、ユーザーアクティビティの大部分を生み出している10の市区町村を返します。

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

このチュートリアルでは、Jupyterノートブックのクエリサービスを使用する場合の使用例をいくつか示しました。 「Jupyter Notebooks [(ジャプターノートブックを使用したデータの](./analyze-your-data.md) 分析)」チュートリアルに従って、データアクセスSDKを使用して同様の操作がどの程度実行されているかを確認します。