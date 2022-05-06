---
keywords: Experience Platform；開発者ガイド；エンドポイント；Data Science Workspace；人気の高いトピック；mlservices;sensei 機械学習 api
solution: Experience Platform
title: MLServices API エンドポイント
topic-legacy: Developer guide
description: MLService は、組織が開発済みのモデルにアクセスし再利用できるようにするための、公開されているトレーニング済みモデルです。MLService の主要な特長は、トレーニングとスコアリングをスケジュールに従って自動化できる点です。スケジュールされたトレーニングの実行は、モデルの効率と精度を維持するのに役立ちます。また、スケジュールされたスコアリングの実行で、新しいインサイトを一貫して生成できるようになります。
exl-id: cd236e0b-3bfc-4d37-83eb-432f6ad5c5b6
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '890'
ht-degree: 97%

---

# MLServices エンドポイント

MLService は、組織が開発済みのモデルにアクセスし再利用できるようにするための、公開されているトレーニング済みモデルです。MLService の主要な特長は、トレーニングとスコアリングをスケジュールに従って自動化できる点です。スケジュールされたトレーニングの実行は、モデルの効率と精度を維持するのに役立ちます。また、スケジュールされたスコアリングの実行で、新しいインサイトを一貫して生成できるようになります。

自動トレーニングおよびスコアリングのスケジュールは、開始タイムスタンプ、終了タイムスタンプおよび頻度を使用して [cron 式](https://jp.wikipedia.org/wiki/Cron)として定義します。スケジュールは、[MLService の作成](#create-an-mlservice)時に定義できますし、[既存の MLService の更新](#update-an-mlservice)で適用することもできます。

## MLService の作成 {#create-an-mlservice}

MLService を作成するには、サービスの名前と有効な MLInstance ID を指定するペイロードを含んだ POST リクエストを実行します。MLService の作成に使用する MLInstance には、既存のトレーニング実験は必要ありませんが、対応する実験 ID とトレーニング実行 ID を指定することで、既存のトレーニング済みモデルを使用して MLService を作成することもできます。

**API 形式**

```http
POST /mlServices
```

**リクエスト**

```shell
curl -X POST \
    https://platform.adobe.io/data/sensei/mlServices \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'content-type: application/vnd.adobe.platform.sensei+json; profile=mlService.v1.json' \
    -d '{
        "name": "A name for this MLService",
        "description": "A description for this MLService",
        "mlInstanceId": "46986c8f-7739-4376-8509-0178bdf32cda",
        "trainingDataSetId": "5ee3cd7f2d34011913c56941",
        "trainingExperimentId": "014d8acf-08fb-421c-8b65-760c8799c627",
        "trainingExperimentRunId": "33408593-2871-4198-a812-6d1b7d939cda",
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
| `name` | 希望する MLService 名です。この MLService に対応するサービスはこの値を継承するので、サービスギャラリー UI にこの値がサービスの名前として表示されます。 |
| `description` | MLService の説明です（省略可能）。この MLService に対応するサービスはこの値を継承するので、サービスギャラリー UI にこの値がサービスの説明として表示されます。 |
| `mlInstanceId` | 有効な MLInstance ID です。 |
| `trainingDataSetId` | トレーニングデータセット ID で、これを指定すると、MLInstance のデフォルトのデータセット ID が上書きされます。MLService の作成に使用する MLInstance にトレーニングデータセットが定義されていない場合は、適切なトレーニングデータセット ID を指定する必要があります。 |
| `trainingExperimentId` | 任意で指定可能な実験 ID です。この値を指定しない場合は、MLService を作成すると、MLInstance のデフォルト設定を使用して新しい実験も作成されます。 |
| `trainingExperimentRunId` | 任意で指定可能なトレーニング実行 ID です。この値を指定しない場合は、MLService を作成すると、MLInstance のデフォルトのトレーニングパラメーターを使用して、トレーニング実行も作成および実行されます。 |
| `trainingSchedule` | 自動トレーニング実行スケジュールです。このプロパティを定義すると、MLService はスケジュールに従って自動的にトレーニングを実行します。 |
| `trainingSchedule.startTime` | スケジュールされたトレーニング実行の開始時点を示すタイムスタンプです。 |
| `trainingSchedule.endTime` | スケジュールされたトレーニング実行の終了時点を示すタイムスタンプです。 |
| `trainingSchedule.cron` | 自動トレーニング実行の頻度を定義する cron 式です。 |
| `scoringSchedule` | 自動スコアリング実行のスケジュールです。このプロパティを定義すると、MLService はスケジュールに従って自動的にスコアリングを実行します。 |
| `scoringSchedule.startTime` | スケジュールされたスコアリング実行の開始時点を示すタイムスタンプです。 |
| `scoringSchedule.endTime` | スケジュールされたスコアリング実行の終了時点を示すタイムスタンプです。 |
| `scoringSchedule.cron` | 自動スコアリング実行の頻度を定義する cron 式です。 |

**応答**

成功時の応答は、新規作成された MLService の詳細を格納したペイロードを返します。この詳細には、一意の ID（`id`）、トレーニングの実験 ID（`trainingExperimentId`）、スコアリングの実験 ID（`scoringExperimentId`）、入力トレーニングデータセット ID（`trainingDataSetId`）などが含まれます。

```json
{
    "id": "68d936d8-17e6-44ef-a4b6-c7502055638b",
    "name": "A name for this MLService",
    "description": "A description for this MLService",
    "mlInstanceId": "46986c8f-7739-4376-8509-0178bdf32cda",
    "trainingExperimentId": "014d8acf-08fb-421c-8b65-760c8799c627",
    "trainingDataSetId": "5ee3cd7f2d34011913c56941",
    "scoringExperimentId": "76c2b1b-fad7-4b31-8c54-19ecc18b1ea0",
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

## MLService のリストの取得 {#retrieve-a-list-of-mlservices}

MLService のリストを取得するには、GET リクエストを 1 回実行します。結果のフィルタリングに役立つように、リクエストパスでクエリパラメーターを指定できます。使用可能なクエリのリストについては、「[アセット取得のためのクエリーパラメーター](./appendix.md#query)」の付録の節を参照してください。

**API 形式**

```http
GET /mlServices
GET /mlServices?{QUERY_PARAMETER}={VALUE}
GET /mlServices?{QUERY_PARAMETER_1}={VALUE_1}&{QUERY_PARAMETER_2}={VALUE_2}
```

| パラメーター | 説明 |
| --- | --- |
| `{QUERY_PARAMETER}` | 結果のフィルタリングに[使用可能なクエリパラメーター](./appendix.md#query)の 1 つです。 |
| `{VALUE}` | 上記クエリパラメーターの値です。 |

**リクエスト**

以下のリクエストは、クエリを含んでおり、同じ MLInstance ID（`{MLINSTANCE_ID}`）を共有する MLService のリストを取得します。

```shell
curl -X GET \
    'https://platform.adobe.io/data/sensei/mlServices?property=mlInstanceId==46986c8f-7739-4376-8509-0178bdf32cda' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功時の応答は、MLService のリストと詳細を返します。この詳細には、MLService ID（`{MLSERVICE_ID}`）、トレーニングの実験 ID（`{TRAINING_ID}`）、スコアリングの実験 ID（`{SCORING_ID}`）、入力トレーニングデータセット ID（`{DATASET_ID}`）などが含まれます。

```json
{
    "children": [
        {
            "id": "68d936d8-17e6-44ef-a4b6-c7502055638b",
            "name": "A service created in UI",
            "mlInstanceId": "46986c8f-7739-4376-8509-0178bdf32cda",
            "trainingExperimentId": "014d8acf-08fb-421c-8b65-760c8799c627",
            "trainingDataSetId": "5ee3cd7f2d34011913c56941",
            "scoringExperimentId": "76c2b1b-fad7-4b31-8c54-19ecc18b1ea0",
            "created": "2019-01-01T00:00:00.000Z",
            "createdBy": {
                "displayName": "Jane Doe",
                "userId": "Jane_Doe@AdobeID"
            },
            "updated": "2019-01-01T00:00:00.000Z"
        }
    ],
    "_page": {
        "property": "mlInstanceId==46986c8f-7739-4376-8509-0178bdf32cda,deleted==false",
        "count": 1
    }
}
```

## 特定の MLService の取得 {#retrieve-a-specific-mlservice}

特定の実験の詳細を取得するには、目的の MLService の ID をリクエストパスに含んだ GET リクエストを実行します。

**API 形式**

```http
GET /mlServices/{MLSERVICE_ID}
```

* `{MLSERVICE_ID}`：有効な MLService ID です。

**リクエスト**

```shell
curl -X GET \
    https://platform.adobe.io/data/sensei/mlServices/68d936d8-17e6-44ef-a4b6-c7502055638b \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功時の応答は、要求された MLService の詳細を格納したペイロードを返します。

```json
{
    "id": "68d936d8-17e6-44ef-a4b6-c7502055638b",
    "name": "A name for this MLService",
    "description": "A description for this MLService",
    "mlInstanceId": "46986c8f-7739-4376-8509-0178bdf32cda",
    "trainingExperimentId": "014d8acf-08fb-421c-8b65-760c8799c627",
    "trainingDataSetId": "5ee3cd7f2d34011913c56941",
    "scoringExperimentId": "76c2b1b-fad7-4b31-8c54-19ecc18b1ea0",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "userId": "Jane_Doe@AdobeID"
    },
    "updated": "2019-01-01T00:00:00.000Z"
}
```

## MLService の更新 {#update-an-mlservice}

既存の MLService を更新するには、対象となる MLService の ID をリクエストパスに含め、更新後のプロパティを JSON ペイロードで指定した PUT リクエストを実行して、プロパティを上書きします。

>[!TIP]
>
> この PUT リクエストを確実に成功させるために、まず GET リクエストを実行して、[対象となる MLService を ID で取得](#retrieve-a-specific-mlservice)することをお勧めします。次に、返された JSON オブジェクトを変更および更新し、変更された JSON オブジェクト全体を PUT リクエストのペイロードとして指定します。

**API 形式**

```http
PUT /mlServices/{MLSERVICE_ID}
```

* `{MLSERVICE_ID}`：有効な MLService ID です。

**リクエスト**

```shell
curl -X PUT \
    https://platform.adobe.io/data/sensei/mlServices/68d936d8-17e6-44ef-a4b6-c7502055638b \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'content-type: application/vnd.adobe.platform.sensei+json; profile=mlService.v1.json' \
    -d '{
        "name": "A name for this MLService",
        "description": "A description for this MLService",
        "mlInstanceId": "46986c8f-7739-4376-8509-0178bdf32cda",
        "trainingExperimentId": "014d8acf-08fb-421c-8b65-760c8799c627",
        "trainingDataSetId": "5ee3cd7f2d34011913c56941",
        "scoringExperimentId": "76c2b1b-fad7-4b31-8c54-19ecc18b1ea0",
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

成功時の応答は、MLService の更新された詳細を格納したペイロードを返します。

```json
{
    "id": "68d936d8-17e6-44ef-a4b6-c7502055638b",
    "name": "A name for this MLService",
    "description": "A description for this MLService",
    "mlInstanceId": "46986c8f-7739-4376-8509-0178bdf32cda",
    "trainingExperimentId": "014d8acf-08fb-421c-8b65-760c8799c627",
    "trainingDataSetId": "5ee3cd7f2d34011913c56941",
    "scoringExperimentId": "76c2b1b-fad7-4b31-8c54-19ecc18b1ea0",
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

## MLService の削除

1 つのMLService を削除するには、対象となる MLService の ID をリクエストパスに含んだ DELETE リクエストを実行します。

**API 形式**

```http
DELETE /mlServices/{MLSERVICE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MLSERVICE_ID}` | 有効な MLService ID です。 |

**リクエスト**

```shell
curl -X DELETE \
    https://platform.adobe.io/data/sensei/mlServices/68d936d8-17e6-44ef-a4b6-c7502055638b \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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

## MLInstance ID による MLService の削除

特定の MLInstance に属するすべての MLService を削除するには、MLInstance ID をクエリパラメーターとして指定した DELETE リクエストを実行します。

**API 形式**

```http
DELETE /mlServices?mlInstanceId={MLINSTANCE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MLINSTANCE_ID}` | 有効な MLInstance ID です。 |

**リクエスト**

```shell
curl -X DELETE \
    https://platform.adobe.io/data/sensei/mlServices?mlInstanceId=46986c8f-7739-4376-8509-0178bdf32cda \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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
