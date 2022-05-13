---
title: 機械学習を使用したクエリサービスでのボットフィルタリング
description: このドキュメントでは、クエリサービスと機械学習を使用してボットのアクティビティを決定し、純粋なオンライン Web サイト訪問者トラフィックからそのアクションをフィルタリングする方法の概要を説明します。
exl-id: fc9dbc5c-874a-41a9-9b60-c926f3fd6e76
source-git-commit: c5b91bd516e876e095a2a6b6e3ba962b29f55a7b
workflow-type: tm+mt
source-wordcount: '873'
ht-degree: 6%

---

# でのボットフィルタリング [!DNL Query Service] 機械学習を伴う

ボットアクティビティは、分析指標に影響を与え、データの整合性が損なわれる可能性があります。 Adobe Experience Platform [!DNL Query Service] を使用すると、ボットフィルタリングのプロセスを通じてデータの品質を維持できます。

ボットフィルタリングを使用すると、Web サイトとの非人間的なやり取りに起因するデータの汚染を幅広く除去し、データの品質を維持できます。 このプロセスは、SQL クエリと機械学習を組み合わせて実現します。

ボットアクティビティは、様々な方法で識別できます。 このドキュメントでとられるアプローチは、アクティビティのスパイク（この場合は、特定の期間にユーザーが実行したアクションの数）に焦点を当てています。 最初は、SQL クエリは、一定期間にボットアクティビティとして認定されるために実行されたアクション数のしきい値を任意に設定します。 このしきい値は、機械学習モデルを使用して動的に絞り込まれ、これらの比率の精度が向上します。

このドキュメントでは、SQL ボットフィルタリングクエリの概要と詳細な例、および環境でプロセスを設定するために必要な機械学習モデルについて説明します。

## はじめに

このプロセスの一環として、機械学習モデルをトレーニングする必要があります。このドキュメントでは、1 つ以上の機械学習環境に関する実務知識を前提としています。

