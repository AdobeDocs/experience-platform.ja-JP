---
keywords: Experience Platform;publish a model;Data Science Workspace;popular topics
solution: Experience Platform
title: モデルをサービスとして公開(API)
topic: Tutorial
translation-type: tm+mt
source-git-commit: 19823c7cf0459e045366f0baae2bd8a98416154c

---


# モデルをサービスとして公開(API)

## 前提条件

- API呼び出しを行う [開始の認証については](../../tutorials/authentication.md) 、このチュートリアルに従ってください。
このチュートリアルでは、次の情報を入手できます。
   - `{ACCESS_TOKEN}`:認証後に指定された特定のベアラトークン値。
   - `{IMS_ORG}`:IMS組織の資格情報が、お使いの一意のAdobe Experience Platform統合で見つかりました。
   - `{API_KEY}`:固有のAdobe Experience Platform統合で見つかった特定のAPIキーの値。
- このチュートリアルでは、既存のMLエンジン、MLインスタンス、およびテストエンティティが必要です。 MLエンジン、MLイン [スタンス](./import-packaged-recipe-api.md) 、またはテストエンティティの作成に関するこのチュートリアルを参照してください。
- このチュートリアルで取り上げるAPIエンドポイントとリクエストについて詳しくは、 [Sensei Machine Learning APIの完全版を参照してください](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml)。

## キーワード

このチュートリアルで使用される一般的な用語：

| 用語 | 定義 |
--- | ---
| **Machine Learning Instance（MLインスタンス）** | 特定のデータ、パラメーター、および先生コードから構成される特定のテナントの先生エンジンのインスタンスである概念エンティティです。 |
| **実験** | トレーニングテストの実行、スコアリングの実行、またはその両方を保持するための傘のエンティティ。 |
| **予定された実験** | トレーニングまたはスコアテストの実行の自動化を表す用語で、ユーザー定義のスケジュールによって制御されます。 |
| **テストの実行** | トレーニングやスコアリングの実験の特定の例。 特定のテストからの複数のテストの実行では、トレーニングやスコアリングに使用されるデータセット値が異なる場合があります。 |
| **トレーニングモデル** | 検証、評価、確定されたモデルに到達する前に、エンジニアリングの実験と機能を試すプロセスによって作成された機械学習モデル。 |
| **公開済みモデル** | トレーニング、検証、および評価の後、確定され、バージョン管理されたモデルが到着しました。 |
| **機械学習サービス（MLサービス）** | エンドポイントを介したトレーニングおよびスコアのオンデマンドリクエストをサポートするために、サービスとしてデプロイされたMLインスタンス。 MLサービスは、既存のトレーニングを受けた実験を使用して作成することもできます。 |


## APIワークフロー

このチュートリアルでは、MLサービスの作成、取得、更新について説明します。

## 既存のトレーニングテストの実行とスケジュールされたスコアリングを使用したMLサービスの作成

トレーニングテストの実行をMLサービスとして公開する場合、{JSON_PAYLOAD}のスコアリングテストの実行の詳細を指定することで、スコアリ `scoringSchedule` ングのスケジュールを設定できます。 これにより、スコアリング用にスケジュールされたテストエンティティが作成されます。 、、、およびがあ `mlInstanceId`り、 `trainingExperimentId`それらが `trainingExperimentRunId`存在し `scoringDataSetId`、有効な値であることを確認します。

開始の場合は、にリクエ `POST` ストを行いま `/mlServices`す。 次に、curlコマンドの例を示します。

**リクエスト**

```SHELL
curl -X POST 
  https://platform.adobe.io/data/sensei/mlServices
  -H 'Authorization: {ACCESS_TOKEN}' 
  -H 'x-api-key: {API_KEY}' 
  -H 'x-gw-ims-org-id: {IMS_ORG}' 
  -d '{JSON_PAYLOAD}'
```

- `{API_KEY}` :固有のAdobe Experience Platform統合で見つかった特定のAPIキーの値。
- `{IMS_ORG}` : IMS組織IDは、Adobe I/Oコンソールの統合の詳細に記載されています。
- `{ACCESS_TOKEN}` :認証後に指定された特定のベアラトークン値。
- `{JSON_PAYLOAD}` :以下に、JSONペイロード形式の例を示します。

