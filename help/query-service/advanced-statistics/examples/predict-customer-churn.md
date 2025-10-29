---
title: SQL ベースのロジスティックリグレッションを使用して顧客チャーンを予測
description: SQL ベースのロジスティックリグレッションを使用して顧客のチャーンを予測する方法を説明します。 このガイドでは、モデルの作成から評価と予測までのプロセス全体を説明します。 顧客の購入行動から実用的なインサイトを得て、プロアクティブな保持戦略を実装し、ビジネス上の意思決定を最適化します。
exl-id: 3b18870d-104c-4dce-8549-a6818dc40d24
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '1126'
ht-degree: 1%

---

# SQL ベースのロジスティックリグレッションを使用して顧客チャーンを予測

顧客離れを予測すると、実用的なインサイトを通じて満足度とロイヤルティを向上させることで、企業が顧客を維持し、リソースを最適化し、収益性を高めるのに役立ちます。

SQL ベースのロジスティック回帰を使用して顧客チャーンを予測する方法を説明します。 この包括的な SQL ガイドを使用すると、生の e コマースデータを、主要な行動指標（購入頻度、平均注文額、最終購入の最新性など）に基づいて意味のある顧客インサイトに変換できます。 このドキュメントでは、データの準備と機能エンジニアリングから、モデルの作成、評価、予測までのプロセス全体を説明します。

このガイドを使用すると、リスクの高い顧客を特定し、リテンション戦略を絞り込み、より良いビジネス上の意思決定を推進する、強力なチャーン予測モデルを作成できます。 ステップバイステップの手順、SQL クエリ、詳細な説明が含まれており、データ環境で自信を持って機械学習技術を適用するのに役立ちます。

## はじめに

チャーンモデルを作成する前に、主な顧客機能とデータ要件を調べることが重要です。 以下の節では、正確なモデルトレーニングを行うために必要な、基本的な顧客属性と必須のデータフィールドの概要を説明します。

### お客様の機能の定義 {#define-customer-features}

チャーンを正確に分類するために、モデルは購入習慣とトレンドを分析します。 次の表に、モデルで使用される主な顧客行動機能の概要を示します。

| 機能 | 説明 |
|---------------------------|-------------------------------------------------------|
| `total_purchases` | 顧客による購入の合計数。 |
| `total_revenue` | 顧客の購入によって生じた合計売上高。 |
| `avg_order_value` | 顧客の購入の平均値。 |
| `customer_lifetime` | 顧客の初回購入から前回購入までの日数。 |
| `days_since_last_purchase` | 顧客の前回購入からの経過日数。 |
| `purchase_frequency` | 顧客が購入を行った個別の月数。 |

### 前提および必須フィールド {#assumptions-required-fields}

顧客チャーン予測を生成するために、モデルは、顧客トランザクションの詳細を取得する `webevents` テーブル内のキーフィールドに基づいています。 データセットには、次のフィールドを含める必要があります。

| フィールド | 説明 |
|--------------------------------|----------------------------------------------------|
| `identityMap['ECID'][0].id` | セッションをまたいで顧客を追跡するために使用される一意の ID。 |
| `productListItems.priceTotal[0]` | トランザクションごとの購入品目の合計金額。 |
| `productListItems.quantity[0]` | 購入のアイテムの合計数。 |
| `timestamp` | 各購入イベントの正確な日時。 |
| `commerce.order.purchaseID` | 完了した購入を確認する必須の値。 |

データセットには、構造化された顧客トランザクションレコードの履歴が含まれている必要があり、各行は購入イベントを表しています。 各イベントには、SQL `DATEDIFF` 関数と互換性のある適切な日時形式（YYYY-MM-DD HH:MI:SS など）のタイムスタンプが含まれている必要があります。 さらに、顧客を一意に識別するために、各レコードの `ECID` フィールドに有効なExperience Cloud ID （`identityMap`）が含まれている必要があります。

>[!TIP]
>
>数百万のレコードを含む大規模なデータセットを処理すると、パフォーマンスに大きな影響を与える可能性があります。 クエリの実行を最適化するには、エクスペリエンスデータセットをタイムスタンプで分割し、スナップショットを使用して増分処理を実行し、必要に応じて効率的な集計機能を適用します。 さらに、集計前にデータをフィルタリングして、処理のオーバーヘッドを削減します。

## モデルを作成 {#create-a-model}

顧客のチャーンを予測するには、顧客の購入履歴と行動指標を分析する SQL ベースのロジスティック回帰モデルを作成する必要があります。 モデルでは、過去 90 日以内に購入したかどうかを判断して、顧客を `churned` または `not churned` に分類します。

