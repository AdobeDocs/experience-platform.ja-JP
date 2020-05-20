---
keywords: Experience Platform;developer guide;endpoint;Data Science Workspace;popular topics
solution: Experience Platform
title: サービス
topic: Developer guide
translation-type: tm+mt
source-git-commit: 19823c7cf0459e045366f0baae2bd8a98416154c
workflow-type: tm+mt
source-wordcount: '811'
ht-degree: 2%

---


# MLServices

MLServiceは、事前に開発されたモデルにアクセスして再利用する機能を組織に提供する、トレーニングを受けた公開モデルです。 MLServicesの主な特徴は、トレーニングとスコアリングをスケジュールに基づいて自動化する機能です。 予定されたトレーニングの実行は、モデルの効率と正確性を維持するのに役立ちますが、予定されたスコアリングの実行は、新しいインサイトが一貫して生成されるようにします。

自動トレーニングおよびスコアリングスケジュールは、開始タイムスタンプ、終了タイムスタンプおよび <a href="https://en.wikipedia.org/wiki/Cron" target="_blank">cron式として表される頻度で定義されます</a>。 スケジュールは、MLServiceを [作成する際に、または既存のMLServiceを](#create-an-mlservice) 更新することによって適用する際に定義できます [](#update-an-mlservice)。

## MLServiceの作成 {#create-an-mlservice}

MLServiceは、POST要求と、サービスの名前と有効なMLInstance IDを提供するペイロードを実行して作成できます。 MLServiceの作成に使用するMLInstanceは、既存のトレーニング実験を行う必要はありませんが、対応するテストIDとトレーニング実行IDを指定することで、既存のトレーニングモデルを使用してMLServiceを作成できます。

**API形式**

```http
POST /mlServices
```

**リクエスト**

```shell
curl -X POST \
    https://platform.adobe.io/data/sensei/mlServices \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'content-type: application/vnd.adobe.platform.sensei+json; profile=mlService.v1.json' \
    -d '{
        "name": "A name for this MLService",
        "description": "A description for this MLService",
        "mlInstanceId": "{MLINSTANCE_ID}",
        "trainingDataSetId": "{DATASET_ID}",
        "trainingExperimentId": "{TRAINING_ID}",
        "trainingExperimentRunId": "{RUN_ID}",
        "trainingSchedule": {
            "startTime": "2019-01-01T00:00",
            "endTime": "2019-12-31T00:00",
            "cron": "20 * * * *"
        },
        "scoringSchedule": {
            "startTime": "2019-01-01T00:00",
            "endTime": "2019-12-31T00:00",
            "cron": "20 * * * *"
        }
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | MLServiceに必要な名前。 このMLServiceに対応するサービスは、この値を継承し、サービスの名前としてサービスギャラリーUIに表示されます。 |
| `description` | MLServiceのオプションの説明。 このMLServiceに対応するサービスは、サービスの説明として[サービスギャラリー] UIに表示されるこの値を継承します。 |
| `mlInstanceId` | 有効なMLInstance IDです。 |
| `trainingDataSetId` | トレーニングデータセットIDを指定すると、MLInstanceのデフォルトのデータセットIDが上書きされます。 MLServiceの作成に使用されるMLInstanceでトレーニングデータセットが定義されていない場合は、適切なトレーニングデータセットIDを指定する必要があります。 |
| `trainingExperimentId` | テストID。オプションで指定できます。 この値を指定しない場合、MLServiceを作成すると、MLInstanceのデフォルト設定を使用して新しいテストも作成されます。 |
| `trainingExperimentRunId` | オプションで指定できるトレーニング実行ID。 この値を指定しない場合、MLServiceを作成すると、MLInstanceのデフォルトのトレーニングパラメータを使用して、トレーニングの実行も作成および実行されます。 |
| `trainingSchedule` | 自動トレーニング実行のスケジュール。 このプロパティを定義すると、MLServiceは、スケジュールに基づいて自動的にトレーニングを実行します。 |
| `trainingSchedule.startTime` | スケジュールされたトレーニングの実行が開始されるタイムスタンプ。 |
| `trainingSchedule.endTime` | スケジュールされたトレーニングの実行が終了するタイムスタンプ。 |
| `trainingSchedule.cron` | 自動トレーニングの実行頻度を定義するcron式。 |
| `scoringSchedule` | 自動スコアリング実行のスケジュール。 このプロパティを定義すると、MLServiceはスケジュールに基づいて自動的にスコアリングの実行を実行します。 |
| `scoringSchedule.startTime` | スケジュール済みスコアの実行が開始されるタイムスタンプ。 |
| `scoringSchedule.endTime` | スケジュールされたスコアリングの実行が終了するタイムスタンプ。 |
| `scoringSchedule.cron` | 自動スコアリング実行の頻度を定義するcron式。 |

**応答**

成功した応答は、新たに作成されたMLServiceの詳細を含むペイロードを返します。このペイロードには、一意の識別子(`id`)、トレーニングのテストID(`trainingExperimentId`)、スコアリングのテストID(`scoringExperimentId`)、入力トレーニングデータセットID(`trainingDataSetId`)が含まれます。

```json
{
    "id": "{MLSERVICE_ID}",
    "name": "A name for this MLService",
    "description": "A description for this MLService",
    "mlInstanceId": "{MLINSTANCE_ID}",
    "trainingExperimentId": "{TRAINING_ID}",
    "trainingDataSetId": "{DATASET_ID}",
    "scoringExperimentId": "{SCORING_ID}",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "userId": "Jane_Doe@AdobeID"
    },
    "trainingSchedule": {
        "startTime": "2019-01-01T00:00",
        "endTime": "2019-12-31T00:00",
        "cron": "20 * * * *"
    },
    "scoringSchedule": {
        "startTime": "2019-01-01T00:00",
        "endTime": "2019-12-31T00:00",
        "cron": "20 * * * *"
    },
    "updated": "2019-01-01T00:00:00.000Z"
}
```

## MLServicesのリストの取得 {#retrieve-a-list-of-mlservices}

MLServicesのリストは、1つのGET要求を実行することで取得できます。 結果をフィルターするのに役立つように、リクエストパスでクエリパラメーターを指定できます。 使用可能なクエリのリストについては、付録の「アセット取得の [クエリパラメータ」の節を参照してください](./appendix.md#query)。

**API形式**

```http
GET /mlServices
GET /mlServices?{QUERY_PARAMETER}={VALUE}
GET /mlServices?{QUERY_PARAMETER_1}={VALUE_1}&{QUERY_PARAMETER_2}={VALUE_2}
```

| パラメーター | 説明 |
| --- | --- |
| `{QUERY_PARAMETER}` | 結果のフィルタリングに [使用できるクエリパラメーターの1つ](./appendix.md#query) 。 |
| `{VALUE}` | 前のクエリパラメーターの値。 |

**リクエスト**

次の要求は、クエリを含み、同じMLInstance ID(`{MLINSTANCE_ID}`)を共有するMLServicesのリストを取得します。

```shell
curl -X GET \
    'https://platform.adobe.io/data/sensei/mlServices?property=mlInstanceId=={MLINSTANCE_ID}' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、MLServicesとその詳細(MLService ID (`{MLSERVICE_ID}`)、トレーニングのテストID (`{TRAINING_ID}`)、スコアリングのテストID (`{SCORING_ID}`)、入力トレーニングデータセットID (`{DATASET_ID}`)など)のリストを返します。

