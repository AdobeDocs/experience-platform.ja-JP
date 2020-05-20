---
keywords: Experience Platform;publish a model;Data Science Workspace;popular topics
solution: Experience Platform
title: モデルをサービスとして公開(API)
topic: Tutorial
translation-type: tm+mt
source-git-commit: 967ca85efba315819c6241d034dc3c25a5b1fc70
workflow-type: tm+mt
source-wordcount: '1487'
ht-degree: 1%

---


# モデルをサービスとして公開(API)

このチュートリアルでは、 [Senesi Machine Learning APIを使用して、モデルをサービスとして公開するプロセスについて説明します](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml)。

## はじめに

このチュートリアルでは、Adobe Experience Platform Data Science Workspaceに関する十分な理解が必要です。 このチュートリアルを始める前に、 [Data Science Workspaceの概要](../home.md) 、サービスの概要を確認してください。

このチュートリアルに従うには、既存のMLエンジン、MLインスタンス、およびテストが必要です。 APIでこれらを作成する手順については、パッケージ化されたレシピの [読み込みに関するチュートリアルを参照してください](./import-packaged-recipe-api.md)。

最後に、このチュートリアルを始める前に、開発者ガイドの [「はじめに](../api/getting-started.md) 」の節を参照し、Senesi Machine Learning APIを正しく呼び出すために必要な重要な情報を確認してください。このチュートリアルでは必須のヘッダーを含みます。

- `{ACCESS_TOKEN}`
- `{IMS_ORG}`
- `{API_KEY}`

すべてのPOST、PUT、およびPATCHリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

### キーワード

次の表に、このチュートリアルで使用される一般的な用語の概要を示します。

| 用語 | 定義 |
--- | ---
| **機械学習インスタンス（MLインスタンス）** | 特定のデータ、パラメーター、先生コードを含む、特定のテナント用の先生エンジンのインスタンス。 |
| **テスト** | トレーニングテストの実行、テストの実行のスコアリング、またはその両方を保持するための傘のエンティティ。 |
| **予定された実験** | テストの実行のトレーニングまたはスコアリングの自動化を表す用語で、ユーザー定義のスケジュールによって制御されます。 |
| **テストの実行** | トレーニングまたはスコアリングの実験の特定の例。 特定のテストからの複数のテストの実行では、トレーニングやスコアリングに使用するデータセットの値が異なる場合があります。 |
| **トレーニングモデル** | 検証、評価、確定されたモデルに到達する前に、エンジニアリングの実験と機能を試すプロセスによって作成された機械学習モデル。 |
| **発行済みモデル** | トレーニング、検証、評価の後、最終的でバージョン管理されたモデルが届きました。 |
| **機械学習サービス（MLサービス）** | APIエンドポイントを使用したトレーニングとスコアリングのオンデマンドリクエストをサポートするために、サービスとしてデプロイされたMLインスタンス。 MLサービスは、トレーニングを受けた既存のテストの実行を使用して作成することもできます。 |

## 既存のトレーニングテストの実行とスケジュール済みスコアを含むMLサービスの作成

トレーニングテスト実行をMLサービスとして発行する場合、スコアリングテストの詳細を入力し、スコアリングをスケジュールできます。POSTリクエストのペイロードを実行します。 これにより、スコアリング用にスケジュールされたテストエンティティが作成されます。

**API形式**

```http
POST /mlServices
```

**リクエスト**

```SHELL
curl -X POST 
  https://platform.adobe.io/data/sensei/mlServices
  -H 'Authorization: {ACCESS_TOKEN}' 
  -H 'x-api-key: {API_KEY}' 
  -H 'x-gw-ims-org-id: {IMS_ORG}'
  -H 'Content-Type: application/json'
  -d '{
        "name": "Service name",
        "description": "Service description",
        "trainingExperimentId": "c4155146-b38f-4a8b-86d8-1de3838c8d87",
        "trainingExperimentRunId": "5c5af39c73fcec153117eed1",
        "scoringDataSetId": "5c5af39c73fcec153117eed1",
        "scoringTimeframe": "20000",
        "scoringSchedule": {
          "startTime": "2019-04-09T00:00",
          "endTime": "2019-04-10T00:00",
          "cron": "10 * * * *"
        }
      }'
```

