---
keywords: Experience Platform、開発者ガイド、エンドポイント、Data Science Workspace、人気の高いトピック、インサイト、sensei 機械学習 API
solution: Experience Platform
title: Insights API エンドポイント
description: インサイトには、関連する評価指標を表示することにより、データサイエンティストが最適な ML モデルを評価および選択できるようにするための指標が含まれています。
exl-id: 603546d6-5686-4b59-99a7-90ecc0db8de3
source-git-commit: 86e6924078c115fb032ce39cd678f1d9c622e297
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 96%

---

# インサイトエンドポイント

インサイトには、関連する評価指標を表示することにより、データサイエンティストが最適な ML モデルを評価および選択できるようにするための指標が含まれています。

## Insights のリストの取得

Insights のリストを取得するには、Insights エンドポイントに対して 1 つの GET リクエストを実行します。  結果をフィルターするのに役立つように、リクエストパスでクエリパラメーターを指定できます。使用可能なクエリのリストについては、「[アセット取得のためのクエリーパラメーター](./appendix.md#query)」の付録の節を参照してください。

**API 形式**

```http
GET /insights
```

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/sensei/insights \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、インサイトのリストを含むペイロードを返し、各インサイトには一意の ID（`id`）があります。さらに、`context` を受け取ります。これには、Insights イベントと指標データに続く特定のインサイトに関連付けられている一意の ID が含まれます。

```json
{
    "children": [
        {
            "id": "08b8d174-6b0d-4d7e-acd8-1c4c908e14b2",
            "context": {
                "experimentId": "5cb25a2d-2cbd-4c99-a619-8ddae5250a7b",
                "experimentRunId": "33408593-2871-4198-a812-6d1b7d939cda",
                "modelId": "15c53796-bd6b-4e09-b51d-7296aa20af71"
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
            "id": "08b8d174-6b0d-4d7e-acd8-1c4c908e14b2",
            "context": {
                "experimentId": "5cb25a2d-2cbd-4c99-a619-8ddae5250a7b",
                "experimentRunId": "33408593-2871-4198-a812-6d1b7d939cda",
                "modelId": "15c53796-bd6b-4e09-b51d-7296aa20af71"
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
| `id` | Insight に対応する ID。 |
| `experimentId` | 有効な実験 ID。 |
| `experimentRunId` | 有効な実験実行 ID。 |
| `modelId` | 有効なモデル ID。 |

## 特定の Insight の取得

特定のインサイトを参照するには、GET リクエストを作成し、リクエストパスで有効な `{INSIGHT_ID}` を指定します。結果をフィルターするのに役立つように、リクエストパスでクエリパラメーターを指定できます。使用可能なクエリのリストについては、「[アセット取得のためのクエリーパラメーター](./appendix.md#query)」の付録の節を参照してください。

**API 形式**

```http
GET /insights/{INSIGHT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{INSIGHT_ID}` | Sensei インサイトの固有の識別子。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/sensei/insights/08b8d174-6b0d-4d7e-acd8-1c4c908e14b2 \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、インサイト固有の識別子（`id`）を含むペイロードを返します。さらに、`context` を受け取ります。これには、Insights イベントと指標データに続く特定のインサイトに関連付けられている一意の ID が含まれます。

```json
{
    "id": "08b8d174-6b0d-4d7e-acd8-1c4c908e14b2",
    "context": {
        "experimentId": "5cb25a2d-2cbd-4c99-a619-8ddae5250a7b",
        "experimentRunId": "33408593-2871-4198-a812-6d1b7d939cda",
        "modelId": "15c53796-bd6b-4e09-b51d-7296aa20af71"
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
| `id` | Insight に対応する ID。 |
| `experimentId` | 有効な実験 ID。 |
| `experimentRunId` | 有効な実験実行 ID。 |
| `modelId` | 有効なモデル ID |

## 新しいモデルインサイトの追加

POST リクエストの実行と、新しいモデルインサイトのコンテキスト、イベント、指標を提供するペイロードで、新しいモデルインサイトを作成できます。新しいモデルインサイトの作成に使用されるコンテキストフィールドに既存のサービスを添付する必要はありませんが、1 つ以上の対応する ID を提供することにより、既存のサービスで新しいモデルインサイトを作成することを選択できます。

```json
"context": {
    "clientId": "f1ab3164-e688-433d-99ef-077b2be84731",
    "notebookId": "T4ab3164-e658-443d-97ef-022b2be84999",
    "experimentId": "5cb25a2d-2cbd-4c99-a619-8ddae5250a7b",
    "engineId": "22f4166f-85ba-4130-a995-a2b8e1edde32",
    "mlInstanceId": "46986c8f-7739-4376-8509-0178bdf32cda",
    "experimentRunId": "33408593-2871-4198-a812-6d1b7d939cda",
    "modelId": "15c53796-bd6b-4e09-b51d-7296aa20af71",
    "dataSetId": "5ee3cd7f2d34011913c56941"
  }
```

**API 形式**

```http
POST /insights
```

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/sensei/insights \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -H `Content-Type: application/vnd.adobe.platform.sensei+json;profile=mlInstance.v1.json`
    -d {
    "context": {
        "experimentId": "5cb25a2d-2cbd-4c99-a619-8ddae5250a7b",
        "experimentRunId": "33408593-2871-4198-a812-6d1b7d939cda",
        "modelId": "15c53796-bd6b-4e09-b51d-7296aa20af71"
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

正常な応答は、`{INSIGHT_ID}` と最初のリクエストで指定したパラメーターを含むペイロードを返します。

```json
{
    "id": "08b8d174-6b0d-4d7e-acd8-1c4c908e14b2",
    "context": {
        "experimentId": "5cb25a2d-2cbd-4c99-a619-8ddae5250a7b",
        "experimentRunId": "33408593-2871-4198-a812-6d1b7d939cda",
        "modelId": "15c53796-bd6b-4e09-b51d-7296aa20af71"
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
| `insightId` | POST リクエストが成功した場合に、この特定のインサイトに対して作成される一意の ID です。 |

## アルゴリズムのデフォルトリストの指標の取得

指標エンドポイントに対して単一の GET リクエストを実行することで、すべてのアルゴリズムとデフォルト指標のリストを取得できます。特定の指標をクエリするには、GET リクエストを作成し、リクエストパスで有効な `{ALGORITHM}` を指定します。

**API 形式**

```http
GET /insights/metrics
GET /insights/metrics?algorithm={ALGORITHM}
```

| パラメーター | 説明 |
| --- | --- |
| `{ALGORITHM}` | アルゴリズムタイプの識別子 |

**リクエスト**

次のリクエストは、クエリを含み、アルゴリズム識別子 `{ALGORITHM}` を使用して特定の指標を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/sensei/insights/metrics?algorithm={ALGORITHM}' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、`algorithm` 一意の ID とデフォルト指標の配列を含むペイロードを返します。

```json
{
    "children": [
        {
            "algorithm": "15c53796-bd6b-4e09-b51d-7296aa20af71",
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
