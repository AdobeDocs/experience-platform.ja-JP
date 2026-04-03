---
keywords: Experience Platform;ホーム;人気のトピック;Flow Service;
title: Flow Service APIを使用したオンデマンド取り込みのフロー実行の作成
description: Flow Service APIを使用してオンデマンド取り込みのフロー実行を作成する方法を説明します
exl-id: a7b20cd1-bb52-4b0a-aad0-796929555e4a
source-git-commit: e4ee4accdb28dafda7e37625eb84062bb6e53644
workflow-type: tm+mt
source-wordcount: '823'
ht-degree: 10%

---

# [!DNL Flow Service] APIを使用したオンデマンド取り込みのフロー実行の作成

フロー実行は、フロー実行のインスタンスを表します。 例えば、フローが午前9:00、午前10:00、および午前11:00に1時間ごとに実行するようにスケジュールされている場合、フロー実行のインスタンスは3つになります。 フロー実行は特定の組織に固有です。

オンデマンド取り込みでは、特定のデータフローに対するフロー実行を作成できます。 これにより、ユーザーは指定されたパラメーターに基づいてフロー実行を作成し、サービストークンなしで取り込みサイクルを作成できます。 オンデマンド取り込みのサポートは、バッチソースに対してのみ使用できます。

このチュートリアルでは、オンデマンド取り込みを使用し、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用してフロー実行を作成する手順について説明します。

>[!TIP]
>
>フロー実行を再試行すると、元の実行の範囲内にあるタイムスタンプを持つファイルのみが処理されます。

## はじめに

>[!NOTE]
>
>フロー実行を作成するには、まず、1回限りの取り込み用にスケジュールされたデータフローのフローIDが必要です。

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Experience Platform APIの使用

Experience Platform APIの呼び出しを正常に行う方法について詳しくは、[Experience Platform APIの概要](../../../landing/api-guide.md)に関するガイドを参照してください。

## テーブルベースのソースに対するフロー実行の作成

テーブルベースのソースのフローを作成するには、実行を作成するフローのIDと、開始時間、終了時間、デルタ列の値を指定しながら、[!DNL Flow Service] APIにPOST リクエストを行います。

>[!TIP]
>
>テーブルベースのソースには、広告、分析、同意と環境設定、CRM、カスタマーサクセス、データベース、マーケティングオートメーション、支払い、プロトコルなどのソースカテゴリが含まれます。

**API 形式**

```http
POST /runs/
```

**リクエスト**

次のリクエストは、フローID `3abea21c-7e36-4be1-bec1-d3bad0e3e0de`のフロー実行を作成します。

>[!NOTE]
>
>最初のフロー実行を作成する際に`deltaColumn`を指定するだけで済みます。 その後、`deltaColumn`はフローの`copy`変換の一部としてパッチが適用され、信頼できる唯一の情報源として扱われます。 フロー実行パラメーターを使用して`deltaColumn`値を変更しようとすると、エラーが発生します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/runs' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json'
  -d '{
      "flowId": "3abea21c-7e36-4be1-bec1-d3bad0e3e0de",
      "params": {
          "startTime": "1663735590",
          "windowStartTime": "1651584991",
          "windowEndTime": "16515859567",
          "deltaColumn": {
              "name": "DOB"
          }
      }
  }'