### SQL を使用したチャーン予測モデルの作成 {#sql-create-model}

SQL ベースのモデルでは、主要指標を集計し、90 日間の非アクティブ状態ルールに基づいてチャーンラベルを割り当てることで、`webevents` データを処理します。 このアプローチは、アクティブな顧客とリスクのある顧客を区別します。 また、SQL クエリは、モデルの精度を向上させ、チャーン分類を改善するための機能エンジニアリングも実行します。 これらのインサイトは、ターゲットを絞ったリテンション戦略を実装し、チャーンを削減し、顧客の生涯価値を最大化するようにビジネスを強化します。

>[!NOTE]
>
>チャーン予測モデルでは、デフォルトのしきい値の 90 日を使用して、顧客をチャーン済みとして分類します。 このしきい値をビジネス目標および保持戦略に合わせて調整するには、SQL クエリで `DATEDIFF(CURRENT_DATE, MAX(timestamp)) > 90` 条件を変更します。

次の SQL ステートメントを使用して、指定された機能とラベルで `retention_model_logistic_reg` モデルを作成します。

```sql
CREATE MODEL retention_model_logistic_reg
TRANSFORM (
  vector_assembler(array(total_purchases, total_revenue, avg_order_value, customer_lifetime, days_since_last_purchase, purchase_frequency)) features
  -- Combines selected customer metrics into a feature vector for model training
)
OPTIONS (
  MODEL_TYPE = 'logistic_reg',  -- Specifies logistic regression as the model type
  LABEL = 'churned'             -- Defines the target label for churn classification
)
AS
WITH customer_features AS (
    SELECT
       identityMap['ECID'][0].id AS customer_id,  -- Extract the unique customer ID from identityMap
       AVG(COALESCE(productListItems.priceTotal[0], 0)) AS avg_order_value,  -- Calculates the average order value, and handles null values with COALESCE
       SUM(COALESCE(productListItems.priceTotal[0], 0)) AS total_revenue, -- The sum of all purchase values per customer
       COUNT(COALESCE(productListItems.quantity[0], 0)) AS total_purchases,  -- The total number of items purchased by the customer
       DATEDIFF(MAX(timestamp), MIN(timestamp)) AS customer_lifetime,  -- The days between first and last recorded purchase
       DATEDIFF(CURRENT_DATE, MAX(timestamp)) AS days_since_last_purchase,  -- The days since the last purchase event
       COUNT(DISTINCT CONCAT(YEAR(timestamp), MONTH(timestamp))) AS purchase_frequency  -- The count of unique months with purchases
    FROM
        webevents
    WHERE EXISTS(productListItems, value -> value.priceTotal > 0)  -- Filters transactions with valid total price
      AND commerce.`order`.purchaseID <> ''  -- Ensures the order has a valid purchase ID
    GROUP BY customer_id 
),
customer_labels AS (
    SELECT
      identityMap['ECID'][0].id AS customer_id,  -- Extract the unique customer ID for labeling
      CASE
          WHEN DATEDIFF(CURRENT_DATE, MAX(timestamp)) > 90 THEN 1  -- Marks the customer as churned if no purchase occurred in the last 90 days
          ELSE 0
      END AS churned
    FROM
        webevents
    WHERE EXISTS(productListItems, value -> value.priceTotal > 0) 
      AND commerce.`order`.purchaseID <> ''  
    GROUP BY customer_id  
)
SELECT
    f.customer_id,
    f.total_purchases,
    f.total_revenue,
    f.avg_order_value,
    f.customer_lifetime,
    f.days_since_last_purchase,
    f.purchase_frequency,
    l.churned
FROM
    customer_features f
JOIN
    customer_labels l
ON f.customer_id = l.customer_id  -- Join features with churn labels
ORDER BY RANDOM()  -- Shuffles rows randomly for training
LIMIT 500000;  -- Limit the dataset to 500,000 rows for model training
```

### モデル出力 {#model-output}

出力データセットには、顧客関連の指標とそのチャーンステータスが含まれています。 各行は、顧客、その機能値およびチャーンステータスを表します。 この出力を使用して、顧客の行動を分析し、予測モデルをトレーニングし、リスクのある顧客を保持するためのターゲット設定された保持戦略を開発できます。 出力テーブルの例を次に示します。

