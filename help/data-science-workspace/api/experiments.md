---
keywords: Experience Platform、開発者ガイド、エンドポイント、Data Science Workspace、人気の高いトピック、実験、sensei 機械学習 api
solution: Experience Platform
title: Experiment API エンドポイント
topic-legacy: Developer guide
description: モデルの開発とトレーニングは、実験レベルで行われます。実験は、MLInstance、トレーニング実行、スコア付け実行で構成されます。
exl-id: 6ca5106e-896d-4c03-aecc-344632d5307d
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '783'
ht-degree: 97%

---

# 実験エンドポイント

モデルの開発とトレーニングは、実験レベルで行われます。実験は、MLInstance、トレーニング実行、スコア付け実行で構成されます。

## 実験の作成 {#create-an-experiment}

実験を作成するには、リクエストペイロードで名前と有効な MLInstance ID を指定すると同時に、POST リクエストを実行します。

>[!NOTE]
>
>UI のモデルトレーニングとは異なり、明示的な API 呼び出しを使用して実験を作成しても、トレーニング実行が自動的に作成および実行されることはありません。

**API 形式**

```http
POST /experiments
```

**リクエスト**

```shell
curl -X POST \
    https://platform.adobe.io/data/sensei/experiments \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'content-type: application/vnd.adobe.platform.sensei+json;profile=experiment.v1.json' \
    -d '{
        "name": "a name for this Experiment",
        "mlInstanceId": "46986c8f-7739-4376-8509-0178bdf32cda"
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | 実験の名前。この実験に対応するトレーニング実行はこの値を継承し、UI にトレーニングの実行名として表示されます。 |
| `mlInstanceId` | 有効な MLInstance ID です。 |

**応答**

成功応答は、新たに作成された実験の詳細（固有の識別子 `id` など）を含むペイロードを返します。

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

## トレーニング実行またはスコア付け実行の作成と実行 {#experiment-training-scoring}

POST リクエストを実行、有効なテスト ID を指定、および実行タスクを指定することで、トレーニング実行またはスコア付け実行を作成できます。スコア付け実行は、成功した既存のトレーニング実行が実行に含まれる場合にのみ作成できます。トレーニング実行を正常に作成すると、モデルトレーニング手順が初期化され、これを正常に完了すると、トレーニングモデルが生成されます。トレーニング済みモデルを作成すると、指定期間に実験で利用できるトレーニング済みモデルを 1 つのみにするよう、既存のモデルが置き換えられます。

**API 形式**

```http
POST /experiments/{EXPERIMENT_ID}/runs
```

| パラメーター | 説明 |
| --- | --- |
| `{EXPERIMENT_ID}` | 有効な実験 ID。 |

**リクエスト**

```shell
curl -X POST \
    https://platform.adobe.io/data/sensei/experiments/5cb25a2d-2cbd-4c99-a619-8ddae5250a7b/runs \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'content-type: application/vnd.adobe.platform.sensei+json;profile=experimentRun.v1.json' \
    -d '{
        "mode": "{TASK}"
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `{TASK}` | 実行のタスクを指定します。この値は、トレーニングの場合は `train`、スコア付けの場合は `score`、フィーチャパイプラインの場合は `featurePipeline` のいずれかに設定します。 |

**応答**

成功応答は、継承されたデフォルトのトレーニングまたはスコア付けパラメーターと、実行の一意の ID（`{RUN_ID}`）を含め、新しく作成された実行の詳細を含むペイロードを返します。

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

## 実験のリストを取得

