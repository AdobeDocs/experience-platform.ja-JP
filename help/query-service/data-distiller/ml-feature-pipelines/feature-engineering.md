---
title: 機械学習のエンジニア機能
description: Adobe Experience Platformのデータを、マシンラーニングモデルで使用できる機能や変数に変換する方法を説明します。 Data Distillerを使用してマシンラーニングの機能を大規模に計算し、その機能をマシンラーニングと共有します。
exl-id: 7fe017c9-ec46-42af-ac8f-734c4c6e24b5
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '1140'
ht-degree: 13%

---

# 機械学習のエンジニア機能

このドキュメントでは、Adobe Experience Platformのデータを&#x200B;**機能**、つまりマシンラーニングモデルで使用できる変数に変換する方法を示します。 このプロセスは&#x200B;**機能エンジニアリング**&#x200B;と呼ばれます。 Data Distillerを使用してマシンラーニングの特徴を大規模に計算し、その特徴をマシンラーニング環境と共有します。 これには次が含まれます。

1. クエリテンプレートを作成して、モデルに計算するターゲットラベルと機能を定義します
2. クエリを実行し、結果をトレーニングデータセットに保存します

## トレーニングデータを定義する {#define-training-data}

次の例は、ニュースレターを購読するユーザーの傾向を予測するモデルのExperience Events データセットからトレーニングデータを導き出すクエリを示しています。 サブスクリプションイベントはイベントタイプ `web.formFilledOut`で表され、データセット内の他の行動イベントは、サブスクリプションを予測するためのプロファイルレベルの機能を導き出すために使用されます。

### 正と負のラベルのクエリ {#query-positive-and-negative-labels}

（教師あり）機械学習モデルを訓練するための完全データセットには、予測される結果を表すターゲット変数またはラベル、およびモデルの訓練に使用されるプロファイルサンプルの記述に使用される一連の機能または説明変数が含まれます。

この場合、ラベルは`subscriptionOccurred`という変数で、ユーザープロファイルにタイプ `web.formFilledOut`のイベントがある場合は1に等しく、それ以外の場合は0になります。 次のクエリは、イベントデータセットから50,000 ユーザーのセットを返します。これには、正のラベル （`subscriptionOccurred = 1`）を持つすべてのユーザーと、50,000 ユーザーのサンプルサイズを完了するために負のラベルを持つランダムに選択されたセットのユーザーが含まれます。 これにより、トレーニングデータに、モデルが学習できるポジティブな例とネガティブな例の両方が含まれるようになります。

```python
from aepp import queryservice

dd_conn = queryservice.QueryService().connection()
dd_cursor = queryservice.InteractiveQuery2(dd_conn)

query_labels = f"""
SELECT *
FROM (
    SELECT
        eventType,
        _{tenant_id}.user_id as userId,
        SUM(CASE WHEN eventType='web.formFilledOut' THEN 1 ELSE 0 END) 
            OVER (PARTITION BY _{tenant_id}.user_id) 
            AS "subscriptionOccurred",
        row_number() OVER (PARTITION BY _{tenant_id}.user_id ORDER BY randn()) AS random_row_number_for_user
    FROM {table_name}
)
WHERE (subscriptionOccurred = 1 AND eventType = 'web.formFilledOut') OR (subscriptionOccurred = 0 AND random_row_number_for_user = 1)
"""

df_labels = dd_cursor.query(query_labels, output="dataframe")
print(f"Number of classes: {len(df_labels)}")
df_labels.head()
```

**出力例**

クラス数：50000

|   | eventType | userId | subscriptionOccurred | random_row_number_for_user |
| ---  |   ---  |   ---  |   ---  |   --- |
| 0 | directMarketing.emailClicked | 01027994177972439148069092698714414382 | 0 | 1 |
| 1 | directMarketing.emailOpened | 01054714817856066632264746967668888198 | 0 | 1 |
| 2 | web.formFilledOut | 01117296890525140996735553609305695042 | 1 | 15 |
| 3 | directMarketing.emailClicked | 01149554820363915324573708359099551093 | 0 | 1 |
| 4 | directMarketing.emailClicked | 01172121447143590196349410086995740317 | 0 | 1 |

{style="table-layout:auto"}

### イベントを集計してマシンラーニングの特徴を定義する {#define-features}

適切なクエリを使用すると、データセット内のイベントを、傾向モデルのトレーニングに使用できる有意義な数値の特徴に収集できます。 イベントの例を以下に示します。

