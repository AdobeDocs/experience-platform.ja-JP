---
keywords: Experience Platform;train and evaluate;Data Science Workspace;popular topics
solution: Experience Platform
title: モデル(API)のトレーニングと評価
topic: Tutorial
translation-type: tm+mt
source-git-commit: 5699022d1f18773c81a0a36d4593393764cb771a

---


# モデル(API)のトレーニングと評価


このチュートリアルでは、API呼び出しを使用してモデルを作成、トレーニング、評価する方法を示します。 APIドキュメ [ントの詳しいリストについては](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml) 、このドキュメントを参照してください。

## 前提条件

APIを使用し [てモデルをトレーニングおよび評価するために必要なAPI](./import-packaged-recipe-api.md) （Engineを作成するためのAPIを使用したパッケージレシピの読み込み）に従います。

API呼び出しを行う [開始の認証については](../../tutorials/authentication.md) 、このチュートリアルに従ってください。

このチュートリアルでは、次の値を使用する必要があります。

- `{ACCESS_TOKEN}`:認証後に指定された特定のベアラトークン値。
- `{IMS_ORG}`:IMS組織の資格情報が、お使いの一意のAdobe Experience Platform統合で見つかりました。
- `{API_KEY}`:固有のAdobe Experience Platform統合で見つかった特定のAPIキーの値。

- インテリジェントサービスのDockerイメージへのリンク

## APIワークフロー

このAPIを使用して、トレーニング用のテスト実行を作成します。 このチュートリアルでは、エンジン、 **MLInstances**、 **Experiments**&#x200B;の各エンドポイ **ントに焦点を当** てます。 次の表に、3つの関係の概要を示し、実行とモデルの概念を示します。

![](../images/models-recipes/train-evaluate-api/engine_hierarchy_api.png)

>[!NOTE] 「Engine」、「MLInstance」、「MLService」、「Tesprient」、「Model」という用語は、UIでは別の用語と呼ばれます。 UIからアクセスしている場合、次の表で違いを示します。
> 
> | UI用語 | API用語 |
> --- | ---
> | レシピ | エンジン |
> | モデル | MLInstance |
> | トレーニングの実行 | 実験 |
> | サービス | MLService |



### MLInstanceの作成

MLInstanceの作成は、次のリクエストを使用して行うことができます。 APIチュートリアルを使用し `{ENGINE_ID}` てパッケージ化されたレシピの読み込みからエンジ [ンを作成する際に返されたを使用します](./import-packaged-recipe-ui.md) 。

**リクエスト**

```SHELL
curl -X POST \
  https://platform.adobe.io/data/sensei/mlInstances \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/vnd.adobe.platform.sensei+json;profile=mlInstance.v1.json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -d `{JSON_PAYLOAD}`
```

`{ACCESS_TOKEN}`:認証後に指定された特定のベアラトークン値。\
`{IMS_ORG}`:IMS組織の資格情報が、お使いの一意のAdobe Experience Platform統合で見つかりました。\
`{API_KEY}`:固有のAdobe Experience Platform統合で見つかった特定のAPIキーの値。\
`{JSON_PAYLOAD}`:MLInstanceの設定。 このチュートリアルで使用する例を次に示します。

```JSON
{
    "name": "Retail - Instance",
    "description": "Instance for ML Instance",
    "engineId": "{ENGINE_ID}",
    "createdBy": {
        "displayName": "John Doe",
        "userId": "johnd"
    },
    "tags": {
        "purpose": "tutorial"
    },
    "tasks": [
        {
            "name": "train",
            "parameters": [
                {
                    "key": "numFeatures",
                    "value": "10"
                },
                {
                    "key": "maxIter",
                    "value": "2"
                },
                {
                    "key": "regParam",
                    "value": "0.15"
                },
                {
                    "key": "trainingDataLocation",
                    "value": "sample_training_data.csv"
                }
            ]
        },
        {
            "name": "score",
            "parameters": [
                {
                    "key": "scoringDataLocation",
                    "value": "sample_scoring_data.csv"
                },
                {
                    "key": "scoringResultsLocation",
                    "value": "scoring_results.net"
                }
            ]
        }
    ]
}
```

>[!NOTE] では、アレ `{JSON_PAYLOAD}`イ内のトレーニングとスコアリングに使用するパラメータを定義 `tasks` します。 は使 `{ENGINE_ID}` 用するエンジンのIDで、フィールドはインスタ `tag` ンスの識別に使用するオプションのパラメータです。