単一の GET リクエストを実行し、クエリパラメーターとして有効な MLInstance ID を指定することで、特定の MLInstance インスタンスに属する実験のリストを取得できます。使用可能なクエリのリストについては、「[アセット取得のためのクエリーパラメーター](./appendix.md#query)」の付録の節を参照してください。


**API 形式**

```http
GET /experiments
GET /experiments?property=mlInstanceId=={MLINSTANCE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MLINSTANCE_ID}` | 有効な MLInstance ID を指定して、特定の MLInstance に属する実験のリストを取得します。 |

**リクエスト**

```shell
curl -X GET \
    https://platform.adobe.io/data/sensei/experiments?property=mlInstanceId==46986c8f-7739-4376-8509-0178bdf32cda \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功応答は、同じ MLInstance ID（`{MLINSTANCE_ID}`）を共有する実験のリストを返します。

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

## 特定の実験の取得 {#retrieve-specific}

特定の実験の詳細を取得するには、リクエストパスに目的の実験 ID 含めた GET リクエストを実行します。

**API 形式**

```http
GET /experiments/{EXPERIMENT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{EXPERIMENT_ID}` | 有効な実験 ID。 |

**リクエスト**

```shell
curl -X GET \
    https://platform.adobe.io/data/sensei/experiments/5cb25a2d-2cbd-4c99-a619-8ddae5250a7b \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功応答は、要求された実験の詳細を含むペイロードを返します。

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

単一の GET リクエストを実行し、有効な実験 ID を指定することで、特定の実験に属するトレーニング実行またはスコア付け実行のリストを取得できます。結果をフィルタリングできるよう、リクエストパスでクエリパラメーターを指定できます。使用可能なクエリパラメーターの完全なリストについては、[アセット取得用クエリパラメーター](./appendix.md#query)の付録の節を参照してください。

>[!NOTE]
>
> 複数のクエリパラメーターを組み合わせる場合は、アンパサンド（&amp;）で区切る必要があります。

**API 形式**

```http
GET /experiments/{EXPERIMENT_ID}/runs
GET /experiments/{EXPERIMENT_ID}/runs?{QUERY_PARAMETER}={VALUE}
GET /experiments/{EXPERIMENT_ID}/runs?{QUERY_PARAMETER_1}={VALUE_1}&{QUERY_PARAMETER_2}={VALUE_2}
```

| パラメーター | 説明 |
| --- | --- |
| `{EXPERIMENT_ID}` | 有効な実験 ID。 |
| `{QUERY_PARAMETER}` | 結果のフィルターに[利用可能なクエリパラメーター](./appendix.md#query)の 1 つ。 |
| `{VALUE}` | 上記クエリパラメーターの値です。 |

**リクエスト**

次のリクエストは、クエリを含み、実験に属するトレーニング実行のリストを取得します。

```shell
curl -X GET \
    https://platform.adobe.io/data/sensei/experiments/5cb25a2d-2cbd-4c99-a619-8ddae5250a7b/runs?property=mode==train \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功応答は、実行のリストと、その実験実行 ID（`{RUN_ID}`）などの各詳細を含むペイロードを返します。

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

## 実験の更新

既存の実験を更新するには、リクエストパスにターゲットとする実験 ID を含む PUT リクエストを介してプロパティを上書きし、更新されたプロパティを含む JSON ペイロードを提供します。

>[!TIP]
>
>この PUT リクエストを確実に成功させるために、まず GET リクエストを実行し、[ID で実験を取得](#retrieve-specific)することをお勧めします。次に、返された JSON オブジェクトを変更および更新し、変更された JSON オブジェクト全体を PUT リクエストのペイロードとして適用します。

次のサンプル API 呼び出しは、初期状態で次のプロパティを持つと同時に、実験の名前を更新します。

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

**API 形式**

```http
PUT /experiments/{EXPERIMENT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{EXPERIMENT_ID}` | 有効な実験 ID。 |

**リクエスト**

```shell
curl -X PUT \
    https://platform.adobe.io/data/sensei/experiments/5cb25a2d-2cbd-4c99-a619-8ddae5250a7b \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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

成功応答は、実験の更新された詳細を含むペイロードを返します。

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

## 実験の削除

リクエストパスにターゲットとする実験 ID を含む DELETE リクエストを実行すると、1 つの実験を削除できます。

**API 形式**

```http
DELETE /experiments/{EXPERIMENT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{EXPERIMENT_ID}` | 有効な実験 ID。 |

**リクエスト**

```shell
curl -X DELETE \
    https://platform.adobe.io/data/sensei/experiments/5cb25a2d-2cbd-4c99-a619-8ddae5250a7b \
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
    "detail": "Experiment successfully deleted"
}
```

## MLInstance ID による実験の削除

MLInstance ID をパラメーターとして含む DELETE リクエストを実行すると、特定の MLInstance に属するすべての実験をクエリできます。

**API 形式**

```http
DELETE /experiments?mlInstanceId={MLINSTANCE_ID}
```

| パラメーター | 説明 |
| --- | ---|
| `{MLINSTANCE_ID}` | 有効な MLInstance ID です。 |

**リクエスト**

```shell
curl -X DELETE \
    https://platform.adobe.io/data/sensei/experiments?mlInstanceId=46986c8f-7739-4376-8509-0178bdf32cda \
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
    "detail": "Experiments successfully deleted"
}
```
