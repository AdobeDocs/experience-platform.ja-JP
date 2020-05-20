---
keywords: Experience Platform;train and evaluate;Data Science Workspace;popular topics
solution: Experience Platform
title: モデル(API)のトレーニングと評価
topic: Tutorial
translation-type: tm+mt
source-git-commit: 5699022d1f18773c81a0a36d4593393764cb771a
workflow-type: tm+mt
source-wordcount: '1191'
ht-degree: 1%

---


# モデル(API)のトレーニングと評価


このチュートリアルでは、API呼び出しを使用してモデルを作成、トレーニング、評価する方法を示します。 APIドキュメントの詳細なリストにつ [いては、このドキュメント](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml) を参照してください。

## 前提条件

APIを使用してモデルのトレーニングと評価を行うために必要な、API [](./import-packaged-recipe-api.md) （パッケージ化されたレシピをAPIを使用して読み込む）に従います。

API呼び出しを行う開始の認証については、この [チュートリアル](../../tutorials/authentication.md) に従ってください。

チュートリアルでは、次の値を使用する必要があります。

- `{ACCESS_TOKEN}`: 認証後に指定された特定のベアラトークン値。
- `{IMS_ORG}`: IMS組織の資格情報が一意のAdobe Experience Platform統合で見つかりました。
- `{API_KEY}`: 独自のAdobe Experience Platform統合に見つかった特定のAPIキー値。

- インテリジェントサービスのDockerイメージへのリンク

## APIワークフロー

このAPIを使用して、トレーニング用のテスト実行を作成します。 このチュートリアルでは、 **エンジン**、 **MLInstances**、 **Experiments** エンドポイントについて説明します。 次の表は、3つの関係の概要と、実行とモデルの概念を示しています。

![](../images/models-recipes/train-evaluate-api/engine_hierarchy_api.png)

>[!NOTE] UIでは、「Engine」、「MLInstance」、「MLService」、「Estript」、「Model」という用語は別の用語と呼ばれます。 UIからアクセスしている場合、次の表に相違点を示します。
> 
> | UI用語 | API用語 |
> --- | ---
> | レシピ | エンジン |
> | モデル | MLInstance |
> | トレーニングの実施 | テスト |
> | サービス | MLService |



### MLInstanceの作成

MLInstanceの作成は、次のリクエストを使用して行うことができます。 API `{ENGINE_ID}` チュートリアルの「パッケージ化されたレシピの [](./import-packaged-recipe-ui.md) 読み込み」からエンジンを作成する際に返されたものを使用します。

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

`{ACCESS_TOKEN}`: 認証後に指定された特定のベアラトークン値。\
`{IMS_ORG}`: IMS組織の資格情報が一意のAdobe Experience Platform統合で見つかりました。\
`{API_KEY}`: 独自のAdobe Experience Platform統合に見つかった特定のAPIキー値。\
`{JSON_PAYLOAD}`: MLInstanceの設定。 チュートリアルで使用する例を次に示します。

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

>[!NOTE] で `{JSON_PAYLOAD}`は、アレイ内のトレーニングとスコアリングに使用するパラメーターを定義し `tasks` ます。 は使用するエンジンのID `{ENGINE_ID}` で、 `tag` フィールドはインスタンスの識別に使用されるオプションのパラメータです。

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

`{ENGINE_ID}`: MLInstanceが作成されるエンジンを表すこのID。\
`{INSTANCE_ID}`: MLInstanceを表すID。

### テストの作成

テストは、データサイエンティストがトレーニング中にパフォーマンスの高いモデルに到達するために使用します。 複数の実験には、データセット、機能、学習パラメータ、ハードウェアの変更が含まれます。 次に、テストの作成例を示します。

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

`{IMS_ORG}`: IMS組織の資格情報が一意のAdobe Experience Platform統合で見つかりました。\
`{ACCESS_TOKEN}`: 認証後に指定された特定のベアラトークン値。\
`{API_KEY}`: 独自のAdobe Experience Platform統合に見つかった特定のAPIキー値。\
`{JSON_PAYLOAD}`: 作成されたテストオブジェクト。 チュートリアルで使用する例を次に示します。

```JSON
{
    "name": "Experiment for Retail ",
    "mlInstanceId": "{INSTANCE_ID}",
    "tags": {
        "test": "guide"
    }
}
```

`{INSTANCE_ID}`: MLInstanceを表すID。

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

`{EXPERIMENT_ID}`: 作成したテストを表すID。
`{INSTANCE_ID}`: MLInstanceを表すID。

### トレーニング用の予定されたテストの作成

スケジュールされた実験は、API呼び出しを使用して1回のテスト実行を作成する必要がないように使用されます。 代わりに、テストの作成時に必要なすべてのパラメーターを指定します。各実行は定期的に作成されます。

スケジュールされたテストの作成を示すには、リクエストの本文に `template` セクションを追加する必要があります。 では、実行のスケジュールに必要なすべてのパラメーター( `template`アクションを示すパラメーター、スケジュールされた実行のタイミング `tasks``schedule`を示すパラメーターなど)が含まれます。

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

`{IMS_ORG}`: IMS組織の資格情報が一意のAdobe Experience Platform統合で見つかりました。\
`{ACCESS_TOKEN}`: 認証後に指定された特定のベアラトークン値。\
`{API_KEY}`: 独自のAdobe Experience Platform統合に見つかった特定のAPIキー値。\
`{JSON_PAYLOAD}`: 転記するデータセット。 チュートリアルで使用する例を次に示します。

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