- マーケティング目的で送信され、ユーザーが受信した&#x200B;**メール数**。
- **開封**&#x200B;されたメールの一部。
- ユーザー&#x200B;**がリンクを選択**&#x200B;したメールの一部。
- **閲覧された製品の数**。
- **とインタラクションした**&#x200B;提案の数。
- 却下された&#x200B;**提案の数**。
- 選択された&#x200B;**リンクの数**。
- 連続した2通のメールを受信するまでの分数。
- 連続した2通のメールが開封されるまでの時間（分）。
- ユーザーが実際にリンクを選択した2つの連続するメール間の分数。
- 連続する2つの製品ビュー間の分数。
- やり取りされた2つの提案の間の分数。
- 却下された2つの提案の間の分数。
- 選択された2つのリンク間の分数。

次のクエリは、これらのイベントを集計します。

+++選択してサンプル クエリを表示

```python
query_features = f"""
SELECT
    _{tenant_id}.user_id as userId,
    SUM(CASE WHEN eventType='directMarketing.emailSent' THEN 1 ELSE 0 END) 
       OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
       AS "emailsReceived",
    SUM(CASE WHEN eventType='directMarketing.emailOpened' THEN 1 ELSE 0 END) 
       OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
       AS "emailsOpened",       
    SUM(CASE WHEN eventType='directMarketing.emailClicked' THEN 1 ELSE 0 END) 
       OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
       AS "emailsClicked",       
    SUM(CASE WHEN eventType='commerce.productViews' THEN 1 ELSE 0 END) 
       OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
       AS "productsViewed",       
    SUM(CASE WHEN eventType='decisioning.propositionInteract' THEN 1 ELSE 0 END) 
       OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
       AS "propositionInteracts",       
    SUM(CASE WHEN eventType='decisioning.propositionDismiss' THEN 1 ELSE 0 END) 
       OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
       AS "propositionDismissed",
    SUM(CASE WHEN eventType='web.webinteraction.linkClicks' THEN 1 ELSE 0 END) 
       OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
       AS "webLinkClicks" ,
    TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'directMarketing.emailSent', 'minutes')
       OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
       AS "minutes_since_emailSent",
    TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'directMarketing.emailOpened', 'minutes')
       OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
       AS "minutes_since_emailOpened",
    TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'directMarketing.emailClicked', 'minutes')
       OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
       AS "minutes_since_emailClick",
    TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'commerce.productViews', 'minutes')
       OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
       AS "minutes_since_productView",
    TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'decisioning.propositionInteract', 'minutes')
       OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
       AS "minutes_since_propositionInteract",
    TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'propositionDismiss', 'minutes')
       OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
       AS "minutes_since_propositionDismiss",
    TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'web.webinteraction.linkClicks', 'minutes')
       OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
       AS "minutes_since_linkClick"
FROM {table_name}
"""

df_features = dd_cursor.query(query_features, output="dataframe")
df_features.head()
```

+++

**出力例**

|   | userId | emailsReceived | emailsOpened | emailsClicked | productsViewed | propositionInteracts | propositionDismissed | webLinkClicks | minutes_since_emailSent | minutes_since_emailOpened | minutes_since_emailClick | minutes_since_productView | minutes_since_propositionInteract | minutes_since_propositionDismiss | minutes_since_linkClick |
| --- |    --- |    ---   |  ---  |   ---  |   ---  |  ---  |  ---  |   ---  |   ---  |   ---  |   ---  |   ---  |   ---  |   ---  |   --- |
| 0 | 01102546977582484968046916668339306826 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0.0 | NaN | NaN | NaN | NaN | なし | NaN |
| 1 | 01102546977582484968046916668339306826 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0.0 | NaN | NaN | NaN | NaN | なし | NaN |
| 2 | 01102546977582484968046916668339306826 | 3 | 0 | 0 | 0 | 0 | 0 | 0 | 0.0 | NaN | NaN | NaN | NaN | なし | NaN |
| 3 | 01102546977582484968046916668339306826 | 3 | 1 | 0 | 0 | 0 | 0 | 0 | 540.0 | 0.0 | NaN | NaN | NaN | なし | NaN |
| 4 | 01102546977582484968046916668339306826 | 3 | 2 | 0 | 0 | 0 | 0 | 0 | 588.0 | 0.0 | NaN | NaN | NaN | なし | NaN |

