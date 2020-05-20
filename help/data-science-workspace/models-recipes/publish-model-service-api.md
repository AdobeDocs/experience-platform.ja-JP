---
keywords: Experience Platform;publish a model;Data Science Workspace;popular topics
solution: Experience Platform
title: モデルをサービスとして公開(API)
topic: Tutorial
translation-type: tm+mt
source-git-commit: 19823c7cf0459e045366f0baae2bd8a98416154c
workflow-type: tm+mt
source-wordcount: '1762'
ht-degree: 1%

---


# モデルをサービスとして公開(API)

## 前提条件

- API呼び出しを行う開始の認証については、この [チュートリアル](../../tutorials/authentication.md) に従ってください。
チュートリアルでは、次の情報を入手する必要があります。
   - `{ACCESS_TOKEN}`: 認証後に指定された特定のベアラトークン値。
   - `{IMS_ORG}`: IMS組織の資格情報が一意のAdobe Experience Platform統合で見つかりました。
   - `{API_KEY}`: 独自のAdobe Experience Platform統合に見つかった特定のAPIキー値。
- このチュートリアルでは、既存のMLエンジン、MLインスタンス、およびテストエンティティが必要です。 MLエンジン、MLインスタンス [](./import-packaged-recipe-api.md) 、またはテストエンティティの作成については、このチュートリアルを参照してください。
- このチュートリアルで説明するAPIエンドポイントやリクエストについて詳しくは、 [Senesie Machine Learning API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml)（先生機械学習API）の全文を参照してください。

## キーワード

このチュートリアルで使用される一般的な用語：

| 用語 | 定義 |
--- | ---
| **機械学習インスタンス（MLインスタンス）** | 特定のデータ、パラメーター、先生コードから構成される特定のテナント用の先生エンジンのインスタンスである概念的エンティティです。 |
| **テスト** | トレーニングテストの実行、テストの実行のスコアリング、またはその両方を保持するための傘のエンティティ。 |
| **予定された実験** | テストの実行のトレーニングまたはスコアリングの自動化を表す用語で、ユーザー定義のスケジュールによって制御されます。 |
| **テストの実行** | トレーニングまたはスコアリングの実験の特定の例。 特定のテストからの複数のテストの実行では、トレーニングやスコアリングに使用するデータセットの値が異なる場合があります。 |
| **トレーニングモデル** | 検証、評価、確定されたモデルに到達する前に、エンジニアリングの実験と機能を試すプロセスによって作成された機械学習モデル。 |
| **発行済みモデル** | トレーニング、検証、評価の後、最終的でバージョン管理されたモデルが届きました。 |
| **機械学習サービス（MLサービス）** | エンドポイントを介したトレーニングとスコアリングのオンデマンドリクエストをサポートするために、サービスとしてデプロイされたMLインスタンス。 MLサービスは、既存のトレーニングを受けたテストの実行を使用して作成することもできます。 |


## APIワークフロー

このチュートリアルでは、MLサービスの作成、取得、更新について説明します。

## 既存のトレーニングテストの実行とスケジュール済みスコアを含むMLサービスの作成

トレーニングテスト実行をMLサービスとして発行する場合、{JSON_PAYLOAD}でスコアリングテスト実行の詳細を指定することで、スコアリングのスケジュールを設定でき `scoringSchedule` ます。 これにより、スコアリング用にスケジュールされたテストエンティティが作成されます。 、、、、 `mlInstanceId`およびそれらが存在し、有効な値であることを確認し `trainingExperimentId``trainingExperimentRunId``scoringDataSetId`ます。

開始するには、に `POST` リクエストを行い `/mlServices`ます。 次に、curlコマンドの例を示します。

**リクエスト**

```SHELL
curl -X POST 
  https://platform.adobe.io/data/sensei/mlServices
  -H 'Authorization: {ACCESS_TOKEN}' 
  -H 'x-api-key: {API_KEY}' 
  -H 'x-gw-ims-org-id: {IMS_ORG}' 
  -d '{JSON_PAYLOAD}'
```

