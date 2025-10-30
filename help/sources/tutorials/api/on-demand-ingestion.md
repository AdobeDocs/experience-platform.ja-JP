---
keywords: Experience Platform;ホーム;人気のトピック;Flow Service;
title: Flow Service API を使用したオンデマンド取り込み用のフロー実行の作成
description: Flow Service API を使用して、オンデマンド取り込み用のフロー実行を作成する方法を説明します
exl-id: a7b20cd1-bb52-4b0a-aad0-796929555e4a
source-git-commit: be2ad7a02d4bdf5a26a0847c8ee7a9a93746c2ad
workflow-type: tm+mt
source-wordcount: '823'
ht-degree: 10%

---

# [!DNL Flow Service] API を使用したオンデマンド取り込み用のフロー実行の作成

フロー実行は、フロー実行のインスタンスを表します。 例えば、フローが 1 時間ごとに午前 9:00、午前 10:00、午前 11:00 に実行されるようにスケジュールされている場合、フロー実行の 3 つのインスタンスが存在します。 フロー実行は、特定の組織に固有です。

オンデマンド取り込みでは、特定のデータフローに対してフロー実行を作成できます。 これにより、ユーザーは、指定されたパラメーターに基づいてフロー実行を作成し、サービストークンなしで取り込みサイクルを作成できます。 オンデマンド取り込みのサポートは、バッチソースでのみ使用できます。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、オンデマンド取り込みを使用し、フロー実行を作成する手順を説明します。

>[!TIP]
>
>フロー実行を再試行すると、元の実行の範囲内にあるタイムスタンプを持つファイルのみが処理されます。

## はじめに

>[!NOTE]
>
>フロー実行を作成するには、まず、1 回限りの取り込みがスケジュールされているデータフローのフロー ID を持つ必要があります。

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 &#x200B;](../../../landing/api-guide.md) を参照してください。

## テーブルベースのソースのフロー実行の作成

テーブルベースのソースのフローを作成するには、実行を作成するフローの ID と、開始時刻、終了時刻、差分列の値を指定して、[!DNL Flow Service] API に対して POST リクエストを行います。

>[!TIP]
>
>テーブルベースのソースには、広告、分析、同意および環境設定、CRM、カスタマーサクセス、データベース、マーケティング自動化、支払いおよびプロトコルのソースカテゴリが含まれます。

**API 形式**

```http
POST /runs/
```

**リクエスト**

次のリクエストは、フロー ID `3abea21c-7e36-4be1-bec1-d3bad0e3e0de` のフロー実行を作成します。

>[!NOTE]
>
>最初のフロー実行を作成する際に `deltaColumn` を指定するだけで済みます。 その後、`deltaColumn` れはフローの変換の一部としてパッチ `copy` 適用され、信頼できる情報源として扱われます。 フロー実行パラメーターを使用して `deltaColumn` 値を変更しようとすると、エラーが発生します。

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
| `flowId` | フロー実行が作成されるフローの ID。 |
| `params.startTime` | オンデマンドフロー実行が開始されるスケジュールされた時間。 この値は Unix 時間で表されます。 |
| `params.windowStartTime` | データの取得元となる最も古い日時。 この値は Unix 時間で表されます。 |
| `params.windowEndTime` | データが取得される日時。 この値は Unix 時間で表されます。 |
| `params.deltaColumn` | 差分列は、データを分割し、新しく取り込んだデータを履歴データから分離するために必要です。 **メモ**:`deltaColumn` は、最初のフロー実行を作成する場合にのみ必要です。 |
| `params.deltaColumn.name` | 差分列の名前。 |

**応答**

応答が成功すると、一意の実行 `id` など、新しく作成されたフロー実行の詳細が返されます。

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
| `id` | 新しく作成されたフロー実行の ID。 テーブルベースの実行仕様について詳しくは、[&#x200B; フロー仕様の取得 &#x200B;](../api/collect/database-nosql.md#specs) に関するガイドを参照してください。 |
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
| `metrics` | This property displays a status summary for the flow run. | -->

## ファイルベースのソースのフロー実行の作成

ファイルベースのソースのフローを作成するには、[!DNL Flow Service] API に POST リクエストを実行し、実行を作成するフローの ID と、開始時刻および終了時刻の値を指定します。

>[!TIP]
>
>ファイルベースのソースには、すべてのクラウドストレージソースが含まれます。

**API 形式**

```http
POST /runs/
```

**リクエスト**

次のリクエストは、フロー ID `3abea21c-7e36-4be1-bec1-d3bad0e3e0de` のフロー実行を作成します。

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
| `flowId` | フロー実行が作成されるフローの ID。 |
| `params.startTime` | オンデマンドフロー実行が開始されるスケジュールされた時間。 この値は Unix 時間で表されます。 |
| `params.windowStartTime` | データの取得元となる最も古い日時。 この値は Unix 時間で表されます。 |
| `params.windowEndTime` | データが取得される日時。 この値は Unix 時間で表されます。 |

**応答**

応答が成功すると、一意の実行 `id` など、新しく作成されたフロー実行の詳細が返されます。


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
| `id` | 新しく作成されたフロー実行の ID。 テーブルベースの実行仕様について詳しくは、[&#x200B; フロー仕様の取得 &#x200B;](../api/collect/database-nosql.md#specs) に関するガイドを参照してください。 |
| `etag` | フロー実行のリソースバージョン。 |

## フロー実行の監視

フロー実行が作成されると、それを通じて取り込まれるデータを監視し、フロー実行、完了ステータスおよびエラーに関する情報を確認できます。 API を使用してフローの実行を監視するには、[API でのデータフローの監視 &#x200B;](./monitor.md) に関するチュートリアルを参照してください。 Experience Platform UI を使用してフローの実行を監視するには、[&#x200B; モニタリングダッシュボードを使用したソースデータフローのモニタリング &#x200B;](../../../dataflows/ui/monitor-sources.md) に関するガイドを参照してください。
