---
keywords: Experience Platform;Score a model;Data Science Workspace;popular topics
solution: Experience Platform
title: モデルにスコアを付ける(API)
topic: Tutorial
translation-type: tm+mt
source-git-commit: 5699022d1f18773c81a0a36d4593393764cb771a
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 1%

---


# モデルにスコアを付ける(API)

このチュートリアルでは、APIを利用してテストとテストの実行を作成する方法を示します。 APIドキュメントの詳細なリストについては、 [このドキュメントを参照してください](https://www.adobe.io/apis/cloudplatform/dataservices/api-reference.html)。

## スコアリング用の予定されたテストの作成

スケジュールされたトレーニングのテストと同様に、スコアリングのためのスケジュールされたテストの作成も、bodyパラメーターの `template` セクションを含めることで行います。 また、本文の下の `name` フィールド `tasks` はに設定され `score`ます。

次の例は、開始から20分ごとに実行し、終了まで実行するテスト `startTime` を作成する場合の例で `endTime`す。

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

`{IMS_ORG}`: IMS組織の資格情報が一意のAdobe Experience Platform統合で見つかりました。\
`{ACCESS_TOKEN}`: 認証後に指定された特定のベアラトークン値。\
`{API_KEY}`: 独自のAdobe Experience Platform統合に見つかった特定のAPIキー値。\
`{JSON_PAYLOAD}`: 送信する実行オブジェクトをテストします。 チュートリアルで使用する例を次に示します。

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

`{INSTANCE_ID}`: MLInstanceを表すID。\
`{MODEL_ID}`: トレーニングを受けたモデルを表すID。

以下は、予定されたテストの作成後の応答です。

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

`{EXPERIMENT_ID}`: テストを表すID。\
`{INSTANCE_ID}`: MLInstanceを表すID。


### スコアリング用のテスト実行の作成

トレーニングを受けたモデルを使用して、スコアリング用のテスト実行を作成できます。 パラメーターの値は、 `modelId` 上のGET Modelリクエストで返される `id` パラメーターです。

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

`{IMS_ORG}`: IMS組織の資格情報が一意のAdobe Experience Platform統合で見つかりました。\
`{ACCESS_TOKEN}`: 認証後に指定された特定のベアラトークン値。\
`{API_KEY}`: 独自のAdobe Experience Platform統合に見つかった特定のAPIキー値。\
`{EXPERIMENT_ID}`: ターゲットするテストに対応するID。 これは、テストの作成時の応答に含まれます。\
`{JSON_PAYLOAD}`: 投稿するデータ。 チュートリアルで使用する例は次のとおりです。

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

`{MODEL_ID}`: モデルに対応するID。

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

`{EXPERIMENT_ID}`:  実行するテストに対応するID。\
`{EXPERIMENT_RUN_ID}`: 先ほど作成したテスト実行に対応するID。


### 予定されているテスト実行のテスト実行ステータスの取得

スケジュール設定した実験のテストを実行するには、クエリを次に示します。

**リクエスト**

```Shell
curl -X GET \
  'https://platform.adobe.io/data/sensei/experiments/{EXPERIMENT_ID}/runs' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
```

`{EXPERIMENT_ID}`:  実行するテストに対応するID。\
`{ACCESS_TOKEN}`: 認証後に指定された特定のベアラトークン値。\
`{IMS_ORG}`: IMS組織の資格情報が一意のAdobe Experience Platform統合で見つかりました。

特定のテストに対して複数のテスト実行があるので、返される応答には実行IDの配列が含まれます。

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

`{EXPERIMENT_RUN_ID}`: テスト実行に対応するID。\
`{EXPERIMENT_ID}`:  実行するテストに対応するID。

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
