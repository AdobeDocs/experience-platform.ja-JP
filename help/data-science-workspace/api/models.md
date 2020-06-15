---
keywords: Experience Platform;developer guide;endpoint;Data Science Workspace;popular topics
solution: Experience Platform
title: モデル
topic: Developer guide
translation-type: tm+mt
source-git-commit: 33f8c424c208bb61319b49e7ecb30e3144ef108a
workflow-type: tm+mt
source-wordcount: '821'
ht-degree: 4%

---


# モデル

モデルは、機械学習手法のインスタンスで、過去のデータと構成を使用してトレーニングを受け、ビジネスの使用事例に対して解決します。

## モデルのリストの取得

/modelsに対して1回のGETリクエストを実行することで、すべてのモデルに属するモデル詳細のリストを取得できます。 デフォルトでは、このリストは最も古い作成モデルから順に並べられ、結果は25に制限されます。 一部のクエリパラメーターを指定して、結果をフィルターできます。 使用可能なクエリのリストについては、付録の「アセット取得の [クエリパラメータ」の節を参照してください](./appendix.md#query)。

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

成功した応答は、各モデル固有の識別子(`id`)を含むモデルの詳細を含むペイロードを返します。

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
| `id` | モデルに対応するID。 |
| `modelArtifactUri` | モデルが保存されている場所を示すURI。 URIの末尾はモデルの `name` 値です。 |
| `experimentId` | 有効なテストID。 |
| `experimentRunId` | 有効なテスト実行ID。 |

## 特定のモデルの取得

単一のGETリクエストを実行し、リクエストパスに有効なモデルIDを指定することで、特定のモデルに属するモデルの詳細のリストを取得できます。 結果をフィルターするのに役立つように、リクエストパスでクエリパラメーターを指定できます。 使用可能なクエリのリストについては、付録の「アセット取得の [クエリパラメータ」の節を参照してください](./appendix.md#query)。

**API形式**

```http
GET /models/{MODEL_ID}
GET /models/?property=experimentRunID=={EXPERIMENT_RUN_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MODEL_ID}` | トレーニングを受けたモデルまたはパブリッシュされたモデルの識別子。 |
| `{EXPERIMENT_RUN_ID}` | テスト実行の識別子。 |

**リクエスト**

次のリクエストにはクエリが含まれ、同じtestRunID ({TEST_RUN_ID})を共有するトレーニングを受けたモデルのリストが取得されます。

```shell
curl -X GET \
  https://platform.adobe.io/data/sensei/models/?property=experimentRunId==33408593-2871-4198-a812-6d1b7d939cda \
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
| `id` | モデルに対応するID。 |
| `modelArtifactUri` | モデルが保存されている場所を示すURI。 URIの末尾はモデルの `name` 値です。 |
| `experimentId` | 有効なテストID。 |
| `experimentRunId` | 有効なテスト実行ID。 |

## 事前生成されたモデルの登録 {#register-a-model}

エンドポイントにPOSTリクエストを行うことで、事前生成されたモデルを登録でき `/models` ます。 モデルを登録するには、 `modelArtifact` ファイルと `model` プロパティの値を要求の本文に含める必要があります。

**API形式**

```http
POST /models
```

**リクエスト**

次のPOSTには、必要な `modelArtifact` ファイルと `model` プロパティの値が含まれています。 これらの値について詳しくは、次の表を参照してください。

```shell
curl -X POST \
  https://platform.adobe.io/data/sensei/models \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -F 'modelArtifact=@/Users/yourname/Desktop/model.onnx' \
    -F 'model={
            "name": "Your Model - 0615-1342-45",
            "originType": "offline"
    }'
```

| パラメーター | 説明 |
| --- | --- |
| `modelArtifact` | 含める完全なモデル加工品の位置。 |
| `model` | 作成する必要があるModelオブジェクトのフォームデータです。 |

**応答**

成功した応答は、モデル固有の識別子(`id`)を含むモデルの詳細を含むペイロードを返します。

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
| `id` | モデルに対応するID。 |
| `modelArtifactUri` | モデルが保存されている場所を示すURI。 URIは、モデルの `id` 値で終わります。 |

## IDによるモデルの更新

リクエストパスにターゲットモデルのIDを含むPUT要求を介してプロパティを上書きし、更新されたプロパティを含むJSONペイロードを提供することで、既存のモデルを更新できます。

>[!TIP] このPUT要求を確実に成功させるために、最初にGET要求を実行して、IDでモデルを取得することをお勧めします。 次に、返されたJSONオブジェクトを変更および更新し、変更されたJSONオブジェクト全体をPUT要求のペイロードとして適用します。

**API形式**

```http
PUT /models/{MODEL_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MODEL_ID}` | トレーニングを受けたモデルまたはパブリッシュされたモデルの識別子。 |

**リクエスト**

```shell
curl -X PUT \
  https://platform.adobe.io/data/sensei/models/15c53796-bd6b-4e09-b51d-7296aa20af71 \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'Content-Type: application/vnd.adobe.platform.sensei+json;profile=mlInstance.v1.json' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
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

成功した応答は、テストの更新された詳細を含むペイロードを返します。

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

## IDによるモデルの削除

リクエストパスにターゲットモデルのIDを含むDELETEリクエストを実行すると、1つのモデルを削除できます。

**API形式**

```http
DELETE /models/{MODEL_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MODEL_ID}` | トレーニングを受けたモデルまたはパブリッシュされたモデルの識別子。 |

**リクエスト**

```shell
curl -X DELETE \
  https://platform.adobe.io/data/sensei/models/15c53796-bd6b-4e09-b51d-7296aa20af71 \
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

## モデル用の新しいトランスコードの作成 {#create-transcoded-model}

トランスコードとは、あるエンコーディングから別のエンコーディングへの直接のデジタルからデジタルへの変換です。 新しい出力を作成するには、とを指定し、モデル用に新しいトランスコード `{MODEL_ID}``targetFormat` を作成します。

**API形式**

```http
POST /models/{MODEL_ID}/transcodings
```

| パラメーター | 説明 |
| --- | --- |
| `{MODEL_ID}` | トレーニングを受けたモデルまたはパブリッシュされたモデルの識別子。 |

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/sensei/models/15c53796-bd6b-4e09-b51d-7296aa20af71/transcodings \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: text/plain' \
    -D '{
 "id": "491a3be5-1d32-4541-94d5-cd1cd07affb5",
 "modelId" : "15c53796-bd6b-4e09-b51d-7296aa20af71",
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

正常な応答が返されると、トランスコードの情報を含むJSONオブジェクトを含むペイロードが返されます。 これには、特定のトランスコードされたモデルを`id`取得する際に使用されるトランスコード固有識別子( [)が含まれます](#retrieve-transcoded-model)。

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

モデルに対して実行されたトランスコーディングのリストを取得するには、GETリクエストを実行し `{MODEL_ID}`ます。

**API形式**

```http
GET /models/{MODEL_ID}/transcodings
```

| パラメーター | 説明 |
| --- | --- |
| `{MODEL_ID}` | トレーニングを受けたモデルまたはパブリッシュされたモデルの識別子。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/sensei/models/15c53796-bd6b-4e09-b51d-7296aa20af71/transcodings \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、モデルで実行された各トランスコードのリストを含むjsonオブジェクトを含むペイロードを返します。 トランスコードされた各モデルは、一意の識別子(`id`)を受け取ります。

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

## 特定のトランスコードされたモデルの取得 {#retrieve-transcoded-model}

トランスコードされた特定のモデルを取得するには、トランスコードされたモデルのIDとGETリクエストを実行 `{MODEL_ID}` します。

**API形式**

```http
GET /models/{MODEL_ID}/transcodings/{TRANSCODING_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MODEL_ID}` | トレーニングを受けた、またはパブリッシュされたモデルの一意の識別子。 |
| `{TRANSCODING_ID}` | トランスコードされたモデルの一意の識別子。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/sensei/models/15c53796-bd6b-4e09-b51d-7296aa20af71/transcodings/460aa5a1-e972-455d-b8dc-4bc6cd91edb6 \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、トランスコードされたモデルのデータを含むJSONオブジェクトを含むペイロードを返します。

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