```console
 customer_id  | total_purchases | total_revenue | avg_order_value  | customer_lifetime | days_since_last_purchase | purchase_frequency | churned |
|--------------+-----------------+---------------+------------------+-------------------+--------------------------+--------------------+----------
  100001      | 25              | 1250.00       | 50.00            | 540               | 20                       | 10                 | 0       
  100002      | 3               | 90.00         | 30.00            | 120               | 95                       | 1                  | 1       
  100003      | 60              | 7200.00       | 120.00           | 800               | 5                        | 24                 | 0       
  100004      | 15              | 750.00        | 50.00            | 365               | 60                       | 8                  | 0       
  100005      | 1               | 25.00         | 25.00            | 60                | 180                      | 1                  | 1       
```

| 列 | 説明 |
|-----------|------------------------------------------------------------------------------------|
| `churned` | 値は、顧客が過去 90 日以内に購入を行ったかどうかを示します（0 = チャーンなし、1 = チャーン）。 |

## SQL を使用してモデルを評価 {#model-evaluation}

次に、チャーン予測モデルを評価して、リスクのある顧客の特定におけるモデルの有効性を判断します。 精度と信頼性を測定する主要指標を使用して、モデルのパフォーマンスを評価します。

顧客チャーンの予測における `retention_model_logistic_reg` モデルの精度を測定するには、`model_evaluate` 関数を使用します。 次の SQL の例では、トレーニングデータのような構造を持つデータセットを使用して、モデルを評価します。

```sql
SELECT * 
FROM model_evaluate(retention_model_logistic_reg, 1,
WITH customer_features AS (
    SELECT
       identityMap['ECID'][0].id AS customer_id,
       AVG(COALESCE(productListItems.priceTotal[0], 0)) AS avg_order_value,
       SUM(COALESCE(productListItems.priceTotal[0], 0)) AS total_revenue,
       COUNT(COALESCE(productListItems.quantity[0], 0)) AS total_purchases, 
       DATEDIFF(MAX(timestamp), MIN(timestamp)) AS customer_lifetime,
       DATEDIFF(CURRENT_DATE, MAX(timestamp)) AS days_since_last_purchase,
       COUNT(DISTINCT CONCAT(YEAR(timestamp), MONTH(timestamp))) AS purchase_frequency 
    FROM
        webevents
    WHERE EXISTS(productListItems, value -> value.priceTotal > 0)
      AND commerce.`order`.purchaseID <> ''
    GROUP BY customer_id
),
customer_labels AS (
    SELECT
      identityMap['ECID'][0].id AS customer_id,
      CASE
          WHEN DATEDIFF(CURRENT_DATE, MAX(timestamp)) > 90 THEN 1 
          ELSE 0
      END AS churned
    FROM
        webevents
    WHERE EXISTS(productListItems, value -> value.priceTotal > 0) 
      AND commerce.`order`.purchaseID <> '' 
    GROUP BY customer_id
)
SELECT
    f.customer_id,
    f.total_purchases,
    f.total_revenue,
    f.avg_order_value,
    f.customer_lifetime,
    f.days_since_last_purchase,
    f.purchase_frequency,
    l.churned
FROM
    customer_features f
JOIN
    customer_labels l
ON f.customer_id = l.customer_id); -- Joins customer features with churn labels
```

### モデル評価出力

評価出力には、AUC-ROC、精度、精度、リコールなどの主要パフォーマンス指標が含まれます。 これらの指標は、モデルの有効性に関するインサイトを提供します。このインサイトを使用して、リテンション戦略を絞り込み、データに基づいた意思決定を行うことができます。

>[!NOTE]
>
>パフォーマンス値の範囲は 0 ～ 1 です。1.0 は完全なパフォーマンスを表します。

```console
 auc_roc | accuracy | precision | recall 
|---------+----------+-----------+--------
1        | 0.99998  |  1        |  1      
```

| 指標 | 説明 |
|------------|-------------------------------------------------------------------------|
| `auc_roc` | この指標は、チャーンされた顧客とチャーンされていない顧客を区別するモデルの機能を示します。 1 に近い値は、パフォーマンスが向上していることを示します。 |
| `accuracy` | 精度指標は、正しい予測の割合を表し、モデルのパフォーマンスの全体的な測定値を提供します。 |
| `precision` | 精度は、正しく識別されたチャーン顧客の割合を示し、チャーン予測の信頼性を示します。 値を大きくすると、誤検出が少なくなります。 |
| `recall` | リコールは、実際にチャーンされたすべての顧客を特定できるモデルの能力を測定します。 リコール値が大きい場合は、欠場したチャーン顧客が少ないことを示します。 |

>[!NOTE]
>
>この例のほぼ完全なスコアは、デモ用です。 実際には、ノイズや変動により、実際のデータでは得られる値が低くなる場合があります。

## モデル予測 {#model-prediction}

モデルを評価したら、`model_predict` を使用して新しいデータセットに適用し、顧客チャーンを予測します。 これらの予測を使用して、リスクのある顧客を特定し、ターゲットを絞った保持戦略を実装できます。

