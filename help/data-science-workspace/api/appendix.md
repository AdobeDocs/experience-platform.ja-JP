---
keywords: Experience Platform;developer guide;endpoint;Data Science Workspace;popular topics
solution: Experience Platform
title: 付録
topic: Developer guide
translation-type: tm+mt
source-git-commit: 2940f69d193ff8a4ec6ad4a58813b5426201ef45
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 4%

---


# 付録

以下の節では、Senesie Machine Learning APIの様々な機能に関するリファレンス情報を提供します。

## アセット取得のクエリパラメータ {#query}

Senesie Machine Learning APIは、アセットの取得に関するクエリパラメーターをサポートしています。 次の表に、使用可能なクエリパラメーターとその使用方法を示します。

| クエリーパラメーター | 説明 | デフォルト値 |
| --------------- | ----------- | ------- |
| `start` | ページネーションの開始インデックスを示します。 | `start=0` |
| `limit` | 返す結果の最大数を示します。 | `limit=25` |
| `orderby` | 並べ替えの優先順に使用するプロパティを示します。 プロパティ名の前にダッシュ(**-**)を付けると、降順で並べ替えられます。ダッシュ(-)を付けると、結果が昇順で並べ替えられます。 | `orderby=created` |
| `property` | オブジェクトが返されるために満たす必要がある比較式を示します。 | `property=deleted==false` |

>[!NOTE] 複数のクエリパラメーターを組み合わせる場合は、アンパサンド(**&amp;**)で区切る必要があります。

## Python CPUとGPUの設定 {#cpu-gpu-config}

Pythonエンジンは、トレーニングまたはスコアリングの目的でCPUまたはGPUのどちらかを選択でき、 [MLInstance](./mlinstances.md) 上でタスク仕様(`tasks.specification`)として定義されます。

次の設定例は、トレーニングにCPUを使用することと、スコアリングにGPUを使用することを指定しています。

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

>[!NOTE] との値 `cpus``gpus` は、CPUまたはGPUの数ではなく、物理マシンの数を表します。 これらの値は許可されており、それ以外 `"1"` の場合は例外がスローされます。

## PySparkとSparkのリソース設定 {#resource-config}

Spark Enginesには、トレーニングとスコアリングの目的で計算リソースを変更する機能があります。 次の表に、これらのリソースを示します。

| リソース | 説明 | タイプ |
| -------- | ----------- | ---- |
| driverMemory | ドライバのメモリ（MB単位） | int |
| driverCores | ドライバが使用するコア数 | int |
| executorMemory | 実行者のメモリ(MB) | int |
| executorCores | 実行者が使用するコア数 | int |
| numExecutors | 実行者数 | int |

リソースは、 [MLInstanceに対して、(A)個別のトレーニングまたはスコアリングパラメーター、](./mlinstances.md) (B)追加の仕様オブジェクト(`specification`)内で指定できます。 例えば、次のリソース設定は、トレーニングとスコアリングの両方で同じです。

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