{style="table-layout:auto"}

#### ラベルとフィーチャのクエリの結合 {#combine-queries}

最後に、ラベルクエリと機能クエリを単一のクエリに組み合わせて、ラベルと機能のトレーニングデータセットを返すことができます。

+++選択してサンプル クエリを表示

```python
query_training_set = f"""
SELECT *
FROM (
    SELECT _{tenant_id}.user_id as userId, 
       eventType,
       timestamp,
       SUM(CASE WHEN eventType='web.formFilledOut' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id) 
           AS "subscriptionOccurred",
       SUM(CASE WHEN eventType='directMarketing.emailSent' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "emailsReceived",
       SUM(CASE WHEN eventType='directMarketing.emailOpened' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "emailsOpened",       
       SUM(CASE WHEN eventType='directMarketing.emailClicked' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "emailsClicked",       
       SUM(CASE WHEN eventType='commerce.productViews' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "productsViewed",       
       SUM(CASE WHEN eventType='decisioning.propositionInteract' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "propositionInteracts",       
       SUM(CASE WHEN eventType='decisioning.propositionDismiss' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "propositionDismissed",
       SUM(CASE WHEN eventType='web.webinteraction.linkClicks' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "webLinkClicks" ,
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'directMarketing.emailSent', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_emailSent",
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'directMarketing.emailOpened', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_emailOpened",
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'directMarketing.emailClicked', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_emailClick",
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'commerce.productViews', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_productView",
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'decisioning.propositionInteract', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_propositionInteract",
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'propositionDismiss', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_propositionDismiss",
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'web.webinteraction.linkClicks', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_linkClick",
        row_number() OVER (PARTITION BY _{tenant_id}.user_id ORDER BY randn()) AS random_row_number_for_user
    FROM {table_name} LIMIT 1000
)
WHERE (subscriptionOccurred = 1 AND eventType = 'web.formFilledOut') OR (subscriptionOccurred = 0 AND random_row_number_for_user = 1)
ORDER BY timestamp
"""

df_training_set = dd_cursor.query(query_training_set, output="dataframe")
df_training_set.head()
```

+++

**出力例**

|  | userId | eventType | タイムスタンプ | subscriptionOccurred | emailsReceived | emailsOpened | emailsClicked | productsViewed | propositionInteracts | propositionDismissed | webLinkClicks | minutes_since_emailSent | minutes_since_emailOpened | minutes_since_emailClick | minutes_since_productView | minutes_since_propositionInteract | minutes_since_propositionDismiss | minutes_since_linkClick | random_row_number_for_user |
| ---  |  --- |   ---  |  ---  |  ---  |  ---  |  ---  |  ---  |  ---  |  ---  |  ---  |  ---  |  ---  |  ---  |  ---  |  ---   | ---  |  ---  |  ---  |  --- |
| 0 | 02554909162592418347780983091131567290 | directMarketing.emailSent | 2023-06-17 13:44:59.086 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0.0 | NaN | NaN | NaN | NaN | なし | NaN | 1 |
| 1 | 01130334080340815140184601481559659945 | directMarketing.emailOpened | 2023-06-19 06:01:55.366 | 0 | 1 | 3 | 0 | 1 | 0 | 0 | 0 | 1921.0 | 0.0 | NaN | 1703.0 | NaN | なし | NaN | 1 |
| 2 | 01708961660028351393477273586554010192 | web.formFilledOut | 2023-06-19 18:36:49.083 | 1 | 1 | 2 | 2 | 0 | 0 | 0 | 0 | 2365.0 | 26.0 | 1.0 | NaN | NaN | なし | NaN | 7 |
| 3 | 01809182902320674899156240602124740853 | directMarketing.emailSent | 2023-06-21 19:17:12.535 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0.0 | NaN | NaN | NaN | NaN | なし | NaN | 1 |
| 4 | 03441761949943678951106193028739001197 | directMarketing.emailSent | 2023-06-21 21:58:29.482 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0.0 | NaN | NaN | NaN | NaN | なし | NaN | 1 |

{style="table-layout:auto"}

## トレーニングデータを段階的に計算するためのクエリテンプレートを作成する