応答には、作成されたMLInstance `{INSTANCE_ID}` を表すが含まれます。 異なる設定の複数のモデルMLInstanceを作成できます。

**応答**

```JSON
{
    "id": "{INSTANCE_ID}",
    "name": "Retail - Instance",
    "description": "Instance for ML Instance",
    "engineId": "{ENGINE_ID}",
    "created": "2018-21-21T11:11:11.111Z",
    "createdBy": {
        "displayName": "John Doe",
        "userId": "johnd"
    },
    "updated": "2018-21-01T11:11:11.111Z",
    "deleted": false,
    "tags": {
        "purpose": "tutorial"
    },
    "tasks": [
        {
            "name": "train",
            "parameters": [...]
        },
        {
            "name": "score",
            "parameters": [...]
        }
    ]
}
```

`{ENGINE_ID}`:MLInstanceが作成されるエンジンを表すこのID。\
`{INSTANCE_ID}`:MLInstanceを表すIDです。

### テストの作成

テストは、トレーニング中にパフォーマンスの高いモデルに到達するために、データサイエンティストが使用します。 複数の実験には、データセット、機能、学習パラメータ、ハードウェアの変更が含まれます。 次に、テストの作成例を示します。

**リクエスト**

```SHELL
curl -X POST \
  https://platform.adobe.io/data/sensei/experiments \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/vnd.adobe.platform.sensei+json;profile=experiment.v1.json' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key: {API_KEY' \
  -d `{JSON PAYLOAD}`
```

`{IMS_ORG}`:IMS組織の資格情報が、お使いの一意のAdobe Experience Platform統合で見つかりました。\
`{ACCESS_TOKEN}`:認証後に指定された特定のベアラトークン値。\
`{API_KEY}`:固有のAdobe Experience Platform統合で見つかった特定のAPIキーの値。\
`{JSON_PAYLOAD}`:作成されたテストオブジェクト。 このチュートリアルで使用する例を次に示します。

```JSON
{
    "name": "Experiment for Retail ",
    "mlInstanceId": "{INSTANCE_ID}",
    "tags": {
        "test": "guide"
    }
}
```

`{INSTANCE_ID}`:MLInstanceを表すIDです。

テストの作成からの応答は次のようになります。

**応答**

```JSON
{
    "id": "{EXPERIMENT_ID}",
    "name": "Experiment for Retail",
    "mlInstanceId": "{INSTANCE_ID}",
    "created": "2018-01-01T11:11:11.111Z",
    "updated": "2018-01-01T11:11:11.111Z",
    "deleted": false,
    "tags": {
        "test": "guide"
    }
}
```

`{EXPERIMENT_ID}`:作成した実験を表すID。
`{INSTANCE_ID}`:MLInstanceを表すIDです。

### トレーニング用の予定されたテストの作成

スケジュールされた実験は、API呼び出しを通じて1回のテスト実行を作成する必要がないように使用されます。 代わりに、テストの作成時に必要なすべてのパラメーターを指定し、各実行は定期的に作成されます。

スケジュールされたテストの作成を示すには、リクエストの本文にセク `template` ションを追加する必要があります。 では、ス `template`ケジュールの実行に必要なすべてのパラメータ(どのアクシ `tasks`ョンを示すか、スケジュールされた実行のタ `schedule`イミングを示すかなど)が含まれます。

**リクエスト**

```Shell
curl -X POST \
  https://platform.adobe.io/data/sensei/experiments \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/vnd.adobe.platform.sensei+json;profile=experiment.v1.json' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key: {API_KEY}' \
  -d '{JSON_PAYLOAD}`