### SQL を使用してチャーン予測を生成する {#sql-model-predict}

以下の SQL クエリでは、`retention_model_logistic_reg` モデルを使用して、トレーニングデータのような構造を持つデータセットで顧客のチャーンを予測します。

```sql
SELECT * 
FROM model_predict(retention_model_logistic_reg, 1,  -- Applies the trained model for churn prediction
WITH customer_features AS (
    SELECT
       identityMap['ECID'][0].id AS customer_id,
       AVG(COALESCE(productListItems.priceTotal[0], 0)) AS avg_order_value,  
       SUM(COALESCE(productListItems.priceTotal[0], 0)) AS total_revenue, 
       COUNT(COALESCE(productListItems.quantity[0], 0)) AS total_purchases,  
       DATEDIFF(MAX(timestamp), MIN(timestamp)) AS customer_lifetime,  
       DATEDIFF(CURRENT_DATE, MAX(timestamp)) AS days_since_last_purchase,  
       COUNT(DISTINCT CONCAT(YEAR(timestamp), MONTH(timestamp))) AS purchase_frequency  
    FROM
        webevents
    WHERE EXISTS(productListItems, value -> value.priceTotal > 0)  -- Ensures only valid purchase data is considered
      AND commerce.`order`.purchaseID <> ''  
    GROUP BY customer_id
),
customer_labels AS (
    SELECT
      identityMap['ECID'][0].id AS customer_id,  
      CASE
          WHEN DATEDIFF(CURRENT_DATE, MAX(timestamp)) > 90 THEN 1  -- Identify customers who have not purchased in the last 90 days
          ELSE 0
      END AS churned
    FROM
        webevents
    WHERE EXISTS(productListItems, value -> value.priceTotal > 0)  
      AND commerce.`order`.purchaseID <> ''  
    GROUP BY customer_id
)
SELECT
    f.customer_id,  
    f.total_purchases,  
    f.total_revenue,  
    f.avg_order_value,  
    f.customer_lifetime,  
    f.days_since_last_purchase,  
    f.purchase_frequency,  
    l.churned  
FROM
    customer_features f
JOIN
    customer_labels l
ON f.customer_id = l.customer_id);  -- Matches features with their churn labels for prediction
```

### モデル予測出力 {#prediction-output}

出力データセットには、主な顧客機能とその予測されるチャーンステータスが含まれています。これは、顧客がチャーンを起こす可能性が高いかどうかを示します。 これらのインサイトを使用して、プロアクティブなリテンション戦略を実装し、顧客チャーンを削減します。

```console
 total_purchases | total_revenue | avg_order_value | customer_lifetime | days_since_last_purchase | purchase_frequency | churned | prediction
|-----------------+---------------+-----------------+-------------------+--------------------------+--------------------+---------+------------
 2               | 299           | 149.5           | 0                 | 13                        | 1                  | 0       | 0
 1               | 710           | 710.00          | 0                 | 149                       | 1                  | 1       | 1
 1               | 19.99         | 19.99           | 0                 | 30                        | 1                  | 0       | 0
 1               | 4528          | 4528.00         | 0                 | 26                        | 1                  | 0       | 0
 1               | 21.84         | 21.84           | 0                 | 90                        | 1                  | 0       | 0
 1               | 16.64         | 16.64           | 0                 | 268                       | 1                  | 1       | 1
```

| 列 | 説明 |
|---------------|-------------------------------------------------------------------------------|
| `prediction` | モデルに基づいて顧客が予測したチャーンステータス（0 = チャーンなし、1 = チャーン）。 |

## 次の手順

これで、SQL ベースのモデルを作成、評価および使用して顧客チャーンを予測する方法を学びました。 この基盤を使用すると、顧客の行動を分析し、リスクのある顧客を特定し、プロアクティブなリテンション戦略を導入して顧客維持を向上させることができます。 チャーン予測モデルをさらに強化して適用するには、次の手順を検討します。

- プロセスの自動化：継続的な監視とリアルタイムのインサイトのために、モデルをデータパイプラインに統合します。 [SQL を使用してデータセットを検証および処理する方法を確認します ](../../../dashboards/query.md)。
- モデルのパフォーマンスの監視：新しいデータでモデルを継続的に評価し、精度と関連性を維持します。  Adobe Experience Platform UI で [AI アシスタント ](../../../ai-assistant/landing.md) を使用して、主要なパフォーマンスの変化と [ オーディエンスのトレンドの予測 ](../../../ai-assistant/new-features/audience-forecasting.md) を監視します。
