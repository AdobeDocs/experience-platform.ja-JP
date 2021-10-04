---
keywords: Experience Platform；開発者ガイド；エンドポイント；Data Science Workspace；人気の高いトピック；
solution: Experience Platform
title: Sensei 機械学習 API ガイドの付録
topic-legacy: Developer guide
description: 以下の節では、Sensei Machine Learning API の様々な機能に関するリファレンス情報を提供します。
exl-id: 2c8d3ae8-7ad7-4ff6-8d6b-3a42d3eccdff
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 80%

---

# [!DNL Sensei Machine Learning] API ガイドの付録

以下の節では、[!DNL Sensei Machine Learning] API の様々な機能に関するリファレンス情報を提供します。

## アセット取得用のクエリーパラメーター {#query}

[!DNL Sensei Machine Learning] API は、アセットの取得に関するクエリパラメーターをサポートしています。 使用可能なクエリパラメーターとその使用方法を次の表に示します。

| クエリーパラメーター | 説明 | デフォルト値 |
| --------------- | ----------- | ------- |
| `start` | ページネーションの開始インデックスを示します。 | `start=0` |
| `limit` | 返す結果の最大数を示します。 | `limit=25` |
| `orderby` | 優先順での並べ替えに使用するプロパティを示します。プロパティ名の前にダッシュ（**-**）を含めると、降順で並べ替えられます。それ以外の場合、結果は昇順で並べ替えられます。 | `orderby=created` |
| `property` | オブジェクトが返される式の中で満たす必要がある比較パラメーターを示します。 | `property=deleted==false` |

>[!NOTE]
>
>複数のクエリパラメーターを組み合わせる場合は、アンパサンド（**&amp;**）で区切る必要があります。

## Python CPU と GPU の設定 {#cpu-gpu-config}

Python Engines は、トレーニングまたはスコアリングの目的で CPU または GPU のいずれかを選択でき、[MLInstance](./mlinstances.md) でタスク仕様（`tasks.specification`）として定義されます。

次の設定例では、トレーニングに CPU を使用し、スコアに GPU を使用するよう指定しています。

```json
[
    {
        "name": "train",
        "parameters": [
            {
                "key": "training parameter",
                "value": "parameter value"
            }    
        ],
        "specification": {
            "type": "ContainerTaskSpec",
            "cpus": "1"
        }
    },
    {
        "name": "score",
        "parameters": [
            {
                "key": "scoring parameter",
                "value": "parameter value" 
            }
        ],
        "specification": {
            "type": "ContainerTaskSpec",
            "gpus": "1"
        }
    }
]
```

>[!NOTE]
>
>`cpus` と `gpus` の値は、CPU と GPU の数ではなく、物理マシンの数を示します。これらの値は許容できる`"1"`であり、それ以外の場合は例外がスローされます。

## PySpark および Spark のリソース設定 {#resource-config}

Spark Engines には、トレーニングやスコアリングの目的で計算リソースを変更する機能があります。 次の表に、これらのリソースを示します。

| リソース | 説明 | タイプ |
| -------- | ----------- | ---- |
| driverMemory | ドライバのメモリ（MB） | int |
| driverCores | ドライバが使用するコアの数 | int |
| executorMemory | 実行者のメモリ（MB） | int |
| executorCores | 実行者が使用するコアの数 | int |
| numExecutors | 実行者の数 | int |

リソースは、[MLInstance](./mlinstances.md) に対して、（A）個々のトレーニングまたはスコアリングパラメーター、または（B）追加の仕様オブジェクト内（`specification`）として指定できます。たとえば、次のリソース設定は、トレーニングとスコアの両方で同じです。

```json
[
    {
        "name": "train",
        "parameters": [
            {
                "key": "driverMemory",
                "value": "2048"
            },
            {
                "key": "driverCores",
                "value": "1"
            },
            {
                "key": "executorMemory",
                "value": "2048"
            },
            {
                "key": "executorCores",
                "value": "2"
            },
            {
                "key": "numExecutors",
                "value": "3"
            }
        ]
    },
    {
        "name": "score",
        "parameters": [
            {
                "key": "scoring parameter",
                "value": "parameter value"
            }
        ],
        "specification": {
            "type": "SparkTaskSpec",
            "name": "Spark Task name",
            "className": "Class name",
            "driverMemoryInMB": 2048,
            "driverCores": 1,
            "executorMemoryInMB": 2048,
            "executorCores": 2,
            "numExecutors": 3
        }
    }
]
```
