---
title: レコード削除リクエスト （作業指示エンドポイント）
description: Data Hygiene API の/workorder エンドポイントを使用すると、ID の削除タスクをプログラムで管理できます。
role: Developer
exl-id: f6d9c21e-ca8a-4777-9e5f-f4b2314305bf
source-git-commit: d569b1d04fa76e0a0e48364a586e8a1a773b9bf2
workflow-type: tm+mt
source-wordcount: '1505'
ht-degree: 48%

---

# レコードの削除リクエスト （作業指示エンドポイント） {#work-order-endpoint}

Data Hygiene API の `/workorder` エンドポイントを使用すると、Adobe Experience Platformのレコード削除リクエストをプログラムで管理できます。

>[!IMPORTANT]
> 
>レコードの削除は、データクレンジング、匿名データの削除、またはデータの最小化のために使用されます。これらは、EU 一般データ保護規則（GDPR）などのプライバシー規制に関するデータサブジェクト権利リクエスト（コンプライアンス）に対して使用するためのものでは&#x200B;**ありません**。コンプライアンスに関するユースケースについて詳しくは、[Adobe Experience Platform Privacy Service](../../privacy-service/home.md) を参照してください。

## はじめに

このガイドで使用するエンドポイントは、Data Hygiene API の一部です。先に進む前に、[概要](./overview.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## 割り当て量と処理タイムライン {#quotas}

レコードの削除リクエストは、組織のライセンス使用権限によって決まる、1 日ごとおよび 1 か月ごとの識別子送信制限の対象です。 これらの制限は、UI ベースの削除リクエストと API ベースの削除リクエストの両方に適用されます。

>[!NOTE]
>
>1 日に最大 **1,000,000 個の識別子を送信できますが** その権限は残りの月別クォータで与えられている場合に限られます。 1 か月の上限が 100 万未満の場合、毎日の送信がその上限を超えることはできません。

### 製品別の月間送信使用権限 {#quota-limits}

次の表に、製品および使用権限レベル別の識別子の送信制限の概要を示します。 各製品の月額上限は、固定の識別子上限またはライセンス取得済みデータボリュームに関連付けられた割合ベースのしきい値の、2 つの値のいずれか小さい方です。

| 製品 | 使用権限の説明 | 月間キャップ （いずれか小さい方） |
|----------|-------------------------|---------------------------------|
| Real-Time CDPまたはAdobe Journey Optimizer | プライバシーとセキュリティシールドまたは Healthcare Shield アドオンなし | 2,000,000 個の識別子（アドレス可能なオーディエンスの 5%） |
| Real-Time CDPまたはAdobe Journey Optimizer | Privacy and Security Shield または Healthcare Shield アドオンを使用 | 15,000,000 個の識別子（アドレス可能なオーディエンスの 10%） |
| Customer Journey Analytics | プライバシーとセキュリティシールドまたは Healthcare Shield アドオンなし | CJAの権利行あたり 2,000,000 の識別子または 1,000 の識別子 |
| Customer Journey Analytics | Privacy and Security Shield または Healthcare Shield アドオンを使用 | CJAの権利行あたり 15,000,000 個の識別子または 2,000 個の識別子 |

>[!NOTE]
>
> ほとんどの組織では、実際のアドレス可能なオーディエンスまたはCJA行の使用権限に基づいて、月間の上限が引き下げられます。

クォータは、毎月 1 日にリセットされます。 未使用の割り当ては引き継がれ **い**。

>[!NOTE]
>
>割り当て量は、組織でライセンスを取得した **送信済み識別子** の月次使用権に基づきます。 これらは、システムガードレールによって適用されるのではなく、監視およびレビューされる可能性があります。
>
>レコード削除は **共有サービス** です。 1 か月の上限には、Real-Time CDP、Adobe Journey Optimizer、Customer Journey Analyticsおよび該当する Shield アドオン全体で最高の使用権限が反映されます。

### 識別子の送信のタイムラインの処理 {#sla-processing-timelines}

送信後、レコードの削除リクエストはキューに入り、使用権限レベルに基づいて処理されます。

| 製品と使用権限の説明 | キューの期間 | 最大処理時間（SLA） |
|------------------------------------------------------------------------------------|---------------------|-------------------------------|
| プライバシーとセキュリティシールドまたは Healthcare Shield アドオンなし | 最長 15 日間 | 30 日 |
| Privacy and Security Shield または Healthcare Shield アドオンを使用 | 通常 24 時間 | 15 日 |

組織でさらに上限が必要な場合は、Adobe担当者に連絡して使用権限のレビューを依頼してください。

>[!TIP]
>
>現在のクォータの使用状況または使用権限層を確認するには、[ クォータのリファレンス ガイド ](../api/quota.md) を参照してください。

## レコード削除リクエストの作成 {#create}

`/workorder` エンドポイントに POST リクエストを行うことで、単一のデータセットまたはすべてのデータセットから 1 つまたは複数の ID を削除できます。

>[!TIP]
>
>API を介して送信された各レコード削除リクエストには、最大 100,000 **の ID** を含めることができます。 効率を最大限に高めるには、リクエストごとに可能な限り多くの ID を送信し、単一 ID の作業指示などの少量の送信を避けます。

**API 形式**

```http
POST /workorder
```

>[!NOTE]
>
>データライフサイクルリクエストでは、プライマリ ID または ID マップに基づいたデータセットのみを変更できます。 リクエストでは、プライマリ ID を指定するか、ID マップを指定する必要があります。

**リクエスト**

リクエストペイロードで指定された `datasetId` の値に応じて、API 呼び出しは、すべてのデータセットまたは指定する単一のデータセットから ID を削除します。 次のリクエストは、特定のデータセットから 3 つの ID を削除します。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/hygiene/workorder \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/json' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "action": "delete_identity",
        "datasetId": "c48b51623ec641a2949d339bad69cb15",
        "displayName": "Example Record Delete Request",
        "description": "Cleanup identities required by Jira request 12345.",
        "identities": [
          {
            "namespace": {
              "code": "email"
            },
            "id": "poul.anderson@example.com"
          },
          {
            "namespace": {
              "code": "email"
            },
            "id": "cordwainer.smith@gmail.com"
          },
          {
            "namespace": {
              "code": "email"
            },
            "id": "cyril.kornbluth@yahoo.com"
          }
        ]
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `action` | 実行するアクション。レコードを削除するには、値を `delete_identity` に設定する必要があります。 |
| `datasetId` | 単一のデータセットから削除する場合、この値は、当該データセットの ID である必要があります。すべてのデータセットから削除する場合、値を `ALL` に設定します。<br><br> 単一のデータセットを指定する場合、データセットの関連するエクスペリエンスデータモデル（XDM）スキーマには、プライマリ ID が定義されている必要があります。 データセットにプライマリ ID がない場合、データライフサイクルリクエストで変更するには、データセットに ID マップが必要です。<br>ID マップが存在する場合、`identityMap` という名前の最上位フィールドとして存在します。<br> データセット行の ID マップに多くの ID が含まれている場合がありますが、プライマリとしてマークできるのは 1 つだけであることに注意してください。 `id` がプライマリ ID と一致するように強制するには、`"primary": true` を含める必要があります。 |
| `displayName` | レコード削除リクエストの表示名。 |
| `description` | レコード削除リクエストの説明。 |
| `identities` | 削除する情報を持つ少なくとも 1 人のユーザーの ID を含む配列。各 ID は、[ID 名前空間](../../identity-service/features/namespaces.md)および値で構成されます。<ul><li>`namespace`：ID 名前空間を表す、単一の文字列プロパティ `code` が含まれます。 </li><li>`id`：ID 値。</ul>`datasetId` が単一のデータセットを指定している場合、`identities` 以下の各エンティティは、スキーマのプライマリ ID と同じ ID 名前空間を使用する必要があります。<br><br>`datasetId` が `ALL` に設定されている場合、`identities` 配列は、各データセットが異なる可能性があるので、単一の名前空間に制限されません。ただし、[ID サービス](https://developer.adobe.com/experience-platform-apis/references/identity-service/#operation/getIdNamespaces)でレポートされるように、リクエストは、依然として組織で使用できる名前空間の制約を受けます。 |

{style="table-layout:auto"}

**応答**

応答が成功すると、レコード削除の詳細が返されます。

```json
{
  "workorderId": "a15345b8-a2d6-4d6f-b33c-5b593e86439a",
  "orgId": "{ORG_ID}",
  "bundleId": "BN-35c1676c-3b4f-4195-8d6c-7cf5aa21efdd",
  "action": "identity-delete",
  "createdAt": "2022-07-21T18:05:28.316029Z",
  "updatedAt": "2022-07-21T17:59:43.217801Z",
  "status": "received",
  "createdBy": "{USER_ID}",
  "datasetId": "c48b51623ec641a2949d339bad69cb15",
  "displayName": "Example Record Delete Request",
  "description": "Cleanup identities required by Jira request 12345."
}
```

| プロパティ | 説明 |
| --- | --- |
| `workorderId` | 削除指示の ID。これは、後で削除のステータスを参照するのに使用できます。 |
| `orgId` | 組織 ID。 |
| `bundleId` | この削除指示が関連付けられているバンドルの ID（デバッグ目的で使用される）。複数の削除指示が 1 つのバンドルにまとめられて、ダウンストリームサービスで処理されます。 |
| `action` | 作業指示によって実行されるアクション。レコードを削除する場合、値は `identity-delete` です。 |
| `createdAt` | 削除指示が作成されたときのタイムスタンプ。 |
| `updatedAt` | 削除指示が最後に更新されたときのタイムスタンプ。 |
| `status` | 削除指示の現在のステータス。 |
| `createdBy` | 削除指示を作成したユーザー。 |
| `datasetId` | リクエストの対象となるデータセットの ID。すべてのデータセットに対するリクエストの場合、値は `ALL` に設定されます。 |

{style="table-layout:auto"}

## レコード削除のステータスの取得 {#lookup}

[ レコード削除リクエストを作成 ](#create) した後は、GET リクエストを使用して、そのステータスを確認できます。

**API 形式**

```http
GET /workorder/{WORK_ORDER_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{WORK_ORDER_ID}` | 参照しているレコード削除の `workorderId`。 |

{style="table-layout:auto"}

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/hygiene/workorder/BN-35c1676c-3b4f-4195-8d6c-7cf5aa21efdd \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答では、現在のステータスを含む、削除操作の詳細が返されます。

```json
{
  "workorderId": "a15345b8-a2d6-4d6f-b33c-5b593e86439a",
  "orgId": "{ORG_ID}",
  "bundleId": "BN-35c1676c-3b4f-4195-8d6c-7cf5aa21efdd",
  "action": "identity-delete",
  "createdAt": "2022-07-21T18:05:28.316029Z",
  "updatedAt": "2022-07-21T17:59:43.217801Z",
  "status": "received",
  "createdBy": "{USER_ID}",
  "datasetId": "c48b51623ec641a2949d339bad69cb15",
  "displayName": "Example Record Delete Request",
  "description": "Cleanup identities required by Jira request 12345.",
  "productStatusDetails": [
    {
        "productName": "Data Management",
        "productStatus": "success",
        "createdAt": "2022-08-08T16:51:31.535872Z"
    },
    {
        "productName": "Identity Service",
        "productStatus": "success",
        "createdAt": "2022-08-08T16:43:46.331150Z"
    },
    {
        "productName": "Profile Service",
        "productStatus": "waiting",
        "createdAt": "2022-08-08T16:37:13.443481Z"
    }
  ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `workorderId` | 削除指示の ID。これは、後で削除のステータスを参照するのに使用できます。 |
| `orgId` | 組織 ID。 |
| `bundleId` | この削除指示が関連付けられているバンドルの ID（デバッグ目的で使用される）。複数の削除指示が 1 つのバンドルにまとめられて、ダウンストリームサービスで処理されます。 |
| `action` | 作業指示によって実行されるアクション。レコードを削除する場合、値は `identity-delete` です。 |
| `createdAt` | 削除指示が作成されたときのタイムスタンプ。 |
| `updatedAt` | 削除指示が最後に更新されたときのタイムスタンプ。 |
| `status` | 削除指示の現在のステータス。 |
| `createdBy` | 削除指示を作成したユーザー。 |
| `datasetId` | リクエストの対象となるデータセットの ID。すべてのデータセットに対するリクエストの場合、値は `ALL` に設定されます。 |
| `productStatusDetails` | リクエストに関連するダウンストリームプロセスの現在のステータスをリストする配列。 各配列オブジェクトには、次のプロパティが含まれています。<ul><li>`productName`：ダウンストリームサービスの名前。</li><li>`productStatus`：ダウンストリームサービスでのリクエストの現在の処理ステータス。</li><li>`createdAt`：最新のステータスがサービスからポストされたときのタイムスタンプ。</li></ul> |

## レコード削除リクエストの更新

PUT リクエストを行うことで、レコード削除の `displayName` と `description` を更新できます。

**API 形式**

```http
PUT /workorder{WORK_ORDER_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{WORK_ORDER_ID}` | 参照しているレコード削除の `workorderId`。 |

{style="table-layout:auto"}

**リクエスト**

```shell
curl -X PUT \
  https://platform.adobe.io/data/core/hygiene/workorder/BN-35c1676c-3b4f-4195-8d6c-7cf5aa21efdd \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "displayName" : "Update - displayName",
        "description" : "Update - description"
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `displayName` | レコード削除リクエストの更新された表示名。 |
| `description` | レコード削除リクエストの更新された説明。 |

{style="table-layout:auto"}

**応答**

応答が成功すると、レコード削除の詳細が返されます。

```json
{
    "workorderId": "DI-61828416-963a-463f-91c1-dbc4d0ddbd43",
    "orgId": "{ORG_ID}",
    "bundleId": "BN-aacacc09-d10c-48c5-a64c-2ced96a78fca",
    "action": "identity-delete",
    "createdAt": "2024-06-12T20:02:49.398448Z",
    "updatedAt": "2024-06-13T21:35:01.944749Z",
    "operationCount": 1,
    "status": "ingested",
    "createdBy": "{USER_ID}",
    "datasetId": "666950e6b7e2022c9e7d7a33",
    "datasetName": "Acme_Dataset_E2E_Identity_Map_Schema_5_1718178022379",
    "displayName": "Updated Display Name",
    "description": "Updated Description",
    "productStatusDetails": [
        {
            "productName": "Data Management",
            "productStatus": "waiting",
            "createdAt": "2024-06-12T20:11:18.447747Z"
        },
        {
            "productName": "Identity Service",
            "productStatus": "success",
            "createdAt": "2024-06-12T20:36:09.020832Z"
        },
        {
            "productName": "Profile Service",
            "productStatus": "waiting",
            "createdAt": "2024-06-12T20:11:18.447747Z"
        },
        {
            "productName": "Journey Orchestrator",
            "productStatus": "success",
            "createdAt": "2024-06-12T20:12:19.843199Z"
        }
    ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `workorderId` | 削除指示の ID。これは、後で削除のステータスを参照するのに使用できます。 |
| `orgId` | 組織 ID。 |
| `bundleId` | この削除指示が関連付けられているバンドルの ID（デバッグ目的で使用される）。複数の削除指示が 1 つのバンドルにまとめられて、ダウンストリームサービスで処理されます。 |
| `action` | 作業指示によって実行されるアクション。レコードを削除する場合、値は `identity-delete` です。 |
| `createdAt` | 削除指示が作成されたときのタイムスタンプ。 |
| `updatedAt` | 削除指示が最後に更新されたときのタイムスタンプ。 |
| `status` | 削除指示の現在のステータス。 |
| `createdBy` | 削除指示を作成したユーザー。 |
| `datasetId` | リクエストの対象となるデータセットの ID。すべてのデータセットに対するリクエストの場合、値は `ALL` に設定されます。 |
| `productStatusDetails` | リクエストに関連するダウンストリームプロセスの現在のステータスをリストする配列。 各配列オブジェクトには、次のプロパティが含まれています。<ul><li>`productName`：ダウンストリームサービスの名前。</li><li>`productStatus`：ダウンストリームサービスでのリクエストの現在の処理ステータス。</li><li>`createdAt`：最新のステータスがサービスからポストされたときのタイムスタンプ。</li></ul> |

{style="table-layout:auto"}