```json
{
    "children": [
        {
            "id": "{MLSERVICE_ID}",
            "name": "A service created in UI",
            "mlInstanceId": "{MLINSTANCE_ID}",
            "trainingExperimentId": "{TRAINING_ID}",
            "trainingDataSetId": "{DATASET_ID}",
            "scoringExperimentId": "{SCORING_ID}",
            "created": "2019-01-01T00:00:00.000Z",
            "createdBy": {
                "displayName": "Jane Doe",
                "userId": "Jane_Doe@AdobeID"
            },
            "updated": "2019-01-01T00:00:00.000Z"
        }
    ],
    "_page": {
        "property": "mlInstanceId=={MLINSTANCE_ID},deleted==false",
        "count": 1
    }
}
```

## 特定のMLServiceの取得 {#retrieve-a-specific-mlservice}

リクエストパスに目的のMLServiceのIDを含むGETリクエストを実行すると、特定のテストの詳細を取得できます。

**API形式**

```http
GET /mlServices/{MLSERVICE_ID}
```

* `{MLSERVICE_ID}`: 有効なMLService ID。

**リクエスト**

```shell
curl -X GET \
    https://platform.adobe.io/data/sensei/mlServices/{MLSERVICE_ID} \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、要求されたMLServiceの詳細を含むペイロードを返します。

```json
{
    "id": "{MLSERVICE_ID}",
    "name": "A name for this MLService",
    "description": "A description for this MLService",
    "mlInstanceId": "{MLINSTANCE_ID}",
    "trainingExperimentId": "{TRAINING_ID}",
    "trainingDataSetId": "{DATASET_ID}",
    "scoringExperimentId": "{SCORING_ID}",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "userId": "Jane_Doe@AdobeID"
    },
    "updated": "2019-01-01T00:00:00.000Z"
}
```

## MLServiceの更新 {#update-an-mlservice}

既存のMLServiceを更新するには、リクエストパスにターゲットMLServiceのIDが含まれるPUT要求を介してプロパティを上書きし、更新されたプロパティを含むJSONペイロードを提供します。

>[!TIP] このPUT要求を確実に行うためには、まずGET要求を実行し、ID別にMLServiceを [取得することをお勧めします](#retrieve-a-specific-mlservice)。 次に、返されたJSONオブジェクトを変更および更新し、変更されたJSONオブジェクト全体をPUT要求のペイロードとして適用します。

**API形式**

```http
PUT /mlServices/{MLSERVICE_ID}
```

* `{MLSERVICE_ID}`: 有効なMLService ID。

**リクエスト**

```shell
curl -X PUT \
    https://platform.adobe.io/data/sensei/mlServices/{MLSERVICE_ID} \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'content-type: application/vnd.adobe.platform.sensei+json; profile=mlService.v1.json' \
    -d '{
        "name": "A name for this MLService",
        "description": "A description for this MLService",
        "mlInstanceId": "{MLINSTANCE_ID}",
        "trainingExperimentId": "{TRAINING_ID}",
        "trainingDataSetId": "{DATASET_ID}",
        "scoringExperimentId": "{SCORING_ID}",
        "trainingSchedule": {
            "startTime": "2019-01-01T00:00",
            "endTime": "2019-12-31T00:00",
            "cron": "20 * * * *"
        },
        "scoringSchedule": {
            "startTime": "2019-01-01T00:00",
            "endTime": "2019-12-31T00:00",
            "cron": "20 * * * *"
        }
    }'