```

`{IMS_ORG}`:IMS組織の資格情報が、お使いの一意のAdobe Experience Platform統合で見つかりました。\
`{ACCESS_TOKEN}`:認証後に指定された特定のベアラトークン値。\
`{API_KEY}`:固有のAdobe Experience Platform統合で見つかった特定のAPIキーの値。\
`{JSON_PAYLOAD}`:転記するデータセット。 このチュートリアルで使用する例を次に示します。

```JSON
{
    "name": "Experiment for Retail",
    "mlInstanceId": "{INSTANCE_ID}",
    "template": {
        "tasks": [{
            "name": "train",
            "parameters": [
                   {
                        "value": "1000",
                        "key": "numFeatures"
                    }
            ],
            "specification": {
                "type": "SparkTaskSpec",
                "executorCores": 5,
                "numExecutors": 5
            }
        }],
        "schedule": {
            "cron": "*/20 * * * *",
            "startTime": "2018-11-11",
            "endTime": "2019-11-11"
        }
    }
}
```

テストを作成する際、ボディには、ま `{JSON_PAYLOAD}`たはパラメータを含め `mlInstanceId` る必要があり `mlInstanceQuery` ます。 この例では、スケジュールされたテストは、パラメーターに設定された20分ごとに実行を呼び出し、 `cron` からが始まるまでの間 `startTime` に実行しま `endTime`す。

**応答**

```JSON
{
    "id": "{EXPERIMENT_ID}",
    "name": "Experiment for Retail",
    "mlInstanceId": "{INSTANCE_ID}",
    "created": "2018-11-11T11:11:11.111Z",
    "updated": "2018-11-11T11:11:11.111Z",
    "deleted": false,
    "workflowId": "endid123_0379bc0b_8f7e_4706_bcd9_1a2s3d4f5g_abcdf",
    "template": {
        "tasks": [
            {
                "name": "train",
                "parameters": [...],
                "specification": {
                    "type": "SparkTaskSpec",
                    "executorCores": 5,
                    "numExecutors": 5
                }
            }
        ],
        "schedule": {
            "cron": "*/20 * * * *",
            "startTime": "2018-07-04",
            "endTime": "2018-07-06"
        }
    }
}
```

`{EXPERIMENT_ID}`:テストを表すID。\
`{INSTANCE_ID}`:MLInstanceを表すIDです。


### トレーニング用のテストの実行の作成

テストエンティティを作成すると、以下の呼び出しを使用して、トレーニング実行を作成し、実行できます。 リクエスト本文でト `{EXPERIMENT_ID}` リガーする内 `mode` 容を、およびに示す必要があります。

**リクエスト**

```Shell
curl -X POST \
  https://platform.adobe.io/data/sensei/experiments/{EXPERIMENT_ID}/runs \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/vnd.adobe.platform.sensei+json;profile=experimentRun.v1.json' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key: {API_KEY}' \
  -d '{JSON_PAYLOAD}'
```

`{EXPERIMENT_ID}`:ターゲットするテストに対応するID。 これは、テストを作成する際の応答に含まれています。\
`{IMS_ORG}`:IMS組織の資格情報が、お使いの一意のAdobe Experience Platform統合で見つかりました。\
`{ACCESS_TOKEN}`:認証後に指定された特定のベアラトークン値。\
`{API_KEY}`:固有のAdobe Experience Platform統合で見つかった特定のAPIキーの値。\
`{JSON_PAYLOAD}`:トレーニング実行を作成するには、本文に次の内容を含める必要があります。

```JSON
{
    "mode":"Train"
}
```

また、配列を含めることで、設定パラメーターを上書きすることも `tasks` できます。

```JSON
{
   "mode":"Train",
   "tasks": [
        {
           "name": "train",
           "parameters": [
                {
                   "key": "numFeatures",
                   "value": "2"
                }
            ]
        }
    ]
}
```

次の応答が表示され、に示すのと設定が `{EXPERIMENT_RUN_ID}` 通知されます `tasks`。

**応答**

```JSON
{
    "id": "{EXPERIMENT_RUN_ID}",
    "mode": "train",
    "experimentId": "{EXPERIMENT_ID}",
    "created": "2018-01-01T11:11:11.903Z",
    "updated": "2018-01-01T11:11:11.903Z",
    "deleted": false,
    "tasks": [
        {
            "name": "Train",
            "parameters": [...]
        }
    ]
}
```

`{EXPERIMENT_RUN_ID}`: テストの実行を表すID。\
`{EXPERIMENT_ID}`:テストの実行が含まれるテストを表すID。

### テストの実行ステータスの取得

テスト実行のステータスをに照会できます `{EXPERIMENT_RUN_ID}`。

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/sensei/experiments/{EXPERIMENT_ID}/runs/{EXPERIMENT_RUN_ID}/status \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key: {API_KEY}'
```

`{EXPERIMENT_ID}`:テストを表すID。\
`{EXPERIMENT_RUN_ID}`:テストの実行を表すID。\
`{ACCESS_TOKEN}`:認証後に指定された特定のベアラトークン値。\
`{IMS_ORG}`:IMS組織の資格情報が、お使いの一意のAdobe Experience Platform統合で見つかりました。\
`{API_KEY}`:固有のAdobe Experience Platform統合で見つかった特定のAPIキーの値。

