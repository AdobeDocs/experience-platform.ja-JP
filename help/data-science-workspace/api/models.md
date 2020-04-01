---
keywords: Experience Platform;developer guide;endpoint;Data Science Workspace;popular topics
solution: Experience Platform
title: モデル
topic: Developer guide
translation-type: tm+mt
source-git-commit: 01cfbc86516a05df36714b8c91666983f7a1b0e8

---


# モデル

モデルとは、ビジネスの使用事例に対して解決するための履歴データと設定を使用してトレーニングを受けた機械学習レシピのインスタンスです。

## モデルのリストの取得

/modelsに対して1つのGETリクエストを実行することで、すべてのモデルに属するモデルの詳細のリストを取得できます。 デフォルトでは、このリストは最も古く作成されたモデルから自動的に並べ替えられ、結果は25に制限されます。 一部のパラメーターを指定して、結果をフィルタークエリできます。 使用可能なクエリのリストについては、アセット取得のための [クエリパラメータの付録の節を参照してください](./appendix.md#query)。

**API形式**

```http
GET /models
```

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/sensei/models/ \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、各モデルの固有な識別子(`id`)を含むモデルの詳細を含むペイロードを返します。

```json
{
    "children": [
        {
            "id": "{MODEL_ID}",
            "name": "A name for this Model",
            "experimentId": "{EXPERIMENT_ID}",
            "experimentRunId": "{EXPERIMENT_RUN_ID}",
            "description": "A description for this Model",
            "modelArtifactUri": "wasb://test-models@mlpreprodstorage.blob.core.windows.net/model-name",
            "created": "2019-01-01T00:00:00.000Z",
            "createdBy": {
                "userId": "Jane_Doe@AdobeID"
            },
            "updated": "2019-01-02T00:00:00.000Z"
       },
        {
            "id": "{MODEL_ID}",
            "name": "Model 2",
            "experimentId": "{EXPERIMENT_ID}",
            "experimentRunId": "{EXPERIMENT_RUN_ID}",
            "description": "A description for Model2",
            "modelArtifactUri": "wasb://test-models@mlpreprodstorage.blob.core.windows.net/model-name",
            "created": "2019-01-01T00:00:00.000Z",
            "createdBy": {
                "userId": "Jane_Doe@AdobeID"
            },
            "updated": "2019-01-02T00:00:00.000Z"
       },
        {
            "id": "{MODEL_ID}",
            "name": "Model 3",
            "experimentId": "{EXPERIMENT_ID}",
            "experimentRunId": "{EXPERIMENT_RUN_ID}",
            "description": "A description for Model3",
            "modelArtifactUri": "wasb://test-models@mlpreprodstorage.blob.core.windows.net/model-name",
            "created": "2019-01-01T00:00:00.000Z",
            "createdBy": {
                "userId": "Jane_Doe@AdobeID"
            },
            "updated": "2019-01-02T00:00:00.000Z"
       },
    ],
    "_page": {
        "property": "deleted==false",
        "count": 3
    }
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | モデルに対応するID。 |
| `modelArtifactUri` | モデルが保存されている場所を示すURI。 URIはモデルの値で終 `name` わります。 |
| `experimentId` | 有効なテストID。 |
| `experimentRunId` | 有効なテスト実行ID。 |

## 特定のモデルの読み込み

単一のGETリクエストを実行し、リクエストパスに有効なモデルIDを指定することで、特定のモデルに属するモデルの詳細のリストを取得できます。 結果のフィルタリングに役立つように、リクエストパスでクエリパラメーターを指定できます。 使用可能なクエリのリストについては、アセット取得のための [クエリパラメータの付録の節を参照してください](./appendix.md#query)。

**API形式**

```http
GET /models/{MODEL_ID}
GET /models/?property=experimentRunID=={EXPERIMENT_RUN_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MODEL_ID}` | トレーニングされた、またはパブリッシュされたモデルの識別子。 |
| `{EXPERIMENT_RUN_ID}` | テスト実行の識別子。 |

**リクエスト**

次の要求にはクエリが含まれ、同じtestRunID ({TEST_RUN_ID})を共有するトレーニングを受けたモデルのリストが取得されます。

```shell
curl -X GET \
  https://platform.adobe.io/data/sensei/models/?property=experimentRunId=={EXPERIMENT_RUN_ID} \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、モデル固有の識別子(`id`)を含むモデルの詳細を含むペイロードを返します。

```json
{
    "children": [
        {
            "id": "{MODEL_ID}",
            "name": "A name for this Model",
            "experimentId": "{EXPERIMENT_ID}",
            "experimentRunId": "{EXPERIMENT_RUN_ID}",
            "description": "A description for this Model",
            "modelArtifactUri": "wasb://test-models@mlpreprodstorage.blob.core.windows.net/model-name",
            "created": "2019-01-01T00:00:00.000Z",
            "createdBy": {
                "userId": "Jane_Doe@AdobeID"
            },
            "updated": "2019-01-02T00:00:00.000Z"
       }
    ],
    "_page": {
        "property": "experimentRunId=={EXPERIMENT_RUN_ID},deleted==false",
        "count": 1
    }
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | モデルに対応するID。 |
| `modelArtifactUri` | モデルが保存されている場所を示すURI。 URIはモデルの値で終 `name` わります。 |
| `experimentId` | 有効なテストID。 |
| `experimentRunId` | 有効なテスト実行ID。 |

## IDによるモデルの更新

既存のモデルを更新するには、要求パスにターゲットモデルのIDを含むPUT要求を使用してプロパティを上書きし、更新されたプロパティを含むJSONペイロードを提供します。

>[!TIP] このPUTリクエストを確実に成功させるために、まずGETリクエストを実行して、IDでモデルを取得することをお勧めします。 次に、返されたJSONオブジェクトを変更および更新し、変更されたJSONオブジェクトの全体をPUT要求のペイロードとして適用します。

**API形式**

```http
PUT /models/{MODEL_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MODEL_ID}` | トレーニングされた、またはパブリッシュされたモデルの識別子。 |

**リクエスト**

```shell
curl -X PUT \
  https://platform.adobe.io/data/sensei/models/{MODEL_ID} \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'Content-Type: application/vnd.adobe.platform.sensei+json;profile=mlInstance.v1.json' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -d '{
        "id": "{MODEL_ID}",
        "name": "A name for this Model",
        "experimentId": "{EXPERIMENT_ID}",
        "experimentRunId": "{EXPERIMENT_RUN_ID}",
        "description": "An updated description for this Model",
        "modelArtifactUri": "wasb://test-models@mlpreprodstorage.blob.core.windows.net/model-name",
        "created": "2019-01-01T00:00:00.000Z",
        "createdBy": {
            "userId": "Jane_Doe@AdobeID"
        },
        "updated": "2019-01-02T00:00:00.000Z"
    }'
```

**応答**

成功した応答は、テストの更新された詳細を含むペイロードを返します。

```json
{
        "id": "{MODEL_ID}",
        "name": "A name for this Model",
        "experimentId": "{EXPERIMENT_ID}",
        "experimentRunId": "{EXPERIMENT_RUN_ID}",
        "description": "An updated description for this Model",
        "modelArtifactUri": "wasb://test-models@mlpreprodstorage.blob.core.windows.net/model-name",
        "created": "2019-01-01T00:00:00.000Z",
        "createdBy": {
            "userId": "Jane_Doe@AdobeID"
        },
        "updated": "2019-01-02T00:00:00.000Z"
    }
```

## IDによるモデルの削除

リクエストパスにターゲットモデルのIDを含むDELETEリクエストを実行すると、1つのモデルを削除できます。

**API形式**

```http
DELETE /models/{MODEL_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MODEL_ID}` | トレーニングされた、またはパブリッシュされたモデルの識別子。 |

**リクエスト**

```shell
curl -X DELETE \
  https://platform.adobe.io/data/sensei/models/{MODEL_ID} \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、モデルの削除を確認する200ステータスを含むペイロードを返します。

```json
{
    "title": "Success",
    "status": 200,
    "detail": "Model deletion was successful"
}
```
