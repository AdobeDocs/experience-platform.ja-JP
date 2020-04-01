---
keywords: Experience Platform;developer guide;endpoint;Data Science Workspace;popular topics
solution: Experience Platform
title: 付録
topic: Developer guide
translation-type: tm+mt
source-git-commit: 0081cbd414e983d767b4a6f3aceed85d72a34816

---


# 付録

以下の節では、Sensei Machine Learning APIの様々な機能に関するリファレンス情報を提供します。

## クエリ取得用のアセットパラメータ {#query}

Senesi Machine Learning APIは、アセットの取得に関するクエリパラメーターをサポートしています。 使用可能なクエリパラメーターとその使用方法を次の表に示します。

| クエリーパラメーター | 説明 | デフォルト値 |
| --------------- | ----------- | ------- |
| `start` | ページネーションの開始インデックスを示します。 | `start=0` |
| `limit` | 返す結果の最大数を示します。 | `limit=25` |
| `orderby` | 優先順での並べ替えに使用するプロパティを示します。 プロパティ名の前にダッシュ(**-**)を含めると、降順で並べ替えられます。それ以外の場合、結果は昇順で並べ替えられます。 | `orderby=created` |
| `property` | オブジェクトが返される式の中で満たす必要がある比較パラメータを示します。 | `property=deleted==false` |

>[!NOTE] 複数のクエリパラメーターを組み合わせる場合は、アンパサンド(**&amp;**)で区切る必要があります。

## Python CPUとGPUの設定 {#cpu-gpu-config}

Pythonエンジンは、トレーニングまたはスコアリングの目的でCPUまたはGPUのいずれかを選択でき、 [MLInstanceでタスク仕様](./mlinstances.md) (`tasks.specification`)として定義されます。

次の設定例では、トレーニングにCPUを使用し、スコアにGPUを使用するよう指定しています。

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

>[!NOTE] との値は、CPU `cpus` ま `gpus` たはGPUの数ではなく、物理マシンの数を示します。 これらの値は許可されており、 `"1"` それ以外の場合は例外がスローされます。

## PySparkおよびSparkのリソース設定 {#resource-config}

PySparkとSpark Enginesには、トレーニングやスコアリングの目的で計算リソースを変更する機能があります。これらのリソースについて次の表で説明します。

| リソース | 説明 | タイプ |
| -------- | ----------- | ---- |
| driverMemory | ドライバのメモリ(MB) | int |
| driverCores | ドライバが使用するコアの数 | int |
| executorMemory | 実行者のメモリ(MB) | int |
| executorCores | 実行者が使用するコアの数 | int |
| numExecutors | 実行者の数 | int |

リソースは、 [MLInstanceに対して](./mlinstances.md) 、(A)個々のトレーニングまたはスコアリングパラメータ、または(B)追加の仕様オブジェクト内(`specification`)として指定できます。 例えば、次のリソース設定は、トレーニングとスコアの両方で同じです。

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