```JSON
{
  "name": "string",
  "description": "string",
  "mlInstanceId": "string",
  "trainingExperimentId": "string",
  "trainingExperimentRunId": "string",
  "scoringDataSetId": "string",
  "scoringTimeframe": "integer",
  "scoringSchedule": {
    "startTime": "2019-03-13T00:00",
    "endTime": "2019-03-14T00:00",
    "cron": "30 * * * *"
  }
}
```

- `mlInstanceId` :既存のMLインスタンスID。MLサービスの作成に使用するトレーニングテストの実行は、この特定のMLインスタンスに対応する必要があります。
- `trainingExperimentId` :MLインスタンスの識別に対応するテストの識別。
- `trainingExperimentRunId` :MLサービスの公開に使用する特定のトレーニングテストの実行。
- `scoringDataSetId` :スケジュールされたスコアリングテストの実行に使用される特定のデータセットを参照するID。
- `scoringTimeframe` :テストの実行のスコアリングに使用するデータをフィルタリングする分を表す整数値です。 例えば、値は、過去10080分 `"10080"` または168時間のデータが、スケジュールされた各スコアリングテストの実行に使用されることを意味します。 の値ではデータがフィルタ `"0"` ーされず、データセット内のすべてのデータがスコアリングに使用されます。
- `scoringSchedule` :スケジュールされたスコアリングテストの実行に関する詳細が含まれます。
- `startTime` : の定義.
- `endTime` : の定義.
- `cron` : の定義.

**応答**

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

JSON応答から、対応する値を持つキ `scoringExperimentId` ーは、リクエストで指定したテストスケジュールと共に新しいスコアリングテストが作成されたことを示し `POST` ます。 応答の `id` キーは、作成されたMLサービスを一意に識別します。

## 既存のMLインスタンスからのMLサービスの作成

具体的な使用事例や要件に応じて、MLインスタンスを使用したMLサービスの作成は、トレーニングのスケジュールとテストの実行のスコアリングの点で柔軟に行えます。 このチュートリアルでは、次のような特定のケースについて説明します。

