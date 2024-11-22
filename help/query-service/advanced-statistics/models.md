---
title: モデル
description: Data Distiller SQL 拡張機能を使用したモデルライフサイクル管理。 SQL を使用して、モデルのバージョン管理、評価、予測などの主要プロセスを含む高度な統計モデルを作成、トレーニング、管理し、データから実用的なインサイトを導き出す方法を説明します。
role: Developer
exl-id: c609a55a-dbfd-4632-8405-55e99d1e0bd8
source-git-commit: 6a61900b19543f110c47e30f4d321d0016b65262
workflow-type: tm+mt
source-wordcount: '1229'
ht-degree: 1%

---

# モデル

>[!AVAILABILITY]
>
>この機能は、Data Distiller アドオンを購入したお客様が利用できます。 詳しくは、アドビ担当者にお問い合わせください。

クエリサービスで、モデルの構築とデプロイのコアプロセスがサポートされるようになりました。 SQL を使用すると、データを使用してモデルをトレーニングし、その精度を評価した後、トレーニング モデルを適用して新しいデータを予測できます。 その後、モデルを使用して過去のデータから一般化し、実際のシナリオについて十分な情報に基づいた決定を下すことができます。

実用的なインサイトを生成するためのモデルライフサイクルの 3 つのステップは、次のとおりです。

1. **トレーニング**：モデルは、指定されたデータセットからパターンを学習します。 （モデルの作成または置換）
2. **テスト/評価**：モデルのパフォーマンスは、別のデータセットを使用して評価されます。（`model_evaluate`）
3. **予測**：トレーニング済みモデルを使用して、目に見えない新しいデータを予測します。

既存の SQL 文法に追加されたモデル SQL 拡張機能を使用して、ビジネスニーズに応じてモデルのライフサイクルを管理します。 このドキュメントでは、モデルの作成または置換、トレーニング、評価、必要に応じた再トレーニング、インサイトの予測に必要な SQL について説明します。

## モデルの作成とトレーニング {#create-and-train}

SQL コマンドを使用して機械学習モデルを定義、設定、トレーニングする方法について説明します。 以下の SQL は、モデルの作成、機能エンジニアリング変換の適用、トレーニングプロセスの開始方法を示しており、モデルが後で使用できるように正しく設定されていることを確認します。 次の SQL コマンドでは、モデルの作成と管理に関する様々なオプションについて詳しく説明します。

- **CREATE MODEL**：指定されたデータセットに新しいモデルを作成し、トレーニングします。 同じ名前のモデルが既に存在する場合、このコマンドはエラーを返します。
- **CREATE MODEL IF NOT EXISTS**：指定されたデータセットに同じ名前のモデルがまだ存在しない場合にのみ、新しいモデルを作成してトレーニングします。
- **モデルを作成または置換**：モデルを作成およびトレーニングし、指定されたデータセット上の同じ名前の既存モデルの最新バージョンを置き換えます。

```sql
CREATE MODEL | CREATE MODEL IF NOT EXISTS | CREATE OR REPLACE MODEL}
model_alias
[TRANSFORM (select_list)]
[OPTIONS(model_option_list)]
[AS {select_query}]
 
model_option_list:
    MODEL_TYPE = { 'LINEAR_REG' |
                   'LOGISTIC_REG' |
                   'KMEANS' }
  [, MAX_ITER = int64_value ]
 [, LABEL = string_array ]
[, REG_PARAM = float64_value ]
```

**例**

```sql
CREATE MODEL churn_model
TRANSFORM (vector_assembler(array(current_customers, previous_customers)) features) 
OPTIONS(MODEL_TYPE='linear_reg', LABEL='churn_rate') 
AS
SELECT *
FROM churn_with_rate
ORDER BY period;
```

モデルの作成とトレーニングプロセスの主要なコンポーネントと設定を理解できるように、次のメモでは上記の SQL 例の各要素の目的と機能を説明します。