```

| パラメーター | 説明 |
| --- | --- |
| `flowId` | フロー実行が作成されるフローのID。 |
| `params.startTime` | オンデマンドフローの実行が開始されるスケジュールされた時間。 この値はunix時間で表されます。 |
| `params.windowStartTime` | データの取得元となる最も古い日時。 この値はunix時間で表されます。 |
| `params.windowEndTime` | データが取得される日時。 この値はunix時間で表されます。 |
| `params.deltaColumn` | 差分列は、データを分割し、新しく取り込まれたデータを履歴データから分離するために必要です。 **注**: `deltaColumn`は、最初のフロー実行を作成する場合にのみ必要です。 |
| `params.deltaColumn.name` | 差分列の名前。 |

**応答**

応答が成功すると、新しく作成されたフロー実行の詳細（一意の実行`id`を含む）が返されます。

```json
{
    "items": [
        {
            "id": "3fb0418e-1804-45d6-8d56-dd51f05c0baf",
            "etag": "\"1100c53e-0000-0200-0000-627138980000\""
        }
    ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | 新しく作成されたフロー実行のID。 テーブルベースの実行仕様について詳しくは、[ フロー仕様の取得](../api/collect/database-nosql.md#specs)に関するガイドを参照してください。 |
| `etag` | フロー実行のリソースバージョン。 |

<!-- 
| `createdAt` | The unix timestamp that designates when the flow run was created. |
| `updatedAt` | The unix timestamp that designates when the flow run was last updated. |
| `createdBy` | The organization ID of the user who created the flow run. |
| `updatedBy` | The organization ID of the user who last updated the flow run. |
| `createdClient` | The application client that created the flow run. |
| `updatedClient` | The application client that last updated the flow run. |
| `sandboxId` | The ID of the sandbox that contains the flow run. |
| `sandboxName` | The name of the sandbox that contains the flow run. |
| `imsOrgId` | The organization ID. |
| `flowId` | The ID of the flow in which the flow run is created against. |
| `params.windowStartTime` | An integer that defines the start time of the window during which data is to be pulled. The value is represented in unix time. |
| `params.windowEndTime` | An integer that defines the end time of the window during which data is to be pulled. The value is represented in unix time. |
| `params.deltaColumn` | The delta column is required to partition the data and separate newly ingested data from historic data. **Note**: The `deltaColumn` is only needed when creating your firs flow run. |
| `params.deltaColumn.name` | The name of the delta column. |
| `etag` | The resource version of the flow run. |
| `metrics` | This property displays a status summary for the flow run. | 
-->

## ファイルベースのソースに対するフロー実行の作成

ファイルベースのソースのフローを作成するには、実行を作成するフローのIDと、開始時間と終了時間の値を指定しながら、[!DNL Flow Service] APIにPOST リクエストを行います。

>[!TIP]
>
>ファイルベースのソースには、すべてのクラウドストレージソースが含まれます。

**API 形式**

```http
POST /runs/
```

**リクエスト**

次のリクエストは、フローID `3abea21c-7e36-4be1-bec1-d3bad0e3e0de`のフロー実行を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/runs' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json'
  -d '{
      "flowId": "3abea21c-7e36-4be1-bec1-d3bad0e3e0de",
      "params": {
          "startTime": "1663735590",
          "windowStartTime": "1651584991",
          "windowEndTime": "16515859567"
      }
  }'
```

| パラメーター | 説明 |
| --- | --- |
| `flowId` | フロー実行が作成されるフローのID。 |
| `params.startTime` | オンデマンドフローの実行が開始されるスケジュールされた時間。 この値はunix時間で表されます。 |
| `params.windowStartTime` | データの取得元となる最も古い日時。 この値はunix時間で表されます。 |
| `params.windowEndTime` | データが取得される日時。 この値はunix時間で表されます。 |

**応答**

応答が成功すると、新しく作成されたフロー実行の詳細（一意の実行`id`を含む）が返されます。


```json
{
    "items": [
        {
            "id": "3fb0418e-1804-45d6-8d56-dd51f05c0baf",
            "etag": "\"1100c53e-0000-0200-0000-627138980000\""
        }
    ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | 新しく作成されたフロー実行のID。 テーブルベースの実行仕様について詳しくは、[ フロー仕様の取得](../api/collect/database-nosql.md#specs)に関するガイドを参照してください。 |
| `etag` | フロー実行のリソースバージョン。 |

## フロー実行の監視

フロー実行を作成したら、それを通じて取り込まれるデータを監視して、フロー実行、完了ステータス、エラーに関する情報を確認できます。 APIを使用してフロー実行を監視するには、[APIでのデータフローの監視](./monitor.md)に関するチュートリアルを参照してください。 Experience Platform UIを使用してフロー実行を監視するには、監視ダッシュボードを使用したソースデータフローの監視[に関するガイド ](../../../dataflows/ui/monitor-sources.md)を参照してください。