- [スケジュール済みのトレーニングは必要ありませんが、スケジュール済みのスコアリングは必要です。](#ml-service-with-scheduled-experiment-for-scoring)
- [トレーニングとスコアの両方で、予定されたテストの実行が必要です。](#ml-service-with-scheduled-experiments-for-training-and-scoring)

MLサービスは、トレーニングやスコアリングの実験をスケジュールしなくても、MLインスタンスを使用して作成できます。 このようなMLサービスは、通常のテストエンティティと、トレーニングとスコアのための1回のテスト実行を作成します。

### スコアリングのための予定されたテストを含むMLサービス {#ml-service-with-scheduled-experiment-for-scoring}

スケジュールされたテストの実行を使用してMLインスタンスを公開してMLサービスを作成すると、トレーニング用の通常のテストエンティティが作成されます。 生成されたトレーニングテストの実行は、すべてのスケジュール済みスコアリングテストの実行に使用されます。 MLサービスの作成 `mlInstanceId`に必要な、 `trainingDataSetId`お `scoringDataSetId` よび、が存在し、それらが有効な値であることを確認してください。

開始の場合は、にリクエ `POST` ストを行いま `/mlServices`す。 次に、curlコマンドの例を示します。

**リクエスト**

```SHELL
curl -X POST 
  https://platform.adobe.io/data/sensei/mlServices
  -H 'Authorization: {ACCESS_TOKEN}' 
  -H 'x-api-key: {API_KEY}' 
  -H 'x-gw-ims-org-id: {IMS_ORG}' 
  -d '{JSON_PAYLOAD}'
```

- `{API_KEY}` :固有のAdobe Experience Platform統合で見つかった特定のAPIキーの値。
- `{IMS_ORG}` : IMS組織IDは、Adobe I/Oコンソールの統合の詳細に記載されています。
- `{ACCESS_TOKEN}` :認証後に指定された特定のベアラトークン値。
- `{JSON_PAYLOAD}` :以下に、JSONペイロード形式の例を示します。

```JSON
{
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
}
```

| JSONキー | 説明 |
| --- | --- |
| **`mlInstanceId`** | MLサービスの作成に使用されるMLインスタンスを表す既存のMLインスタンスの識別。 |
| **`trainingDataSetId`** | トレーニングテストに使用する特定のデータセットを参照するID。 |
| **`trainingTimeframe`** | トレーニングテストに使用するデータをフィルタリングする分を表す整数値です。 例えば、の値は、過去10080分 `"10080"` または168時間のデータがトレーニングテストの実施に使用されることを意味します。 の値ではデータがフィルタ `"0"` ーされず、データセット内のすべてのデータがトレーニングに使用されます。 |
| **`scoringDataSetId`** | スケジュールされたスコアリングテストの実行に使用される特定のデータセットを参照するID。 |
| **`scoringTimeframe`** | テストの実行のスコアリングに使用するデータをフィルタリングする分を表す整数値です。 例えば、値は、過去10080分 `"10080"` または168時間のデータが、スケジュールされた各スコアリングテストの実行に使用されることを意味します。 の値ではデータがフィルタ `"0"` ーされず、データセット内のすべてのデータがスコアリングに使用されます。 |
| **`scoringSchedule`** | スケジュールされたスコアリングテストの実行に関する詳細が含まれます。 |

**応答**

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

応答から、キ `JSON` ーと提案は、こ `trainingExperimentId` のMLサービ `scoringExperimentId` ス用に新しいトレーニングとスコアリングのテストエンティティが作成されたことを示しています。 オブジェクトの存在とは、ス `scoringSchedule` コアリングテストの実行スケジュールの詳細を指します。 応答の `id` キーは、先ほど作成したMLサービスを参照します。

### トレーニングとスコアリングのための予定された実験を含むMLサービス {#ml-service-with-scheduled-experiments-for-training-and-scoring}

既存のMLインスタンスをMLサービスとして公開し、スケジュールされたトレーニングとスコアリングテストの実行を行うには、トレーニングとスコアリングの両方のスケジュールを提供する必要があります。 この設定のMLサービスが作成されると、トレーニングとスコアリングの両方に対する予定されたテストエンティティも作成されます。 トレーニングとスコアリングのスケジュールが同じである必要はありません。 スコアリングジョブの実行中に、スケジュール済みのトレーニング実験の実行によって生成された最新のトレーニングモデルが取得され、スケジュール済みのスコアリングの実行に使用されます。

MLサービスを作成するには、追加するML `POST` サービス `/mlServices` オブジェクトを `{JSON_PAYLOAD}` 表すを使用してにリクエストします。 、、およびが `mlInstanceId`有効な `trainingDataSetId`値であ `scoringDataSetId` ることを確認します。

**リクエスト**

```SHELL
curl -X POST "https://platform-int.adobe.io/data/sensei/mlServices" 
  -H "Authorization: Bearer {ACCESS_TOKEN}" 
  -H "x-api-key: {API_KEY}" 
  -H "x-gw-ims-org-id: {IMS_ORG}" 
  -d "{JSON_PAYLOAD}"
```

- `{API_KEY}` :固有のAdobe Experience Platform統合で見つかった特定のAPIキーの値。
- `{IMS_ORG}` : IMS組織IDは、Adobe I/Oコンソールの統合の詳細に記載されています。
- `{ACCESS_TOKEN}` :認証後に指定された特定のベアラトークン値。
- `{JSON_PAYLOAD}` :以下に、JSONペイロード形式の例を示します。

```JSON
{
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
}
```

| JSONキー | 説明 |
| --- | --- |
| **`mlInstanceId`** | MLサービスの作成に使用されるMLインスタンスを表す既存のMLインスタンスの識別。 |
| **`trainingDataSetId`** | トレーニングテストに使用する特定のデータセットを参照するID。 |
| **`trainingTimeframe`** | トレーニングテストに使用するデータをフィルタリングする分を表す整数値です。 例えば、の値は、過去10080分 `"10080"` または168時間のデータがトレーニングテストの実施に使用されることを意味します。 の値ではデータがフィルタ `"0"` ーされず、データセット内のすべてのデータがトレーニングに使用されます。 |
| **`scoringDataSetId`** | スケジュールされたスコアリングテストの実行に使用される特定のデータセットを参照するID。 |
| **`scoringTimeframe`** | テストの実行のスコアリングに使用するデータをフィルタリングする分を表す整数値です。 例えば、値は、過去10080分 `"10080"` または168時間のデータが、スケジュールされた各スコアリングテストの実行に使用されることを意味します。 の値ではデータがフィルタ `"0"` ーされず、データセット内のすべてのデータがスコアリングに使用されます。 |
| **`trainingSchedule`** | 予定されたトレーニング実験の実行に関する詳細が含まれます。 |
| **`scoringSchedule`** | スケジュールされたスコアリングテストの実行に関する詳細が含まれます。 |

**応答**

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

とを応答本体に追加 `trainingExperimentId` するこ `scoringExperimentId` とで、トレーニングとスコアの両方のテストエンティティの作成を提案します。 上記のテストエンテ `trainingSchedule` ィテ `scoringSchedule` ィのトレーニングとスコアリングが予定されていることを示し、が存在します。 応答の `id` キーは、先ほど作成したMLサービスを参照します。

## MLサービスの取得 {#retrieving-ml-services}

既存のMLサービスを取得する方法は、エンドポイントにリクエストを送信する `GET` 場合と同じ `/mlServices` くらい簡単です。 取得しようとしている特定のMLサービスのMLサービスIDを持っていることを確認します。

**リクエスト**

```SHELL
curl -X GET "https://platform.adobe.io/data/sensei/mlServices/{SERVICE_ID}" 
  -H "Authorization: Bearer {ACCESS_TOKEN}" 
  -H "x-api-key: {API_KEY}" 
  -H "x-gw-ims-org-id: {IMS_ORG}" 
```

- `{API_KEY}` :固有のAdobe Experience Platform統合で見つかった特定のAPIキーの値。
- `{IMS_ORG}` : IMS組織IDは、Adobe I/Oコンソールの統合の詳細に記載されています。
- `{ACCESS_TOKEN}` :認証後に指定された特定のベアラトークン値。

**応答**

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

JSON応答は、MLサービスオブジェクトを表します。 このオブジェクトは、MLサービスが作成されたときの応答と同じです。 異なるMLサービスを取得すると、キーと値のペアの数を増減した応答が返される場合があります。 上記の回答は、予定されたトレーニングとスコア [テストの実行の両方を含むMLサービスを表したものです](#ml-service-with-scheduled-experiments-for-training-and-scoring)。


## トレーニングまたはスコアのスケジュール

既に公開済みのMLサービスに対するスコアリングとトレーニングをスケジュールする場合、既存のMLサービスをリクエストで更新することでスケジュールを設定 `PUT` できま `/mlServices`す。 更新するMLサービスのIDが必要です。 最初に、更新す [るMLサービスを取得する](#retrieving-ml-services) 、参照のために役立つ手順を考えてください。

**リクエスト**

```SHELL
curl -X PUT "https://platform.adobe.io/data/sensei/mlServices/{SERVICE_ID}" 
  -H "Authorization: {ACCESS_TOKEN}" 
  -H "x-api-key: {API_KEY}" 
  -H "x-gw-ims-org-id: {IMS_ORG}" 
  -d "{JSON_PAYLOAD}"
```

- `{SERVICE_ID}` :更新するMLサービスを参照する一意のIDです。
- `{API_KEY}` :固有のAdobe Experience Platform統合で見つかった特定のAPIキーの値。
- `{IMS_ORG}` : IMS組織IDは、Adobe I/Oコンソールの統合の詳細に記載されています。
- `{ACCESS_TOKEN}` :認証後に指定された特定のベアラトークン値。
- `{JSON_PAYLOAD}` :以下に、JSONペイロード形式の例を示します。

```JSON
{
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
}
```

トレーニングとスコアのスケジュール設定は、およびキーを `trainingSchedule` それぞれ `scoringSchedule` に追加して行うこ `startTime`とで `endTime`行うことがで `cron` きます。

>[!NOTE] リクエスト `PUT` をオンにすると、 `mlServices` 既存の予定されたテストの実行を使用してサービスを変更できます。 既存のス **ケジュール済みの** トレーニングおよびス `startTime` コアリングジョブを変更しないでください。 変更する必要が `startTime` ある場合は、同じモデルを公開し、トレーニングジョブとスコアジョブを再スケジュールすることを検討します。

**応答**

応答は、オブジェクト内のキ `{JSON_PAYLOAD}` ー、キー、およびキ `id`ーを含む `created`butと `updated` なります。

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
