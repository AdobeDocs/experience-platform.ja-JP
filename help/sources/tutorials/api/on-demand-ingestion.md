---
keywords: Experience Platform；ホーム；人気の高いトピック；フローサービス；
title: （ベータ版）フローサービス API を使用したオンデマンド取り込み用のフロー実行の作成
description: このチュートリアルでは、フローサービス API を使用して、オンデマンド取り込みのフロー実行を作成する手順を説明します
hide: true
hidefromtoc: true
exl-id: a7b20cd1-bb52-4b0a-aad0-796929555e4a
source-git-commit: 24f16e315607a1076ff2efef129d9e97040a9500
workflow-type: tm+mt
source-wordcount: '1147'
ht-degree: 10%

---

# （ベータ版） [!DNL Flow Service] API

>[!IMPORTANT]
>
>オンデマンド取り込みは現在ベータ版で、組織がまだアクセスできない可能性があります。 このドキュメントで説明されている機能は変更されることがあります。

フロー実行は、フロー実行のインスタンスを表します。 例えば、時間別のフローの実行が午前 9 時、午前 10 時、午前 11 時にスケジュールされている場合、フローの実行には 3 つのインスタンスがあります。 フロー実行は、特定の組織に固有です。

オンデマンド取り込みでは、特定のデータフローに対するフロー実行を作成できます。 これにより、ユーザーは、サービストークンを使用せずに、指定されたパラメーターに基づいてフロー実行を作成し、取り込みサイクルを作成できます。

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
>テーブルベースのソースには、次のソースカテゴリが含まれます。広告、analytics、同意および環境設定、CRM、顧客の成功、データベース、マーケティングの自動化、支払いおよびプロトコル。

**API 形式**

```http
POST /runs/
```

**リクエスト**

次のリクエストは、フロー ID のフロー実行を作成します `3abea21c-7e36-4be1-bec1-d3bad0e3e0de`.

>[!NOTE]
>
>必要なのは、 `deltaColumn` 最初のフロー実行を作成する際に使用します。 その後 `deltaColumn` の一部としてパッチを適用します `copy` 流れの中の変換であり、真理の源として扱われます。 すべての `deltaColumn` フロー実行パラメーターを通じた値は、エラーを引き起こします。

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
| `params.windowStartTime` | データを取り込む期間のウィンドウの開始時間を定義する整数。 値は UNIX 時間で表されます。 |
| `params.windowEndTime` | データを取り込む期間の終了時間を定義する整数。 値は UNIX 時間で表されます。 |
| `params.deltaColumn` | デルタ列は、データをパーティション化し、新しく取り込んだデータを履歴データから分離するために必要です。 |
| `params.deltaColumn.name` | デルタ列の名前。 |

**応答**

正常な応答は、新しく作成されたフロー実行の詳細（一意の実行を含む）を返します `id`.

```json
{
    "id": "3fb0418e-1804-45d6-8d56-dd51f05c0baf",
    "createdAt": 1651587212543,
    "updatedAt": 1651587223839,
    "createdBy": "{CREATED_BY}",
    "updatedBy": "{UPDATED_BY}",
    "createdClient": "{CREATED_CLIENT}",
    "updatedClient": "{UPDATED_CLIENT}",
    "sandboxId": "{SANDBOX_ID}",
    "sandboxName": "prod",
    "imsOrgId": "{ORGANIZATION_ID}",
    "flowId": "3abea21c-7e36-4be1-bec1-d3bad0e3e0de",
    "params": {
        "windowStartTime": "1651584991",
        "windowEndTime": "16515859567",
        "deltaColumn": {
            "name": "DOB"
        }
    },
    "etag": "\"1100c53e-0000-0200-0000-627138980000\"",
    "metrics": {
        "statusSummary": {
            "status": "scheduled"
        }
    },
    "activities": []
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | 新しく作成されたフロー実行の ID。 詳しくは、 [フロー仕様の検索](../api/collect/database-nosql.md#specs) を参照してください。 |
| `createdAt` | フロー実行が作成された日時を指定する unix タイムスタンプ。 |
| `updatedAt` | フロー実行が最後に更新された日時を示す unix タイムスタンプ。 |
| `createdBy` | フロー実行を作成したユーザーの組織 ID。 |
| `updatedBy` | フロー実行を最後に更新したユーザーの組織 ID。 |
| `createdClient` | フロー実行を作成したアプリケーションクライアント。 |
| `updatedClient` | フローの実行を最後に更新したアプリケーションクライアント。 |
| `sandboxId` | フロー実行を含むサンドボックスの ID。 |
| `sandboxName` | フロー実行を含むサンドボックスの名前。 |
| `imsOrgId` | 組織 ID。 |
| `flowId` | フロー実行が作成されるフローの ID。 |
| `params.windowStartTime` | データを取り込む期間のウィンドウの開始時間を定義する整数。 値は UNIX 時間で表されます。 |
| `params.windowEndTime` | データを取り込む期間の終了時間を定義する整数。 値は UNIX 時間で表されます。 |
| `params.deltaColumn` | デルタ列は、データをパーティション化し、新しく取り込んだデータを履歴データから分離するために必要です。 **注意**:この `deltaColumn` は、最初のフロー実行を作成する場合にのみ必要です。 |
| `params.deltaColumn.name` | デルタ列の名前。 |
| `etag` | フロー実行のリソースのバージョン。 |
| `metrics` | このプロパティは、フロー実行のステータスの概要を表示します。 |

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
          "windowStartTime": "1651584991",
          "windowEndTime": "16515859567"
      }
  }'
```

