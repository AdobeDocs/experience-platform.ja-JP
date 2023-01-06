---
keywords: Experience Platform、開発者ガイド、エンドポイント、Data Science Workspace、人気の高いトピック、モデル、sensei 機械学習 API
solution: Experience Platform
title: モデル API エンドポイント
description: モデルとは、履歴データと設定を使用してトレーニングされた機械学習レシピのインスタンスであり、ビジネス上の使用例について解決します。
exl-id: e66119a9-9552-497c-9b3a-b64eb3b51fcf
source-git-commit: 86e6924078c115fb032ce39cd678f1d9c622e297
workflow-type: tm+mt
source-wordcount: '864'
ht-degree: 65%

---

# モデルエンドポイント

モデルは、履歴データと構成を使用してビジネス使用例を解決するために訓練される機械学習レシピのインスタンスです。

## モデルのリストの取得

/models に対して単一の GET リクエストを実行することにより、すべてのモデルに属するモデル詳細のリストを取得できます。デフォルトでは、このリストは最も古く作成されたモデルから自動的に並べ替えられ、結果は 25 に制限されます。クエリパラメーターを指定して、結果をフィルターできます。使用可能なクエリのリストについては、付録の「[アセット取得のためのクエリパラメーター](./appendix.md#query)」の節を参照してください。

**API 形式**

```http
GET /models
```

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/sensei/models/ \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、各モデルの固有な識別子（`id`）を含むモデルの詳細を含むペイロードを返します。

```json
{
    "children": [
        {
            "id": "15c53796-bd6b-4e09-b51d-7296aa20af71",
            "name": "A name for this Model",
            "experimentId": "5cb25a2d-2cbd-4c99-a619-8ddae5250a7b",
            "experimentRunId": "33408593-2871-4198-a812-6d1b7d939cda",
            "description": "A description for this Model",
            "modelArtifactUri": "wasb://test-models@mlpreprodstorage.blob.core.windows.net/model-name",
            "created": "2019-01-01T00:00:00.000Z",
            "createdBy": {
                "userId": "Jane_Doe@AdobeID"
            },
            "updated": "2019-01-02T00:00:00.000Z"
       },
        {
            "id": "27c53796-bd6b-4u59-b51d-7296aa20er23",
            "name": "Model 2",
            "experimentId": "3cb25a2d-2cbd-4d34-a619-8ddae5259a5t",
            "experimentRunId": "33408593-2871-4198-a812-6d1b7d939cda",
            "description": "A description for Model2",
            "modelArtifactUri": "wasb://test-models@mlpreprodstorage.blob.core.windows.net/model-name",
            "created": "2019-01-01T00:00:00.000Z",
            "createdBy": {
                "userId": "Jane_Doe@AdobeID"
            },
            "updated": "2019-01-02T00:00:00.000Z"
       },
        {
            "id": "15c53796-bd6b-4e09-b51d-7296aa20af71",
            "name": "Model 3",
            "experimentId": "5cb25a2d-2cbd-4c99-a619-8ddae5250a7b",
            "experimentRunId": "33408593-2871-4198-a812-6d1b7d939cda",
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
| `id` | モデルに対応する ID。 |
| `modelArtifactUri` | モデルが保存されている場所を示す URI。URI はモデルの `name` 値で終わります。 |
| `experimentId` | 有効な実験 ID。 |
| `experimentRunId` | 有効な実験実行 ID。 |

## 特定のモデルの取得

単一の GET リクエストを実行し、リクエストパスに有効なモデル ID を指定することで、特定のモデルに属するモデルの詳細のリストを取得できます。結果のフィルタリングに役立つように、リクエストパスでクエリパラメーターを指定できます。使用可能なクエリのリストについては、「[アセット取得のためのクエリーパラメーター](./appendix.md#query)」の付録の節を参照してください。

**API 形式**

```http
GET /models/{MODEL_ID}
GET /models/?property=experimentRunID=={EXPERIMENT_RUN_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MODEL_ID}` | 訓練済みまたは公開済みモデルの識別子。 |
| `{EXPERIMENT_RUN_ID}` | 実験実行の識別子。 |

**リクエスト**

次のリクエストにはクエリが含まれており、同じ ExperimentRunID（{EXPERIMENT_RUN_ID}）を共有する訓練済みモデルのリストを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/sensei/models/?property=experimentRunId==33408593-2871-4198-a812-6d1b7d939cda \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、モデル固有の識別子（`id`）を含むモデルの詳細を含むペイロードを返します。

```json
{
    "children": [
        {
            "id": "15c53796-bd6b-4e09-b51d-7296aa20af71",
            "name": "A name for this Model",
            "experimentId": "5cb25a2d-2cbd-4c99-a619-8ddae5250a7b",
            "experimentRunId": "33408593-2871-4198-a812-6d1b7d939cda",
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
        "property": "experimentRunId==33408593-2871-4198-a812-6d1b7d939cda,deleted==false",
        "count": 1
    }
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | モデルに対応する ID。 |
| `modelArtifactUri` | モデルが保存されている場所を示す URI。URI はモデルの `name` 値で終わります。 |
| `experimentId` | 有効な実験 ID。 |
| `experimentRunId` | 有効な実験実行 ID。 |

## 事前生成されたモデルの登録 {#register-a-model}

生成済みのモデルを登録するには、 `/models` endpoint. モデルを登録するには、 `modelArtifact` ファイルと `model` プロパティの値をリクエストの本文に含める必要があります。

**API 形式**

```http
POST /models
```

**リクエスト**

次のPOSTには、 `modelArtifact` ファイルと `model` 必要なプロパティ値です。 これらの値の詳細については、次の表を参照してください。

```shell
curl -X POST \
  https://platform.adobe.io/data/sensei/models \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -F 'modelArtifact=@/Users/yourname/Desktop/model.onnx' \
    -F 'model={
            "name": "Your Model - 0615-1342-45",
            "originType": "offline"
    }'
```

| パラメーター | 説明 |
| --- | --- |
| `modelArtifact` | 含める完全なモデルアーティファクトの場所。 |
| `model` | 作成する必要がある Model オブジェクトのフォームデータです。 |

**応答**

正常な応答は、モデル固有の識別子（`id`）を含むモデルの詳細を含むペイロードを返します。

```json
{
  "id": "a28f151a-597a-4a7e-87e9-1c1dbc9c2af7",
  "name": "Your Model - 0615-1342-45",
  "originType": "offline",
  "modelArtifactUri": "http://storageblobml.blob.core.windows.net/prod-models/a28f151a-597a-4a7e-87e9-1c1dbc9c2af7",
  "created": "2020-06-15T20:55:41.520Z",
  "updated": "2020-06-15T20:55:41.520Z",
  "deprecated": false
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | モデルに対応する ID。 |
| `modelArtifactUri` | モデルが保存されている場所を示す URI。URI の末尾が `id` モデルの値。 |

## ID によるモデルの更新

既存のモデルを更新するには、リクエストパスにターゲットモデルの ID を含む PUT リクエストを使用してプロパティを上書きし、更新されたプロパティを含む JSON ペイロードを提供します。

>[!TIP]
>
> この PUT リクエストを確実に成功させるために、まず GET リクエストを実行して、ID でモデルを取得することをお勧めします。次に、返された JSON オブジェクトを変更および更新し、変更された JSON オブジェクト全体を PUT リクエストのペイロードとして指定します。

**API 形式**

```http
PUT /models/{MODEL_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MODEL_ID}` | 訓練済みまたは公開済みモデルの識別子。 |

**リクエスト**

```shell
curl -X PUT \
  https://platform.adobe.io/data/sensei/models/15c53796-bd6b-4e09-b51d-7296aa20af71 \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'Content-Type: application/vnd.adobe.platform.sensei+json;profile=mlInstance.v1.json' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -d '{
        "id": "15c53796-bd6b-4e09-b51d-7296aa20af71",
        "name": "A name for this Model",
        "experimentId": "5cb25a2d-2cbd-4c99-a619-8ddae5250a7b",
        "experimentRunId": "33408593-2871-4198-a812-6d1b7d939cda",
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

正常な応答は、実験の更新された詳細を含むペイロードを返します。

```json
{
        "id": "15c53796-bd6b-4e09-b51d-7296aa20af71",
        "name": "A name for this Model",
        "experimentId": "5cb25a2d-2cbd-4c99-a619-8ddae5250a7b",
        "experimentRunId": "33408593-2871-4198-a812-6d1b7d939cda",
        "description": "An updated description for this Model",
        "modelArtifactUri": "wasb://test-models@mlpreprodstorage.blob.core.windows.net/model-name",
        "created": "2019-01-01T00:00:00.000Z",
        "createdBy": {
            "userId": "Jane_Doe@AdobeID"
        },
        "updated": "2019-01-02T00:00:00.000Z"
    }
```

## ID によるモデルの削除

リクエストパスにターゲットモデルの ID を含む DELETE リクエストを実行すると、単一のモデルを削除できます。

**API 形式**

```http
DELETE /models/{MODEL_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MODEL_ID}` | 訓練済みまたは公開済みモデルの識別子。 |

**リクエスト**

```shell
curl -X DELETE \
  https://platform.adobe.io/data/sensei/models/15c53796-bd6b-4e09-b51d-7296aa20af71 \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、モデルの削除を確認する 200 ステータスを含むペイロードを返します。

```json
{
    "title": "Success",
    "status": 200,
    "detail": "Model deletion was successful"
}
```

## モデルの新しいトランスコーディングの作成 {#create-transcoded-model}

トランスコーディングとは、あるエンコーディングから別のエンコーディングへの直接のデジタル変換です。 モデルの新しいトランスコーディングを作成するには、 `{MODEL_ID}` および `targetFormat` 新しい出力を含めたい場合。

**API 形式**

```http
POST /models/{MODEL_ID}/transcodings
```

| パラメーター | 説明 |
| --- | --- |
| `{MODEL_ID}` | 訓練済みまたは公開済みモデルの識別子。 |

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/sensei/models/15c53796-bd6b-4e09-b51d-7296aa20af71/transcodings \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: text/plain' \
    -D '{
 "id": "491a3be5-1d32-4541-94d5-cd1cd07affb5",
 "modelId": "15c53796-bd6b-4e09-b51d-7296aa20af71",
 "targetFormat": "CoreML",
 "created": "2019-12-16T19:59:08.360Z",
 "createdBy": {
    "userId": "FDD760CD5CD467380A495FE2@AdobeID"
 },
 "updated": "2019-12-19T18:37:43.696Z",
 "deleted": false,
}'
```

**応答**

正常な応答は、トランスコードの情報を含む JSON オブジェクトを含むペイロードを返します。 これには、トランスコーディングの一意の識別子 (`id`) で使用 [特定のトランスコード済みモデルの取得](#retrieve-transcoded-model).

```json
{
  "id": "491a3be5-1d32-4541-94d5-cd1cd07affb5",
  "modelId": "15c53796-bd6b-4e09-b51d-7296aa20af71",
  "targetFormat": "CoreML",
  "created": "2020-06-12T22:01:55.886Z",
  "createdBy": {
    "userId": "FDD760CD5CD467380A495FE2@AdobeID"
  },
  "updated": "2020-06-12T22:01:55.886Z",
  "deleted": false
}
```

## モデルのトランスコーディングのリストの取得 {#retrieve-transcoded-model-list}

モデルで実行されたトランスコーディングのリストを取得するには、 `{MODEL_ID}`.

**API 形式**

```http
GET /models/{MODEL_ID}/transcodings
```

| パラメーター | 説明 |
| --- | --- |
| `{MODEL_ID}` | 訓練済みまたは公開済みモデルの識別子。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/sensei/models/15c53796-bd6b-4e09-b51d-7296aa20af71/transcodings \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、モデルで実行された各トランスコードのリストを含む json オブジェクトを含むペイロードを返します。 トランスコードされた各モデルは、一意の識別子 (`id`) をクリックします。

```json
{
    "children": [
        {
            "id": "460aa5a1-e972-455d-b8dc-4bc6cd91edb6",
            "modelId": "15c53796-bd6b-4e09-b51d-7296aa20af71",
            "created": "2019-12-20T01:07:50.978Z",
            "createdBy": {
                "userId": "FDD760CD5CD467380A495FE2@AdobeID"
            },
            "updated": "2019-12-20T01:07:50.978Z",
            "deprecated": false
        },
        {
            "id": "bdb3e4c2-4702-4045-86b4-17ee40df91cc",
            "modelId": "15c53796-bd6b-4e09-b51d-7296aa20af71",
            "created": "2019-12-20T17:48:26.473Z",
            "createdBy": {
                "userId": "FDD760CD5CD467380A495FE2@AdobeID"
            },
            "updated": "2019-12-20T17:48:26.473Z",
            "deprecated": false
        }
    ],
    "_page": {
        "property": "modelId==15c53796-bd6b-4e09-b51d-7296aa20af71,deleted==false,deprecated==false",
        "count": 2
    }
}
```

## 特定のトランスコード済みモデルの取得 {#retrieve-transcoded-model}

特定のトランスコード済みモデルを取得するには、 `{MODEL_ID}` トランスコードされたモデルの id。

**API 形式**

```http
GET /models/{MODEL_ID}/transcodings/{TRANSCODING_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MODEL_ID}` | トレーニング済みまたは公開済みのモデルの一意の ID。 |
| `{TRANSCODING_ID}` | トランスコードされたモデルの一意の識別子。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/sensei/models/15c53796-bd6b-4e09-b51d-7296aa20af71/transcodings/460aa5a1-e972-455d-b8dc-4bc6cd91edb6 \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、トランスコードされたモデルのデータを含む JSON オブジェクトを含むペイロードを返します。

```json
{
    "id": "460aa5a1-e972-455d-b8dc-4bc6cd91edb6",
    "modelId": "15c53796-bd6b-4e09-b51d-7296aa20af71",
    "created": "2019-12-20T01:07:50.978Z",
    "createdBy": {
        "userId": "FDD760CD5CD467380A495FE2@AdobeID"
    },
    "updated": "2019-12-20T01:07:50.978Z",
    "deprecated": false
}
```
