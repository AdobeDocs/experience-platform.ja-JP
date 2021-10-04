---
keywords: Experience Platform；トレーニングと評価；Data Science Workspace；よく読まれるトピック；Sensei 機械学習 API
solution: Experience Platform
title: Sensei 機械学習 API を使用したモデルのトレーニングと評価
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、Sensei 機械学習 API 呼び出しを使用して、モデルの作成、トレーニング、評価をおこなう方法について説明します。
exl-id: 8107221f-184c-426c-a33e-0ef55ed7796e
source-git-commit: 441d7822f287fabf1b06cdf3f6982f9c910387a8
workflow-type: tm+mt
source-wordcount: '1235'
ht-degree: 92%

---

# [!DNL Sensei Machine Learning] API を使用したモデルのトレーニングと評価


このチュートリアルでは、API 呼び出しを使用してモデルを作成、トレーニング、評価する方法を示します。API ドキュメントの詳しいリストについては、[こちらのドキュメント](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml)を参照してください。

## 前提条件

API を使用したモデルのトレーニングと評価に必要なエンジンを作成するには、[API を使用してパッケージ化されたレシピをインポート](./import-packaged-recipe-api.md)します。

[Experience PlatformAPI 認証に関するチュートリアル ](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis) に従って、API 呼び出しの開始を行います。

このチュートリアルから、次の値を入手できます。

- `{ACCESS_TOKEN}`：認証後に提供される特定の Bearer トークン値。
- `{IMS_ORG}`：固有の Adobe Experience Platform 統合にある IMS 組織の資格情報。
- `{API_KEY}`：固有の Adobe Experience Platform 統合にある特定の API キー値。

- インテリジェントサービスの Docker イメージへのリンク

## API ワークフロー

この API を使用して、トレーニング用の Experiment Run を作成します。このチュートリアルでは、エンジン、MLInstance および Experiment の各エンドポイントに重点を置いています。次の図に、これら 3 つのエンドポイントの関係を示し、Run とモデルの概念を示します。

![](../images/models-recipes/train-evaluate-api/engine_hierarchy_api.png)

>[!NOTE]
>
>「エンジン」、「MLInstance」、「MLService」、「Experiment」、「モデル」という用語は、UI では別の用語になります。UI から使用する場合、次の表に違いを示します。

| UI 用語 | API 用語 |
| --- | --- |
| レシピ | エンジン |
| モデル | MLInstance |
| トレーニングの実行 | Experiment |
| サービス | MLService |

### MLInstance の作成

MLInstance を作成するには、次のリクエストを使用します。[API を使用してパッケージ化されたレシピをインポートする](./import-packaged-recipe-ui.md)チュートリアルで、エンジンを作成した際に返された `{ENGINE_ID}` を使用します。

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

`{ACCESS_TOKEN}`：認証後に提供される特定の Bearer トークン値。\
`{IMS_ORG}`：固有の Adobe Experience Platform 統合にある IMS 組織の資格情報。\
`{API_KEY}`：固有の Adobe Experience Platform 統合にある特定の API キー値。\
`{JSON_PAYLOAD}`：MLInstance の設定。次に、このチュートリアルで使用する例を示します。

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

>[!NOTE]
>
>`{JSON_PAYLOAD}` では、トレーニングとスコアリングに使用するパラメーターを `tasks` 配列で定義します。`{ENGINE_ID}` は使用するエンジンの ID で、`tag` フィールドはインスタンスの識別に使用するオプションのパラメーターです。

応答には、作成された MLInstance を表す `{INSTANCE_ID}` が含まれます。 設定が異なる複数のモデル MLInstance を作成できます。

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

`{ENGINE_ID}`：MLInstance が作成されるエンジンを表す ID。\
`{INSTANCE_ID}`：MLInstance を表す ID。

### Experiment の作成

データサイエンティストは、トレーニング中に高性能モデルに到達するために、Experiment を使用します。各 Experiment では、データセット、機能、学習パラメーター、ハードウェアが変更されます。次に、Experiment の作成例を示します。

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

`{IMS_ORG}`：固有の Adobe Experience Platform 統合にある IMS 組織の資格情報。\
`{ACCESS_TOKEN}`：認証後に提供される特定の Bearer トークン値。\
`{API_KEY}`：固有の Adobe Experience Platform 統合にある特定の API キー値。\
`{JSON_PAYLOAD}`：作成する Experiment オブジェクト。次に、このチュートリアルで使用する例を示します。

```JSON
{
    "name": "Experiment for Retail ",
    "mlInstanceId": "{INSTANCE_ID}",
    "tags": {
        "test": "guide"
    }
}
```

`{INSTANCE_ID}`：MLInstance を表す ID。

Experiment の作成リクエストを実行すると、次のようなレスポンスが返されます。

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

`{EXPERIMENT_ID}`：作成した Experiment を表す ID。`{INSTANCE_ID}`：MLInstance を表す ID。

### スケジュールに沿ったトレーニング Experimentの作成

スケジュールに沿った Experiment を使用すると、Experiment Run を作成するたびに、毎回 API 呼び出しを実行する必要がなくなります。Experiment の作成に必要なパラメーターはすべて用意されており、各 Run は定期的に作成されます。

スケジュールに沿った Experiment の作成を指定するには、リクエストの本文に `template` セクションを追加する必要があります。スケジュールに沿った Run に必要なパラメーターは、すべて `template` に含まれています。たとえば、実行するアクションを示す `tasks`、スケジュールに沿った Run の実行予定を示す `schedule` などです。

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

