---
keywords: Experience Platform;Score a model;Data Science Workspace;popular topics
solution: Experience Platform
title: モデルのスコア(API)
topic: Tutorial
translation-type: tm+mt
source-git-commit: 5699022d1f18773c81a0a36d4593393764cb771a

---


# モデルのスコア(API)

このチュートリアルでは、APIを利用してテストとテストの実行を作成する方法を示します。 APIドキュメントの詳細なリストについては、このドキュメントを参照し [てくださ](https://www.adobe.io/apis/cloudplatform/dataservices/api-reference.html)い。

## スコアリング用のスケジュールされたテストの作成

トレーニングの予定実験と同様に、スコアリングのための予定された実験の作成も、bodyパラメーターのセクショ `template` ンを含めることで行います。 また、本文の下 `name` のフィー `tasks` ルドはに設定されます `score`。

次の例は、開始してから20分ごとに実行し、まで実行するテストを作成 `startTime` する場合の例です `endTime`。

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

`{IMS_ORG}`:IMS組織の資格情報が、お使いの一意のAdobe Experience Platform統合で見つかりました。\
`{ACCESS_TOKEN}`:認証後に指定された特定のベアラトークン値。\
`{API_KEY}`:固有のAdobe Experience Platform統合で見つかった特定のAPIキーの値。\
`{JSON_PAYLOAD}`:送信する実行オブジェクトをテストします。 このチュートリアルで使用する例を次に示します。

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

`{INSTANCE_ID}`:MLInstanceを表すIDです。\
`{MODEL_ID}`:トレーニングを受けたモデルを表すID。

次に、予定されたテストを作成した後の応答を示します。

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

`{EXPERIMENT_ID}`:テストを表すID。\
`{INSTANCE_ID}`:MLInstanceを表すIDです。


### スコアリング用のテスト実行の作成

トレーニングを受けたモデルを使って、スコアリング用のテスト実行を作成できます。 パラメータの値は、上 `modelId` 記のGETモデ `id` ルリクエストで返されるパラメータです。

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

`{IMS_ORG}`:IMS組織の資格情報が、お使いの一意のAdobe Experience Platform統合で見つかりました。\
`{ACCESS_TOKEN}`:認証後に指定された特定のベアラトークン値。\
`{API_KEY}`:固有のAdobe Experience Platform統合で見つかった特定のAPIキーの値。\
`{EXPERIMENT_ID}`:ターゲットするテストに対応するID。 これは、テストを作成する際の応答に含まれています。\
`{JSON_PAYLOAD}`:投稿するデータ。 このチュートリアルで使用する例を次に示します。

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

`{MODEL_ID}`:モデルに対応するID。

テスト実行の作成からの応答を次に示します。

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

`{EXPERIMENT_ID}`: 実行するテストに対応するID。\
`{EXPERIMENT_RUN_ID}`:作成したテスト実行に対応するID。


### 予定されたテストの実行のテストの実行ステータスの取得

スケジュールされた実験のテスト実行を取得するには、クエリを次に示します。

**リクエスト**

```Shell
curl -X GET \
  'https://platform.adobe.io/data/sensei/experiments/{EXPERIMENT_ID}/runs' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
```

`{EXPERIMENT_ID}`: 実行するテストに対応するID。\
`{ACCESS_TOKEN}`:認証後に指定された特定のベアラトークン値。\
`{IMS_ORG}`:IMS組織の資格情報が、お使いの一意のAdobe Experience Platform統合で見つかりました。

特定のテストに対して複数のテストの実行があるので、返される応答には実行IDの配列が含まれます。

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

`{EXPERIMENT_RUN_ID}`:テスト実行に対応するID。\
`{EXPERIMENT_ID}`: 実行するテストに対応するID。

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
