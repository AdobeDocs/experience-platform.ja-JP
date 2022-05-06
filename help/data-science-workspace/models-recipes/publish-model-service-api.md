---
keywords: Experience Platform；モデルの公開；Data Science Workspace；人気の高いトピック；sensei 機械学習 api
solution: Experience Platform
title: Sensei Machine Learning API を使用したモデルのサービスとしての公開
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、Sensei Machine Learning API を使用して、モデルをサービスとして公開するプロセスについて説明します。
exl-id: f78b1220-0595-492d-9f8b-c3a312f17253
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '1516'
ht-degree: 47%

---

# を使用して、モデルをサービスとして公開する [!DNL Sensei Machine Learning API]

このチュートリアルでは、 [[!DNL Sensei Machine Learning API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml).

## はじめに

このチュートリアルでは、Adobe Experience Platform Data Science Workspace に関する十分な知識が必要です。 このチュートリアルを開始する前に、 [Data Science Workspace の概要](../home.md) を参照してください。

このチュートリアルに従うには、既存の ML エンジン、ML インスタンス、Experiment が必要です。 API でこれらを作成する手順については、 [パッケージ化されたレシピのインポート](./import-packaged-recipe-api.md).

最後に、このチュートリアルを開始する前に、 [はじめに](../api/getting-started.md) を正しく呼び出すために知っておく必要がある重要な情報については、開発者ガイドの「 」の節を参照してください。 [!DNL Sensei Machine Learning] API（このチュートリアル全体で使用される必要なヘッダーを含む）:

- `{ACCESS_TOKEN}`
- `{ORG_ID}`
- `{API_KEY}`

すべての POST、PUT、および PATCH リクエストには、次の追加ヘッダーが必要です。

- Content-Type: application/json

### キーワード

次の表に、このチュートリアルで使用される一般的な用語の概要を示します。

| 用語 | 定義 |
| --- | --- |
| **機械学習インスタンス（ML インスタンス）** | のインスタンス [!DNL Sensei] 特定のテナントのエンジン ( 特定のデータ、パラメーター、および [!DNL Sensei] コード。 |
| **Experiment** | トレーニング Experiment Run、スコアリングExperiment Run、またはその両方を保持するための包括的なエンティティ。 |
| **スケジュールに沿った Experiment** | トレーニング Experiment Run またはスコアリング Experiment Run の自動化を表す用語。これらの実験は、ユーザー定義のスケジュールに従って実行されます。 |
| **Experiment Run** | トレーニング Experiment やスコアリング Experiment の特定のインスタンス。特定の Experiment から複数の Experiment Run をおこなう場合、トレーニングやスコアリングに使用されるデータセット値が異なる場合があります。 |
| **トレーニング済みモデル** | モデルを検証、評価、および確定する前に、実験と機能の設計プロセスから作成された機械学習モデル。 |
| **公開済みモデル** | トレーニング、検証、および評価を経て確定された、バージョン管理されたモデル。 |
| **機械学習サービス（ML サービス）** | API エンドポイントを使用したトレーニングとスコアリングのオンデマンドリクエストをサポートするため、サービスとしてデプロイされた ML インスタンス。 ML サービスは、既存のトレーニング済み Experiment Run を使用して作成することもできます。 |

## 既存のトレーニング Experiment Run とスケジュールに沿ったスコアリングを使用して ML サービスを作成する

トレーニング Experiment Run を ML サービスとして公開する場合、スコアリング Experiment Run の詳細を指定して、POSTリクエストのペイロードをスコアリングのスケジュールを設定できます。 こうすると、スケジュールに沿ったスコアリング Experiment エンティティが作成されます。

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
  -H 'x-gw-ims-org-id: {ORG_ID}'
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
| `mlInstanceId` | 既存の ML インスタンス ID。ML サービスの作成に使用するトレーニング Experiment Run は、この特定の ML インスタンスに対応している必要があります。 |
| `trainingExperimentId` | ML インスタンスの ID に対応する Experiment ID。 |
| `trainingExperimentRunId` | ML サービスの公開に使用する特定のトレーニング Experiment Run。 |
| `scoringDataSetId` | スケジュールに沿ったスコアリング Experiment Run に使用する特定のデータセットを表す ID。 |
| `scoringTimeframe` | 分数を表す整数値。スコアリング Experiment Run に使用するデータをフィルタリングします。例えば、値 `10080` を指定すると、過去 10,080 分（168 時間）のデータが、スケジュールに沿ったスコアリング Experiment Run に使用されます。値 `0` を指定すると、データはフィルタリングされず、データセット内のすべてのデータがスコアリングに使用されます。 |
| `scoringSchedule` | スケジュールに沿ったスコアリング Experiment Run に関する詳細が含まれます。 |
| `scoringSchedule.startTime` | スコアリングを開始するタイミングを示す日時。 |
| `scoringSchedule.endTime` | スコアリングを開始するタイミングを示す日時。 |
| `scoringSchedule.cron` | Experiment Run のスコアを付ける間隔を示す Cron 値。 |

**応答**

正常な応答は、新しく作成された ML サービスの詳細（一意の ML サービスを含む）を返します `id` そして `scoringExperimentId` を返します。


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

## 既存の ML インスタンスから ML サービスを作成する

具体的な使用例や要件に応じて、トレーニング Experiment Run とスコアリング Experiment Run のスケジュールを設定し、ML サービスを ML インスタンスで柔軟に作成できます。 このチュートリアルでは、次のような特定のケースについて説明します。

- [スケジュールに沿ったトレーニングは必要ない一方で、スケジュールに沿ったスコアリングは必要な場合。](#ml-service-with-scheduled-experiment-for-scoring)
- [トレーニングとスコアの両方で、スケジュールに沿った Experiment Run が必要な場合。](#ml-service-with-scheduled-experiments-for-training-and-scoring)

ML サービスは、トレーニング Experiment やスコアリング Experiment のスケジュールを設定しなくても、ML インスタンスを使用して作成できます。 このような ML サービスでは、通常の Experiment エンティティと、トレーニングとスコアリングに対して 1 つの Experiment Run が作成されます。

### スケジュールに沿ったスコアリング Experiment を含む ML サービス {#ml-service-with-scheduled-experiment-for-scoring}

ML サービスを作成するには、スケジュールに沿ったスコアリング Experiment Run を含む ML インスタンスを公開します。この ML インスタンスは、トレーニング用の通常の Experiment エンティティを作成します。 トレーニング Experiment Run が生成され、スケジュールに沿ったすべてのスコアリング Experiment Run に使用されます。 MLサービスの作成に必要な `mlInstanceId`、`trainingDataSetId` および `scoringDataSetId` があること、これらが存在し、有効な値であることを確認します。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' 
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
| `scoringSchedule.startTime` | スコアリングを開始するタイミングを示す日時。 |
| `scoringSchedule.endTime` | スコアリングを開始するタイミングを示す日時。 |
| `scoringSchedule.cron` | Experiment Run のスコアを付ける間隔を示す Cron 値。 |

**応答**

正常な応答は、新しく作成された ML サービスの詳細を返します。 これには、サービスの一意の `id`、および `trainingExperimentId` および `scoringExperimentId` を取得する必要があります。

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

スケジュールに沿ったトレーニング Experiment Run とスコアリング Experiment Run を含む ML サービスとして既存の ML インスタンスを公開するには、トレーニングとスコアリングの両方のスケジュールを指定する必要があります。 この設定の ML サービスを作成すると、トレーニングとスコアの両方にスケジュールに沿った Experiment エンティティが作成されます。 トレーニングとスコアリングのスケジュールが同じである必要はありません。スコアリングジョブの実行中に、スケジュールに沿ったトレーニング Experiment Run によって生成された最新のトレーニング済みモデルが取得され、スケジュールに沿ったスコアリングの実行に使用されます。

**API 形式**

```http
POST /mlServices
```

**リクエスト**

```SHELL
curl -X POST 'https://platform.adobe.io/data/sensei/mlServices' 
  -H 'Authorization: Bearer {ACCESS_TOKEN}' 
  -H 'x-api-key: {API_KEY}' 
  -H 'x-gw-ims-org-id: {ORG_ID}' 
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
| `scoringSchedule.startTime` | スコアリングを開始するタイミングを示す日時。 |
| `scoringSchedule.endTime` | スコアリングを開始するタイミングを示す日時。 |
| `scoringSchedule.cron` | Experiment Run のスコアを付ける間隔を示す Cron 値。 |

**応答**

正常な応答は、新しく作成された ML サービスの詳細を返します。 これには、サービスの一意の `id`、および `trainingExperimentId` および `scoringExperimentId` に含まれる値を格納します。 以下のレスポンスの例では、 `trainingSchedule` および `scoringSchedule` は、トレーニングとスコアリングの Experiment エンティティがスケジュールに沿った Experiment であることを示しています。

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

## ML サービスの検索 {#retrieving-ml-services}

既存の ML サービスを検索するには、 `GET` ～を要求する `/mlServices` そして、一意の `id` を設定します。

**API 形式**

```http
GET /mlServices/{SERVICE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SERVICE_ID}` | 一意の `id` 検索する ML サービスの |

**リクエスト**

```SHELL
curl -X GET 'https://platform.adobe.io/data/sensei/mlServices/{SERVICE_ID}' 
  -H 'Authorization: Bearer {ACCESS_TOKEN}' 
  -H 'x-api-key: {API_KEY}' 
  -H 'x-gw-ims-org-id: {ORG_ID}' 
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、ML サービスの詳細を返します。

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
>異なる ML サービスを取得すると、キーと値のペアの数が多い、または少ない応答が返される場合があります。 上記のレスポンスは、[スケジュールに沿ったトレーニング Experiment Run とスコアリング Experiment Run の両方を含む ML サービス](#ml-service-with-scheduled-experiments-for-training-and-scoring)を表したものです。


## トレーニングまたはスコアリングのスケジュール

公開済みの ML サービスでスコアリングとトレーニングのスケジュールを設定するには、 `PUT` リクエストオン `/mlServices`.

**API 形式**

```http
PUT /mlServices/{SERVICE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SERVICE_ID}` | 一意の `id` 更新する ML サービスの |

**リクエスト**

次のリクエストは、 `trainingSchedule` および `scoringSchedule` それぞれのキー `startTime`, `endTime`、および `cron` キー。

```SHELL
curl -X PUT 'https://platform.adobe.io/data/sensei/mlServices/{SERVICE_ID}' 
  -H 'Authorization: {ACCESS_TOKEN}' 
  -H 'x-api-key: {API_KEY}' 
  -H 'x-gw-ims-org-id: {ORG_ID}' 
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
>この `startTime` 既存のスケジュール済みトレーニングジョブとスコアリングジョブの `startTime` を変更する必要がある場合は、同じモデルを公開して、トレーニングジョブとスコアリングジョブのスケジュールを再設定することを検討してください。

**応答** 

正常な応答は、更新された ML サービスの詳細を返します。

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
