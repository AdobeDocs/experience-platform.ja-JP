---
keywords: Experience Platform、開発者ガイド、エンドポイント、Data Science Workspace、人気の高いトピック、mlinstance、sensei 機械学習 api
solution: Experience Platform
title: MLInstance API エンドポイント
topic-legacy: Developer guide
description: MLInstance は、既存のエンジンと適切な設定セット（トレーニングパラメーター、スコアリングパラメーター、またはハードウェアリソース設定を定義する）とのペアリングです。
exl-id: e78cda69-1ff9-47ce-b25d-915de4633e11
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '619'
ht-degree: 96%

---

# MLInstance エンドポイント

MLInstance は、既存の[エンジン](./engines.md)と適切な設定セット（トレーニングパラメーター、スコアリングパラメーター、またはハードウェアリソース設定を定義する）とのペアリングです。

## MLInstance の作成 {#create-an-mlinstance}

MLInstance を作成するには、有効なエンジン ID（`{ENGINE_ID}`）と適切なデフォルト設定セットで構成されるリクエストペイロードを指定して POST リクエストを実行します。

エンジン ID が PySpark エンジンや Spark エンジンを参照する場合は、コア数やメモリ量など、計算リソースの量を設定できます。Python エンジンが参照される場合は、トレーニングとスコアリングのために、CPU と GPU のいずれかの使用を選択できます。詳しくは、「[PySpark と Spark のリソース設定](./appendix.md#resource-config)」と「[Python CPU と GPU の設定](./appendix.md#cpu-gpu-config)」の付録の節を参照してください。

**API 形式**

```http
POST /mlInstances
```

**リクエスト**

```shell
curl -X POST \
    https://platform.adobe.io/data/sensei/mlInstances \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -H 'content-type: application/vnd.adobe.platform.sensei+json;profile=mlInstance.v1.json' \
    -d '{
        "name": "A name for this MLInstance",
        "description": "A description for this MLInstance",
        "engineId": "22f4166f-85ba-4130-a995-a2b8e1edde32",
        "tasks": [
            {
                "name": "train",
                "parameters": [
                    {
                        "key": "training parameter",
                        "value": "parameter value"
                    }
                ]
            },
            {
                "name": "score",
                "parameters": [
                    {
                        "key": "scoring parameter",
                        "value": "parameter value"
                    }
                ]
            },
            {
                "name": "fp",
                "parameters": [
                    {
                        "key": "feature pipeline parameter",
                        "value": "parameter value"
                    }
                ]
            }
        ],
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | MLInstance の任意の名前。この MLInstance に対応するモデルは、この値を継承し、UI にモデルの名前として表示されます。 |
| `description` | MLInstance のオプションの説明。この MLInstance に対応するモデルは、この値を継承し、UI にモデルの説明として表示されます。このプロパティは必須です。説明を指定しない場合は、その値を空の文字列に設定します。 |
| `engineId` | 既存のエンジンの ID。 |
| `tasks` | トレーニング、スコアリング、または機能のパイプラインの一連の設定。 |

**応答**

成功した応答は、一意の ID（`id`）など、新たに作成された MLInstance の詳細が含まれるペイロードを返します。

```json
{
    "id": "46986c8f-7739-4376-8509-0178bdf32cda",
    "name": "A name for this MLInstance",
    "description": "A description for this MLInstance",
    "engineId": "22f4166f-85ba-4130-a995-a2b8e1edde32",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "userId": "Jane_Doe@AdobeID"
    },
    "updated": "2019-01-01T00:00:00.000Z",
    "tasks": [
        {
            "name": "train",
            "parameters": [
                {
                    "key": "training parameter",
                    "value": "parameter value"
                }
            ]
        },
        {
            "name": "score",
            "parameters": [
                {
                    "key": "scoring parameter",
                    "value": "parameter value"
                }
            ]
        },
        {
            "name": "fp",
            "parameters": [
                {
                    "key": "feature pipeline parameter",
                    "value": "parameter value"
                }
            ]
        }
    ]
}
```

## MLInstance のリストの取得

MLInstance のリストは、単一の GET リクエストを実行することで取得できます。結果のフィルター処理を支援するために、リクエストパスでクエリーパラメーターを指定できます。使用可能なクエリのリストについては、「[アセット取得のためのクエリーパラメーター](./appendix.md#query)」の付録の節を参照してください。

**API 形式**

```http
GET /mlInstances
GET /mlInstances?{QUERY_PARAMETER}={VALUE}
GET /mlInstances?{QUERY_PARAMETER_1}={VALUE_1}&{QUERY_PARAMETER_2}={VALUE_2}
```

| パラメーター | 説明 |
| --- | --- |
| `{QUERY_PARAMETER}` | 結果のフィルタリングに[使用可能なクエリパラメーター](./appendix.md#query)の 1 つです。 |
| `{VALUE}` | 上記クエリパラメーターの値です。 |

**リクエスト**

```shell
curl -X GET \
    https://platform.adobe.io/data/sensei/mlInstances \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、MLInstance のリストとその詳細を返します。

