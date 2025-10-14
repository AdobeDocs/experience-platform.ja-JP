---
keywords: Experience Platform；モデルの公開；Data Science Workspace；人気のトピック；sensei machine learning api
solution: Experience Platform
title: Sensei機械学習 API を使用したサービスとしてのモデルのPublish
type: Tutorial
description: このチュートリアルでは、Sensei Machine Learning API を使用してサービスとしてモデルを公開するプロセスについて説明します。
exl-id: f78b1220-0595-492d-9f8b-c3a312f17253
source-git-commit: 863889984e5e77770638eb984e129e720b3d4458
workflow-type: tm+mt
source-wordcount: '1541'
ht-degree: 44%

---

# [!DNL Sensei Machine Learning API] を使用したサービスとしてのモデルのPublish

>[!NOTE]
>
>Data Science Workspaceは購入できなくなりました。
>
>このドキュメントは、Data Science Workspaceの以前の使用権限を持つ既存のお客様を対象としています。

このチュートリアルでは、[[!DNL Sensei Machine Learning API]](https://developer.adobe.com/experience-platform-apis/references/sensei-machine-learning/) を使用してモデルをサービスとして公開するプロセスについて説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platform Data Science Workspaceに関する十分な知識が必要です。 このチュートリアルを開始する前に、[Data Science Workspaceの概要 &#x200B;](../home.md) を参照して、サービスの概要を確認してください。

このチュートリアルを進めるには、既存の ML エンジン、ML インスタンス、および実験が必要です。 API でこれらを作成する手順については、[&#x200B; パッケージ化されたレシピの読み込み &#x200B;](./import-packaged-recipe-api.md) に関するチュートリアルを参照してください。

最後に、このチュートリアルを開始する前に、デベロッパーガイドの [&#x200B; はじめに &#x200B;](../api/getting-started.md) の節を参照して、このチュートリアルで使用される必須ヘッダーなど、[!DNL Sensei Machine Learning] API の呼び出しを正常に行うために必要となる重要な情報を確認してください。

- `{ACCESS_TOKEN}`
- `{ORG_ID}`
- `{API_KEY}`

すべての POST、PUT、および PATCH リクエストには、次の追加ヘッダーが必要です。

- Content-Type: application/json

### キーワード

このチュートリアルで使用される一般的な用語の概要を次の表に示します。

| 用語 | 定義 |
| --- | --- |
| **機械学習インスタンス（ML インスタンス）** | 特定のデータ、パラメーター、[!DNL Sensei] コードを含む、特定のテナント用の [!DNL Sensei] Engine のインスタンス。 |
| **Experiment** | トレーニング Experiment Run、スコアリングExperiment Run、またはその両方を保持するための包括的なエンティティ。 |
| **スケジュールに沿った Experiment** | トレーニング Experiment Run またはスコアリング Experiment Run の自動化を表す用語。これらの実験は、ユーザー定義のスケジュールに従って実行されます。 |
| **Experiment Run** | トレーニング Experiment やスコアリング Experiment の特定のインスタンス。特定の Experiment から複数の Experiment Run をおこなう場合、トレーニングやスコアリングに使用されるデータセット値が異なる場合があります。 |
| **トレーニング済みモデル** | モデルを検証、評価、および確定する前に、実験と機能の設計プロセスから作成された機械学習モデル。 |
| **公開済みモデル** | トレーニング、検証、および評価を経て確定された、バージョン管理されたモデル。 |
| **機械学習サービス（ML サービス）** | API エンドポイントを使用して、トレーニングとスコアリングのオンデマンドリクエストをサポートするために、サービスとしてデプロイされた ML インスタンス。 また、既存のトレーニング済み実験実行を使用して ML サービスを作成することもできます。 |

## 既存のトレーニング実験実行とスケジュールされたスコアリングを使用して ML サービスを作成

トレーニング実験を ML サービスとして実行を公開する場合、POSTリクエストのペイロードを実行するスコアリング実験の詳細を指定して、スコアリングをスケジュールできます。 その結果、スコアリング用にスケジュールされた実験エンティティが作成されます。

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
| `mlInstanceId` | 既存の ML インスタンスの識別、ML サービスの作成に使用するトレーニング実験実行は、この特定の ML インスタンスに対応している必要があります。 |
| `trainingExperimentId` | ML インスタンス ID に対応する実験 ID。 |
| `trainingExperimentRunId` | ML サービスの公開に使用される特定のトレーニング実験実行。 |
| `scoringDataSetId` | スケジュールに沿ったスコアリング Experiment Run に使用する特定のデータセットを表す ID。 |
| `scoringTimeframe` | 分数を表す整数値。スコアリング Experiment Run に使用するデータをフィルタリングします。例えば、値 `10080` を指定すると、過去 10,080 分（168 時間）のデータが、スケジュールに沿ったスコアリング Experiment Run に使用されます。値 `0` を指定すると、データはフィルタリングされず、データセット内のすべてのデータがスコアリングに使用されます。 |
| `scoringSchedule` | スケジュールに沿ったスコアリング Experiment Run に関する詳細が含まれます。 |
| `scoringSchedule.startTime` | スコアリングを開始する日時を示す日時。 |
| `scoringSchedule.endTime` | スコアリングを開始する日時を示す日時。 |
| `scoringSchedule.cron` | 実験を実行したスコアを取得する間隔を示す Cron 値。 |

**応答**

応答が成功すると、一意の `id` と、対応するスコアリング実験の `scoringExperimentId` を含む、新しく作成された ML サービスの詳細が返されます。


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

## 既存の ML インスタンスからの ML サービスの作成

特定のユースケースと要件に応じて、ML インスタンスを使用した ML サービスの作成は、トレーニング実行とスコアリング実験実行のスケジュールを柔軟に設定できます。 このチュートリアルでは、次のような特定のケースについて説明します。

- [予定されたトレーニングは必要ありませんが、予定されたスコアリングは必要です。](#ml-service-with-scheduled-experiment-for-scoring)
- [トレーニングとスコアリングの両方に、スケジュールされた実験実行が必要です。](#ml-service-with-scheduled-experiments-for-training-and-scoring)

ML サービスは、トレーニングやスコアリング実験をスケジュールすることなく、ML インスタンスを使用して作成できます。 このような ML サービスは、通常の実験エンティティと、トレーニングおよびスコアリング用の単一の実験実行を作成します。

### スケジュールに沿ったスコアリング Experiment を含む ML サービス {#ml-service-with-scheduled-experiment-for-scoring}

スコアリング用にスケジュールされた実験実行を含む ML インスタンスを公開することで、ML サービスを作成できます。これにより、トレーニング用の通常の実験エンティティが作成されます。 トレーニング実験実行が生成され、スケジュールされたすべてのスコアリング実験実行に使用されます。 MLサービスの作成に必要な `mlInstanceId`、`trainingDataSetId` および `scoringDataSetId` があること、これらが存在し、有効な値であることを確認します。

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
| `scoringSchedule.startTime` | スコアリングを開始する日時を示す日時。 |
| `scoringSchedule.endTime` | スコアリングを開始する日時を示す日時。 |
| `scoringSchedule.cron` | 実験を実行したスコアを取得する間隔を示す Cron 値。 |

**応答**

応答が成功すると、新しく作成した ML サービスの詳細が返されます。 これには、サービスの一意の `id` のほか、対応するトレーニング実験とスコアリング実験の `trainingExperimentId` と `scoringExperimentId` が含まれます。

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

スケジュールされたトレーニングおよびスコアリング実験実行を使用して既存の ML インスタンスを ML サービスとして公開するには、トレーニングスケジュールとスコアリングスケジュールの両方を提供する必要があります。 この設定の ML サービスを作成すると、トレーニングとスコアリングの両方のスケジュール済み実験エンティティも作成されます。 トレーニングとスコアリングのスケジュールが同じである必要はありません。スコアリングジョブの実行中に、スケジュールに沿ったトレーニング Experiment Run によって生成された最新のトレーニング済みモデルが取得され、スケジュールに沿ったスコアリングの実行に使用されます。

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
| `scoringSchedule.startTime` | スコアリングを開始する日時を示す日時。 |
| `scoringSchedule.endTime` | スコアリングを開始する日時を示す日時。 |
| `scoringSchedule.cron` | 実験を実行したスコアを取得する間隔を示す Cron 値。 |

**応答**

応答が成功すると、新しく作成した ML サービスの詳細が返されます。 これには、サービスの一意の `id` のほか、対応するトレーニング実験とスコアリング実験の `trainingExperimentId` と `scoringExperimentId` が含まれます。 以下の応答例で `trainingSchedule` と `scoringSchedule` の存在は、トレーニングとスコアリングの実験エンティティがスケジュールされた実験であることを示しています。

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

`/mlServices` に対して `GET` リクエストを実行し、パスで ML サービスの一意の `id` を指定することで、既存の ML サービスを検索できます。

**API 形式**

```http
GET /mlServices/{SERVICE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SERVICE_ID}` | 参照している ML サービスの一意の `id`。 |

**リクエスト**

```SHELL
curl -X GET 'https://platform.adobe.io/data/sensei/mlServices/{SERVICE_ID}' 
  -H 'Authorization: Bearer {ACCESS_TOKEN}' 
  -H 'x-api-key: {API_KEY}' 
  -H 'x-gw-ims-org-id: {ORG_ID}' 
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、ML サービスの詳細が返されます。

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
>異なる ML サービスを取得すると、キーと値のペアが多くなったり少なくなったりする応答が返される場合があります。 上記のレスポンスは、[スケジュールに沿ったトレーニング Experiment Run とスコアリング Experiment Run の両方を含む ML サービス](#ml-service-with-scheduled-experiments-for-training-and-scoring)を表したものです。


## トレーニングまたはスコアリングのスケジュール

公開済みの ML サービスに対するスコアリングとトレーニングをスケジュールする場合は、`/mlServices` の `PUT` リクエストで既存の ML サービスを更新することで実行できます。

**API 形式**

```http
PUT /mlServices/{SERVICE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SERVICE_ID}` | 更新する ML サービスの一意の `id`。 |

**リクエスト**

次のリクエストは、`trainingSchedule` キーと `scoringSchedule` キーをそれぞれの `startTime`、`endTime` キーおよび `cron` キーに追加して、既存の ML サービスのトレーニングとスコアリングをスケジュールします。

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
>既存のスケジュール済トレーニング・ジョブおよびスコアリング・ジョブの `startTime` を変更しようとしないでください。 `startTime` を変更する必要がある場合は、同じモデルを公開して、トレーニングジョブとスコアリングジョブのスケジュールを再設定することを検討してください。

**応答** 

応答が成功すると、更新された ML サービスの詳細が返されます。

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
