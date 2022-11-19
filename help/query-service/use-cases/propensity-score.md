---
title: 機械学習で生成された予測モデルを使用した傾向スコアの決定
description: クエリサービスを使用して、予測モデルを Platform データに適用する方法を説明します。 このドキュメントでは、Platform データを使用して、各訪問での顧客の購入傾向を予測する方法を説明します。
source-git-commit: af1c8f94d1758b3a4e7ea00c46b0f9a71a01c6be
workflow-type: tm+mt
source-wordcount: '1294'
ht-degree: 0%

---

# 機械学習で生成された予測モデルを使用した傾向スコアの決定

クエリサービスを使用すると、機械学習プラットフォームでExperience Platformデータを活用して、傾向スコアなどの予測モデルを生成できます。 このガイドでは、クエリサービスを使用してデータを機械学習プラットフォームに送信し、計算ノートブックでモデルをトレーニングする方法について説明します。 トレーニング済みモデルは、SQL を使用してデータに適用し、訪問ごとに顧客が購入する傾向を予測できます。

## はじめに

このプロセスの一環として、機械学習モデルをトレーニングする必要があります。このドキュメントでは、1 つ以上の機械学習環境に関する実務知識を前提としています。

この例では、 [!DNL Jupyter Notebook] を開発環境として使用する。 多くのオプションを使用できますが、 [!DNL Jupyter Notebook] の計算要件が小さいオープンソース web アプリケーションなので、をお勧めします。 次のことが可能です。 [公式サイトからダウンロードされる](https://jupyter.org/).

まだおこなっていない場合は、次の手順に従います。 [接続 [!DNL Jupyter Notebook] Adobe Experience Platform Query Service を使用](../clients/jupyter-notebook.md) このガイドを続行する前に。

この例で使用されるライブラリには、以下が含まれます。

```console
python=3.6.7
psycopg2
sklearn
pandas
matplotlib
numpy
tqdm
```

## Platform からに分析テーブルをインポート [!DNL Jupyter Notebook] {#import-analytics-tables}

傾向スコアモデルを生成するには、Platform に保存された分析データの投影を [!DNL Jupyter Notebook]. から [!DNL Python] 3 [!DNL Jupyter Notebook] クエリサービスに接続した次のコマンドは、架空の衣料品店 Luma から顧客行動データセットをインポートします。 Platform データは Experience Data Model(XDM) 形式を使用して保存されるので、スキーマの構造に準拠するサンプル JSON オブジェクトを生成する必要があります。 手順については、ドキュメントを参照してください。 [サンプルの JSON オブジェクトを生成します。](../../xdm/ui/sample.md).

![この [!DNL Jupyter Notebook] 複数のコマンドがハイライト表示されたダッシュボード](../images/use-cases/jupyter-commands.png)

出力には、Luma の行動データセット内のすべての列の表形式の表示が [!DNL Jupyter Notebook] ダッシュボード。

![Luma が読み込んだ顧客行動データセットの、 [!DNL Jupyter Notebook].](../images/use-cases/behavioural-dataset-results.png)

## 機械学習用のデータの準備 {#prepare-data-for-machine-learning}

機械学習モデルをトレーニングするには、ターゲット列を識別する必要があります。 購入傾向がこのユースケースの目標なので、 `analytic_action` 列が Luma の結果からターゲット列として選択されます。 値 `productPurchase` は、顧客購入の指標です。 この `purchase_value` および `purchase_num` 列も、製品購入アクションに直接関連するので、削除されます。

これらの操作を実行するコマンドは次のとおりです。

```python
#define the target label for prediction
df['target'] = (df['analytic_action'] == 'productPurchase').astype(int)
#remove columns that are dependent on the label
df.drop(['analytic_action','purchase_value'],axis=1,inplace=True)
```

次に、Luma データセットのデータを適切な表現に変換する必要があります。 次の 2 つの手順が必要です。

1. 数値を表す列を数値列に変換します。 これをおこなうには、 `dataframe`.
1. 分類された列も数値列に変換します。

```python
#convert columns that represent numbers
num_cols = ['purchase_num', 'value_cart', 'value_lifetime']
df[num_cols] = df[num_cols].apply(pd.to_numeric, errors='coerce')
```

という技術 *1 つのホットエンコーディング* は、機械学習およびディープラーニングアルゴリズムで使用する分類データ変数を変換するために使用されます。 これにより、予測とモデルの分類精度が向上します。 以下を使用： `Sklearn` ライブラリを使用して、各カテゴリ値を別々の列で表すことができます。

```python
from sklearn.preprocessing import OneHotEncoder

#get the categorical columns
cat_columns = list(set(df.columns) - set(num_cols + ['target']))

#get the dataframe with categorical columns only
df_cat = df.loc[:,cat_columns]

#initialize sklearn's OneHotEncoder
enc = OneHotEncoder(handle_unknown='ignore')

#fit the data into the encoder
enc.fit(df_cat)

#define OneHotEncoder's columns names
ohc_columns = [[c+'='+c_ for c_ in cat] for c,cat in zip(cat_columns,enc.categories_)]
ohc_columns = [item for sublist in ohc_columns for item in sublist]

#finalize the data input to the ML models
X = pd.DataFrame( np.concatenate((enc.transform(df_cat).toarray(),df[num_cols]),axis=1),
                 columns =  ohc_columns + num_cols)

#define target column
y = df['target']
```

次のように定義されたデータ： `X` はタブ化され、次のように表示されます。

![X の X が [!DNL Jupyter Notebook].](../images/use-cases/x-output-table.png)


これで、機械学習に必要なデータが利用できるようになったので、 [!DNL Python]&#39;s `sklearn` ライブラリ。 [!DNL Logistics Regression] は、傾向モデルのトレーニングに使用され、テストデータの精度を確認できます。 この場合、約 85%です。

この [!DNL Logistic Regression] 機械学習アルゴリズムのパフォーマンスの予測に使用するアルゴリズムとトレーニングテスト分割方法は、次のコードブロックにインポートされます。

```python
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.33, random_state=42)

clf = LogisticRegression(max_iter=2000, random_state=0).fit(X_train, y_train)

print("Test data accuracy: {}".format(clf.score(X_test, y_test)))
```

テストデータの精度は 0.8518518518518519です。

物流回帰を使用すると、購入の理由を視覚化し、降順でのランクの重要度別に傾向を判断する機能を並べ替えることができます。 最初の列は、購入行動につながる高い因果関係を示します。 後者の列は、購入行動につながらない要因を示します。

結果を 2 つの棒グラフとして視覚化するコードは、次のとおりです。

```python
from matplotlib import pyplot as plt

#get feature importance as a sorted list of columns
feature_importance = np.argsort(-clf.coef_[0])
top_10_features_purchase_names = X.columns[feature_importance[:10]]
top_10_features_purchase_values = clf.coef_[0][feature_importance[:10]]
top_10_features_not_purchase_names = X.columns[feature_importance[-10:]]
top_10_features_not_purchase_values = clf.coef_[0][feature_importance[-10:]]

#plot the figures
fig, (ax1, ax2) = plt.subplots(1, 2,figsize=(10,5))

ax1.bar(np.arange(10),top_10_features_purchase_values)
ax1.set_xticks(np.arange(10))
ax1.set_xticklabels(top_10_features_purchase_names,rotation = 90)
ax1.set_ylim([np.min(clf.coef_[0])-0.1,np.max(clf.coef_[0])+0.1])
ax1.set_title("Top 10 features to define \n a propensity to purchase")

ax2.bar(np.arange(10),top_10_features_not_purchase_values, color='#E15750')
ax2.set_xticks(np.arange(10))
ax2.set_xticklabels(top_10_features_not_purchase_names,rotation = 90)
ax2.set_ylim([np.min(clf.coef_[0])-0.1,np.max(clf.coef_[0])+0.1])
ax2.set_title("Top 10 features to define \n a propensity to NOT purchase")

plt.show()
```

結果の縦棒グラフビジュアライゼーションを次に示します。

![購入傾向を定義する上位 10 の機能のビジュアライゼーション。](../images/use-cases/visualized-results.png)

棒グラフからは、複数のパターンを識別できます。 チャネルの POS（販売時点管理）と呼び出しのトピックは、購入行動を決定する最も重要な要素です。 通話トピックを苦情と請求書として扱うのは、購入しない行動を定義する上で重要な役割です。 これらは、マーケターがマーケティングキャンペーンの実施に活用し、顧客の購入傾向に対処できる、定量化可能で実用的なインサイトです。

## クエリサービスを使用したトレーニング済みモデルの適用 {#use-query-service-to-apply-trained-model}

トレーニング済みモデルを作成したら、Experience Platformで保持されるデータに適用する必要があります。 これをおこなうには、機械学習パイプラインのロジックを SQL に変換する必要があります。 このトランジションの 2 つの主な構成要素を次に示します。

- まず、SQL が [!DNL Logistics Regression] モジュールを使用して、予測ラベルの確率を取得する。 ロジスティクス回帰で作成されたモデルが回帰モデルを生成しました `y = wX + c`  重み付けを行う `w` および切片 `c` はモデルの出力です。 SQL 機能を使用して、重みを乗算し、確率を取得できます。

- 第 2 に、で達成されたエンジニアリングプロセスです。 [!DNL Python] また、1 つのホットエンコードを SQL に組み込む必要があります。 例えば、元のデータベースには、 `geo_county` 列を保存するが、列は `geo_county=Bexar`, `geo_county=Dallas`, `geo_county=DeKalb`. 次の SQL 文は、同じ変換を実行します。ここで、 `w1`, `w2`、および `w3` は、 [!DNL Python]:

```sql
SELECT  CASE WHEN geo_state = 'Bexar' THEN FLOAT(w1) ELSE 0 END AS f1,
        CASE WHEN geo_state = 'Dallas' THEN FLOAT(w2) ELSE 0 END AS f2,
        CASE WHEN geo_state = 'Bexar' THEN FLOAT(w3) ELSE 0 END AS f3,
```

数値フィーチャの場合は、以下の SQL 文で示すように、列に重みを直接乗算できます。

```sql
SELECT FLOAT(purchase_num) * FLOAT(w4) AS f4,
```

数値が得られた後、それらは、ロジスティクス回帰アルゴリズムが最終予測を生成する S 状結合関数に移植することができます。 以下の文で、 `intercept` は、回帰の切片の数です。
        

```sql
SELECT CASE WHEN 1 / (1 + EXP(- (f1 + f2 + f3 + f4 + FLOAT(intercept)))) > 0.5 THEN 1 ELSE 0 END AS Prediction;
```
 
### エンドツーエンドの例

列が 2 つある場合 (`c1` および `c2`) `c1` には、 [!DNL Logistic Regression] アルゴリズムは、次の関数を使用して学習します。
 

```python
y = 0.1 * "c1=category 1"+ 0.2 * "c1=category 2" +0.3 * c2+0.4
```
 
SQL では、次のように同等です。

```sql
SELECT
  CASE WHEN 1 / (1 + EXP(- (f1 + f2 + f3 + FLOAT(0.4)))) > 0.5 THEN 1 ELSE 0 END AS Prediction
FROM
  (
    SELECT
      CASE WHEN c1 = 'Cateogry 1' THEN FLOAT(0.1) ELSE 0 END AS f1,
      CASE WHEN c1 = 'Cateogry 2' THEN FLOAT(0.2) ELSE 0 END AS f2,
      FLOAT(c2) * FLOAT(0.3) AS f3
    FROM TABLE
  )
```
 
この [!DNL Python] 翻訳プロセスを自動化するコードは次のとおりです。

```python
def generate_lr_inference_sql(ohc_columns, num_cols, clf, db):
    features_sql = []
    category_sql_text = "case when {col} = '{val}' then float({coef}) else 0 end as f{name}"
    numerical_sql_text = "float({col}) * float({coef}) as f{name}"
    for i, (column, coef) in enumerate(zip(ohc_columns+num_cols, clf.coef_[0])):
        if i < len(ohc_columns):
            col,val = column.split('=')
            val = val.replace("'","%''%")
            sql = category_sql_text.format(col=col,val=val,coef=coef,name=i+1)
        else:
            sql = numerical_sql_text.format(col=column,coef=coef,name=i+1)
        features_sql.append(sql)
    features_sum = '+'.join(['f{}'.format(i) for i in range(1,len(features_sql)+1)])
    final_sql = '''
    select case when 1/(1 + EXP(-({features} + float({intercept})))) > 0.5 then 1 else 0 end as Prediction
    from
        (select {cols}
        from {db})
    '''.format(features=features_sum,cols=",".join(features_sql),intercept=clf.intercept_[0],db=db)
    return final_sql
```

SQL を使用してデータベースを推論する場合、出力は次のようになります。

```python
sql = generate_lr_inference_sql(ohc_columns, num_cols, clf, "fdu_luma_raw")
cur.execute(sql)    
samples = [r for r in cur]
colnames = [desc[0] for desc in cur.description]
pd.DataFrame(samples,columns=colnames)
```

集計した結果は、顧客セッションごとに購入傾向を表示します。 `0` 買う傾向がなく `1` というのは、確かに購入傾向を意味している。

![SQL を使用したデータベースの推論の集計結果。](../images/use-cases/inference-results.png)

## サンプル済みデータの操作：ブートストラップ {#working-on-sampled-data}

ローカルマシンがモデルトレーニング用のデータを保存するにはデータサイズが大きすぎる場合は、クエリサービスの完全なデータの代わりにサンプルを取得できます。 クエリサービスからサンプリングするために必要なデータ量を知るには、ブートストラップと呼ばれる手法を適用します。 この点で、ブートストラップとは、モデルが様々なサンプルを使用して複数回トレーニングされ、異なるサンプル間でのモデルの精度の相違が調べられることを意味します。 上記の傾向モデルの例を調整するには、まず、機械学習ワークフロー全体を関数にカプセル化します。 コードは次のようになります。

```python
def end_to_end_pipeline(df):
    
    #define the target label for prediction
    df['target'] = (df['analytic_action'] == 'productPurchase').astype(int)
    #remove columns that are dependent on the label
    df.drop(['analytic_action','purchase_value'],axis=1,inplace=True)
    
    num_cols = ['purchase_num','value_cart','value_lifetime']
    df[num_cols] = df[num_cols].apply(pd.to_numeric, errors='coerce')
    
    #get the categorical columns
    cat_columns = list(set(df.columns) - set(num_cols + ['target']))

    #get the dataframe with categorical columns only
    df_cat = df.loc[:,cat_columns]

    #initialize sklearn's One Hot Encoder
    enc = OneHotEncoder(handle_unknown='ignore')

    #fit the data into the encoder
    enc.fit(df_cat)

    #define one hot encoder's columns names
    ohc_columns = [[c+'='+c_ for c_ in cat] for c,cat in zip(cat_columns,enc.categories_)]
    ohc_columns = [item for sublist in ohc_columns for item in sublist]

    #finalize the data input to the ML models
    X = pd.DataFrame( np.concatenate((enc.transform(df_cat).toarray(),df[num_cols]),axis=1),
                     columns =  ohc_columns + num_cols)

    #define target column
    y = df['target']
    
    X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.33, random_state=42)

    clf = LogisticRegression(max_iter=2000,random_state=0).fit(X_train, y_train)

    return clf.score(X_test, y_test)
```

その後、この関数は、1 回のループで複数回実行できます（例：10 回）。 前のコードとの違いは、サンプルがテーブル全体から取り出されるのではなく、行のサンプルのみが取り出される点です。 例えば、以下のサンプルコードでは 1000 行しか取りません。 各反復の精度を保存できます。

```python
from tqdm import tqdm

bootstrap_accuracy = []
for i in tqdm(range(100)):
    
    #sample data from QS
    cur.execute('''SELECT *
    FROM fdu_luma_raw
    ORDER BY random()
    LIMIT 1000
    ''')    
    samples = [r for r in cur]
    colnames = [desc[0] for desc in cur.description]
    df_samples = pd.DataFrame(samples,columns=colnames)
    df_samples.fillna(0,inplace=True)
    
    #train the propensity model with sampled data and output its accuracy
    bootstrap_accuracy.append(end_to_end_pipeline(df_samples))
    
bootstrap_accuracy = np.sort(bootstrap_accuracy)
```

次に、ブートストラップされたモデルの精度が並べ替えられます。 その後、モデルの精度の 10 番目と 90 番目のクォールは、指定されたサンプルサイズを持つモデルの精度の 95%信頼区間になります。

![傾向スコアの信頼区間を表示する印刷コマンド。](../images/use-cases/confidence-interval.png)

上記の図では、モデルのトレーニングに 1000 行しかかからない場合、精度は約 84%～88%に低下すると予想されます。 次の項目を調整できます。 `LIMIT` 句を使用して、モデルのパフォーマンスを確保する必要がある場合に役立ちます。


