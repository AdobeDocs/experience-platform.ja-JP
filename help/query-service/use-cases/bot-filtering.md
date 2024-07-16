---
title: 機械学習を使用したクエリサービスでのボットフィルタリング
description: このドキュメントでは、クエリサービスと機械学習を使用してボットアクティビティを決定し、純粋なオンライン web サイト訪問者トラフィックからそのアクションをフィルタリングする方法の概要を説明します。
exl-id: fc9dbc5c-874a-41a9-9b60-c926f3fd6e76
source-git-commit: cde7c99291ec34be811ecf3c85d12fad09bcc373
workflow-type: tm+mt
source-wordcount: '909'
ht-degree: 5%

---

# 機械学習に [!DNL Query Service] るボットフィルタリング

ボットアクティビティは、分析指標に影響を与え、データの整合性に影響を与える可能性があります。 Adobe Experience Platform [!DNL Query Service] を使用すると、ボットフィルタリングのプロセスを通じてデータ品質を維持できます。

ボットフィルタリングを使用すると、web サイトに対する人間ではないインタラクションに起因するデータ汚染を幅広く削除することで、データ品質を維持できます。 このプロセスは、SQL クエリと機械学習を組み合わせることで実現されます。

ボットアクティビティは、様々な方法で識別できます。 このドキュメントで取り上げるアプローチは、アクティビティのスパイク、この場合は、特定の期間にユーザーが実行したアクション数に焦点を当てています。 最初、SQL クエリは、ボットアクティビティと見なすために、一定期間にわたって実行されたアクション数のしきい値を任意に設定します。 次に、この閾値は、機械学習モデルを使用して動的に絞り込まれ、これらの比の精度が向上します。

このドキュメントでは、環境でプロセスを設定するために必要な SQL ボットフィルタリングクエリと機械学習モデルの概要と詳細な例を示します。

## はじめに

このプロセスの一部として機械学習モデルのトレーニングが必要なため、このドキュメントでは 1 つ以上の機械学習環境に関する実務知識を前提としています。

この例では、[!DNL Jupyter Notebook] を開発環境として使用します。 使用できるオプションは多数ありますが、[!DNL Jupyter Notebook] れはオープンソースの web アプリケーションであり、計算要件が低いので、推奨されます。 [ 公式サイトからダウンロード ](https://jupyter.org/) できます。

## [!DNL Query Service] を使用して、ボットアクティビティのしきい値を定義します

ボット検出用のデータの抽出に使用する 2 つの属性は次のとおりです。

* Experience Cloud訪問者 ID （ECID、MCID とも呼ばれます）：すべてのAdobeソリューションで訪問者を特定する永続的な汎用 ID を提供します。
* タイムスタンプ：web サイトでアクティビティが発生した際の時刻と日付を UTC 形式で提供します。

>[!NOTE]
>
>以下の例に示すように、Experience Cloud訪問者 ID への名前空間リファレンスでは、引き続き `mcid` の使用が見つかります。

次の SQL ステートメントは、ボットアクティビティを識別する最初の例を示しています。 このステートメントは、訪問者が 1 分以内に 50 回クリックした場合、ユーザーはボットであると想定しています。

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

式では、しきい値を満たすものの、他の間隔からのトラフィックのスパイクに対処していないすべての訪問者の ECID （`mcid`）をフィルタリングします。

## 機械学習によるボット検出の向上

最初の SQL 文を絞り込んで、機械学習の機能抽出クエリにすることができます。 改善されたクエリは、高い精度を持つ機械学習モデルをトレーニングするための様々な間隔に対して、より多くの機能を生成する必要があります。

サンプルステートメントは、最大 60 回のクリックで 1 分から拡張され、5 分と 30 分の期間に対して、それぞれクリック数 300 と 1800 が含まれます。

このステートメントの例では、様々な期間にわたる各 ECID （`mcid`）の最大クリック数を収集します。 最初のステートメントが拡張されて、1 分（60 秒）、5 分（300 秒）、1 時間（1800 秒）の期間が含まれるようになりました。

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

この式の結果は、次の表のようになります。

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

次に、結果のクエリデータセットを CSV 形式で書き出し、[!DNL Jupyter Notebook] に読み込みます。 その環境から、現在の機械学習ライブラリを使用して機械学習モデルをトレーニングできます。 [CSV 形式でデータを書き出す方法  [!DNL Query Service]  について詳しくは、トラブルシューティングガイドを参照してください ](../troubleshooting-guide.md#export-csv)

最初に設定したアドホックスパイクしきい値はデータ駆動型ではないので、精度に欠けます。 機械学習モデルは、パラメーターをしきい値としてトレーニングするために使用できます。 その結果、不要な機能を削除して `GROUP BY` キーワードの数を減らし、クエリの効率を高めることができます。

この例では、[!DNL Jupyter Notebook] と共にデフォルトでインストールされる Scikit-Learn 機械学習ライブラリを使用しています。 「pandas」 Python ライブラリもここで使用するために読み込まれます。 次のコマンドが [!DNL Jupyter Notebook] に入力されます。

```shell
import pandas as ps

df = pd_read.csv('data/bot.csv')
df = df[df['count_1-min'] > 1]
df['is_bot'] = 0
df.loc[df['count_1_min'] > 50,'is_bot'] = 1
df
```

次に、データセットに対してデシジョンツリー分類子をトレーニングし、モデルから生成されたロジックを確認する必要があります。

以下の例では、「Matplotlib」ライブラリを使用して、デシジョンツリー分類子を視覚化します。

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

この例で [!DNL Jupyter Notebook] から返される値は次のとおりです。

```text
Model Accuracy: 0.99935
```

![ 機械学習モデルから [!DNL Jupyter Notebook] 統計出力。](../images/use-cases/jupiter-notebook-output.png)

上記の例に示すモデルの結果は、99% 以上が正確です。

デシジョンツリー分類子は、スケジュールされたクエリを使用して通常のサイクルで [!DNL Query Service] からのデータを使用してトレーニングできるので、ボットアクティビティを高い精度でフィルタリングすることで、データの整合性を確保できます。 機械学習モデルから派生したパラメータを用いることで、モデルが作成した高精度なパラメータで元のクエリを更新することができる。

このサンプルモデルでは、5 分間にインタラクションが 130 を超える訪問者はボットであると高精度に判定されています。 この情報を使用して、ボットフィルタリング SQL クエリを絞り込めるようになりました。

## 次の手順

このドキュメントを参照することで、[!DNL Query Service] と機械学習を使用してボットアクティビティを決定およびフィルタリングする方法をより深く理解できます。

組織の戦略的ビジネスインサイトに対する [!DNL Query Service] の利点を示すその他のドキュメントには、[ 放棄された参照のユースケース ](./abandoned-browse.md) 例があります。