```

**応答**

正常な応答は、MLServiceの更新された詳細を含むペイロードを返します。

```json
{
    "id": "{MLSERVICE_ID}",
    "name": "A name for this MLService",
    "description": "A description for this MLService",
    "mlInstanceId": "{MLINSTANCE_ID}",
    "trainingExperimentId": "{TRAINING_ID}",
    "trainingDataSetId": "{DATASET_ID}",
    "scoringExperimentId": "{SCORING_ID}",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "userId": "Jane_Doe@AdobeID"
    },
    "trainingSchedule": {
        "startTime": "2019-01-01T00:00",
        "endTime": "2019-12-31T00:00",
        "cron": "20 * * * *"
    },
    "scoringSchedule": {
        "startTime": "2019-01-01T00:00",
        "endTime": "2019-12-31T00:00",
        "cron": "20 * * * *"
    },
    "updated": "2019-01-02T00:00:00.000Z"
}
```

## MLServiceの削除

リクエストパスにターゲットMLServiceのIDを含むDELETEリクエストを実行すると、1つのMLServiceを削除できます。

**API形式**

```http
DELETE /mlServices/{MLSERVICE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MLSERVICE_ID}` | 有効なMLService ID。 |

**リクエスト**

```shell
curl -X DELETE \
    https://platform.adobe.io/data/sensei/mlServices/{MLSERVICE_ID} \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

```json
{
    "title": "Success",
    "status": 200,
    "detail": "MLService deletion was successful"
}
```

## MLInstance IDによるMLServicesの削除

MLInstance IDをクエリパラメータとして指定するDELETE要求を実行すると、特定のMLInstanceに属するすべてのMLServicesを削除できます。

**API形式**

```http
DELETE /mlServices?mlInstanceId={MLINSTANCE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MLSERVICE_ID}` | 有効なMLService ID。 |

**リクエスト**

```shell
curl -X DELETE \
    https://platform.adobe.io/data/sensei/mlServices?mlInstanceId={MLINSTANCE_ID} \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

```json
{
    "title": "Success",
    "status": 200,
    "detail": "MLServices deletion was successful"
}
```