- `{API_KEY}` : 独自のAdobe Experience Platform統合に見つかった特定のAPIキー値。
- `{IMS_ORG}` :  IMS組織IDは、Adobe I/Oコンソールの統合の詳細に記載されています。
- `{ACCESS_TOKEN}` : 認証後に指定された特定のベアラトークン値。
- `{JSON_PAYLOAD}` : 以下に、JSONペイロード形式の例を示します。

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

- `mlInstanceId` : 既存のMLインスタンスのID。MLサービスの作成に使用するトレーニング・テストの実行は、この特定のMLインスタンスに対応する必要があります。
- `trainingExperimentId` : MLインスタンスの識別に対応するテストの識別。
- `trainingExperimentRunId` : MLサービスの公開に使用するトレーニング実験の実行。
- `scoringDataSetId` : スケジュール済みスコアリングテストの実行に使用される特定のデータセットを参照する識別。
- `scoringTimeframe` : テストの実行のスコアリングに使用するデータをフィルタリングする時間（分）を表す整数値です。 例えば、値が、過去10080分または168時間のデータが、スケジュールされた各スコアリングテストの実行に使用されます。 `"10080"` の値ではデータがフィルターされ `"0"` ず、データセット内のすべてのデータがスコアリングに使用されます。
- `scoringSchedule` : スケジュール済みスコアリングテストの実行に関する詳細が含まれます。
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

JSON応答から、対応する値 `scoringExperimentId` を持つキーは、新しいスコアリングテストが作成されたことと、リク `POST` エストで指定したテストスケジュールを示します。 応答の `id` キーは、作成されたMLサービスを一意に識別します。

## 既存のMLインスタンスからMLサービスを作成する

具体的な使用事例や要件に応じて、MLインスタンスを使用してMLサービスを作成すると、トレーニングとテストの実行スコアリングの観点から柔軟に対応できます。 このチュートリアルでは、次のような特定のケースについて説明します。