- `<model_alias>`：モデルのエイリアスは、モデルに割り当てられた再利用可能な名前で、後で参照できます。 モデルに名前を付ける必要があります。
- `transform`:transform 句は、モデルのトレーニング前にデータセットにフィーチャーエンジニアリング変換（ワンホットエンコーディングや文字列インデックス作成など）を適用するために使用します。 `TRANSFORM` ステートメントの最後の句は、モデルのトレーニング用の機能を構成する列のリストを含む `vector_assembler`、または `vector_assembler` の派生型（`max_abs_scaler(feature)`、`standard_scaler(feature)` など）にする必要があります。 最後の句で言及されている列のみがトレーニングに使用されます。その他のすべての列は、`SELECT` クエリに含まれている場合でも、除外されます。
- `label = <label-COLUMN>`：モデルが予測しようとするターゲットまたは結果を指定するトレーニングデータセットのラベル列。
- `training-dataset`：この構文は、モデルのトレーニングに使用するデータを選択します。
- `type = 'LogisticRegression'`：この構文は、使用する機械学習アルゴリズムのタイプを指定します。 オプションには、`LinearRegression`、`LogisticRegression`、`KMeans` があります。
- `options`：このキーワードは、モデルを設定するためのキーと値のペアの柔軟なセットを提供します。
   - `Key model_type`: `model_type = '<supported algorithm>'`：使用する機械学習アルゴリズムのタイプを指定します。 サポートされるオプションは、`LinearRegression`、`LogisticRegression`、`KMeans` です。
   - `Key label`: `label = <label_COLUMN>`: トレーニングデータセットのラベル列を定義します。この列は、モデルが予測しようとしているターゲットまたは結果を示します。

SQL を使用して、トレーニングに使用するデータセットを参照します。

## モデルの更新 {#update}

新しいフィーチャーエンジニアリング変換を適用し、アルゴリズムタイプやラベル列などのオプションを設定することで、既存の機械学習モデルを更新する方法を説明します。 更新のたびに、モデルの最新バージョンが作成され、最新バージョンから増分されます。 これにより、変更が追跡され、今後の評価や予測の手順でモデルを再利用できます。

次の例は、新しい変換およびオプションを使用してモデルを更新する方法を示しています。

```sql
UPDATE MODEL <model_alias> TRANSFORM (vector_assembler(array(current_customers, previous_customers)) features)  OPTIONS(MODEL_TYPE='logistic_reg', LABEL='churn_rate')  AS SELECT * FROM churn_with_rate ORDER BY period;
```

**例**

バージョン管理プロセスを理解するために、次のコマンドを使用できます。

```sql
UPDATE MODEL model_vdqbrja OPTIONS(MODEL_TYPE='logistic_reg', LABEL='Survived') AS SELECT * FROM titanic_e2e_dnd;
```

このコマンドを実行すると、次の表に示すように、モデルのバージョンが新しくなります。

| 更新されたモデル ID | 更新されたモデル | 新しいバージョン |
|--------------------------------------------|---------------|-------------|
| a8f6a254-8f28-42ec-8b26-94edeb4698e8 | model_vdqbrja | 2 |

以下のメモでは、モデル更新ワークフローの主要なコンポーネントとオプションについて説明します。

- `UPDATE model <model_alias>`：更新コマンドはバージョン管理を行い、最新バージョンから増分された新しいモデル バージョンを作成します。
- `version`：更新時にのみ使用されるオプションのキーワードで、新しいバージョンを作成することを明示的に指定します。 省略すると、バージョンが自動的に増分されます。


## モデルの評価 {#evaluate-model}

信頼性の高い結果を得るには、モデルをデプロイする前に、モデルの精度と有効性を評価し、`model_evaluate` キーワードで予測します。 以下の SQL 文は、テストデータセット、特定の列、モデルのバージョンを指定し、パフォーマンスを評価してモデルをテストします。

```sql
SELECT *
FROM   model_evaluate(model-alias, version-number,SELECT col1,
       col2,
       label-COLUMN
FROM   test -dataset)
```