この例では、 [!DNL Jupyter Notebook] を開発環境として使用する。 多くのオプションを使用できますが、 [!DNL Jupyter Notebook] の計算要件が小さいオープンソース web アプリケーションなので、をお勧めします。 次のことが可能です。 [公式サイトからダウンロードされる](https://jupyter.org/).

## 用途 [!DNL Query Service] ボットアクティビティのしきい値を定義するには

ボット検出用にデータを抽出する際に使用する属性は次の 2 つです。

* Marketing CloudID (MCID):これにより、すべてのAdobeソリューションにわたって訪問者を識別する、永続的な汎用 ID が提供されます。
* タイムスタンプ：Web サイト上でアクティビティが発生した日時を UTC 形式で表示します。

次の SQL 文は、ボットアクティビティを識別するための最初の例を示しています。 この文は、訪問者が 1 分以内に 50 回のクリックを実行した場合、そのユーザーがボットであると想定しています。

```sql
SELECT * 
FROM   <YOUR_TABLE_NAME> 
WHERE  enduserids._experience.mcid NOT IN (SELECT enduserids._experience.mcid 
                                           FROM   <YOUR_TABLE_NAME> 
                                           GROUP  BY Unix_timestamp(timestamp) / 
                                                     60, 
                                                     enduserids._experience.mcid 
                                           HAVING Count(*) > 50);  
```

式は、しきい値を満たすすべての訪問者の MCID をフィルタリングしますが、他の間隔からのトラフィックのスパイクには対処しません。

## 機械学習によるボット検出の改善

最初の SQL 文を絞り込んで、機械学習用の機能抽出クエリにすることができます。 向上したクエリでは、精度の高い機械学習モデルのトレーニングに、様々な間隔でより多くの機能を生成する必要があります。

この例の文は、1 分から 60 回までのクリック数で展開され、5 分と 30 分の期間が含まれ、それぞれ 300、1800 のクリック数が含まれます。

この例の文は、様々な期間における各 MCID の最大クリック数を収集します。 最初の文が、1 分（60 秒）、5 分（300 秒）、1 時間（1800 秒）の期間を含むように拡張されました。

```sql
SELECT table_count_1_min.mcid AS id, 
       count_1_min, 
       count_5_mins, 
       count_30_mins 
FROM   ( ( (SELECT mcid, 
                 Max(count_1_min) AS count_1_min 
          FROM   (SELECT enduserids._experience.mcid.id AS mcid, 
                         Count(*)                       AS count_1_min 
                  FROM 
       <YOUR_TABLE_NAME> 
                  GROUP  BY Unix_timestamp(timestamp) / 60, 
                            enduserids._experience.mcid.id) 
          GROUP BY mcid) AS table_count_1_min 
           LEFT JOIN (SELECT mcid, 
                             Max(count_5_mins) AS count_5_mins 
                      FROM   (SELECT enduserids._experience.mcid.id AS mcid, 
                                     Count(*)                       AS 
                                     count_5_mins 
                              FROM 
           <YOUR_TABLE_NAME> 
                              GROUP  BY Unix_timestamp(timestamp) / 300, 
                                        enduserids._experience.mcid.id) 
                      GROUP  BY mcid) AS table_count_5_mins 
                  ON table_count_1_min.mcid = table_count_5_mins.mcid ) 
         LEFT JOIN (SELECT mcid, 
                           Max(count_30_mins) AS count_30_mins 
                    FROM   (SELECT enduserids._experience.mcid.id AS mcid, 
                                   Count(*)                       AS 
                                   count_30_mins 
                            FROM 
         <YOUR_TABLE_NAME> 
                            GROUP  BY Unix_timestamp(timestamp) / 1800, 
                                      enduserids._experience.mcid.id) 
                    GROUP  BY mcid) AS table_count_30_mins 
                ON table_count_1_min.mcid = table_count_30_mins.mcid ) 
```

この式の結果は、以下の表のようになります。

| `id` | `count_1_min` | `count_5_min` | `count_30_min` |
|---|---|---|---|
| 4833075303848917832 | 1 | 1 | 1 |
| 1469109497068938520 | 1 | 1 | 1 |
| 5045682519445554093 | 1 | 1 | 1 |
| 7148203816406620066 | 3 | 3 | 3 |
| 1013065429311352386 | 1 | 1 | 1 |
| 3077475871984695013 | 7 | 7 | 7 |
| 4151486040344674930 | 2 | 2 | 2 |
| 6563366098591762751 | 6 | 6 | 6 |
| 2403566105776993627 | 4 | 4 | 4 |
| 5710530640819698543 | 1 | 1 | 1 |
| 3675089655839425960 | 1 | 1 | 1 |
| 9091930660723241307 | 1 | 1 | 1 |

## 機械学習を使用した新しいスパイクしきい値の特定

次に、結果のクエリデータセットを CSV 形式に書き出し、に読み込みます。 [!DNL Jupyter Notebook]. その環境から、現在の機械学習ライブラリを使用して機械学習モデルをトレーニングできます。 詳しくは、トラブルシューティングガイドを参照してください。 [からデータをエクスポートする方法 [!DNL Query Service] CSV 形式](../troubleshooting-guide.md#export-csv)

最初に確立されたアドホックスパイクのしきい値は、データ主導ではないので、正確性に欠けます。 機械学習モデルを使用して、パラメーターをしきい値としてトレーニングできます。 その結果、 `GROUP BY` 不要な機能を削除してキーワードを設定できます。

この例では、 Scikit-Learn 機械学習ライブラリを使用します。このライブラリは、デフォルトでと共にインストールされます。 [!DNL Jupyter Notebook]. Python ライブラリ「pandas」も、ここで使用するために読み込まれます。 次のコマンドは、 [!DNL Jupyter Notebook].

```shell
import pandas as ps

df = pd_read.csv('data/bot.csv')
df = df[df['count_1-min'] > 1]
df['is_bot'] = 0
df.loc[df['count_1_min'] > 50,'is_bot'] = 1
df
```

次に、データセットでデシジョンツリー分類子をトレーニングし、モデルから生じるロジックを観察する必要があります。

「Matplotlib」ライブラリは、以下の例でデシジョンツリー分類子を視覚化するために使用されます。

```shell
from sklearn.tree import DecisionTreeClassifier
from sklearn import tree
from matplotlib import pyplot as plt

X = df.iloc[:,:[1,3]]
y = df.iloc[:,-1]

clf = DecisionTreeClassifier(max_leaf_nodes=2, random_state=0)
clf.fit(X,y)

print("Model Accuracy: {:.5f}".format(clf.scre(X,y)))

tree.plot_tree(clf,feature_names=X.columns)
plt.show()
```

から返される値 [!DNL Jupyter Notebook] 例を次に示します。

```text
Model Accuracy: 0.99935
```

![統計的出力元 [!DNL Jupyter Notebook] 機械学習モデル。](../images/use-cases/jupiter-notebook-output.png)

上の例で示したモデルの結果は、99%以上の正確性です。

デシジョンツリー分類子は、 [!DNL Query Service] スケジュールされたクエリを使用する定期的なサイクルでは、高い精度でボットアクティビティをフィルタリングすることで、データの整合性を確保できます。 機械学習モデルから派生したパラメーターを使用すると、モデルで作成された高精度のパラメーターで元のクエリを更新できます。

この例モデルは、130 件を超えるインタラクションを 5 分で行う訪問者がボットであるという高い精度で特定されました。 この情報を使用して、ボットフィルタリング SQL クエリを絞り込むことができるようになりました。

## 次の手順

このドキュメントを読むと、の使用方法をより深く理解できます [!DNL Query Service] および機械学習を使用してボットアクティビティを決定し、フィルタリングします。

の利点を示すその他のドキュメント [!DNL Query Service] 組織の戦略的ビジネスインサイトに対して、 [廃止された参照の使用例](./abandoned-browse.md) 例：
