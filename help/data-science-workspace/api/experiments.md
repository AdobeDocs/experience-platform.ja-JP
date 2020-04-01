---
keywords: Experience Platform;developer guide;endpoint;Data Science Workspace;popular topics
solution: Experience Platform
title: 実験
topic: Developer guide
translation-type: tm+mt
source-git-commit: 01cfbc86516a05df36714b8c91666983f7a1b0e8

---


# 実験

モデルの開発とトレーニングは、テストレベルで行われます。テストは、MLInstance、トレーニングの実行、スコアの実行で構成されます。

## テストの作成

テストを作成するには、リクエストペイロードで名前と有効なMLInstance IDを指定しながらPOSTリクエストを実行します。

>[!NOTE] UIのモデルトレーニングとは異なり、明示的なAPI呼び出しを使用してテストを作成しても、トレーニング実行が自動的に作成されて実行されることはありません。

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
        "mlInstanceId": "{MLINSTANCE_ID}"
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | テストの名前。 この実験に対応するトレーニングの実行は、この値を継承し、UIにトレーニングの実行名として表示されます。 |
| `mlInstanceId` | 有効なMLInstance IDです。 |

**応答**

成功した応答は、新たに作成された実験の詳細(固有の識別子(`id`)を含む)を含むペイロードを返します。

```json
{
    "id": "{EXPERIMENT_ID}",
    "name": "A name for this Experiment",
    "mlInstanceId": "{MLINSTANCE_ID}",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "userId": "Jane_Doe@AdobeID"
    },
    "updated": "2019-01-01T00:00:00.000Z",
    "createdByService": false
}
```

## トレーニングまたはスコアリングの実行の作成と実行

POSTリクエストを実行し、有効なテストIDを指定し、実行タスクを指定することで、トレーニングまたはスコアリングの実行を作成できます。 スコアリングの実行は、テストに既存の成功したトレーニングの実行がある場合にのみ作成できます。 トレーニング実行を正常に作成すると、モデルトレーニング手順が初期化され、完了に成功すると、トレーニングモデルが生成されます。 トレーニングを受けたモデルを作成すると、テストでは常に1つのトレーニングを受けたモデルのみを利用できるように、既存のモデルが置き換えられます。

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
    https://platform.adobe.io/data/sensei/experiments/{EXPERIMENT_ID}/runs \
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
| `{TASK}` | 実行のタスク。 この値は、トレーニング、スコア `train` リング、またはフ `score` ィーチャパイプラインのい `fp` ずれかに設定します。 |

**応答**

成功した応答は、継承されたデフォルトのトレーニングまたはスコアリングパラメーターと、実行の一意のID(`{RUN_ID}`)を含む、新しく作成された実行の詳細を含むペイロードを返します。

```json
{
    "id": "{RUN_ID}",
    "mode": "{TASK}",
    "experimentId": "{EXPERIMENT_ID}",
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

1つのGETリクエストを実行し、有効なMLInstance IDをクエリパラメータとして指定することで、特定のMLInstanceに属する実験のリストを取得できます。 使用可能なクエリのリストについては、アセット取得のための [クエリパラメータの付録の節を参照してください](./appendix.md#query)。


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
    https://platform.adobe.io/data/sensei/experiments?property=mlInstanceId=={MLINSTANCE_ID} \
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
            "id": "{EXPERIMENT_ID}",
            "name": "A name for this Experiment",
            "mlInstanceId": "{MLINSTANCE_ID}",
            "created": "2019-01-01T00:00:00.000Z",
            "updated": "2019-01-01T00:00:00.000Z",
            "createdByService": false
        },
        {
            "id": "{EXPERIMENT_ID}",
            "name": "Training Run 1",
            "mlInstanceId": "{MLINSTANCE_ID}",
            "created": "2019-01-01T00:00:00.000Z",
            "updated": "2019-01-01T00:00:00.000Z",
            "createdByService": false
        },
        {
            "id": "{EXPERIMENT_ID}",
            "name": "Training Run 2",
            "mlInstanceId": "{MLINSTANCE_ID}",
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

特定のテストの詳細を取得するには、リクエストパスに目的のテストのIDを含むGETリクエストを実行します。

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
    https://platform.adobe.io/data/sensei/experiments/{EXPERIMENT_ID} \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```


**応答**

成功した応答は、要求された実験の詳細を含むペイロードを返します。

```json
{
    "id": "{EXPERIMENT_ID}",
    "name": "A name for this Experiment",
    "mlInstanceId": "{MLINSTANCE_ID}",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "userId": "Jane_Doe@AdobeID"
    },
    "updated": "2019-01-01T00:00:00.000Z",
    "createdByService": false
}
```

## テスト実行のリストの取得

単一のGETリクエストを実行し、有効なテストIDを指定することで、特定のテストに属するトレーニングまたはスコアリングの実行のリストを取得できます。 結果のフィルタリングに役立つように、リクエストパスでクエリパラメーターを指定できます。 使用可能なクエリパラメータの完全なリストについては、アセット取得のための [クエリパラメータに関する付録の節を参照してくださ](./appendix.md#query)い。

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
| `{QUERY_PARAMETER}` | 結果のフィルターに使用 [するクエリ](./appendix.md#query) ・パラメータの1つ。 |
| `{VALUE}` | 前のパラメーターのクエリ値。 |

**リクエスト**

次のリクエストは、クエリを含み、ある種のテストに属するトレーニング実行のリストを取得します。

```shell
curl -X GET \
    https://platform.adobe.io/data/sensei/experiments/{EXPERIMENT_ID}/runs?property=mode==train \
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
            "id": "{RUN_ID}",
            "mode": "train",
            "experimentId": "{EXPERIMENT_ID}",
            "created": "2019-01-01T00:00:00.000Z",
            "createdBy": {
                "userId": "Jane_Doe@AdobeID"
            },
            "createdBySchedule": false
        }
    ],
    "_page": {
        "property": "mode==train,experimentId=={EXPERIMENT_ID},deleted==false",
        "totalCount": 1,
        "count": 1
    }
}
```

## テストの更新

既存のテストを更新するには、リクエストパスにターゲットテストのIDを含むPUTリクエストを使用してプロパティを上書きし、更新されたプロパティを含むJSONペイロードを提供します。

>[!TIP] このPUTリクエストを確実に成功させるために、まずGETリクエストを実行し、IDでテストを取得す [ることをお勧めします](#retrieve-specific)。 次に、返されたJSONオブジェクトを変更および更新し、変更されたJSONオブジェクトの全体をPUT要求のペイロードとして適用します。

以下のサンプルAPI呼び出しは、初期状態でこれらのプロパティを持つと同時に、実験の名前を更新します。

```json
{
    "name": "A name for this Experiment",
    "mlInstanceId": "{MLINSTANCE_ID}",
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
    https://platform.adobe.io/data/sensei/experiments/{EXPERIMENT_ID} \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'content-type: application/vnd.adobe.platform.sensei+json;profile=experiments.v1.json' \
    -d '{
        "name": "An upated name",
        "mlInstanceId": "{MLINSTANCE_ID}",
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
    "id": "{EXPERIMENT_ID}",
    "name": "An updated name",
    "mlInstanceId": "{MLINSTANCE_ID}",
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
    https://platform.adobe.io/data/sensei/experiments/{EXPERIMENT_ID} \
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

MLInstance IDをパラメータとして含むDELETEリクエストを実行すると、特定のMLInstanceに属するすべての実験をクエリできます。

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
    https://platform.adobe.io/data/sensei/experiments?mlInstanceId={MLINSTANCE_ID} \
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