`model_evaluate` 関数は、最初の引数として `model-alias` を使用し、2 番目の引数として柔軟な `SELECT` ステートメントを使用します。 クエリサービスは、最初に `SELECT` 文を実行し、結果を `model_evaluate` Adobe定義関数（ADF）にマップします。 システムは、`SELECT` ステートメントの結果の列名とデータタイプが、トレーニングステップで使用されたものと一致することを想定しています。 これらの列名とデータタイプは、評価用のテストデータとラベルデータとして扱われます。

>[!IMPORTANT]
>
>（`model_evaluate`）の評価と（`model_predict`）の予測には、トレーニング時に行われた変換を使用します。

## 予測 {#predict}

次に、`model_predict` キーワードを使用して、指定したモデルとバージョンをデータセットに適用し、選択した列の予測を生成します。 次の SQL は、このプロセスを示しており、モデルのエイリアスとバージョンを使用して結果を予測する方法を示しています。

```sql
SELECT *
FROM   model_predict(model-alias, version-number,SELECT col1,
       col2,
       label-COLUMN
FROM   dataset)
```

`model_predict` は、最初の引数としてモデルエイリアスを受け入れ、2 番目の引数としてフレキシブル `SELECT` ステートメントを受け入れます。 クエリサービスは、最初に `SELECT` 文を実行し、結果を `model_predict` の ADF にマップします。 `SELECT` ステートメントの結果の列名とデータタイプが、トレーニングステップの列名とデータタイプと一致することが想定されます。 その後、このデータは、スコアリングおよび予測の生成に使用されます。

>[!IMPORTANT]
>
>（`model_evaluate`）の評価と（`model_predict`）の予測には、トレーニング時に行われた変換を使用します。

## モデルの評価と管理

`SHOW MODELS` コマンドを使用して、作成した使用可能なすべてのモデルを一覧表示します。 トレーニング済みで、評価または予測に使用できるモデルを表示する場合に使用します。 クエリの際に、モデルの作成時に更新されるモデルリポジトリから情報が取得されます。 返される詳細は、モデル ID、モデル名、バージョン、ソースデータセット、アルゴリズムの詳細、オプション/パラメーター、作成時間/更新時間、モデルを作成したユーザーです。

```sql
SHOW MODELS;
```

結果は、次に示すようなテーブルに表示されます。

| model-id | model-name | version | ソースデータセット | タイプ | オプション | 変換 | フィールド | created | 更新日 | 作成者 |
|--------------------|---------------|---------|------------------|-----------------------|------------------------------|---------------------------------------------------------------------------|----------------------|---------------------|---------------------|------------|
| `model-84362-mdunj` | `SalesModel` | 1.0 | `sales_data_2023` | `LogisticRegression` | `{"label": "label-field"}` | `one_hot_encoder(name)`、`ohe_name`、`string_indexer(gender)`、`genderSI` | \[&quot;name&quot;, &quot;gender&quot;\] | 2024-08-14 10:30 AM | 2024-08-14 11:00 AM | `JohnSnow@adobe.com` |

## モデルのクリーンアップと保守

`DROP MODELS` コマンドを使用して、作成したモデルを model レジストリから削除します。 古くなったモデル、未使用のモデル、不要なモデルを削除する場合に使用できます。 これにより、リソースが解放され、関連するモデルのみが維持されるようになります。 また、特異性を向上させるために、オプションでモデル名を含めることもできます。 指定したモデルバージョンのモデルのみが削除されます。

```sql
DROP MODEL IF EXISTS modelName
DROP MODEL IF EXISTS modelName modelVersion ;
```

## 次の手順

このドキュメントでは、Data Distillerを使用して信頼できるモデルを作成、トレーニング、管理するために必要なベース SQL 構文を確認しました。 次に、[ 高度な統計モデルの実装 ](./implement-models/implement-models.md) ドキュメントを参照して、使用可能な様々な信頼できるモデルと、SQL ワークフロー内で効果的に実装する方法について学びます。 [ 機能エンジニアリング ](./feature-engineering.md) ドキュメントを確認し、モデルトレーニングに最適なデータが準備されていることを確認します。
