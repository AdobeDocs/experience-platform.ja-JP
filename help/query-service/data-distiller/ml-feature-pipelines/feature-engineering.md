---
title: 機械学習のエンジニア機能
description: Adobe Experience Platformのデータを、機械学習モデルで使用できる機能または変数に変換する方法を説明します。 Data Distillerを使用して、ML 機能を大規模に計算し、それらの機能をマシンラーニング環境と共有します。
exl-id: 7fe017c9-ec46-42af-ac8f-734c4c6e24b5
source-git-commit: 308d07cf0c3b4096ca934a9008a13bf425dc30b6
workflow-type: tm+mt
source-wordcount: '1140'
ht-degree: 13%

---

# 機械学習のエンジニア機能

このドキュメントでは、Adobe Experience Platformのデータを、機械学習モデルで使用できる **feature** または変数に変換する方法について説明します。 このプロセスは、**フィーチャーエンジニアリング** と呼ばれます。 Data Distillerを使用して、ML 機能を大規模に計算し、それらの機能を機械学習環境と共有します。 これには以下が含まれます。

1. クエリテンプレートを作成して、モデルに対して計算するターゲットラベルおよび機能を定義します
2. クエリを実行し、結果をトレーニングデータセットに保存します

## トレーニングデータを定義 {#define-training-data}

次の例では、ニュースレターを購読するユーザーの傾向を予測するモデルのエクスペリエンスイベントデータセットからトレーニングデータを取得するクエリを示しています。 購読イベントはイベントタイプ `web.formFilledOut` で表され、データセット内の他の行動イベントを使用して、購読を予測するためのプロファイルレベルの機能を派生させます。

### 正のラベルと負のラベルのクエリ {#query-positive-and-negative-labels}

（監視対象の）機械学習モデルをトレーニングするための完全なデータセットには、予測する結果を表すターゲット変数またはラベルと、モデルのトレーニングに使用されるサンプルプロファイルを記述するために使用される一連の特徴または説明変数が含まれる。

この場合、ラベルは `subscriptionOccurred` という変数になります。これは、ユーザープロファイルにタイプ `web.formFilledOut` のイベントがある場合は 1、それ以外の場合は 0 に等しくなります。 次のクエリは、イベントデータセットから 50,000 人のユーザーのセットを返します。このセットには、正のラベル（`subscriptionOccurred = 1`）を持つすべてのユーザーと、50,000 ユーザーサンプルサイズを満たすために負のラベルを持つランダムに選択されたユーザーのセットが含まれます。 これにより、トレーニングデータに、モデルの学習対象となるポジティブな例とネガティブな例の両方が含まれるようになります。

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

### イベントを集計して ML の機能を定義します {#define-features}

適切なクエリを使用すると、データセット内のイベントを、傾向モデルのトレーニングに使用できる意味のある数値特性に収集できます。 イベントの例を次に示します。

- マーケティング目的で送信され、ユーザーが受信した **メールの数**。
- これらのメールのうち、**開封** された部分。
- ユーザーが **選択** したリンクを含む、これらのメールの一部。
- 表示された **製品**。
- 操作された **提案** の数。
- 却下された **提案** の数。
- **選択されたリンク** 数。
- 2 つの連続した E メールが受信された間隔（分）。
- 開かれた 2 つの連続した E メールの間隔（分）。
- ユーザーが実際にリンクを選択した、連続する 2 通のメール間の分数。
- 2 つの連続した製品表示間の分数。
- 操作された 2 つの提案の間の分数。
- 破棄された 2 つの提案の間の分数。
- 選択された 2 つのリンク間の分数。

次のクエリは、これらのイベントを集計します。

+++選択するとサンプルクエリが表示されます

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

|   | userId | emailsReceived | emailOpened | emailsClicked | productsViewed | propositionInteractes | propositionDiscelled | webLinkClicks | minutes_since_emailSent | minutes_since_emailOpened | minutes_since_emailClick | minutes_since_productView | minutes_since_propositionInteract | minutes_since_propositionDismiss | minutes_since_linkClick |
| --- |    --- |    ---   |  ---  |   ---  |   ---  |  ---  |  ---  |   ---  |   ---  |   ---  |   ---  |   ---  |   ---  |   ---  |   --- | 
| 0 | 01102546977582484968046916668339306826 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0.0 | 該当なし | 該当なし | 該当なし | 該当なし | なし | 該当なし |
| 1 | 01102546977582484968046916668339306826 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0.0 | 該当なし | 該当なし | 該当なし | 該当なし | なし | 該当なし |
| 2 | 01102546977582484968046916668339306826 | 3 | 0 | 0 | 0 | 0 | 0 | 0 | 0.0 | 該当なし | 該当なし | 該当なし | 該当なし | なし | 該当なし |
| 3 | 01102546977582484968046916668339306826 | 3 | 1 | 0 | 0 | 0 | 0 | 0 | 540.0 | 0.0 | 該当なし | 該当なし | 該当なし | なし | 該当なし |
| 4 | 01102546977582484968046916668339306826 | 3 | 2 | 0 | 0 | 0 | 0 | 0 | 588.0 | 0.0 | 該当なし | 該当なし | 該当なし | なし | 該当なし |