**応答**

GET呼び出しは、次に示すように、パラメーター内のス `state` テータスを提供します。

```JSON
{
    "id": "{EXPERIMENT_ID}",
    "name": "RunStatus for experimentRunId {EXPERIMENT_RUN_ID}",
    "experimentRunId": "{EXPERIMENT_RUN_ID}",
    "deleted": false,
    "status": {
        "tasks": [
            {
                "id": "{MODEL_ID}",
                "state": "DONE",
                "tasklogs": [
                    {
                        "name": "execution",
                        "url": "https://mlbaprod1sapwd7jzid.file.core.windows.net/..."
                    },
                    {
                        "name": "stderr",
                        "url": "https://mlbaprod1sapwd7jzid.file.core.windows.net/..."
                    },
                    {
                        "name": "stdout",
                        "url": "https://mlbaprod1sapwd7jzid.file.core.windows.net/..."
                    }
                ]
            }
        ]
    }
}
```

`{EXPERIMENT_RUN_ID}`: テストの実行を表すID。\
`{EXPERIMENT_ID}`:テストの実行が含まれるテストを表すID。

状態に加えて、次の `DONE` 状態も含まれます。
- `PENDING`
- `RUNNING`
- `FAILED`

詳細な情報を得るには、パラメーターの下に詳細なログがあ `tasklogs` ります。

### トレーニングを受けたモデルの取得

トレーニング中に上記のトレーニングモデルを作成するには、次のリクエストを行います。

**リクエスト**

```Shell
curl -X GET \
  'https://platform.adobe.io/data/sensei/models/?property=experimentRunId=={EXPERIMENT_RUN_ID}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
```

`{EXPERIMENT_RUN_ID}`:ターゲットするテスト実行に対応するID。 これは、テストの実行を作成する際の応答に含まれています。\
`{ACCESS_TOKEN}`:認証後に指定された特定のベアラトークン値。\
`{IMS_ORG}`:IMS組織の資格情報が、お使いの一意のAdobe Experience Platform統合で見つかりました。

応答は、作成されたトレーニングを受けたモデルを表します。

**応答**

```JSON
{
    "children": [
        {
            "id": "{MODEL_ID}",
            "name": "Tutorial trained Model",
            "experimentId": "{EXPERIMENT_ID}",
            "experimentRunId": "{EXPERIMENT_RUN_ID}",
            "description": "trained model for ID",
            "modelArtifactUri": "wasb://test-models@mlpreprodstorage.blob.core.windows.net/{MODEL_ID}",
            "created": "2018-01-01T11:11:11.011Z",
            "updated": "2018-01-01T11:11:11.011Z",
            "deleted": false
        }
    ],
    "_page": {
        "property": "ExperimentRunId=={EXPERIMENT_RUN_ID},deleted!=true",
        "count": 1
    }
}
```

`{MODEL_ID}`:モデルに対応するID。\
`{EXPERIMENT_ID}`: テスト実行の対象となるテストに対応するID。\
`{EXPERIMENT_RUN_ID}`:テスト実行に対応するID。

### スケジュールされたテストの停止と削除

スケジュールされたテストの実行をその前に停止した `endTime`い場合は、DELETEリクエストを `{EXPERIMENT_ID}`

**リクエスト**

```Shell
curl -X DELETE \
  'https://platform.adobe.io/data/sensei/experiments/{EXPERIMENT_ID}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
```

`{EXPERIMENT_ID}`: テストに対応するID。\
`{ACCESS_TOKEN}`:認証後に指定された特定のベアラトークン値。\
`{IMS_ORG}`:IMS組織の資格情報が、お使いの一意のAdobe Experience Platform統合で見つかりました。

>[!NOTE] API呼び出しは、新しいテストの実行の作成を無効にします。 ただし、既に実行中のテストの実行は停止しません。

テストが正常に削除されたことを通知する応答を次に示します。

**応答**

```JSON
{
    "title": "Success",
    "status": 200,
    "detail": "Experiment successfully deleted"
}
```

## 次の手順

このチュートリアルでは、APIを使用して、エンジン、テスト、予定された実験の実行、トレーニングを受けたモデルを作成する方法について説明しました。 次の練習で [は](./score-model-api.md)、最もパフォーマンスの高いトレーニングモデルを使用して新しいデータセットをスコアリングし、予測を行います。