`{IMS_ORG}`：固有の Adobe Experience Platform 統合にある IMS 組織の資格情報。\
`{ACCESS_TOKEN}`：認証後に提供される特定の Bearer トークン値。\
`{API_KEY}`：固有の Adobe Experience Platform 統合にある特定の API キー値。\
`{JSON_PAYLOAD}`：使用するデータセット。次に、このチュートリアルで使用する例を示します。

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

Experiment を作成する際、本文の `{JSON_PAYLOAD}` には `mlInstanceId` パラメーターまたは `mlInstanceQuery` パラメーターを含める必要があります。この例では、スケジュールに沿った Experiment により、`startTime` から `endTime` までの間、`cron` パラメーターに設定されたとおり 20 分ごとに Run が呼び出されます。

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

`{EXPERIMENT_ID}`：Experiment を表す ID。\
`{INSTANCE_ID}`：MLInstance を表す ID。


### トレーニング用の Experiment Run の作成

Experiment エンティティを作成したら、以下の呼び出しを使用して、トレーニング Run を作成および実行できます。リクエスト本文で、`{EXPERIMENT_ID}` と、トリガーする `mode` を指定する必要があります。

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

`{EXPERIMENT_ID}`：目的の Experiment に対応する ID。これは、Experiment を作成した際のレスポンスに含まれています。\
`{IMS_ORG}`：固有の Adobe Experience Platform 統合にある IMS 組織の資格情報。\
`{ACCESS_TOKEN}`：認証後に提供される特定の Bearer トークン値。\
`{API_KEY}`：固有の Adobe Experience Platform 統合にある特定の API キー値。\
`{JSON_PAYLOAD}`：トレーニング Run を作成するには、本文に次の内容を含める必要があります。

```JSON
{
    "mode":"Train"
}
```

また、`tasks` 配列を含めることで、設定パラメーターを上書きすることもできます。

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

次のレスポンスが返され、`{EXPERIMENT_RUN_ID}` と設定（`tasks` の下）が示されます。

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

`{EXPERIMENT_RUN_ID}`：Experiment Run を表す ID。\
`{EXPERIMENT_ID}`：Experiment Run が含まれる Experiment を表す ID。

### Experiment Run のステータスの取得

Experiment Run のステータスを照会するには、`{EXPERIMENT_RUN_ID}` を使用します。

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/sensei/experiments/{EXPERIMENT_ID}/runs/{EXPERIMENT_RUN_ID}/status \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key: {API_KEY}'
```

`{EXPERIMENT_ID}`：Experiment を表す ID。\
`{EXPERIMENT_RUN_ID}`：Experiment Run を表す ID。\
`{ACCESS_TOKEN}`：認証後に提供される特定の Bearer トークン値。\
`{IMS_ORG}`：固有の Adobe Experience Platform 統合にある IMS 組織の資格情報。\
`{API_KEY}`：固有の Adobe Experience Platform 統合にある特定の API キー値。

**応答** 

GET 呼び出しを実行すると、次に示すように、`state` にス テータスが示されます。

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

`{EXPERIMENT_RUN_ID}`：Experiment Run を表す ID。\
`{EXPERIMENT_ID}`：Experiment Run が含まれる Experiment を表す ID。

`DONE` の状態意外に、次の状態も示されます。
- `PENDING`
- `RUNNING`
- `FAILED`

詳細については、`tasklogs` パラメーターの下の詳細ログを確認してください。

### トレーニング済みモデルの取得

トレーニング中に、前述の手順で作成したトレーニング済みモデルを取得するには、次のリクエストを実行します。

**リクエスト**

```Shell
curl -X GET \
  'https://platform.adobe.io/data/sensei/models/?property=experimentRunId=={EXPERIMENT_RUN_ID}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
```

`{EXPERIMENT_RUN_ID}`：目的の Experiment Run に対応する ID。これは、Experiment Run を作成した際のレスポンスに含まれています。\
`{ACCESS_TOKEN}`：認証後に提供される特定の Bearer トークン値。\
`{IMS_ORG}`：固有の Adobe Experience Platform 統合にある IMS 組織の資格情報。

レスポンスは、作成されたトレーニング済みモデルを表します。

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

`{MODEL_ID}`：モデルに対応する ID。\
`{EXPERIMENT_ID}`：Experiment Run が含まれる Experiment に対応する ID。\
`{EXPERIMENT_RUN_ID}`：Experiment Run に対応する ID。

### スケジュールに沿った Experiment の停止と削除

`endTime` よりも前に、スケジュールに沿った Experiment の実行を停止する場合は、`{EXPERIMENT_ID}` に DELETE リクエストを実行します。

**リクエスト**

```Shell
curl -X DELETE \
  'https://platform.adobe.io/data/sensei/experiments/{EXPERIMENT_ID}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
```

`{EXPERIMENT_ID}`：Experiment に対応する ID。\
`{ACCESS_TOKEN}`：認証後に提供される特定の Bearer トークン値。\
`{IMS_ORG}`：固有の Adobe Experience Platform 統合にある IMS 組織の資格情報。

>[!NOTE]
>
>API 呼び出しを実行すると、新しい Experiment Run の作成が無効になります。ただし、既に実行中の Experiment Run は停止されません。

次に、Experiment が正常に削除されたことを通知するレスポンスを示します。

**応答** 

```JSON
{
    "title": "Success",
    "status": 200,
    "detail": "Experiment successfully deleted"
}
```

## 次の手順

このチュートリアルでは、API を使用してエンジン、Experiment、スケジュールに沿った Experiment Run およびトレーニング済みモデルを作成する方法について説明しました。次の[演習](./score-model-api.md)では、最もパフォーマンスの高いトレーニング済みモデルを使用して新しいデータセットをスコアリングし、予測を行います。
