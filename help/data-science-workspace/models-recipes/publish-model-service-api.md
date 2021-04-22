---
keywords: Experience Platform；モデルの発行；Data Science Workspace；人気の高いトピック；Senesie機械学習api
solution: Experience Platform
title: Senesie Machine Learning APIを使用したサービスとしてのモデルの公開
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、Senesie Machine Learning APIを使用して、モデルをサービスとして公開するプロセスについて説明します。
exl-id: f78b1220-0595-492d-9f8b-c3a312f17253
translation-type: tm+mt
source-git-commit: a6d047d52dad085ba662bd684c896bdffe3eef2e
workflow-type: tm+mt
source-wordcount: '1516'
ht-degree: 47%

---

# [!DNL Sensei Machine Learning API]

このチュートリアルでは、[[!DNL Sensei Machine Learning API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml)を使用して、モデルをサービスとしてパブリッシュするプロセスについて説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformデータサイエンスワークスペースに関する実用的な理解が必要です。 このチュートリアルを始める前に、[データサイエンスワークスペースの概要](../home.md)で、サービスの概要を確認してください。

このチュートリアルに従うには、既存のMLエンジン、MLインスタンス、およびテストが必要です。 APIでこれらを作成する手順については、[パッケージ化されたレシピの読み込み](./import-packaged-recipe-api.md)のチュートリアルを参照してください。

最後に、このチュートリアルを始める前に、[!DNL Sensei Machine Learning] APIの呼び出しを成功させるために知っておく必要のある重要な情報について、開発者ガイドの[はじめに](../api/getting-started.md)の節を参照してください。

- `{ACCESS_TOKEN}`
- `{IMS_ORG}`
- `{API_KEY}`

すべての POST、PUT、および PATCH リクエストには、次の追加ヘッダーが必要です。

- Content-Type: application/json

### キーワード

次の表に、このチュートリアルで使用される一般的な用語の概要を示します。

| 用語 | 定義 |
| --- | --- |
| **機械学習インスタンス（ML インスタンス）** | 特定のテナント用の[!DNL Sensei]エンジンのインスタンス。特定のデータ、パラメーター、および[!DNL Sensei]コードを含みます。 |
| **Experiment** | トレーニング Experiment Run、スコアリングExperiment Run、またはその両方を保持するための包括的なエンティティ。 |
| **スケジュールに沿った Experiment** | トレーニング Experiment Run またはスコアリング Experiment Run の自動化を表す用語。これらの実験は、ユーザー定義のスケジュールに従って実行されます。 |
| **Experiment Run** | トレーニング Experiment やスコアリング Experiment の特定のインスタンス。特定の Experiment から複数の Experiment Run をおこなう場合、トレーニングやスコアリングに使用されるデータセット値が異なる場合があります。 |
| **トレーニング済みモデル** | モデルを検証、評価、および確定する前に、実験と機能の設計プロセスから作成された機械学習モデル。 |
| **公開済みモデル** | トレーニング、検証、および評価を経て確定された、バージョン管理されたモデル。 |
| **機械学習サービス（ML サービス）** | APIエンドポイントを使用したトレーニングとスコアリングのオンデマンドリクエストをサポートするために、サービスとしてデプロイされたMLインスタンス。 MLサービスは、トレーニングを受けた既存のテストの実行を使用して作成することもできます。 |

## 既存のトレーニングテストの実行とスケジュール済みスコアを含むMLサービスの作成

トレーニングテスト実行をMLサービスとして発行する場合、スコアリングテストの詳細を入力し、POSTリクエストのペイロードを実行することで、スコアリングをスケジュールできます。 こうすると、スケジュールに沿ったスコアリング Experiment エンティティが作成されます。

**API 形式**

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
| --- | --- |
| `mlInstanceId` | 既存のMLインスタンスのID。MLサービスの作成に使用するトレーニング・テストの実行は、この特定のMLインスタンスに対応する必要があります。 |
| `trainingExperimentId` | MLインスタンスの識別に対応するテストの識別。 |
| `trainingExperimentRunId` | MLサービスの公開に使用するトレーニング実験の実行。 |
| `scoringDataSetId` | スケジュールに沿ったスコアリング Experiment Run に使用する特定のデータセットを表す ID。 |
| `scoringTimeframe` | 分数を表す整数値。スコアリング Experiment Run に使用するデータをフィルタリングします。例えば、値 `10080` を指定すると、過去 10,080 分（168 時間）のデータが、スケジュールに沿ったスコアリング Experiment Run に使用されます。値 `0` を指定すると、データはフィルタリングされず、データセット内のすべてのデータがスコアリングに使用されます。 |
| `scoringSchedule` | スケジュールに沿ったスコアリング Experiment Run に関する詳細が含まれます。 |
| `scoringSchedule.startTime` | 開始スコアリングを行うタイミングを示す日時。 |
| `scoringSchedule.endTime` | 開始スコアリングを行うタイミングを示す日時。 |
| `scoringSchedule.cron` | テストの実行にスコアを付ける間隔を示すCron値。 |

**応答**

成功した応答は、新たに作成されたMLサービスの詳細を返します。この詳細には、対応するスコアリングテストの一意の`id`と`scoringExperimentId`が含まれます。


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

- [スケジュールに沿ったトレーニングは必要ない一方で、スケジュールに沿ったスコアリングは必要な場合。](#ml-service-with-scheduled-experiment-for-scoring)
- [トレーニングとスコアの両方で、スケジュールに沿った Experiment Run が必要な場合。](#ml-service-with-scheduled-experiments-for-training-and-scoring)

MLサービスは、トレーニングやスコアリングの実験をスケジュールせずに、MLインスタンスを使用して作成できます。 このようなMLサービスは、通常のテストエンティティと、トレーニングとスコアリングのための1つのテスト実行を作成します。

### スケジュールに沿ったスコアリング Experiment を含む ML サービス {#ml-service-with-scheduled-experiment-for-scoring}

MLサービスを作成するには、スコアリングのために予定されたテスト実行を含むMLインスタンスを発行します。これにより、トレーニング用の通常のテストエンティティが作成されます。 トレーニングテストの実行が生成され、スケジュールされたすべてのスコアリングテストの実行に使用されます。 MLサービスの作成に必要な `mlInstanceId`、`trainingDataSetId` および `scoringDataSetId` があること、これらが存在し、有効な値であることを確認します。

**API 形式**

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

| JSON キー | 説明 |
| --- | --- |
| `mlInstanceId` | 既存の ML インスタンスの ID。ML サービスの作成に使用する ML インスタンスを表します。 |
| `trainingDataSetId` | トレーニング Experiment に使用する特定のデータセットを表す ID。 |
| `trainingTimeframe` | 分数を表す整数値。トレーニング Experiment に使用するデータをフィルタリングします。例えば、値 `"10080"` を指定すると、過去 10,080 分（168 時間）のデータがトレーニング Experiment Run に使用されます。値 `"0"` を指定すると、データはフィルタリングされず、データセット内のすべてのデータがトレーニングに使用されます。 |
| `scoringDataSetId` | スケジュールに沿ったスコアリング Experiment Run に使用する特定のデータセットを表す ID。 |
| `scoringTimeframe` | 分数を表す整数値。スコアリング Experiment Run に使用するデータをフィルタリングします。例えば、値 `"10080"` を指定すると、過去 10,080 分（168 時間）のデータが、スケジュールに沿ったスコアリング Experiment Run に使用されます。値 `"0"` を指定すると、データはフィルタリングされず、データセット内のすべてのデータがスコアリングに使用されます。 |
| `scoringSchedule` | スケジュールに沿ったスコアリング Experiment Run に関する詳細が含まれます。 |
| `scoringSchedule.startTime` | 開始スコアリングを行うタイミングを示す日時。 |
| `scoringSchedule.endTime` | 開始スコアリングを行うタイミングを示す日時。 |
| `scoringSchedule.cron` | テストの実行にスコアを付ける間隔を示すCron値。 |

**応答**

正常に応答すると、新しく作成されたMLサービスの詳細が返されます。 これには、サービス固有の`id`と、対応するトレーニングとスコアリングの実験の`trainingExperimentId`と`scoringExperimentId`が含まれます。

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

### スケジュールに沿ったトレーニングおよびスコアリング Experiment を含む ML サービス {#ml-service-with-scheduled-experiments-for-training-and-scoring}

既存のMLインスタンスをMLサービスとして発行し、スケジュールされたトレーニングとスコアリングテストの実行を行うには、トレーニングとスコアリングの両方のスケジュールを指定する必要があります。 この設定のMLサービスが作成されると、トレーニングとスコアリングの両方に対してスケジュールされたテストエンティティも作成されます。 トレーニングとスコアリングのスケジュールが同じである必要はありません。スコアリングジョブの実行中に、スケジュールに沿ったトレーニング Experiment Run によって生成された最新のトレーニング済みモデルが取得され、スケジュールに沿ったスコアリングの実行に使用されます。

**API 形式**

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

| JSON キー | 説明 |
| --- | --- |
| `mlInstanceId` | 既存の ML インスタンスの ID。ML サービスの作成に使用する ML インスタンスを表します。 |
| `trainingDataSetId` | トレーニング Experiment に使用する特定のデータセットを表す ID。 |
| `trainingTimeframe` | 分数を表す整数値。トレーニング Experiment に使用するデータをフィルタリングします。例えば、値 `"10080"` を指定すると、過去 10,080 分（168 時間）のデータがトレーニング Experiment Run に使用されます。値 `"0"` を指定すると、データはフィルタリングされず、データセット内のすべてのデータがトレーニングに使用されます。 |
| `scoringDataSetId` | スケジュールに沿ったスコアリング Experiment Run に使用する特定のデータセットを表す ID。 |
| `scoringTimeframe` | 分数を表す整数値。スコアリング Experiment Run に使用するデータをフィルタリングします。例えば、値 `"10080"` を指定すると、過去 10,080 分（168 時間）のデータが、スケジュールに沿ったスコアリング Experiment Run に使用されます。値 `"0"` を指定すると、データはフィルタリングされず、データセット内のすべてのデータがスコアリングに使用されます。 |
| `trainingSchedule` | スケジュールに沿ったトレーニング Experiment Run に関する詳細が含まれます。 |
| `scoringSchedule` | スケジュールに沿ったスコアリング Experiment Run に関する詳細が含まれます。 |
| `scoringSchedule.startTime` | 開始スコアリングを行うタイミングを示す日時。 |
| `scoringSchedule.endTime` | 開始スコアリングを行うタイミングを示す日時。 |
| `scoringSchedule.cron` | テストの実行にスコアを付ける間隔を示すCron値。 |

**応答**

正常に応答すると、新しく作成されたMLサービスの詳細が返されます。 これには、サービス固有の`id`と、対応するトレーニングとスコアリングの実験の`trainingExperimentId`と`scoringExperimentId`が含まれます。 以下の応答例では、`trainingSchedule`と`scoringSchedule`の存在が示しているので、トレーニングとスコアリングのテストエンティティが「Everiments」というスケジュールになっていることが示されています。

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

## MLサービスの検索{#retrieving-ml-services}

`/mlServices`に`GET`リクエストを作成し、パスにMLサービスの一意の`id`を提供することで、既存のMLサービスを検索できます。

**API 形式**

```http
GET /mlServices/{SERVICE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SERVICE_ID}` | 検索しているMLサービスの一意の`id`。 |

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

>[!NOTE]
>
>異なるMLサービスを取得すると、キーと値のペアの数が多いか少ない応答が返される場合があります。 上記のレスポンスは、[スケジュールに沿ったトレーニング Experiment Run とスコアリング Experiment Run の両方を含む ML サービス](#ml-service-with-scheduled-experiments-for-training-and-scoring)を表したものです。


## トレーニングまたはスコアリングのスケジュール

既に発行済みのMLサービスに対するスコアリングとトレーニングをスケジュールする場合は、`/mlServices`の`PUT`リクエストを使用して既存のMLサービスを更新します。

**API 形式**

```http
PUT /mlServices/{SERVICE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SERVICE_ID}` | 更新するMLサービスの一意の`id`。 |

**リクエスト**

次のリクエストは、`trainingSchedule`キーと`scoringSchedule`キーをそれぞれ`startTime`キー、`endTime`キー、`cron`キーと共に追加して、既存のMLサービスのトレーニングとスコアリングをスケジュールします。

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

>[!WARNING]
>
>既存の予定されているトレーニングジョブとスコアリングジョブの`startTime`を変更しないでください。 `startTime` を変更する必要がある場合は、同じモデルを公開して、トレーニングジョブとスコアリングジョブのスケジュールを再設定することを検討してください。

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
