---
keywords: Experience Platform;開発者ガイド;エンドポイント;データ科学ワークスペース;人気のあるトピック;
solution: Experience Platform
title: Sensei マシン学習 API ガイド付録
description: 以下の節では、Sensei Machine Learning API の様々な機能に関するリファレンス情報を提供します。
role: Developer
exl-id: 2c8d3ae8-7ad7-4ff6-8d6b-3a42d3eccdff
source-git-commit: 5d98dc0cbfaf3d17c909464311a33a03ea77f237
workflow-type: tm+mt
source-wordcount: '353'
ht-degree: 62%

---

# [!DNL Sensei Machine Learning] API ガイド付録

>[!NOTE]
>
>データサイエンスワークスペースは購入できなくなりました。
>
>このドキュメントは、以前に データ Science ワークスペース の利用資格を持つ既存のお客様を対象としています。

以下の節では、 [!DNL Sensei Machine Learning] API の様々な機能に関するリファレンス情報を提供します。

## アセットを取得するためのクエリパラメーター {#query}

[!DNL Sensei Machine Learning] API は、クエリパラメーターとアセットの取得をサポートします。使用可能なクエリパラメーターとその使用方法を次の表に示します。

| クエリーパラメーター | 説明 | デフォルト値 |
| --------------- | ----------- | ------- |
| `start` | ページネーションの開始インデックスを示します。 | `start=0` |
| `limit` | 返す結果の最大数を示します。 | `limit=25` |
| `orderby` | 優先順での並べ替えに使用するプロパティを示します。プロパティ名の前にダッシュ（**-**）を含めると、降順で並べ替えられます。それ以外の場合、結果は昇順で並べ替えられます。 | `orderby=created` |
| `property` | オブジェクトが返される式の中で満たす必要がある比較パラメーターを示します。 | `property=deleted==false` |

>[!NOTE]
>
>複数のクエリパラメーターを組み合わせる場合は、アンパサンド(**&amp;**)で区切る必要があります。

## Python の CPU および GPU 構成 {#cpu-gpu-config}

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
>`cpus` と `gpus` の値は、CPU または GPU の数を示すのではなく、物理マシンの数を表します。これらの値は許容できる`"1"`であり、それ以外の場合は例外がスローされます。

## PySpark および Spark リソースの設定 {#resource-config}

Spark エンジンには、トレーニングとスコアリングの目的で計算リソースを変更する機能があります。 これらのリソースについては、次の表で説明します。

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
