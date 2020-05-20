---
keywords: Experience Platform;developer guide;endpoint;Data Science Workspace;popular topics
solution: Experience Platform
title: インサイト
topic: Developer guide
translation-type: tm+mt
source-git-commit: 01cfbc86516a05df36714b8c91666983f7a1b0e8
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 4%

---


# インサイト

インサイトには、データサイエンティストが関連する評価指標を表示することで最適なMLモデルを評価および選択できるようにするために使用される指標が含まれます。

## インサイトのリストの取得

インサイトエンドポイントに対して1つのGET要求を実行することで、インサイトのリストを取得できます。  結果をフィルターするのに役立つように、リクエストパスでクエリパラメーターを指定できます。 使用可能なクエリのリストについては、付録の「アセット取得の [クエリパラメータ」の節を参照してください](./appendix.md#query)。

**API形式**

```http
GET /insights
```

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/sensei/insights \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、インサイトのリストを含むペイロードを返し、各インサイトには一意の識別子( `id` )があります。 さらに、Insightsのイベントと指標のデータに続く特定のインサイトに関連付けられた一意の識別子を含む `context` 識別子を受け取ります。

```json
{
    "children": [
        {
            "id": "{INSIGHT_ID}",
            "context": {
                "experimentId": "{EXPERIMENT_ID}",
                "experimentRunId": "{EXPERIMENT_RUN_ID}",
                "modelId": "{MODEL_ID}"
            },
            "events": {
                "name": "fit",
                "eventValues": {
                    "algorithm": null,
                    "ratio": "0.8"
                }
            },
            "metrics": [
                {
                    "name": "MAPE",
                    "value": "0.0111111111111",
                    "valueType": "double"
                }
            ],
            "created": "2019-01-01T00:00:00.000Z",
            "updated": "2019-01-02T00:00:00.000Z"
        },
        {
            "id": "{INSIGHT_ID}",
            "context": {
                "experimentId": "{EXPERIMENT_ID}",
                "experimentRunId": "{EXPERIMENT_RUN_ID}",
                "modelId": "{MODEL_ID}"
            },
            "events": {
                "name": "fit",
                "eventValues": {
                    "algorithm": null,
                    "ratio": "0.8"
                }
            },
            "metrics": [
                {
                    "name": "MAPE",
                    "value": "0.0111111111111",
                    "valueType": "double"
                }
            ],
            "created": "2019-01-01T00:00:00.000Z",
            "updated": "2019-01-02T00:00:00.000Z"
            }
        ],
    "_page": {
        "count": 2
    }
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | インサイトに対応するID。 |
| `experimentId` | 有効なテストID。 |
| `experimentRunId` | 有効なテスト実行ID。 |
| `modelId` | 有効なモデルID。 |

## 特定のインサイトの取得

特定のインサイトを検索するには、GETリクエストを作成し、リクエストパスで有効なGET `{INSIGHT_ID}` を指定します。 結果をフィルターするのに役立つように、リクエストパスでクエリパラメーターを指定できます。 使用可能なクエリのリストについては、付録の「アセット取得の [クエリパラメータ」の節を参照してください](./appendix.md#query)。

**API形式**

```http
GET /insights/{INSIGHT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{INSIGHT_ID}` | 先生インサイトの一意の識別子。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/sensei/insights/{INSIGHT_ID} \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、インサイトの固有な識別子(`id`)を含むペイロードを返します。 さらに、Insightsのイベントと指標データに続く特定のインサイトに関連付けられた一意の識別子を含む `context` 識別子を受け取ります。

```json
{
    "id": "{INSIGHT_ID}",
    "context": {
        "experimentId": "{EXPERIMENT_ID}",
        "experimentRunId": "{EXPERIMENT_RUN_ID}",
        "modelId": "{MODEL_ID}"
    },
    "events": {
        "name": "fit",
        "eventValues": {
            "algorithm": null,
            "ratio": "0.8"
        }
    },
    "metrics": [
        {
            "name": "MAPE",
            "value": "0.0111111111111",
            "valueType": "double"
        }
    ],
    "created": "2019-01-01T00:00:00.000Z",
    "updated": "2019-01-02T00:00:00.000Z"
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | インサイトに対応するID。 |
| `experimentId` | 有効なテストID。 |
| `experimentRunId` | 有効なテスト実行ID。 |
| `modelId` | 有効なモデルID。 |

## 新追加しいモデルインサイト

新しいモデルインサイトのコンテキスト、イベントおよび指標を提供するPOSTリクエストおよびペイロードを実行して、新しいモデルインサイトを作成できます。 新しいモデルインサイトの作成に使用するコンテキストフィールドには、既存のサービスを割り当てる必要はありませんが、対応する1つ以上のIDを指定することで、既存のサービスを使用して新しいモデルインサイトを作成できます。

```json
"context": {
    "clientId": "{CLIENT_ID}",
    "notebookId": "{NOTEBOOK_ID}",
    "experimentId": "{EXPERIMENT_ID}",
    "engineId": "{ENGINE_ID}",
    "mlInstanceId": "{MLINSTANCE_ID}",
    "experimentRunId": "{EXPERIMENT_RUN_ID}",
    "modelId": "{MODEL_ID}",
    "dataSetId": "{DATASET_ID}"
  }
```

**API形式**

```http
POST /insights
```

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/sensei/insights \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -H `Content-Type: application/vnd.adobe.platform.sensei+json;profile=mlInstance.v1.json`
    -d {
    "context": {
        "experimentId": "{EXPERIMENT_ID}",
        "experimentRunId": "{EXPERIMENT_RUN_ID}",
        "modelId": "{MODEL_ID}"
    },
    "events": {
        "name": "fit2",
        "eventValues": {
            "algorithm": null,
            "ratio": "0.99"
        }
    },
    "metrics": [
        {
            "name": "MAPE2",
            "value": "0.11111111111",
            "valueType": "double"
        }
    ],
    "created": "2019-01-01T00:00:00.000Z",
    "updated": "2019-01-02T00:00:00.000Z"
}
```

**応答**

成功した応答は、最初の要求で指定したパラメーターとパラメーターを含むペイロード `{INSIGHT_ID}` を返します。

```json
{
    "id": "{INSIGHT_ID}",
    "context": {
        "experimentId": "{EXPERIMENT_ID}",
        "experimentRunId": "{EXPERIMENT_RUN_ID}",
        "modelId": "{MODEL_ID}"
    },
    "events": {
        "name": "fit2",
        "eventValues": {
            "algorithm": null,
            "ratio": "0.99"
        }
    },
    "metrics": [
        {
            "name": "MAPE2",
            "value": "0.11111111111",
            "valueType": "double"
        }
    ],
    "created": "2019-01-01T00:00:00.000Z",
    "updated": "2019-01-02T00:00:00.000Z"
}
```

| プロパティ | 説明 |
| --- | --- |
| `insightId` | 成功したPOSTリクエストがおこなわれた場合に、この特定のインサイトに対して作成される一意のID。 |

## アルゴリズムのデフォルト指標のリストの取得

指標エンドポイントに対して1つのGETリクエストを実行することで、すべてのアルゴリズムとデフォルトの指標のリストを取得できます。 特定の指標をクエリするには、GETリクエストを作成し、リクエストパスで有効な値 `{ALGORITHM}` を指定します。

**API形式**

```http
GET /insights/metrics
GET /insights/metrics?algorithm={ALGORITHM}
```

| パラメーター | 説明 |
| --- | --- |
| `{ALGORITHM}` | アルゴリズムタイプの識別子。 |

**リクエスト**

次のリクエストには、クエリが含まれ、アルゴリズム識別子を使用して特定の指標を取得します `{ALGORITHM}`

```shell
curl -X GET \
  'https://platform.adobe.io/data/sensei/insights/metrics?algorithm={ALGORITHM}' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、 `algorithm` 一意の識別子とデフォルト指標の配列を含むペイロードを返します。

```json
{
    "children": [
        {
            "algorithm": "{ALGORITHM}",
            "defaultMetrics": [
                "f-score",
                "auroc",
                "roc",
                "precision",
                "recall",
                "accuracy",
                "confusion matrix"
            ]
        }
    ]
}
```