{style="table-layout:auto"}

#### ラベルとフィーチャ クエリーを組み合わせる {#combine-queries}

最後に、ラベルクエリと機能クエリを 1 つのクエリに組み合わせ、ラベルと機能のトレーニングデータセットを返すことができます。

+++選択するとサンプルクエリが表示されます

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

|  | userId | eventType | タイムスタンプ | subscriptionOccurred | emailsReceived | emailOpened | emailsClicked | productsViewed | propositionInteractes | propositionDiscelled | webLinkClicks | minutes_since_emailSent | minutes_since_emailOpened | minutes_since_emailClick | minutes_since_productView | minutes_since_propositionInteract | minutes_since_propositionDismiss | minutes_since_linkClick | random_row_number_for_user |
| ---  |  --- |   ---  |  ---  |  ---  |  ---  |  ---  |  ---  |  ---  |  ---  |  ---  |  ---  |  ---  |  ---  |  ---  |  ---   | ---  |  ---  |  ---  |  --- |    
| 0 | 02554909162592418347780983091131567290 | directMarketing.emailSent | 2023-06-17 13:44:59.086 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0.0 | 該当なし | 該当なし | 該当なし | 該当なし | なし | 該当なし | 1 |
| 1 | 01130334080340815140184601481559659945 | directMarketing.emailOpened | 2023-06-19 06:01:55.366 | 0 | 1 | 3 | 0 | 1 | 0 | 0 | 0 | 1921.0 | 0.0 | 該当なし | 1703.0 | 該当なし | なし | 該当なし | 1 |
| 2 | 01708961660028351393477273586554010192 | web.formFilledOut | 2023-06-19 18:36:49.083 | 1 | 1 | 2 | 2 | 0 | 0 | 0 | 0 | 2365.0 | 26.0 | 1.0 | 該当なし | 該当なし | なし | 該当なし | 7 |
| 3 | 01809182902320674899156240602124740853 | directMarketing.emailSent | 2023-06-21 19:17:12.535 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0.0 | 該当なし | 該当なし | 該当なし | 該当なし | なし | 該当なし | 1 |
| 4 | 03441761949943678951106193028739001197 | directMarketing.emailSent | 2023-06-21 21:58:29.482 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0.0 | 該当なし | 該当なし | 該当なし | 該当なし | なし | 該当なし | 1 |

{style="table-layout:auto"}

## クエリテンプレートを作成して、トレーニングデータを増分的に計算します

定期的に更新されたトレーニングデータでモデルを再トレーニングし、モデルの精度を経時的に維持するのが一般的です。 トレーニングデータセットを効率的に更新するためのベストプラクティスとして、トレーニングセットクエリからテンプレートを作成して、新しいトレーニングデータを増分的に計算することができます。 これにより、トレーニングデータが最後に更新された以降に元のエクスペリエンスイベントデータセットに追加されたデータからのみラベルと機能を計算し、新しいラベルと機能を既存のトレーニングデータセットに挿入できます。

それには、トレーニングセットクエリにいくつかの変更を加える必要があります。

- 存在しない場合は、新しいトレーニングデータセットを作成するロジックを追加し、存在しない場合は、新しいラベルと機能を既存のトレーニングデータセットに挿入します。 これには、次の 2 つのバージョンのトレーニングセットクエリが必要です。
   - まず、`CREATE TABLE IF NOT EXISTS {table_name} AS` ステートメントを使用します。
   - 次に、トレーニングデータセットが既に存在する場合は、`INSERT INTO {table_name}` 文を使用します
- `SNAPSHOT BETWEEN $from_snapshot_id AND $to_snapshot_id` ステートメントを追加して、指定した期間内に追加されたイベントデータにクエリを制限します。 スナップショット ID の `$` プレフィックスは、クエリテンプレートの実行時に渡される変数であることを示します。

これらの変更を適用すると、次のクエリが表示されます。

+++選択するとサンプルクエリが表示されます

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

最後に、次のコードでデータDistillerにクエリテンプレートを保存します。

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

テンプレートを保存しておくと、テンプレート ID を参照して、クエリに含めるスナップショット ID の範囲を指定することで、いつでもクエリを実行できます。 次のクエリでは、元のエクスペリエンスイベントデータセットのスナップショットを取得します。

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

## 次の手順：

このドキュメントを読むことで、Adobe Experience Platformのデータを機械学習モデルで使用できる機能（変数）に変換する方法を学びました。 Experience Platformから機能パイプラインを作成して、機械学習環境でカスタムモデルにフィードする次の手順は、[ 機能データセットの書き出し ](./export-data.md) です。