| プロパティ | 説明 |
--- | ---
| `mlInstanceId` | 既存のMLインスタンスのID。MLサービスの作成に使用するトレーニング・テストの実行は、この特定のMLインスタンスに対応する必要があります。 |
| `trainingExperimentId` | MLインスタンスの識別に対応するテストの識別。 |
| `trainingExperimentRunId` | MLサービスの公開に使用するトレーニング実験の実行。 |
| `scoringDataSetId` | スケジュール済みスコアリングテストの実行に使用される特定のデータセットを参照する識別。 |
| `scoringTimeframe` | テストの実行のスコアリングに使用するデータをフィルタリングする時間（分）を表す整数値です。 例えば、値が、過去10080分または168時間のデータが、スケジュールされた各スコアリングテストの実行に使用されます。 `10080` の値ではデータがフィルターされ `0` ず、データセット内のすべてのデータがスコアリングに使用されます。 |
| `scoringSchedule` | スケジュール済みスコアリングテストの実行に関する詳細が含まれます。 |
| `scoringSchedule.startTime` | 開始スコアリングを行うタイミングを示す日時。 |
| `scoringSchedule.endTime` | 開始スコアリングを行うタイミングを示す日時。 |
| `scoringSchedule.cron` | テストの実行にスコアを付ける間隔を示すCron値。 |

**応答**

成功した応答は、新たに作成されたMLサービスの詳細を返します。この詳細には、その独自の値と、対応するスコアリングテスト `id` の値 `scoringExperimentId` が含まれます。


```JSON
{
  "id": "string",
  "name": "string",
  "description": "string",
  "mlInstanceId": "string",
  "trainingExperimentId": "string",
  "trainingExperimentRunId": "string",
  "scoringExperimentId": "string",
  "scoringDataSetId": "string",
  "scoringTimeframe": "integer",
  "scoringSchedule": {
    "startTime": "2019-03-13T00:00",
    "endTime": "2019-03-14T00:00",
    "cron": "30 * * * *"
  },
  "created": "2019-04-08T14:45:25.981Z",
  "updated": "2019-04-08T14:45:25.981Z"
}
```

## 既存のMLインスタンスからのMLサービスの作成

具体的な使用事例や要件に応じて、MLインスタンスを使用したMLサービスの作成は、トレーニングのスケジュール設定とテストの実行のスコアリングの観点から柔軟に行うことができます。 このチュートリアルでは、次のような特定のケースについて説明します。

