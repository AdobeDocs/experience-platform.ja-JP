---
keywords: Experience Platform；モデルのスコア付け；Data Science Workspace；人気のあるトピック；sensei 機械学習 api
solution: Experience Platform
title: Sensei Machine Learning API を使用したモデルのスコア
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、Sensei Machine Learning API を活用して Experiment と Experiment Run を作成する方法を説明します。
exl-id: 202c63b0-86d8-4a82-8ec8-d144a8911d08
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 91%

---

# [!DNL Sensei Machine Learning API] を使用してモデルにスコアを付けます

このチュートリアルでは、API を活用して実験を作成し、実行する方法を示します。API ドキュメントの詳細なリストについては、[このドキュメント](https://www.adobe.io/apis/cloudplatform/dataservices/api-reference.html)を参照してください。

## スコアリングのためのスケジュールされた実験の作成

トレーニングのためのスケジュールされた実験と同様に、スコアリングのためのスケジュールされた実験の作成も、ボディパラメーターに `template` セクションを含めることでおこなわれます。また、本文の `tasks` の下の `name` フィールドは `score` に設定されます。

以下は、`startTime` に開始してから 20 分ごとに実行し、`endTime` まで実行する実験を作成する例です。

**リクエスト**

```Shell
curl -X POST \
  https://platform.adobe.io/data/sensei/experiments \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/vnd.adobe.platform.sensei+json;profile=experiment.v1.json' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key: {API_KEY}' \
  -d '{JSON_PAYLOAD}'
```

`{IMS_ORG}`：固有の Adobe Experience Platform 統合にある IMS 組織の資格情報。\
`{ACCESS_TOKEN}`：認証後に提供される特定の Bearer トークン値。\
`{API_KEY}`：固有の Adobe Experience Platform 統合での特定の API キーの値。\
`{JSON_PAYLOAD}`：送信する実験実行オブジェクト。このチュートリアルで使用する例を次に示します。

```JSON
{
    "name": "Experiment for Retail",
    "mlInstanceId": "{INSTANCE_ID}",
    "template": {
        "tasks": [{
            "name": "score",
            "parameters": [
                {
                    "key": "modelId",
                    "value": "{MODEL_ID}"
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
            "startTime": "2018-07-04",
            "endTime": "2018-07-06"
        }
    }
}
```

`{INSTANCE_ID}`：MLInstance を表す ID。\
`{MODEL_ID}`：トレーニング済みモデルを表す ID。

次に、スケジュールされた実験を作成した後の応答を示します。

**応答**

```JSON
{
  "id": "{EXPERIMENT_ID}",
  "name": "Experiment for Retail",
  "mlInstanceId": "{INSTANCE_ID}",
  "created": "2018-11-11T11:11:11.111Z",
  "updated": "2018-11-11T11:11:11.111Z",
  "template": {
    "tasks": [
      {
        "name": "score",
        "parameters": [...],
        "specification": {
          "type": "SparkTaskSpec",
          "executorCores": 5,
          "numExecutors": 5
        }
      }
    ],
    "schedule": {
      "cron": "*\/20 * * * *",
      "startTime": "2018-07-04",
      "endTime": "2018-07-06"
    }
  }
}
```

`{EXPERIMENT_ID}`：Experiment を表す ID。\
`{INSTANCE_ID}`：MLInstance を表す ID。


### スコアリングのための実験実行の作成

トレーニング済みモデルを使って、スコアリングのための実験実行を作成できます。`modelId` パラメーターの値は、上記の GET モデルリクエストで返される `id` パラメーターです。

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

`{IMS_ORG}`：固有の Adobe Experience Platform 統合にある IMS 組織の資格情報。\
`{ACCESS_TOKEN}`：認証後に提供される特定の Bearer トークン値。\
`{API_KEY}`：固有の Adobe Experience Platform 統合での特定の API キーの値。\
`{EXPERIMENT_ID}`：ターゲットする実験に対応する ID。これは、実験を作成する際の応答に含まれています。\
`{JSON_PAYLOAD}`：投稿するデータ。このチュートリアルで使用する例を次に示します。

```JSON
{
   "mode":"score",
    "tasks": [
        {
            "name": "score",
            "parameters": [
                {
                    "key": "modelId",
                    "value": "{MODEL_ID}"
                }
            ]
        }
    ]
}
```

`{MODEL_ID}`：モデルに対応する ID。

実験実行の作成からの応答を次に示します。

**応答**

```JSON
{
    "id": "{EXPERIMENT_RUN_ID}",
    "mode": "score",
    "experimentId": "{EXPERIMENT_ID}",
    "created": "2018-01-01T11:11:11.011Z",
    "updated": "2018-01-01T11:11:11.011Z",
    "deleted": false,
    "tasks": [
        {
            "name": "score",
            "parameters": [...]
        }
    ]
}
```

`{EXPERIMENT_ID}`：実行する実験に対応する ID。\
`{EXPERIMENT_RUN_ID}`：作成した実験実行に対応する ID。


### スケジュールされた実験実行の実験実行ステータスの取得

スケジュールされた実験の実験実行を取得するためのクエリを次に示します。

**リクエスト**

```Shell
curl -X GET \
  'https://platform.adobe.io/data/sensei/experiments/{EXPERIMENT_ID}/runs' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
```

`{EXPERIMENT_ID}`：実行する実験に対応する ID。\
`{ACCESS_TOKEN}`：認証後に提供される特定の Bearer トークン値。\
`{IMS_ORG}`：固有の Adobe Experience Platform 統合での IMS 組織の資格情報。

特定のテストに対して複数の実験実行があるので、返される応答には実行 ID の配列が含まれます。

**応答**

```JSON
{
    "children": [
        {
            "id": "{EXPERIMENT_RUN_ID}",
            "experimentId": "{EXPERIMENT_ID}",
            "created": "2018-01-01T11:11:11.011Z",
            "updated": "2018-01-01T11:11:11.011Z"
        },
        {
            "id": "{EXPERIMENT_RUN_ID}",
            "experimentId": "{EXPERIMENT_ID}",
            "created": "2018-01-01T11:11:11.011Z",
            "updated": "2018-01-01T11:11:11.011Z"
        }
    ]
}
```

`{EXPERIMENT_RUN_ID}`：実験実行に対応する ID。\
`{EXPERIMENT_ID}`：実行する実験に対応する ID。

### スケジュールされた実験の停止と削除

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