モデルの精度を継続的に維持するために、更新されたトレーニングデータを使用してモデルを定期的に再トレーニングすることが一般的です。 トレーニングデータセットを効率的に更新するためのベストプラクティスとして、トレーニングセットクエリからテンプレートを作成し、新しいトレーニングデータを段階的に計算できます。 これにより、トレーニングデータが最後に更新されてから元のExperience Events データセットに追加されたデータからのみラベルと機能を計算し、新しいラベルと機能を既存のトレーニングデータセットに挿入できます。

これには、トレーニングセットクエリに対していくつかの変更が必要です。

- 存在しない場合は新しいトレーニングデータセットを作成するロジックを追加し、存在しない場合は新しいラベルと機能を既存のトレーニングデータセットに挿入します。 これには、トレーニングセットクエリの一連の2つのバージョンが必要です。
   - まず、`CREATE TABLE IF NOT EXISTS {table_name} AS` ステートメントを使用します
   - 次に、トレーニングデータセットが既に存在する場合の`INSERT INTO {table_name}` ステートメントを使用します
- 指定された期間内に追加されたイベントデータにクエリを制限するには、`SNAPSHOT BETWEEN $from_snapshot_id AND $to_snapshot_id` ステートメントを追加します。 スナップショット IDの`$`接頭辞は、クエリテンプレートの実行時に渡される変数であることを示します。

これらの変更を適用すると、次のクエリが表示されます。

+++選択してサンプル クエリを表示

```python
ctas_table_name = "propensity_training_set"

query_training_set_template = f"""
$$ BEGIN

SET @my_table_exists = SELECT table_exists('{ctas_table_name}');

CREATE TABLE IF NOT EXISTS {ctas_table_name} AS
SELECT *
FROM (
    SELECT _{tenant_id}.user_id as userId, 
       eventType,
       timestamp,
       SUM(CASE WHEN eventType='web.formFilledOut' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id) 
           AS "subscriptionOccurred",
       SUM(CASE WHEN eventType='directMarketing.emailSent' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "emailsReceived",
       SUM(CASE WHEN eventType='directMarketing.emailOpened' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "emailsOpened",       
       SUM(CASE WHEN eventType='directMarketing.emailClicked' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "emailsClicked",       
       SUM(CASE WHEN eventType='commerce.productViews' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "productsViewed",       
       SUM(CASE WHEN eventType='decisioning.propositionInteract' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "propositionInteracts",       
       SUM(CASE WHEN eventType='decisioning.propositionDismiss' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "propositionDismissed",
       SUM(CASE WHEN eventType='web.webinteraction.linkClicks' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "webLinkClicks" ,
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'directMarketing.emailSent', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_emailSent",
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'directMarketing.emailOpened', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_emailOpened",
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'directMarketing.emailClicked', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_emailClick",
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'commerce.productViews', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_productView",
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'decisioning.propositionInteract', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_propositionInteract",
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'propositionDismiss', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_propositionDismiss",
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'web.webinteraction.linkClicks', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_linkClick",
        row_number() OVER (PARTITION BY _{tenant_id}.user_id ORDER BY randn()) AS random_row_number_for_user
    FROM {table_name}
    SNAPSHOT BETWEEN $from_snapshot_id AND $to_snapshot_id
)
WHERE (subscriptionOccurred = 1 AND eventType = 'web.formFilledOut') OR (subscriptionOccurred = 0 AND random_row_number_for_user = 1)
ORDER BY timestamp;

INSERT INTO {ctas_table_name}
SELECT *
FROM (
    SELECT _{tenant_id}.user_id as userId, 
       eventType,
       timestamp,
       SUM(CASE WHEN eventType='web.formFilledOut' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id) 
           AS "subscriptionOccurred",
       SUM(CASE WHEN eventType='directMarketing.emailSent' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "emailsReceived",
       SUM(CASE WHEN eventType='directMarketing.emailOpened' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "emailsOpened",       
       SUM(CASE WHEN eventType='directMarketing.emailClicked' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "emailsClicked",       
       SUM(CASE WHEN eventType='commerce.productViews' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "productsViewed",       
       SUM(CASE WHEN eventType='decisioning.propositionInteract' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "propositionInteracts",       
       SUM(CASE WHEN eventType='decisioning.propositionDismiss' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "propositionDismissed",
       SUM(CASE WHEN eventType='web.webinteraction.linkClicks' THEN 1 ELSE 0 END) 
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "webLinkClicks" ,
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'directMarketing.emailSent', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_emailSent",
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'directMarketing.emailOpened', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_emailOpened",
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'directMarketing.emailClicked', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_emailClick",
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'commerce.productViews', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_productView",
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'decisioning.propositionInteract', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_propositionInteract",
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'propositionDismiss', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_propositionDismiss",
       TIME_BETWEEN_PREVIOUS_MATCH(timestamp, eventType = 'web.webinteraction.linkClicks', 'minutes')
           OVER (PARTITION BY _{tenant_id}.user_id ORDER BY timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
           AS "minutes_since_linkClick",
        row_number() OVER (PARTITION BY _{tenant_id}.user_id ORDER BY randn()) AS random_row_number_for_user
    FROM {table_name}
    SNAPSHOT BETWEEN $from_snapshot_id AND $to_snapshot_id
)
WHERE 
    @my_table_exists = 't' AND
    ((subscriptionOccurred = 1 AND eventType = 'web.formFilledOut') OR (subscriptionOccurred = 0 AND random_row_number_for_user = 1))
ORDER BY timestamp;

EXCEPTION
  WHEN OTHER THEN
    SELECT 'ERROR';

END $$;
"""
```

