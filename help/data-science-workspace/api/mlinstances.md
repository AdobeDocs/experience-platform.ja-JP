---
keywords: Experience Platform;developer guide;endpoint;Data Science Workspace;popular topics
solution: Experience Platform
title: MLInstances
topic: Developer guide
translation-type: tm+mt
source-git-commit: 19823c7cf0459e045366f0baae2bd8a98416154c
workflow-type: tm+mt
source-wordcount: '575'
ht-degree: 4%

---


# MLInstances

MLInstanceは、既存の [エンジン](./engines.md) 、トレーニングパラメーター、スコアリングパラメーターまたはハードウェアリソース設定を定義する適切な設定セットとのペアです。

## MLInstanceの作成 {#create-an-mlinstance}

MLInstanceを作成するには、有効なエンジンID(`{ENGINE_ID}`)と適切なデフォルト設定のセットから成るリクエストペイロードを提供しながら、POSTリクエストを実行します。

Engine IDがPySparkまたはSpark Engineを参照する場合は、コア数やメモリ量などの計算リソースの量を設定できます。 Pythonエンジンを参照する場合は、トレーニングとスコアリングの目的でCPUまたはGPUを使用するかを選択できます。 詳細は、PySparkとSparkのリソース設定 [、](./appendix.md#resource-config) Python CPUとGPUの設定に関する付録の節を参照してください [](./appendix.md#cpu-gpu-config) 。

**API形式**

```http
POST /mlInstances
```

**リクエスト**

```shell
curl -X POST \
    https://platform.adobe.io/data/sensei/mlInstances \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -H 'content-type: application/vnd.adobe.platform.sensei+json;profile=mlInstance.v1.json' \
    -d '{
        "name": "A name for this MLInstance",
        "description": "A description for this MLInstance",
        "engineId": "{ENGINE_ID}",
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
| `name` | MLInstanceに必要な名前。 このMLInstanceに対応するモデルは、この値を継承し、UIにモデル名として表示されます。 |
| `description` | MLInstanceのオプションの説明です。 このMLInstanceに対応するモデルは、この値を継承し、モデルの説明としてUIに表示されます。 このプロパティが必要です。説明を指定しない場合は、値を空の文字列に設定します。 |
| `engineId` | 既存のエンジンのID。 |
| `tasks` | トレーニング、スコアリング、またはフィーチャパイプラインの設定のセット。 |

**応答**

成功した応答は、新たに作成されたMLInstanceの詳細を含むペイロードを返します。この詳細には、固有な識別子(`id`)が含まれます。

```json
{
    "id": "{MLINSTANCE_ID}",
    "name": "A name for this MLInstance",
    "description": "A description for this MLInstance",
    "engineId": "{ENGINE_ID}",
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

## MLInstancesのリストを取得する

単一のGETリクエストを実行して、MLInstancesのリストを取得できます。 結果をフィルターするのに役立つように、リクエストパスでクエリパラメーターを指定できます。 使用可能なクエリのリストについては、付録の「アセット取得の [クエリパラメータ」の節を参照してください](./appendix.md#query)。

**API形式**

```http
GET /mlInstances
GET /mlInstances?{QUERY_PARAMETER}={VALUE}
GET /mlInstances?{QUERY_PARAMETER_1}={VALUE_1}&{QUERY_PARAMETER_2}={VALUE_2}
```

| パラメーター | 説明 |
| --- | --- |
| `{QUERY_PARAMETER}` | 結果のフィルタリングに [使用できるクエリパラメーターの1つ](./appendix.md#query) 。 |
| `{VALUE}` | 前のクエリパラメーターの値。 |

**リクエスト**

```shell
curl -X GET \
    https://platform.adobe.io/data/sensei/mlInstances \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、MLInstancesとその詳細のリストを返します。

```json
{
    "children": [
        {
            "id": "{MLINSTANCE_ID}",
            "name": "A name for this MLInstance",
            "description": "A description for this MLInstance",
            "engineId": "{ENGINE_ID}",
            "created": "2019-01-01T00:00:00.000Z",
            "createdBy": {
                "displayName": "Jane Doe",
                "userId": "Jane_Doe@AdobeID"
            },
            "updated": "2019-01-01T00:00:00.000Z"
        },
        {
            "id": "{MLINSTANCE_ID}",
            "name": "Retail Sales Model",
            "description": "A Model created with the Retail Sales Recipe",
            "engineId": "{ENGINE_ID}",
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

## 特定のMLInstanceの取得 {#retrieve-specific}

特定のMLInstanceの詳細を取得するには、目的のMLInstanceのIDをリクエストパスに含むGETリクエストを実行します。

**API形式**

```http
GET /mlInstances/{MLINSTANCE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MLINSTANCE_ID}` | 目的のMLInstanceのID。 |

**リクエスト**

```shell
curl -X GET \
    https://platform.adobe.io/data/sensei/mlInstances/{MLINSTANCE_ID} \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、MLInstanceの詳細を返します。

```json
{
    "id": "{MLINSTANCE_ID}",
    "name": "A name for this MLInstance",
    "description": "A description for this MLInstance",
    "engineId": "{ENGINE_ID}",
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

## MLInstanceの更新

既存のMLInstanceを更新するには、要求パスにターゲットMLInstanceのIDが含まれるPUT要求を介してプロパティを上書きし、更新されたプロパティを含むJSONペイロードを提供します。

>[!TIP] このPUTリクエストを確実に成功させるために、まずGETリクエストを実行し、IDでMLInstanceを [取得することをお勧めします](#retrieve-specific)。 次に、返されたJSONオブジェクトを変更および更新し、変更されたJSONオブジェクト全体をPUT要求のペイロードとして適用します。

以下のサンプルAPI呼び出しは、MLInstanceのトレーニングパラメーターとスコアリングパラメーターを更新し、これらのプロパティを最初に持つようにします。

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

**API形式**

```http
PUT /mlInstances/{MLINSTANCE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MLINSTANCE_ID}` | 有効なMLInstance IDです。 |

**リクエスト**

```shell
curl -X PUT \
    https://platform.adobe.io/data/sensei/mlInstances/{MLINSTANCE_ID} \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
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

成功した応答は、MLInstanceの更新された詳細を含むペイロードを返します。

```json
{
    "id": "{MLINSTANCE_ID}",
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

## エンジンIDによるMLInstancesの削除

エンジンIDをクエリパラメーターとして含むDELETEリクエストを実行すると、同じエンジンを共有するすべてのMLInstanceを削除できます。

**API形式**

```http
DELETE /mlInstances?engineId={ENGINE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{ENGINE_ID}` | 有効なエンジンID。 |

**リクエスト**

```shell
curl -X DELETE \
    https://platform.adobe.io/data/sensei/mlInstances?engineId={ENGINE_ID} \
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
    "detail": "MLInstances successfully deleted"
}
```

## MLInstanceの削除

リクエストパスにターゲットMLInstanceのIDを含むDELETEリクエストを実行すると、1つのMLInstanceを削除できます。

**API形式**

```http
DELETE /mlInstances/{MLINSTANCE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MLINSTANCE_ID}` | 有効なMLInstance IDです。 |

**リクエスト**

```shell
curl -X DELETE \
    https://platform.adobe.io/data/sensei/mlInstances/{MLINSTANCE_ID} \
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
    "detail": "MLInstance deletion was successful"
}
```
