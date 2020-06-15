---
keywords: Experience Platform;developer guide;endpoint;Data Science Workspace;popular topics
solution: Experience Platform
title: 実験
topic: Developer guide
translation-type: tm+mt
source-git-commit: 76f68fea1bea970bab4c25061527b7ebae33faf3
workflow-type: tm+mt
source-wordcount: '744'
ht-degree: 4%

---


# 実験

モデルの開発とトレーニングは、テストレベルで行われます。テストは、MLInstance、トレーニングの実行、スコアの実行で構成されます。

## テストの作成 {#create-an-experiment}

リクエストペイロードで名前と有効なMLInstance IDを指定しながらPOSTリクエストを実行すると、テストを作成できます。

>[!NOTE] UIのモデルトレーニングとは異なり、明示的なAPI呼び出しを使用してテストを作成しても、トレーニング実行が自動的に作成および実行されるわけではありません。

**API形式**

```http
POST /experiments
```

**リクエスト**

```shell
curl -X POST \
    https://platform.adobe.io/data/sensei/experiments \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'content-type: application/vnd.adobe.platform.sensei+json;profile=experiment.v1.json' \
    -d '{
        "name": "a name for this Experiment",
        "mlInstanceId": "46986c8f-7739-4376-8509-0178bdf32cda"
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | テストの名前。 このテストに対応するトレーニング実行は、この値を継承して、UIに表示されるトレーニング実行名として表示されます。 |
| `mlInstanceId` | 有効なMLInstance IDです。 |

**応答**

成功した応答は、新たに作成されたテストの詳細(固有の識別子(`id`)を含む)を含むペイロードを返します。

```json
{
    "id": "5cb25a2d-2cbd-4c99-a619-8ddae5250a7b",
    "name": "A name for this Experiment",
    "mlInstanceId": "46986c8f-7739-4376-8509-0178bdf32cda",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "userId": "Jane_Doe@AdobeID"
    },
    "updated": "2019-01-01T00:00:00.000Z",
    "createdByService": false
}
```

## トレーニングまたはスコアリングの実行を作成して実行します {#experiment-training-scoring}

POSTリクエストを実行し、有効なテストIDを入力し、実行タスクを指定することで、トレーニングまたはスコアリングの実行を作成できます。 スコアリングの実行は、テストに既存のトレーニングの実行が成功している場合にのみ作成できます。 トレーニングの実行を正常に作成すると、モデルのトレーニング手順が初期化され、完了に成功すると、トレーニングを受けたモデルが生成されます。 トレーニングを受けたモデルの作成は、テストが常に1つのトレーニングを受けたモデルを利用できるよう、以前からあるモデルを置き換えます。

**API形式**

```http
POST /experiments/{EXPERIMENT_ID}/runs
```

| パラメーター | 説明 |
| --- | --- |
| `{EXPERIMENT_ID}` | 有効なテストID。 |

**リクエスト**

```shell
curl -X POST \
    https://platform.adobe.io/data/sensei/experiments/5cb25a2d-2cbd-4c99-a619-8ddae5250a7b/runs \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'content-type: application/vnd.adobe.platform.sensei+json;profile=experimentRun.v1.json' \
    -d '{
        "mode": "{TASK}"
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `{TASK}` | 実行のタスクを指定します。 この値は、トレーニング、スコアリング `train` 、またはフィーチャーパイプライン `score``featurePipeline` に対して設定します。 |

**応答**

成功した応答は、継承されたデフォルトのトレーニングまたはスコアリングパラメーター、および実行の一意のID(`{RUN_ID}`)を含む、新しく作成された実行の詳細を含むペイロードを返します。

```json
{
    "id": "33408593-2871-4198-a812-6d1b7d939cda",
    "mode": "{TASK}",
    "experimentId": "5cb25a2d-2cbd-4c99-a619-8ddae5250a7b",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "userId": "Jane_Doe@AdobeID"
    },
    "updated": "2019-01-01T00:00:00.000Z",
    "createdBySchedule": false,
    "tasks": [
        {
            "name": "{TASK}",
            "parameters": [
                {
                    "key": "parameter",
                    "value": "parameter value"
                }
            ]
        }
    ]
}
```

## 実験のリストを取得する