- [スケジュールされたトレーニングは必要ありませんが、スケジュールされたスコアリングは必要です。](#ml-service-with-scheduled-experiment-for-scoring)
- [トレーニングとスコアリングの両方で、予定されたテストの実行が必要です。](#ml-service-with-scheduled-experiments-for-training-and-scoring)

MLサービスは、トレーニングやスコアリングの実験をスケジュールせずに、MLインスタンスを使用して作成できます。 このようなMLサービスは、通常のテストエンティティと、トレーニングとスコアリングのための1つのテスト実行を作成します。

### スコアリングのための予定されたテストを含むMLサービス {#ml-service-with-scheduled-experiment-for-scoring}

スコアリングのために予定されたテスト実行を含むMLインスタンスを発行してMLサービスを作成すると、トレーニング用の通常のテストエンティティが作成されます。 生成されたトレーニング実験の実行は、すべてのスケジュール済みスコアリングテストの実行に使用されます。 MLサービスの作成に必要な、 `mlInstanceId`、 `trainingDataSetId`および `scoringDataSetId` があり、それらが存在し、有効な値であることを確認します。

開始するには、に `POST` リクエストを行い `/mlServices`ます。 次に、curlコマンドの例を示します。

**リクエスト**

```SHELL
curl -X POST 
  https://platform.adobe.io/data/sensei/mlServices
  -H 'Authorization: {ACCESS_TOKEN}' 
  -H 'x-api-key: {API_KEY}' 
  -H 'x-gw-ims-org-id: {IMS_ORG}' 
  -d '{JSON_PAYLOAD}'
```

- `{API_KEY}` : 独自のAdobe Experience Platform統合に見つかった特定のAPIキー値。
- `{IMS_ORG}` :  IMS組織IDは、Adobe I/Oコンソールの統合の詳細に記載されています。
- `{ACCESS_TOKEN}` : 認証後に指定された特定のベアラトークン値。
- `{JSON_PAYLOAD}` : 以下に、JSONペイロード形式の例を示します。

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
| **`mlInstanceId`** | MLサービスの作成に使用されるMLインスタンスを表す、既存のMLインスタンスの識別。 |
| **`trainingDataSetId`** | トレーニングテストに使用する特定のデータセットを参照する識別。 |
| **`trainingTimeframe`** | トレーニングテストに使用するデータをフィルターする時間（分）を表す整数値です。 例えば、の値は、過去10080分または168時間のデータがトレーニング・テスト・ランに使用されることを `"10080"` 意味します。 の値ではデータがフィルタリングされないので、データセット内のすべてのデータがトレーニングに使用されます。 `"0"` |
| **`scoringDataSetId`** | スケジュール済みスコアリングテストの実行に使用される特定のデータセットを参照する識別。 |
| **`scoringTimeframe`** | テストの実行のスコアリングに使用するデータをフィルタリングする時間（分）を表す整数値です。 例えば、値が、過去10080分または168時間のデータが、スケジュールされた各スコアリングテストの実行に使用されます。 `"10080"` の値ではデータがフィルターされ `"0"` ず、データセット内のすべてのデータがスコアリングに使用されます。 |
| **`scoringSchedule`** | スケジュール済みスコアリングテストの実行に関する詳細が含まれます。 |

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

この `JSON` 回答から、キー `trainingExperimentId` と `scoringExperimentId` 提案は、このMLサービスに対して新しいトレーニングとスコアリングのテストエンティティが作成されたことを示しています。 オブジェクトの存在は、スコアリングテストの実行スケジュールの詳細を参照し `scoringSchedule` ます。 応答の `id` キーは、先ほど作成したMLサービスを参照します。

### トレーニングとスコアリングのための予定された実験を含むMLサービス {#ml-service-with-scheduled-experiments-for-training-and-scoring}

既存のMLインスタンスをMLサービスとして発行し、スケジュールされたトレーニングとスコアリングテストの実行を行うには、トレーニングとスコアリングの両方のスケジュールを指定する必要があります。 この設定のMLサービスが作成されると、トレーニングとスコアリングの両方に対してスケジュールされたテストエンティティも作成されます。 トレーニングとスコアリングのスケジュールは同じである必要はありません。 スコアリング・ジョブの実行中に、予定されたトレーニング・テストの実行によって生成された最新のトレーニング・モデルが取得され、スケジュールされたスコアリングの実行に使用されます。

MLサービスを作成するには、追加するML Serviceオブジェクトを `POST` 表すオブジェクトを使用して、に `/mlServices``{JSON_PAYLOAD}` リクエストを行います。 、、、 `mlInstanceId`およびが有効な値であるこ `trainingDataSetId`と `scoringDataSetId` を確認します。

**リクエスト**

```SHELL
curl -X POST "https://platform-int.adobe.io/data/sensei/mlServices" 
  -H "Authorization: Bearer {ACCESS_TOKEN}" 
  -H "x-api-key: {API_KEY}" 
  -H "x-gw-ims-org-id: {IMS_ORG}" 
  -d "{JSON_PAYLOAD}"
```

- `{API_KEY}` : 独自のAdobe Experience Platform統合に見つかった特定のAPIキー値。
- `{IMS_ORG}` :  IMS組織IDは、Adobe I/Oコンソールの統合の詳細に記載されています。
- `{ACCESS_TOKEN}` : 認証後に指定された特定のベアラトークン値。
- `{JSON_PAYLOAD}` : 以下に、JSONペイロード形式の例を示します。

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
| **`mlInstanceId`** | MLサービスの作成に使用されるMLインスタンスを表す、既存のMLインスタンスの識別。 |
| **`trainingDataSetId`** | トレーニングテストに使用する特定のデータセットを参照する識別。 |
| **`trainingTimeframe`** | トレーニングテストに使用するデータをフィルターする時間（分）を表す整数値です。 例えば、の値は、過去10080分または168時間のデータがトレーニング・テスト・ランに使用されることを `"10080"` 意味します。 の値ではデータがフィルタリングされないので、データセット内のすべてのデータがトレーニングに使用されます。 `"0"` |
| **`scoringDataSetId`** | スケジュール済みスコアリングテストの実行に使用される特定のデータセットを参照する識別。 |
| **`scoringTimeframe`** | テストの実行のスコアリングに使用するデータをフィルタリングする時間（分）を表す整数値です。 例えば、値が、過去10080分または168時間のデータが、スケジュールされた各スコアリングテストの実行に使用されます。 `"10080"` の値ではデータがフィルターされ `"0"` ず、データセット内のすべてのデータがスコアリングに使用されます。 |
| **`trainingSchedule`** | 予定されているトレーニングテストの実行に関する詳細が含まれます。 |
| **`scoringSchedule`** | スケジュール済みスコアリングテストの実行に関する詳細が含まれます。 |

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

レスポンス本文にお `trainingExperimentId` よびが追加され `scoringExperimentId` たことは、トレーニングとスコアリングの両方に対してテストエンティティが作成されることを示唆します。 およ `trainingSchedule` び `scoringSchedule` 示唆するところにより、上述のトレーニングおよびスコアのテストエンティティは、「Everiments」というスケジュールに従っています。 応答の `id` キーは、先ほど作成したMLサービスを参照します。

## MLサービスの取得 {#retrieving-ml-services}

既存のMLサービスを取得するには、エンドポイントに `GET` リクエストを行うだけで済み `/mlServices` ます。 取得しようとしている特定のMLサービスのMLサービスIDを持っていることを確認します。

**リクエスト**

```SHELL
curl -X GET "https://platform.adobe.io/data/sensei/mlServices/{SERVICE_ID}" 
  -H "Authorization: Bearer {ACCESS_TOKEN}" 
  -H "x-api-key: {API_KEY}" 
  -H "x-gw-ims-org-id: {IMS_ORG}" 
```

- `{API_KEY}` : 独自のAdobe Experience Platform統合に見つかった特定のAPIキー値。
- `{IMS_ORG}` :  IMS組織IDは、Adobe I/Oコンソールの統合の詳細に記載されています。
- `{ACCESS_TOKEN}` : 認証後に指定された特定のベアラトークン値。

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

JSON応答は、ML Serviceオブジェクトを表します。 このオブジェクトは、MLサービスが作成されたときの応答と同じです。 異なるMLサービスを取得すると、キーと値のペアの数が多かれ少なかれ応答が返される場合があります。 上記の回答は、スケジュールされたトレーニングとスコアリングテストの実行の両方を含む [MLサービスを表したものです](#ml-service-with-scheduled-experiments-for-training-and-scoring)。


## トレーニングまたはスコアリングのスケジュール設定

既に発行済みのMLサービスでスコアリングとトレーニングをスケジュールする場合、既存のMLサービスを更新して、 `PUT` 要求をオンにして実行でき `/mlServices`ます。 更新するMLサービスIDが必要です。 参照用に、更新するMLサービス [を](#retrieving-ml-services) 取得するのは、最初のステップとして便利です。

**リクエスト**

```SHELL
curl -X PUT "https://platform.adobe.io/data/sensei/mlServices/{SERVICE_ID}" 
  -H "Authorization: {ACCESS_TOKEN}" 
  -H "x-api-key: {API_KEY}" 
  -H "x-gw-ims-org-id: {IMS_ORG}" 
  -d "{JSON_PAYLOAD}"
```

- `{SERVICE_ID}` : 更新するMLサービスを参照する一意の識別子。
- `{API_KEY}` : 独自のAdobe Experience Platform統合に見つかった特定のAPIキー値。
- `{IMS_ORG}` :  IMS組織IDは、Adobe I/Oコンソールの統合の詳細に記載されています。
- `{ACCESS_TOKEN}` : 認証後に指定された特定のベアラトークン値。
- `{JSON_PAYLOAD}` : 以下に、JSONペイロード形式の例を示します。

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

トレーニングとスコアリングのスケジュール設定は、およびキーをそれぞれに追加して、およ `trainingSchedule` びキーを追加するこ `scoringSchedule` とで行うことができ `startTime``endTime``cron` ます。

>[!NOTE] リクエストをオンにす `PUT``mlServices` ると、既存の予定されたテストの実行でサービスを変更できます。 既存の予定され **ているトレーニングジョブとスコアリングジョブ**`startTime` を変更しないでください。 を変更する `startTime` 必要がある場合は、同じモデルを公開し、トレーニングおよびスコアリングジョブを再スケジュールすることを検討します。

**応答**

応答は、オブジェクト内 `{JSON_PAYLOAD}` のキーが増えた `id``created``updated` ときのbutです。

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