| パラメーター | 説明 |
| --- | --- |
| `flowId` | フロー実行が作成されるフローの ID。 |
| `params.windowStartTime` | データを取り込む期間のウィンドウの開始時間を定義する整数。 値は UNIX 時間で表されます。 |
| `params.windowEndTime` | データを取り込む期間の終了時間を定義する整数。 値は UNIX 時間で表されます。 |

**応答**

正常な応答は、新しく作成されたフロー実行の詳細（一意の実行を含む）を返します `id`.


```json
{
    "id": "3fb0418e-1804-45d6-8d56-dd51f05c0baf",
    "createdAt": 1651587212543,
    "updatedAt": 1651587223839,
    "createdBy": "{CREATED_BY}",
    "updatedBy": "{UPDATED_BY}",
    "createdClient": "{CREATED_CLIENT}",
    "updatedClient": "{UPDATED_CLIENT}",
    "sandboxId": "{SANDBOX_ID}",
    "sandboxName": "prod",
    "imsOrgId": "{ORGANIZATION_ID}",
    "flowId": "3abea21c-7e36-4be1-bec1-d3bad0e3e0de",
    "params": {
        "windowStartTime": "1651584991",
        "windowEndTime": "16515859567"
    },
    "etag": "\"1100c53e-0000-0200-0000-627138980000\"",
    "metrics": {
        "statusSummary": {
            "status": "scheduled"
        }
    },
    "activities": []
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | 新しく作成されたフロー実行の ID。 詳しくは、 [フロー仕様の検索](../api/collect/cloud-storage.md#specs) ファイルベースの実行仕様の詳細については、を参照してください。 |
| `createdAt` | フロー実行が作成された日時を指定する unix タイムスタンプ。 |
| `updatedAt` | フロー実行が最後に更新された日時を示す unix タイムスタンプ。 |
| `createdBy` | フロー実行を作成したユーザーの組織 ID。 |
| `updatedBy` | フロー実行を最後に更新したユーザーの組織 ID。 |
| `createdClient` | フロー実行を作成したアプリケーションクライアント。 |
| `updatedClient` | フローの実行を最後に更新したアプリケーションクライアント。 |
| `sandboxId` | フロー実行を含むサンドボックスの ID。 |
| `sandboxName` | フロー実行を含むサンドボックスの名前。 |
| `imsOrgId` | 組織 ID。 |
| `flowId` | フロー実行が作成されるフローの ID。 |
| `params.windowStartTime` | データを取り込む期間のウィンドウの開始時間を定義する整数。 値は UNIX 時間で表されます。 |
| `params.windowEndTime` | データを取り込む期間の終了時間を定義する整数。 値は UNIX 時間で表されます。 |
| `etag` | フロー実行のリソースのバージョン。 |
| `metrics` | このプロパティは、フロー実行のステータスの概要を表示します。 |


## フロー実行の監視

フロー実行が作成されたら、フロー実行を通じて取り込まれるデータを監視して、フロー実行、完了ステータス、エラーに関する情報を確認できます。 API を使用してフロー実行を監視するには、 [API でのデータフローの監視 ](./monitor.md). Platform UI を使用してフロー実行を監視するには、 [監視ダッシュボードを使用したソースデータフローの監視](../../../dataflows/ui/monitor-sources.md).