テストを作成する場合、本体 `{JSON_PAYLOAD}`には、またはのいずれかの `mlInstanceId` パラメータ `mlInstanceQuery` を含める必要があります。 この例では、スケジュール設定されたテストは、パラメーターに設定された20分ごとに実行を呼び出します。この `cron` パラメーターは、の最初 `startTime` からがに至るまでとなります `endTime`。

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

`{EXPERIMENT_ID}`: テストを表すID。\
`{INSTANCE_ID}`: MLInstanceを表すID。


### トレーニング用のテスト実行の作成

テストエンティティを作成した場合、トレーニング実行は以下の呼び出しを使用して作成し、実行できます。 リクエスト本文でトリガ `{EXPERIMENT_ID}` ーす `mode` る対象を、および表示する必要があります。

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

`{EXPERIMENT_ID}`: ターゲットするテストに対応するID。 これは、テストの作成時の応答に含まれます。\
`{IMS_ORG}`: IMS組織の資格情報が一意のAdobe Experience Platform統合で見つかりました。\
`{ACCESS_TOKEN}`: 認証後に指定された特定のベアラトークン値。\
`{API_KEY}`: 独自のAdobe Experience Platform統合に見つかった特定のAPIキー値。\
`{JSON_PAYLOAD}`: トレーニングの実行を作成するには、本文に次の内容を含める必要があります。

```JSON
{
    "mode":"Train"
}
```

配列を含めることで、設定パラメーターを上書きすることもで `tasks` きます。

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

以下の応答が表示され、の設定と内容がわかり `{EXPERIMENT_RUN_ID}``tasks`ます。

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

`{EXPERIMENT_RUN_ID}`:  テスト実行を表すID。\
`{EXPERIMENT_ID}`: テストの実行が含まれるテストを表すID。

### テストの実行ステータスの取得

テスト実行のステータスをに照会でき `{EXPERIMENT_RUN_ID}`ます。

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/sensei/experiments/{EXPERIMENT_ID}/runs/{EXPERIMENT_RUN_ID}/status \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key: {API_KEY}'
```

`{EXPERIMENT_ID}`: テストを表すID。\
`{EXPERIMENT_RUN_ID}`: テスト実行を表すID。\
`{ACCESS_TOKEN}`: 認証後に指定された特定のベアラトークン値。\
`{IMS_ORG}`: IMS組織の資格情報が一意のAdobe Experience Platform統合で見つかりました。\
`{API_KEY}`: 独自のAdobe Experience Platform統合に見つかった特定のAPIキー値。

**応答**

GET呼び出しは、次のように、パラメーター内のステータスを `state` 提供します。

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

`{EXPERIMENT_RUN_ID}`:  テスト実行を表すID。\
`{EXPERIMENT_ID}`: テストの実行が含まれるテストを表すID。

状態に加えて、 `DONE` 次の状態もあります。
- `PENDING`
- `RUNNING`
- `FAILED`

詳細を取得するには、詳細なログを `tasklogs` パラメーターの下に表示します。

### トレーニングを受けたモデルの取得

トレーニング中に上記のトレーニングモデルを作成するために、次のリクエストを行います。

**リクエスト**

```Shell
curl -X GET \
  'https://platform.adobe.io/data/sensei/models/?property=experimentRunId=={EXPERIMENT_RUN_ID}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
```

`{EXPERIMENT_RUN_ID}`: ターゲットするテスト実行に対応するID。 これは、テスト実行の作成時の応答に含まれています。\
`{ACCESS_TOKEN}`: 認証後に指定された特定のベアラトークン値。\
`{IMS_ORG}`: IMS組織の資格情報が一意のAdobe Experience Platform統合で見つかりました。

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

`{MODEL_ID}`: モデルに対応するID。\
`{EXPERIMENT_ID}`:  テスト実行のテストに対応するID。\
`{EXPERIMENT_RUN_ID}`: テスト実行に対応するID。

### スケジュールされたテストの停止と削除

スケジュールされたテストの実行をその前に停止したい場合 `endTime`は、 `{EXPERIMENT_ID}`

**リクエスト**

```Shell
curl -X DELETE \
  'https://platform.adobe.io/data/sensei/experiments/{EXPERIMENT_ID}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
```

`{EXPERIMENT_ID}`:  テストに対応するID。\
`{ACCESS_TOKEN}`: 認証後に指定された特定のベアラトークン値。\
`{IMS_ORG}`: IMS組織の資格情報が一意のAdobe Experience Platform統合で見つかりました。

>[!NOTE] API呼び出しは、新しいテスト実行の作成を無効にします。 ただし、既に実行中のテスト実行の実行は停止しません。

次に、テストが正常に削除されたことを通知する応答を示します。

**応答**

```JSON
{
    "title": "Success",
    "status": 200,
    "detail": "Experiment successfully deleted"
}
```

## 次の手順

このチュートリアルでは、APIを使用してエンジン、テスト、予定されたテストの実行、トレーニングを受けたモデルを作成する方法について説明しました。 次の練習 [では](./score-model-api.md)、最もパフォーマンスの高いトレーニングモデルを使用して新しいデータセットをスコアリングし、予測を行います。