```json
{
    "children": [
        {
            "id": "46986c8f-7739-4376-8509-0178bdf32cda",
            "name": "A name for this MLInstance",
            "description": "A description for this MLInstance",
            "engineId": "22f4166f-85ba-4130-a995-a2b8e1edde32",
            "created": "2019-01-01T00:00:00.000Z",
            "createdBy": {
                "displayName": "Jane Doe",
                "userId": "Jane_Doe@AdobeID"
            },
            "updated": "2019-01-01T00:00:00.000Z"
        },
        {
            "id": "56986c8f-7739-4376-8509-0178bdf32cda",
            "name": "Retail Sales Model",
            "description": "A Model created with the Retail Sales Recipe",
            "engineId": "32f4166f-85ba-4130-a995-a2b8e1edde32",
            "created": "2019-01-01T00:00:00.000Z",
            "createdBy": {
                "displayName": "Jane Doe",
                "userId": "Jane_Doe@AdobeID"
            },
            "updated": "2019-01-01T00:00:00.000Z"
        }
    ],
    "_page": {
        "property": "deleted==false",
        "totalCount": 2,
        "count": 2
    }
}
```

## 特定の MLInstance の取得 {#retrieve-specific}

特定の MLInstance の詳細を取得するには、リクエストパスに目的の MLInstance の ID を含む GET リクエストを実行します。

**API 形式**

```http
GET /mlInstances/{MLINSTANCE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MLINSTANCE_ID}` | 目的の MLInstance の ID。 |

**リクエスト**