- [スケジュールされたトレーニングは必要ありませんが、スケジュールされたスコアリングは必要です。](#ml-service-with-scheduled-experiment-for-scoring)
- [トレーニングとスコアリングの両方で、予定されたテストの実行が必要です。](#ml-service-with-scheduled-experiments-for-training-and-scoring)

MLサービスは、トレーニングやスコアリングの実験をスケジュールせずに、MLインスタンスを使用して作成できます。 このようなMLサービスは、通常のテストエンティティと、トレーニングとスコアリングのための1つのテスト実行を作成します。

### スコアリングのための予定されたテストを含むMLサービス {#ml-service-with-scheduled-experiment-for-scoring}

MLサービスを作成するには、スコアリングのために予定されたテスト実行を含むMLインスタンスを発行します。これにより、トレーニング用の通常のテストエンティティが作成されます。 トレーニングテストの実行が生成され、スケジュールされたすべてのスコアリングテストの実行に使用されます。 MLサービスの作成に必要な、 `mlInstanceId`、 `trainingDataSetId`および `scoringDataSetId` があり、それらが存在し、有効な値であることを確認します。

**API形式**

```http
POST /mlServices
```

**リクエスト**

```SHELL
curl -X POST 
  https://platform.adobe.io/data/sensei/mlServices
  -H 'Authorization: {ACCESS_TOKEN}' 
  -H 'x-api-key: {API_KEY}' 
  -H 'x-gw-ims-org-id: {IMS_ORG}' 
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -d '{
        "name": "Service name",
        "description": "Service description",
        "mlInstanceId": "c4155146-b38f-4a8b-86d8-1de3838c8d87",
        "trainingDataSetId": "5c5af39c73fcec153117eed1",
        "trainingTimeframe": "10000",
        "scoringDataSetId": "5c5af39c73fcec153117eed1",
        "scoringTimeframe": "20000",
        "scoringSchedule": {
          "startTime": "2019-04-09T00:00",
          "endTime": "2019-04-10T00:00",
          "cron": "10 * * * *"
        }
      }'
```

| JSONキー | 説明 |
| --- | --- |
| `mlInstanceId` | MLサービスの作成に使用されるMLインスタンスを表す、既存のMLインスタンスの識別。 |
| `trainingDataSetId` | トレーニングテストに使用する特定のデータセットを参照する識別。 |
| `trainingTimeframe` | トレーニングテストに使用するデータをフィルターする時間（分）を表す整数値です。 例えば、の値は、過去10080分または168時間のデータがトレーニング・テスト・ランに使用されることを `"10080"` 意味します。 の値ではデータがフィルタリングされないので、データセット内のすべてのデータがトレーニングに使用されます。 `"0"` |
| `scoringDataSetId` | スケジュール済みスコアリングテストの実行に使用される特定のデータセットを参照する識別。 |
| `scoringTimeframe` | テストの実行のスコアリングに使用するデータをフィルタリングする時間（分）を表す整数値です。 例えば、値が、過去10080分または168時間のデータが、スケジュールされた各スコアリングテストの実行に使用されます。 `"10080"` の値ではデータがフィルターされ `"0"` ず、データセット内のすべてのデータがスコアリングに使用されます。 |
| `scoringSchedule` | スケジュール済みスコアリングテストの実行に関する詳細が含まれます。 |
| `scoringSchedule.startTime` | 開始スコアリングを行うタイミングを示す日時。 |
| `scoringSchedule.endTime` | 開始スコアリングを行うタイミングを示す日時。 |
| `scoringSchedule.cron` | テストの実行にスコアを付ける間隔を示すCron値。 |

**応答**

正常に応答すると、新しく作成されたMLサービスの詳細が返されます。 これには、サービス独自のもの `id`に加え、対応するトレーニング `trainingExperimentId` とスコアリングの実験が含ま `scoringExperimentId` れます。

```JSON
{
  "id": "string",
  "name": "string",
  "description": "string",
  "mlInstanceId": "string",
  "trainingExperimentId": "string",
  "trainingDataSetId": "string",
  "trainingTimeframe": "integer",
  "scoringExperimentId": "string",
  "scoringDataSetId": "string",
  "scoringTimeframe": "integer",
  "scoringSchedule": {
    "startTime": "2019-04-09T00:00",
    "endTime": "2019-04-10T00:00",
    "cron": "10 * * * *"
  },
  "created": "2019-04-09T08:58:10.956Z",
  "updated": "2019-04-09T08:58:10.956Z"
}
```

### トレーニングとスコアリングのための予定された実験を含むMLサービス {#ml-service-with-scheduled-experiments-for-training-and-scoring}

既存のMLインスタンスをMLサービスとして発行し、スケジュールされたトレーニングとスコアリングテストの実行を行うには、トレーニングとスコアリングの両方のスケジュールを指定する必要があります。 この設定のMLサービスが作成されると、トレーニングとスコアリングの両方に対してスケジュールされたテストエンティティも作成されます。 トレーニングとスコアリングのスケジュールは同じである必要はありません。 スコアリング・ジョブの実行中に、予定されたトレーニング・テストの実行によって生成された最新のトレーニング・モデルが取得され、スケジュールされたスコアリングの実行に使用されます。

**API形式**

```http
POST /mlServices
```

**リクエスト**

```SHELL
curl -X POST 'https://platform-int.adobe.io/data/sensei/mlServices' 
  -H 'Authorization: Bearer {ACCESS_TOKEN}' 
  -H 'x-api-key: {API_KEY}' 
  -H 'x-gw-ims-org-id: {IMS_ORG}' 
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -d '{
        "name": "string",
        "description": "string",
        "mlInstanceId": "string",
        "trainingDataSetId": "string",
        "trainingTimeframe": "string",
        "scoringDataSetId": "string",
        "scoringTimeframe": "string",
        "trainingSchedule": {
          "startTime": "2019-04-09T00:00",
          "endTime": "2019-04-10T00:00",
          "cron": "10 * * * *"
        },
        "scoringSchedule": {
          "startTime": "2019-04-09T00:00",
          "endTime": "2019-04-10T00:00",
          "cron": "10 * * * *"
        }
      }'
```

| JSONキー | 説明 |
| --- | --- |
| `mlInstanceId` | MLサービスの作成に使用されるMLインスタンスを表す、既存のMLインスタンスの識別。 |
| `trainingDataSetId` | トレーニングテストに使用する特定のデータセットを参照する識別。 |
| `trainingTimeframe` | トレーニングテストに使用するデータをフィルターする時間（分）を表す整数値です。 例えば、の値は、過去10080分または168時間のデータがトレーニング・テスト・ランに使用されることを `"10080"` 意味します。 の値ではデータがフィルタリングされないので、データセット内のすべてのデータがトレーニングに使用されます。 `"0"` |
| `scoringDataSetId` | スケジュール済みスコアリングテストの実行に使用される特定のデータセットを参照する識別。 |
| `scoringTimeframe` | テストの実行のスコアリングに使用するデータをフィルタリングする時間（分）を表す整数値です。 例えば、値が、過去10080分または168時間のデータが、スケジュールされた各スコアリングテストの実行に使用されます。 `"10080"` の値ではデータがフィルターされ `"0"` ず、データセット内のすべてのデータがスコアリングに使用されます。 |
| `trainingSchedule` | 予定されているトレーニングテストの実行に関する詳細が含まれます。 |
| `scoringSchedule` | スケジュール済みスコアリングテストの実行に関する詳細が含まれます。 |
| `scoringSchedule.startTime` | 開始スコアリングを行うタイミングを示す日時。 |
| `scoringSchedule.endTime` | 開始スコアリングを行うタイミングを示す日時。 |
| `scoringSchedule.cron` | テストの実行にスコアを付ける間隔を示すCron値。 |

**応答**

正常に応答すると、新しく作成されたMLサービスの詳細が返されます。 これには、サービス独自のトレーニング `id`とスコアリングの実験が含まれ `trainingExperimentId` 、それぞれ `scoringExperimentId` に対応するトレーニングとスコアリングの実験が含まれます。 以下の応答例では、の存在 `trainingSchedule` と `scoringSchedule` 示唆に基づいて、トレーニングとスコアリングのテストエンティティが「Everiments」というスケジュールに従っています。

```JSON
{
  "id": "string",
  "name": "string",
  "description": "string",
  "mlInstanceId": "string",
  "trainingExperimentId": "string",
  "trainingDataSetId": "string",
  "trainingTimeframe": "integer",
  "scoringExperimentId": "string",
  "scoringDataSetId": "string",,
  "scoringTimeframe": "integer",
  "trainingSchedule": {
    "startTime": "2019-04-09T00:00",
    "endTime": "2019-04-10T00:00",
    "cron": "10 * * * *"
  },
  "scoringSchedule": {
    "startTime": "2019-04-09T00:00",
    "endTime": "2019-04-10T00:00",
    "cron": "10 * * * *"
  },
  "created": "2019-04-09T08:58:10.956Z",
  "updated": "2019-04-09T08:58:10.956Z"
}
```

## MLサービスの検索 {#retrieving-ml-services}

既存のMLサービスを検索するには、に `GET` リクエストを作成し、パスに一意のMLサービス `/mlServices``id` を提供します。

**API形式**

```http
GET /mlServices/{SERVICE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SERVICE_ID}` | 検索してい `id` るMLサービスの一意の名前。 |

**リクエスト**

```SHELL
curl -X GET 'https://platform.adobe.io/data/sensei/mlServices/{SERVICE_ID}' 
  -H 'Authorization: Bearer {ACCESS_TOKEN}' 
  -H 'x-api-key: {API_KEY}' 
  -H 'x-gw-ims-org-id: {IMS_ORG}' 
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、MLサービスの詳細を返します。

```JSON
{
  "id": "string",
  "name": "string",
  "description": "string",
  "mlInstanceId": "string",
  "trainingExperimentId": "string",
  "trainingDataSetId": "string",
  "trainingTimeframe": "integer",
  "scoringExperimentId": "string",
  "scoringDataSetId": "string",
  "scoringTimeframe": "integer",
  "trainingSchedule": {
    "startTime": "2019-04-09T00:00",
    "endTime": "2019-04-10T00:00",
    "cron": "10 * * * *"
  },
  "scoringSchedule": {
    "startTime": "2019-04-09T00:00",
    "endTime": "2019-04-10T00:00",
    "cron": "10 * * * *"
  },
  "created": "2019-05-13T23:46:03.478Z",
  "updated": "2019-05-13T23:46:03.478Z"
}
```

>[!NOTE] 異なるMLサービスを取得すると、キーと値のペアの数が多いか少ない応答が返される場合があります。 上記の回答は、スケジュールされたトレーニングとスコアリングテストの実行の両方を含む [MLサービスを表したものです](#ml-service-with-scheduled-experiments-for-training-and-scoring)。


## トレーニングまたはスコアリングのスケジュール設定

既に発行済みのMLサービスに対するスコアリングとトレーニングをスケジュールする場合は、既存のMLサービスを、に対する `PUT` 要求で更新することで実行でき `/mlServices`ます。

**API形式**

```http
PUT /mlServices/{SERVICE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SERVICE_ID}` | 更新 `id` するMLサービスの一意の値。 |

**リクエスト**

次のリクエストは、既存のMLサービスのトレーニングとスコアリングをスケジュールします。これには、キーとキーをそれぞれのキー、キー、キーと共 `trainingSchedule` に追加します `scoringSchedule``startTime``endTime``cron` 。

```SHELL
curl -X PUT 'https://platform.adobe.io/data/sensei/mlServices/{SERVICE_ID}' 
  -H 'Authorization: {ACCESS_TOKEN}' 
  -H 'x-api-key: {API_KEY}' 
  -H 'x-gw-ims-org-id: {IMS_ORG}' 
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -d '{
        "name": "string",
        "description": "string",
        "mlInstanceId": "string",
        "trainingExperimentId": "string",
        "trainingDataSetId": "string",
        "trainingTimeframe": "integer",
        "scoringExperimentId": "string",
        "scoringDataSetId": "string",
        "scoringTimeframe": "integer",
        "trainingSchedule": {
          "startTime": "2019-04-09T00:00",
          "endTime": "2019-04-11T00:00",
          "cron": "20 * * * *"
        },
        "scoringSchedule": {
          "startTime": "2019-04-09T00:00",
          "endTime": "2019-04-11T00:00",
          "cron": "20 * * * *"
        }
      }'
```

>[!WARNING] 既存のスケジュール済みトレーニングジョブとスコアリングジョブ `startTime` に対して、変更を行わないでください。 を変更する `startTime` 必要がある場合は、同じモデルを公開し、トレーニングおよびスコアリングジョブを再スケジュールすることを検討します。

**応答**

正常に応答すると、更新されたMLサービスの詳細が返されます。

```JSON
{
  "id": "string",
  "name": "string",
  "description": "string",
  "mlInstanceId": "string",
  "trainingExperimentId": "string",
  "trainingDataSetId": "string",
  "trainingTimeframe": "integer",
  "scoringExperimentId": "string",
  "scoringDataSetId": "string",
  "scoringTimeframe": "integer",
  "trainingSchedule": {
    "startTime": "2019-04-09T00:00",
    "endTime": "2019-04-11T00:00",
    "cron": "20 * * * *"
  },
  "scoringSchedule": {
    "startTime": "2019-04-09T00:00",
    "endTime": "2019-04-11T00:00",
    "cron": "20 * * * *"
  },
  "created": "2019-04-09T08:58:10.956Z",
  "updated": "2019-04-09T09:43:55.563Z"
}
```