1つのGETリクエストを実行し、有効なMLInstance IDをクエリパラメーターとして指定することで、特定のMLInstanceに属する実験のリストを取得できます。 使用可能なクエリのリストについては、付録の「アセット取得の [クエリパラメータ」の節を参照してください](./appendix.md#query)。


**API形式**

```http
GET /experiments
GET /experiments?property=mlInstanceId=={MLINSTANCE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MLINSTANCE_ID}` | 有効なMLInstance IDを指定して、そのMLInstanceに属する実験のリストを取得します。 |

**リクエスト**

```shell
curl -X GET \
    https://platform.adobe.io/data/sensei/experiments?property=mlInstanceId==46986c8f-7739-4376-8509-0178bdf32cda \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、同じMLInstance ID(`{MLINSTANCE_ID}`)を共有する実験のリストを返します。

```json
{
    "children": [
        {
            "id": "5cb25a2d-2cbd-4c99-a619-8ddae5250a7b",
            "name": "A name for this Experiment",
            "mlInstanceId": "46986c8f-7739-4376-8509-0178bdf32cda",
            "created": "2019-01-01T00:00:00.000Z",
            "updated": "2019-01-01T00:00:00.000Z",
            "createdByService": false
        },
        {
            "id": "6cb25a2d-2cbd-4c99-a619-8ddae5250a7b",
            "name": "Training Run 1",
            "mlInstanceId": "46986c8f-7839-4376-8509-0178bdf32cda",
            "created": "2019-01-01T00:00:00.000Z",
            "updated": "2019-01-01T00:00:00.000Z",
            "createdByService": false
        },
        {
            "id": "7cb25a2d-2cbd-4c99-a619-8ddae5250a7b",
            "name": "Training Run 2",
            "mlInstanceId": "46986c8f-7939-4376-8509-0178bdf32cda",
            "created": "2019-01-01T00:00:00.000Z",
            "updated": "2019-01-01T00:00:00.000Z",
            "createdByService": false
        }
    ],
    "_page": {
        "property": "deleted==false",
        "count": 3
    }
}
```

## 特定のテストの取得 {#retrieve-specific}

リクエストパスに目的のテストのIDを含むGETリクエストを実行すると、特定のテストの詳細を取得できます。

**API形式**

```http
GET /experiments/{EXPERIMENT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{EXPERIMENT_ID}` | 有効なテストID。 |

**リクエスト**

```shell
curl -X GET \
    https://platform.adobe.io/data/sensei/experiments/5cb25a2d-2cbd-4c99-a619-8ddae5250a7b \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、要求されたテストの詳細を含むペイロードを返します。

```json
{
    "id": "5cb25a2d-2cbd-4c99-a619-8ddae5250a7b",
    "name": "A name for this Experiment",
    "mlInstanceId": "46986c8f-7739-4376-8509-0178bdf32cda",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "userId": "Jane_Doe@AdobeID"
    },
    "updated": "2019-01-01T00:00:00.000Z",
    "createdByService": false
}
```

## テスト実行のリストの取得

単一のGETリクエストを実行し、有効なテストIDを入力することで、特定のテストに属するトレーニングまたはスコアリングの実行のリストを取得できます。 結果をフィルターするのに役立つように、リクエストパスでクエリパラメーターを指定できます。 使用可能なクエリパラメータの完全なリストについては、付録の「アセット取得の [クエリパラメータ」の節を参照してください](./appendix.md#query)。

>[!NOTE] 複数のクエリパラメーターを組み合わせる場合は、アンパサンド(&amp;)で区切る必要があります。

**API形式**

```http
GET /experiments/{EXPERIMENT_ID}/runs
GET /experiments/{EXPERIMENT_ID}/runs?{QUERY_PARAMETER}={VALUE}
GET /experiments/{EXPERIMENT_ID}/runs?{QUERY_PARAMETER_1}={VALUE_1}&{QUERY_PARAMETER_2}={VALUE_2}
```

| パラメーター | 説明 |
| --- | --- |
| `{EXPERIMENT_ID}` | 有効なテストID。 |
| `{QUERY_PARAMETER}` | 結果のフィルタリングに [使用できるクエリパラメーターの1つ](./appendix.md#query) 。 |
| `{VALUE}` | 前のクエリパラメーターの値。 |

**リクエスト**

次のリクエストにはクエリが含まれ、テストに属するトレーニングの実行のリストが取得されます。

```shell
curl -X GET \
    https://platform.adobe.io/data/sensei/experiments/5cb25a2d-2cbd-4c99-a619-8ddae5250a7b/runs?property=mode==train \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、実行のリストと、その実験実行ID(`{RUN_ID}`)を含む各詳細を含むペイロードを返します。

```json
{
    "children": [
        {
            "id": "33408593-2871-4198-a812-6d1b7d939cda",
            "mode": "train",
            "experimentId": "5cb25a2d-2cbd-4c99-a619-8ddae5250a7b",
            "created": "2019-01-01T00:00:00.000Z",
            "createdBy": {
                "userId": "Jane_Doe@AdobeID"
            },
            "createdBySchedule": false
        }
    ],
    "_page": {
        "property": "mode==train,experimentId==5cb25a2d-2cbd-4c99-a619-8ddae5250a7b,deleted==false",
        "totalCount": 1,
        "count": 1
    }
}
```

## テストの更新

リクエストパスにターゲットテストのIDを含むPUT要求を通じてプロパティを上書きし、更新されたプロパティを含むJSONペイロードを提供することで、既存のテストを更新できます。

>[!TIP] このPUTリクエストを確実に成功させるために、最初にGETリクエストを実行し、IDでテストを [取得することをお勧めします](#retrieve-specific)。 次に、返されたJSONオブジェクトを変更および更新し、変更されたJSONオブジェクト全体をPUT要求のペイロードとして適用します。

以下のサンプルAPI呼び出しは、初期状態で次のプロパティを持つと同時に、実験の名前を更新します。

```json
{
    "name": "A name for this Experiment",
    "mlInstanceId": "46986c8f-7739-4376-8509-0178bdf32cda",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "userId": "Jane_Doe@AdobeID"
    },
    "createdByService": false
}
```

**API形式**

```http
PUT /experiments/{EXPERIMENT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{EXPERIMENT_ID}` | 有効なテストID。 |

**リクエスト**

```shell
curl -X PUT \
    https://platform.adobe.io/data/sensei/experiments/5cb25a2d-2cbd-4c99-a619-8ddae5250a7b \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'content-type: application/vnd.adobe.platform.sensei+json;profile=experiments.v1.json' \
    -d '{
        "name": "An upated name",
        "mlInstanceId": "46986c8f-7739-4376-8509-0178bdf32cda",
        "created": "2019-01-01T00:00:00.000Z",
        "createdBy": {
            "userId": "Jane_Doe@AdobeID"
        },
        "createdByService": false
    }'
```

**応答**

成功した応答は、テストの更新された詳細を含むペイロードを返します。

```json
{
    "id": "5cb25a2d-2cbd-4c99-a619-8ddae5250a7b",
    "name": "An updated name",
    "mlInstanceId": "46986c8f-7739-4376-8509-0178bdf32cda",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "userId": "Jane_Doe@AdobeID"
    },
    "updated": "2019-01-02T00:00:00.000Z",
    "createdByService": false
}
```

## テストの削除

リクエストパスにターゲットテストのIDを含むDELETEリクエストを実行すると、1つのテストを削除できます。

**API形式**

```http
DELETE /experiments/{EXPERIMENT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{EXPERIMENT_ID}` | 有効なテストID。 |

**リクエスト**

```shell
curl -X DELETE \
    https://platform.adobe.io/data/sensei/experiments/5cb25a2d-2cbd-4c99-a619-8ddae5250a7b \
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
    "detail": "Experiment successfully deleted"
}
```

## MLInstance IDによる実験の削除

MLInstance IDをクエリパラメータとして含むDELETEリクエストを実行すると、特定のMLInstanceに属するすべての実験を削除できます。

**API形式**

```http
DELETE /experiments?mlInstanceId={MLINSTANCE_ID}
```

| パラメーター | 説明 |
| --- | ---|
| `{MLINSTANCE_ID}` | 有効なMLInstance IDです。 |

**リクエスト**

```shell
curl -X DELETE \
    https://platform.adobe.io/data/sensei/experiments?mlInstanceId=46986c8f-7739-4376-8509-0178bdf32cda \
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
    "detail": "Experiments successfully deleted"
}
```
