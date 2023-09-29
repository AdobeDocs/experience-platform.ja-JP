---
keywords: Experience Platform;ホーム;人気のトピック;Flow Service;
title: フローサービス API を使用したオンデマンド取り込み用のフロー実行の作成
description: フローサービス API を使用して、オンデマンド取り込み用のフロー実行を作成する方法を説明します。
exl-id: a7b20cd1-bb52-4b0a-aad0-796929555e4a
source-git-commit: cea12160656ba0724789db03e62213022bacd645
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 14%

---

# を使用したオンデマンド取り込みのフロー実行の作成 [!DNL Flow Service] API

フロー実行は、フロー実行のインスタンスを表します。 例えば、時間別のフローの午前 9:00、午前 10:00、午前 11:00 に実行するようにスケジュールされている場合、フロー実行の 3 つのインスタンスが存在します。 フロー実行は、特定の組織に固有です。

オンデマンド取り込みでは、特定のデータフローに対するフロー実行を作成できます。 これにより、ユーザーは、サービストークンを使用せずに、指定されたパラメーターに基づいてフロー実行を作成し、取り込みサイクルを作成できます。 オンデマンド取り込みのサポートは、バッチソースに対してのみ使用できます。

このチュートリアルでは、オンデマンド取り込みを使用し、 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

>[!NOTE]
>
>フロー実行を作成するには、まず、1 回の取り込み用にスケジュールされたデータフローのフロー ID が必要です。

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../landing/api-guide.md)のガイドを参照してください。

## テーブルベースのソースに対するフロー実行の作成

テーブルベースのソースのフローを作成するには、 [!DNL Flow Service] 実行を作成するフローの ID と、開始時間、終了時間、デルタ列の値を提供する際の API。

>[!TIP]
>
>テーブルベースのソースには、広告、分析、同意および環境設定、CRM、顧客の成功、データベース、マーケティングの自動化、支払い、プロトコルの各ソースカテゴリが含まれます。

**API 形式**

```http
POST /runs/
```

**リクエスト**

次のリクエストは、フロー ID のフロー実行を作成します `3abea21c-7e36-4be1-bec1-d3bad0e3e0de`.

>[!NOTE]
>
>必要なのは、 `deltaColumn` 最初のフロー実行を作成する際に使用します。 その後 `deltaColumn` の一部としてパッチを適用します `copy` 流れの中の変換であり、真理の源として扱われます。 変更を試みた場合 `deltaColumn` フロー実行パラメーターを通じた値は、エラーを引き起こします。

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
| `params.startTime` | オンデマンドフローの実行が開始される予定時間。 この値は UNIX 時間で表されます。 |
| `params.windowStartTime` | データの取得元となる最も早い日時。 この値は UNIX 時間で表されます。 |
| `params.windowEndTime` | データが取得されるまでの日時。 この値は UNIX 時間で表されます。 |
| `params.deltaColumn` | デルタ列は、データをパーティション化し、新しく取り込んだデータを履歴データから分離するために必要です。 **注意**: `deltaColumn` は、最初のフロー実行を作成する場合にのみ必要です。 |
| `params.deltaColumn.name` | デルタ列の名前。 |

**応答**

正常な応答は、新しく作成されたフロー実行の詳細（一意の実行を含む）を返します `id`.

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
| `id` | 新しく作成されたフロー実行の ID。 次のガイドを参照してください： [フロー仕様の検索](../api/collect/database-nosql.md#specs) を参照してください。 |
| `etag` | フロー実行のリソースのバージョン。 |
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

## ファイルベースのソースに対するフロー実行の作成

ファイルベースのソースのフローを作成するには、 [!DNL Flow Service] 実行を作成するフローの ID を指定する際の API と、開始時間および終了時間の値。

>[!TIP]
>
>ファイルベースのソースには、すべてのクラウドストレージソースが含まれます。

**API 形式**

```http
POST /runs/
```

**リクエスト**

次のリクエストは、フロー ID のフロー実行を作成します `3abea21c-7e36-4be1-bec1-d3bad0e3e0de`.

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
| `params.startTime` | オンデマンドフローの実行が開始される予定時間。 この値は UNIX 時間で表されます。 |
| `params.windowStartTime` | データの取得元となる最も早い日時。 この値は UNIX 時間で表されます。 |
| `params.windowEndTime` | データが取得されるまでの日時。 この値は UNIX 時間で表されます。 |

**応答**

正常な応答は、新しく作成されたフロー実行の詳細（一意の実行を含む）を返します `id`.


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
| `id` | 新しく作成されたフロー実行の ID。 次のガイドを参照してください： [フロー仕様の検索](../api/collect/database-nosql.md#specs) を参照してください。 |
| `etag` | フロー実行のリソースのバージョン。 |

## フロー実行の監視

フロー実行が作成されたら、フロー実行を通じて取り込まれるデータを監視して、フロー実行、完了ステータス、エラーに関する情報を確認できます。 API を使用してフロー実行を監視するには、 [API でのデータフローの監視](./monitor.md). Platform UI を使用してフロー実行を監視するには、 [監視ダッシュボードを使用したソースデータフローの監視](../../../dataflows/ui/monitor-sources.md).
