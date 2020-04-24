---
keywords: Experience Platform;developer guide;endpoint;Data Science Workspace;popular topics
solution: Experience Platform
title: サービス
topic: Developer guide
translation-type: tm+mt
source-git-commit: 19823c7cf0459e045366f0baae2bd8a98416154c

---


# MLServices

MLServiceは、事前に開発されたモデルにアクセスして再利用する機能を組織に提供する、公開されたトレーニングを受けたモデルです。 MLServicesの主な特徴は、トレーニングとスコアリングをスケジュールに基づいて自動化する機能です。 スケジュールされたトレーニングの実行は、モデルの効率と正確性を維持するのに役立ちます。また、スケジュールされたスコアリングの実行は、新しいインサイトを一貫して生成するために役立ちます。

自動トレーニングおよびスコアリングスケジュールは、開始タイムスタンプ、終了タイムスタンプおよび頻度をCron式として <a href="https://en.wikipedia.org/wiki/Cron" target="_blank">定義しま</a>す。 スケジュールは、MLServiceの作成時 [に定義するか](#create-an-mlservice) 、既存のMLServiceを更新するこ [とによって適用できます](#update-an-mlservice)。

## MLServiceの作成 {#create-an-mlservice}

MLServiceは、POSTリクエストと、サービスの名前と有効なMLInstance IDを提供するペイロードを実行することで作成できます。 MLServiceの作成に使用するMLInstanceは、既存のトレーニング実験を持つ必要はありませんが、対応するテストIDとトレーニング実行IDを指定することで、既存のトレーニングモデルを使用してMLServiceを作成することもできます。

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
| `name` | MLServiceに必要な名前。 このMLServiceに対応するサービスは、この値を継承し、サービスの名前としてService Gallery UIに表示されます。 |
| `description` | MLServiceのオプションの説明。 このMLServiceに対応するサービスは、この値を継承し、サービスの説明としてサービスギャラリーUIに表示されます。 |
| `mlInstanceId` | 有効なMLInstance IDです。 |
| `trainingDataSetId` | トレーニングデータセットIDを指定すると、MLInstanceのデフォルトのデータセットIDが上書きされます。 MLServiceの作成に使用するMLInstanceでトレーニングデータセットが定義されていない場合は、適切なトレーニングデータセットIDを指定する必要があります。 |
| `trainingExperimentId` | オプションで指定できるテストID。 この値を指定しない場合、MLServiceを作成すると、MLInstanceのデフォルト設定を使用して新しいテストも作成されます。 |
| `trainingExperimentRunId` | オプションで指定できるトレーニング実行ID。 この値を指定しない場合、MLServiceを作成すると、MLInstanceのデフォルトのトレーニングパラメータを使用して、トレーニングの実行も作成および実行されます。 |
| `trainingSchedule` | 自動トレーニングの実行スケジュール。 このプロパティを定義すると、MLServiceは、スケジュールに基づいて自動的にトレーニングを実行します。 |
| `trainingSchedule.startTime` | スケジュールされたトレーニングの実行が開始されるタイムスタンプ。 |
| `trainingSchedule.endTime` | スケジュールされたトレーニングの実行が終了するタイムスタンプ。 |
| `trainingSchedule.cron` | 自動トレーニング式の頻度を定義するcronイベント。 |
| `scoringSchedule` | 自動スコアリング実行のスケジュール。 このプロパティを定義すると、MLServiceはスケジュールに基づいて自動的にスコアリングの実行を実行します。 |
| `scoringSchedule.startTime` | スケジュールされたスコアの実行が開始されるタイムスタンプ。 |
| `scoringSchedule.endTime` | スケジュールされたスコアの実行が終了するタイムスタンプ。 |
| `scoringSchedule.cron` | 自動スコア式の頻度を定義するcronパラメーター。 |

**応答**

成功した応答は、新たに作成されたMLServiceの詳細を含むペイロードを返します。この詳細には、一意の識別子(`id`)、トレーニング用のテストID(`trainingExperimentId`)、スコアリング用のテストID(`scoringExperimentId`)、入力トレーニングデータセットID(`trainingDataSetId`)が含まれます。

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

MLServicesのリストは、1回のGET要求を実行することで取得できます。 結果のフィルタリングに役立つように、リクエストパスでクエリパラメーターを指定できます。 使用可能なクエリのリストについては、アセット取得のための [クエリパラメータの付録の節を参照してください](./appendix.md#query)。

**API形式**

```http
GET /mlServices
GET /mlServices?{QUERY_PARAMETER}={VALUE}
GET /mlServices?{QUERY_PARAMETER_1}={VALUE_1}&{QUERY_PARAMETER_2}={VALUE_2}
```

| パラメーター | 説明 |
| --- | --- |
| `{QUERY_PARAMETER}` | 結果のフィルターに使用 [するクエリ](./appendix.md#query) ・パラメータの1つ。 |
| `{VALUE}` | 前のパラメーターのクエリ値。 |

**リクエスト**

次のリクエストは、クエリを含み、同じMLInstance ID(`{MLINSTANCE_ID}`)を共有するMLServicesのリストを取得します。

```shell
curl -X GET \
    'https://platform.adobe.io/data/sensei/mlServices?property=mlInstanceId=={MLINSTANCE_ID}' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、MLServicesとその詳細(MLService ID(`{MLSERVICE_ID}`)、トレーニングのテストID(`{TRAINING_ID}`)、スコアリングのテストID(`{SCORING_ID}`)、入力トレーニングデータセットID(`{DATASET_ID}`)など)のリストを返します。

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

リクエストパスに目的のMLServiceのIDを含むGETリクエストを実行することで、特定のテストの詳細を取得できます。

**API形式**

```http
GET /mlServices/{MLSERVICE_ID}
```

* `{MLSERVICE_ID}`:有効なMLService ID。

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

既存のMLServiceを更新するには、リクエストパスにターゲットMLServiceのIDを含むPUT要求を使用してプロパティを上書きし、更新されたプロパティを含むJSONペイロードを提供します。

>[!TIP] このPUTリクエストを確実に成功させるために、まずGETリクエストを実行し、IDでMLServiceを取得する [ことをお勧めします](#retrieve-a-specific-mlservice)。 次に、返されたJSONオブジェクトを変更および更新し、変更されたJSONオブジェクトの全体をPUT要求のペイロードとして適用します。

**API形式**

```http
PUT /mlServices/{MLSERVICE_ID}
```

* `{MLSERVICE_ID}`:有効なMLService ID。

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

成功した応答は、MLServiceの更新された詳細を含むペイロードを返します。

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

## MLInstance IDによるMLServiceの削除

MLInstance IDをクエリパラメータとして指定するDELETEリクエストを実行することで、特定のMLInstanceに属するすべてのMLServiceを削除できます。

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