+++

最後に、次のコードを使用して、クエリテンプレートをData Distillerに保存します。

```python
template_res = dd.createQueryTemplate({
  "sql": query_training_set_template,
  "queryParameters": {},
  "name": "Template for propensity training data"
})
template_id = template_res["id"]

print(f"Template for propensity training data created as ID {template_id}")
```

**出力例**

`Template for propensity training data created as ID f3d1ec6b-40c2-4d13-93b6-734c1b3c7235`

テンプレートを保存した後は、テンプレート IDを参照して、クエリにいつでもクエリを実行し、クエリに含めるスナップショット IDの範囲を指定できます。 次のクエリは、元のExperience Events データセットのスナップショットを取得します。

```python
query_snapshots = f"""
SELECT snapshot_id 
FROM (
    SELECT history_meta('{table_name}')
) 
WHERE is_current = true OR snapshot_generation = 0 
ORDER BY snapshot_generation ASC
"""

df_snapshots = dd_cursor.query(query_snapshots, output="dataframe")
```

次のコードは、最初と最後のスナップショットを使用してデータセット全体をクエリする、クエリテンプレートの実行を示しています。

```python
snapshot_start_id = str(df_snapshots["snapshot_id"].iloc[0])
snapshot_end_id = str(df_snapshots["snapshot_id"].iloc[1])

query_final_res = qs.postQueries(
    name=f"[CMLE][Week2] Query to generate training data created by {username}",
    templateId=template_id,
    queryParameters={
        "from_snapshot_id": snapshot_start_id,
        "to_snapshot_id": snapshot_end_id,
    },
    dbname=f"{cat_conn.sandbox}:all"
)
query_final_id = query_final_res["id"]
print(f"Query started successfully and got assigned ID {query_final_id} - it will take some time to execute")
```

**出力例**

`Query started successfully and got assigned ID c6ea5009-1315-4839-b072-089ae01e74fd - it will take some time to execute`

次の関数を定義して、クエリのステータスを定期的に確認できます。

```python
def wait_for_query_completion(query_id):
    while True:
        query_info = qs.getQuery(query_id)
        query_state = query_info["state"]
        if query_state in ["SUCCESS", "FAILED"]:
            break
        print("Query is still in progress, sleeping…")
        time.sleep(60)

    duration_secs = query_info["elapsedTime"] / 1000
    if query_state == "SUCCESS":
        print(f"Query completed successfully in {duration_secs} seconds")
    else:
        print(f"Query failed with the following errors:", file=sys.stderr)
        for error in query_info["errors"]:
            print(f"Error code {error['code']}: {error['message']}", file=sys.stderr)

wait_for_query_completion(query_final_id)
```

**出力例**

```console
Query is still in progress, sleeping…
Query is still in progress, sleeping…
Query is still in progress, sleeping…
Query is still in progress, sleeping…
Query is still in progress, sleeping…
Query is still in progress, sleeping…
Query is still in progress, sleeping…
Query is still in progress, sleeping…
Query completed successfully in 473.8 seconds
```

## 次のステップ：

このドキュメントでは、Adobe Experience Platformのデータを、マシンラーニングモデルで使用できる機能または変数に変換する方法について説明しました。 Experience Platformから機能パイプラインを作成し、マシンラーニング環境でカスタムモデルをフィードする次のステップは、[機能データセットを書き出すことです](./export-data.md)。
