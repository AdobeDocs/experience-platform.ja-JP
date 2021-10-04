---
keywords: Experience Platform;JupyterLab；ノートブック；Data Science Workspace；よく読まれるトピック；クエリサービス
solution: Experience Platform
title: Jupyter ノートブックでのクエリサービス
topic-legacy: tutorial
type: Tutorial
description: Adobe Experience Platform を使用すると、クエリサービスを標準機能として JupyterLab に統合することにより、Data Science Workspace で構造化照会言語（SQL）を使用できます。このチュートリアルでは、Adobe Analyticsデータを調査、変換、分析する一般的な使用例に関する SQL クエリの例を示します。
exl-id: c5ac7d11-a3bd-4ef8-a650-9f496a8bbaa7
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '822'
ht-degree: 58%

---

# Jupyter ノートブックでのクエリサービス

[!DNL Adobe Experience Platform] では、を標準機能として統合することで、構造化クエ [!DNL Data Science Workspace] リ言語 (SQL [!DNL Query Service] ) を [!DNL JupyterLab] 使用できます。

このチュートリアルでは、[!DNL Adobe Analytics] データを調査、変換、分析する一般的な使用例に対する SQL クエリの例を示します。

## はじめに

このチュートリアルを開始する前に、次の前提条件を満たす必要があります。

- [!DNL Adobe Experience Platform] へのアクセス [!DNL Experience Platform] の IMS 組織にアクセスできない場合は、先に進む前にシステム管理者にお問い合わせください

- [!DNL Adobe Analytics] データセット

- このチュートリアルで使用する次の主要概念に対する十分な理解
   - [[!DNL Experience Data Model (XDM) and XDM System]](../../xdm/home.md)
   - [[!DNL Query Service]](../../query-service/home.md)
   - [[!DNL Query Service SQL Syntax]](../../query-service/sql/overview.md)
   - Adobe Analytics

## [!DNL JupyterLab] と [!DNL Query Service] にアクセス {#access-jupyterlab-and-query-service}