```shell
curl -X GET \
    https://platform.adobe.io/data/sensei/mlInstances/46986c8f-7739-4376-8509-0178bdf32cda \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、MLInstance の詳細を返します。

```json
{
    "id": "46986c8f-7739-4376-8509-0178bdf32cda",
    "name": "A name for this MLInstance",
    "description": "A description for this MLInstance",
    "engineId": "22f4166f-85ba-4130-a995-a2b8e1edde32",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "displayName": "Jane Doe",
        "userId": "Jane_Doe@AdobeID"
    },
    "updated": "2019-01-01T00:00:00.000Z",
    "tasks": [
        {
            "name": "train",
            "parameters": [
                {
                    "key": "training parameter",
                    "value": "parameter value"
                }
            ]
        },
        {
            "name": "score",
            "parameters": [
                {
                    "key": "scoring parameter",
                    "value": "parameter value"
                }
            ]
        },
        {
            "name": "featurePipeline",
            "parameters": [
                {
                    "key": "feature pipeline parameter",
                    "value": "parameter value"
                }
            ]
        }
    ]
}
```

## MLInstance の更新

既存の MLInstance を更新するには、リクエストパスにターゲット MLInstance の ID を含む PUT リクエストを実行してプロパティを上書きし、更新されたプロパティを含む JSON ペイロードを指定します。

>[!TIP]
>
> この PUT リクエストを確実に成功させるために、まず GET リクエストを実行し、[ID で MLInstance を取得する](#retrieve-specific)ことをお勧めします。次に、返された JSON オブジェクトを変更および更新し、変更された JSON オブジェクト全体を PUT リクエストのペイロードとして適用します。

以下のサンプル API 呼び出しは、MLInstance のトレーニングパラメーターとスコアリングパラメーターを更新し、これらのプロパティを最初に次のように設定します。

```json
{
    "name": "A name for this MLInstance",
    "description": "A description for this MLInstance",
    "engineId": "00000000-0000-0000-0000-000000000000",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "displayName": "Jane Doe",
        "userId": "Jane_Doe@AdobeID"
    },
    "tasks": [
        {
            "name": "train",
            "parameters": [
                {
                    "key": "learning_rate",
                    "value": "0.3"
                }
            ]
        },
        {
            "name": "score",
            "parameters": [
                {
                    "key": "output_dataset_id",
                    "value": "output-dataset-000"
                }
            ]
        }
    ]
}
```

**API 形式**

```http
PUT /mlInstances/{MLINSTANCE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MLINSTANCE_ID}` | 有効な MLInstance ID です。 |

**リクエスト**

```shell
curl -X PUT \
    https://platform.adobe.io/data/sensei/mlInstances/46986c8f-7739-4376-8509-0178bdf32cda \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'content-type: application/vnd.adobe.platform.sensei+json;profile=mlInstance.v1.json' \
    -d '{
        "name": "A name for this MLInstance",
        "description": "A description for this MLInstance",
        "engineId": "00000000-0000-0000-0000-000000000000",
        "created": "2019-01-01T00:00:00.000Z",
        "createdBy": {
            "displayName": "Jane Doe",
            "userId": "Jane_Doe@AdobeID"
        },
        "tasks": [
            {
                "name": "train",
                "parameters": [
                    {
                        "key": "learning_rate",
                        "value": "0.5"
                    }
                ]
            },
            {
                "name": "score",
                "parameters": [
                    {
                        "key": "output_dataset_id",
                        "value": "output-dataset-001"
                    }
                ]
            }
        ]
    }'
```

**応答**

成功した応答は、MLInstance の更新された詳細を含むペイロードを返します。

```json
{
    "id": "46986c8f-7739-4376-8509-0178bdf32cda",
    "name": "A name for this MLInstance",
    "description": "A description for this MLInstance",
    "engineId": "00000000-0000-0000-0000-000000000000",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "displayName": "Jane Doe",
        "userId": "Jane_Doe@AdobeID"
    },
    "updated": "2019-01-02T00:00:00.000Z",
    "tasks": [
        {
            "name": "train",
            "parameters": [
                {
                    "key": "learning_rate",
                    "value": "0.5"
                }
            ]
        },
        {
            "name": "score",
            "parameters": [
                {
                    "key": "output_dataset_id",
                    "value": "output-data-set-001"
                }
            ]
        }
    ]
}
```

## エンジン ID による MLInstance の削除

エンジン ID をクエリーパラメーターとして含む DELETE リクエストを実行すると、同じエンジンを共有するすべての MLInstance を削除できます。

**API 形式**

```http
DELETE /mlInstances?engineId={ENGINE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{ENGINE_ID}` | 有効なエンジン ID。 |

**リクエスト**

```shell
curl -X DELETE \
    https://platform.adobe.io/data/sensei/mlInstances?engineId=22f4166f-85ba-4130-a995-a2b8e1edde32 \
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
    "detail": "MLInstances successfully deleted"
}
```

## MLInstance の削除

リクエストパスにターゲット MLInstance の ID を含む DELETE リクエストを実行すると、1 つの MLInstance を削除できます。

**API 形式**

```http
DELETE /mlInstances/{MLINSTANCE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MLINSTANCE_ID}` | 有効な MLInstance ID です。 |

**リクエスト**

```shell
curl -X DELETE \
    https://platform.adobe.io/data/sensei/mlInstances/46986c8f-7739-4376-8509-0178bdf32cda \
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
    "detail": "MLInstance deletion was successful"
}
```
