---
title: 作業指示 API エンドポイント
description: Data Hygiene API の/workorder エンドポイントを使用すると、ID の削除タスクをプログラムで管理できます。
badgeBeta: label="ベータ版" type="Informative"
role: Developer
badge: ベータ版
exl-id: f6d9c21e-ca8a-4777-9e5f-f4b2314305bf
source-git-commit: bf819d506b0ee6f3aba6850f598ee46f16695dfa
workflow-type: tm+mt
source-wordcount: '1278'
ht-degree: 56%

---

# 作業指示エンドポイント {#work-order-endpoint}

Data Hygiene API の `/workorder` エンドポイントを使用すると、Adobe Experience Platformのレコード削除リクエストをプログラムで管理できます。

>[!IMPORTANT]
> 
>レコード削除機能は現在Betaにあり、**限定リリース** でのみ使用できます。 すべてのお客様にご利用いただけるわけではありません。 レコード削除リクエストは、限定リリースの組織でのみ使用できます。
>
>レコードの削除は、データクレンジング、匿名データの削除、またはデータの最小化のために使用されます。これらは、EU 一般データ保護規則（GDPR）などのプライバシー規制に関するデータサブジェクト権利リクエスト（コンプライアンス）に対して使用するためのものでは&#x200B;**ありません**。コンプライアンスに関するユースケースについて詳しくは、[Adobe Experience Platform Privacy Service](../../privacy-service/home.md) を参照してください。

## はじめに

このガイドで使用するエンドポイントは、Data Hygiene API の一部です。先に進む前に、[概要](./overview.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## レコード削除リクエストの作成 {#create}

`/workorder` エンドポイントにPOSTリクエストを行うことで、単一のデータセットまたはすべてのデータセットから 1 つまたは複数の ID を削除できます。

>[!IMPORTANT]
> 
>毎月送信できる一意の ID レコード削除の合計数には、異なる制限があります。 これらの制限は、ライセンス契約に基づいています。 Adobe Real-time Customer Data PlatformとAdobe Journey Optimizerのすべてのエディションを購入した組織は、毎月最大 100,000 件の ID レコード削除を送信できます。 **Adobeの Healthcare Shield** または **Adobeのプライバシーとセキュリティシールド** を購入した組織は、毎月、最大 600,000 件の ID レコード削除を送信できます。<br>UI を使用した単一の [ レコード削除リクエスト ](../ui/record-delete.md) によって、一度に 10,000 個の ID を送信できます。 レコードを削除する API メソッドでは、一度に 100,000 個の ID を送信できます。<br> ベストプラクティスとして、ID の制限を上限とするリクエストあたり、できるだけ多くの ID を送信します。 大量の ID を削除する場合は、少量の ID や、レコード削除リクエストごとに 1 つの ID を送信しないでください。

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

[ レコードの削除リクエストを作成 ](#create) した後は、GETリクエストを使用してそのステータスを確認できます。

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

PUTリクエストを行うことで、レコード削除の `displayName` と `description` を更新できます。

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