1. [[!DNL Experience Platform]](https://platform.adobe.com) で、左側のナビゲーション列から **[!UICONTROL ノートブック]** に移動します。 JupyterLab が読み込まれるまで、しばらく待ちます。

   ![](../images/jupyterlab/query/jupyterlab-launcher.png)

   >[!NOTE]
   >
   >新しい「ランチャー」タブが自動的に表示されなかった場合は、「**[!UICONTROL ファイル]**」をクリックして新しい「ランチャー」タブを開き、「**[!UICONTROL 新規ランチャー]**」を選択します。

2. 「ランチャー」タブで、Python 3 環境の「**[!UICONTROL 空白]**」アイコンをクリックして、空のノートブックを開きます。

   ![](../images/jupyterlab/query/blank_notebook.png)

   >[!NOTE]
   >
   > 現在、ノートブックのクエリサービスでサポートされている環境は Python 3 のみです。

3. 左側の選択パネルで、**[!UICONTROL データ]**&#x200B;アイコンをクリックし、「**[!UICONTROL データセット]**」ディレクトリをダブルクリックして、すべてのデータセットをリストします。

   ![](../images/jupyterlab/query/dataset.png)

4. 調査する [!DNL Adobe Analytics] データセットを見つけ、リストを右クリックし、「**[!UICONTROL ノートブックのデータをクエリ]**」をクリックして、空のノートブックで SQL クエリを生成します。

5. `qs_connect()` 関数が含まれる最初の生成済みセルをクリックし、再生ボタンをクリックして実行します。この関数は、ノートブックインスタンスと [!DNL Query Service] の間に接続を作成します。

   ![](../images/jupyterlab/query/execute.png)

6. 2 番目に生成された SQL クエリから [!DNL Adobe Analytics] データセット名をコピーします。`FROM` の後の値になります。

   ![](../images/jupyterlab/query/dataset_name.png)

7. 「**+**」ボタンをクリックして、ノートブックの新しいセルを挿入します。

   ![](../images/jupyterlab/query/insert_cell.gif)

8. 次のインポートステートメントをコピーし、新しいセルに貼り付けて実行します。これらのステートメントは、データを視覚化するために使用されます。

   ```python
   import plotly.plotly as py
   import plotly.graph_objs as go
   from plotly.offline import iplot
   ```

9. 次に、以下の変数をコピーし、新しいセルに貼り付けます。必要に応じて値を変更してから、値を実行します。

   ```python
   target_table = "your Adobe Analytics dataset name"
   target_year = "2019"
   target_month = "04"
   target_day = "01"
   ```

   - `target_table` :データセットの [!DNL Adobe Analytics] 名前。
   - `target_year`：ターゲットデータの元の年。
   - `target_month`：ターゲットデータの元の月。
   - `target_day`：ターゲットデータの元の日。

   >[!NOTE]
   >
   > これらの値はいつでも変更できます。値を変更する場合は、変数セルを実行して、変更を適用する必要があります。

## データのクエリ {#query-your-data}

個々のノートブックセルに次の SQL クエリを入力します。クエリを実行するには、セルでを選択し、「**[!UICONTROL 再生]**」ボタンを選択します。 成功したクエリの結果またはエラーログは、実行されたセルの下に表示されます。

ノートブックが長期間非アクティブになると、ノートブックと [!DNL Query Service] の間の接続が切断される場合があります。 その場合は、電源ボタンの隣の右上隅にある **再起動** ボタン ![ 再起動ボタン ](../images/jupyterlab/user-guide/restart_button.png) を選択して [!DNL JupyterLab] を再起動します。

ノートブックのカーネルはリセットされますが、セルは残り、すべてのセルを再実行して、中断した場所から続行します。

### 1 時間ごとの訪問者数 {#hourly-visitor-count}

次のクエリは、指定した日付の 1 時間ごとの訪問者数を返します。

#### クエリ

```sql
%%read_sql hourly_visitor -c QS_CONNECTION
SELECT Substring(timestamp, 1, 10)                               AS Day,
       Substring(timestamp, 12, 2)                               AS Hour, 
       Count(DISTINCT concat(enduserids._experience.aaid.id, 
                             _experience.analytics.session.num)) AS Visit_Count 
FROM   {target_table}
WHERE TIMESTAMP = to_timestamp('{target_year}-{target_month}-{target_day}')
GROUP  BY Day, Hour
ORDER  BY Hour;
```

上記のクエリでは、`WHERE` 句のタイムスタンプが `target_year` の値に設定されます。 変数を中括弧（`{}`）で囲んで、SQL クエリに含めます。

オプションの変数 `hourly_visitor` は、クエリの最初の行に含まれます。クエリの結果は、この変数に Pandas データフレームとして保存されます。結果をデータフレームに保存すると、後で目的の [!DNL Python] パッケージを使用してクエリ結果を視覚化できます。 新しいセルで次の [!DNL Python] コードを実行して、棒グラフを生成します。

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

### 1 時間ごとのアクティビティ数 {#hourly-activity-count}

次のクエリは、指定した日付の 1 時間ごとのアクション数を返します。

#### クエリ <!-- omit in toc -->

```sql
%%read_sql hourly_actions -d -c QS_CONNECTION
SELECT Substring(timestamp, 1, 10)                        AS Day,
       Substring(timestamp, 12, 2)                        AS Hour, 
       Count(concat(enduserids._experience.aaid.id, 
                    _experience.analytics.session.num,
                    _experience.analytics.session.depth)) AS Count 
FROM   {target_table}
WHERE TIMESTAMP = to_timestamp('{target_year}-{target_month}-{target_day}')
GROUP  BY Day, Hour
ORDER  BY Hour;
```

上記のクエリを実行すると、結果が `hourly_actions` にデータフレームとして保存されます。新しいセルで次の関数を実行し、結果をプレビューします。

```python
hourly_actions.head()
```

上記のクエリを変更し、**WHERE** 句で論理演算子を使用して、指定した日付範囲の 1 時間ごとのアクション数を返すことができます。

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

変更したクエリを実行すると、結果が `hourly_actions_date_range` にデータフレームとして保存されます。 新しいセルで次の関数を実行し、結果をプレビューします。

```python
hourly_actions_date_rage.head()
```

### 訪問者セッションごとのイベント数 {#number-of-events-per-visitor-session}

次のクエリは、指定した日付の訪問者セッションごとのイベント数を返します。

#### クエリ <!-- omit in toc -->

```sql
%%read_sql events_per_session -c QS_CONNECTION
SELECT concat(enduserids._experience.aaid.id, 
              '-#', 
              _experience.analytics.session.num) AS aaid_sess_key, 
       Count(timestamp)                          AS Count 
FROM   {target_table}
WHERE TIMESTAMP = to_timestamp('{target_year}-{target_month}-{target_day}')
GROUP BY aaid_sess_key
ORDER BY Count DESC;
```

次の [!DNL Python] コードを実行して、訪問セッションごとのイベント数のヒストグラムを生成します。

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

### 特定の日の人気ページ {#popular-pages-for-a-given-day}

次のクエリは、指定した日付の最も人気の高い 10 ページを返します。

#### クエリ <!-- omit in toc -->

```sql
%%read_sql popular_pages -c QS_CONNECTION
SELECT web.webpagedetails.name                 AS Page_Name, 
       Sum(web.webpagedetails.pageviews.value) AS Page_Views 
FROM   {target_table}
WHERE TIMESTAMP = to_timestamp('{target_year}-{target_month}-{target_day}')
GROUP  BY web.webpagedetails.name 
ORDER  BY page_views DESC 
LIMIT  10;
```

### 特定の日のアクティブユーザー {#active-users-for-a-given-day}

次のクエリは、指定した日付の最もアクティブな 10 人のユーザーを返します。

#### クエリ <!-- omit in toc -->

```sql
%%read_sql active_users -c QS_CONNECTION
SELECT enduserids._experience.aaid.id AS aaid, 
       Count(timestamp)               AS Count
FROM   {target_table}
WHERE TIMESTAMP = to_timestamp('{target_year}-{target_month}-{target_day}')
GROUP  BY aaid
ORDER  BY Count DESC
LIMIT  10;
```

### ユーザアクティビティごとのアクティブな都市 {#active-cities-by-user-activity}

次のクエリは、指定した日付のユーザーアクティビティの大部分を生成している 10 都市を返します。

#### クエリ <!-- omit in toc -->

```sql
%%read_sql active_cities -c QS_CONNECTION
SELECT concat(placeContext.geo.stateProvince, ' - ', placeContext.geo.city) AS state_city, 
       Count(timestamp)                                                     AS Count
FROM   {target_table}
WHERE TIMESTAMP = to_timestamp('{target_year}-{target_month}-{target_day}')
GROUP  BY state_city
ORDER  BY Count DESC
LIMIT  10;
```

## 次の手順

このチュートリアルでは、[!DNL Jupyter] ノートブックの [!DNL Query Service] を使用する使用例をいくつか示しました。 「[Jupyter ノートブックによるデータの分析](./analyze-your-data.md)」のチュートリアルに従って、Data Access SDK を使用して同様の操作がどのように実行されるかを確認します。
