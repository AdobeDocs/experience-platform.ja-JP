---
title: 作業指示 API エンドポイント
description: Data Hygiene API の /workorder エンドポイントを使用すると、消費者 ID の削除タスクをプログラムで管理できます。
exl-id: f6d9c21e-ca8a-4777-9e5f-f4b2314305bf
source-git-commit: e4cc78591d0d3b4abd660956b1263092697d63d5
workflow-type: tm+mt
source-wordcount: '989'
ht-degree: 49%

---

# 作業指示エンドポイント

>[!IMPORTANT]
>
>現在、Adobe Experience Platformのデータ衛生機能は、Adobeヘルスケアシールドまたはプライバシーシールドを購入した組織でのみ使用できます。

この `/workorder` Data Whealthy API のエンドポイントを使用すると、Adobe Experience Platformで消費者の削除要求をプログラムで管理できます。

## はじめに

このガイドで使用するエンドポイントは、Data Hygiene API の一部です。先に進む前に、[概要](./overview.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## 消費者削除リクエストの作成 {#delete-consumers}

`/workorder` エンドポイントに POST リクエストを行うことで、単一のデータセットまたはすべてのデータセットから 1 つまたは複数の消費者 ID を削除できます。

**API 形式**

```http
POST /workorder
```

**リクエスト**

リクエストペイロードで指定された `datasetId` の値に応じて、API 呼び出しは、すべてのデータセットまたは指定する単一のデータセットから消費者 ID を削除します。次のリクエストは、特定のデータセットから 3 つの消費者 ID を削除します。

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
        "displayName": "Example Consumer Delete Request",
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
| `action` | 実行するアクション。値はに設定する必要があります `delete_identity` （消費者の削除用） |
| `datasetId` | 単一のデータセットから削除する場合、この値は、当該データセットの ID である必要があります。すべてのデータセットから削除する場合、値を `ALL` に設定します。<br><br>単一のデータセットを指定する場合、データセットの関連するエクスペリエンスデータモデル（XDM）スキーマには、プライマリ ID が定義されている必要があります。 |
| `displayName` | 消費者削除要求の表示名。 |
| `description` | 消費者削除要求の説明。 |
| `identities` | 削除する情報を持つ少なくとも 1 人のユーザーの ID を含む配列。各 ID は、[ID 名前空間](../../identity-service/namespaces.md)および値で構成されます。<ul><li>`namespace`：ID 名前空間を表す、単一の文字列プロパティ `code` が含まれます。 </li><li>`id`：ID 値。</ul>`datasetId` が単一のデータセットを指定している場合、`identities` 以下の各エンティティは、スキーマのプライマリ ID と同じ ID 名前空間を使用する必要があります。<br><br>`datasetId` が `ALL` に設定されている場合、`identities` 配列は、各データセットが異なる可能性があるので、単一の名前空間に制限されません。ただし、[ID サービス](https://developer.adobe.com/experience-platform-apis/references/identity-service/#operation/getIdNamespaces)でレポートされるように、リクエストは、依然として組織で使用できる名前空間の制約を受けます。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、消費者削除の詳細を返します。

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
  "displayName": "Example Consumer Delete Request",
  "description": "Cleanup identities required by Jira request 12345."
}
```

| プロパティ | 説明 |
| --- | --- |
| `workorderId` | 削除指示の ID。これは、後で削除のステータスを参照するのに使用できます。 |
| `orgId` | 組織 ID。 |
| `bundleId` | この削除順序が関連付けられているバンドルの ID。デバッグ目的で使用されます。 複数の削除オーダーはまとめてバンドルされ、ダウンストリームサービスによって処理されます。 |
| `action` | 作業指示によって実行されるアクション。 消費者による削除の場合、値は `identity-delete`. |
| `createdAt` | 削除指示が作成された際のタイムスタンプ。 |
| `updatedAt` | 削除順序が最後に更新された日時のタイムスタンプ。 |
| `status` | 削除指示の現在のステータス。 |
| `createdBy` | 削除指示を作成したユーザー。 |
| `datasetId` | リクエストの対象となるデータセットの ID。 リクエストがすべてのデータセットに対しての場合、値はに設定されます。 `ALL`. |

{style=&quot;table-layout:auto&quot;}

## 消費者削除のステータスの取得 (#lookup)

後 [消費者削除リクエストの作成](#delete-consumers)の場合は、データリクエストを使用して、ステータスをGETで確認できます。

**API 形式**

```http
GET /workorder/{WORK_ORDER_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{WORK_ORDER_ID}` | この `workorderId` 検索している消費者の削除数を示します。 |

{style=&quot;table-layout:auto&quot;}

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
  "displayName": "Example Consumer Delete Request",
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
| `bundleId` | この削除順序が関連付けられているバンドルの ID。デバッグ目的で使用されます。 複数の削除オーダーはまとめてバンドルされ、ダウンストリームサービスによって処理されます。 |
| `action` | 作業指示によって実行されるアクション。 消費者による削除の場合、値は `identity-delete`. |
| `createdAt` | 削除指示が作成された際のタイムスタンプ。 |
| `updatedAt` | 削除順序が最後に更新された日時のタイムスタンプ。 |
| `status` | 削除指示の現在のステータス。 |
| `createdBy` | 削除指示を作成したユーザー。 |
| `datasetId` | リクエストの対象となるデータセットの ID。 リクエストがすべてのデータセットに対しての場合、値はに設定されます。 `ALL`. |
| `productStatusDetails` | リクエストに関連するダウンストリームプロセスの現在のステータスをリストする配列。 各配列オブジェクトには、次のプロパティが含まれます。<ul><li>`productName`:ダウンストリームサービスの名前。</li><li>`productStatus`:ダウンストリームサービスからのリクエストの現在の処理ステータス。</li><li>`createdAt`:サービスが最新のステータスを投稿した日時のタイムスタンプ。</li></ul> |

## 消費者削除要求の更新

次の項目を更新し、 `displayName` および `description` 消費者の削除を行うために、PUTリクエストを実行します。

**API 形式**

```http
PUT /workorder{WORK_ORDER_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{WORK_ORDER_ID}` | この `workorderId` 検索している消費者の削除数を示します。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
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
| `displayName` | 消費者削除要求の更新された表示名。 |
| `description` | 消費者削除要求の更新された説明。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、消費者削除の詳細を返します。

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
  "displayName" : "Update - displayName",
  "description" : "Update - description",
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
| `bundleId` | この削除順序が関連付けられているバンドルの ID。デバッグ目的で使用されます。 複数の削除オーダーはまとめてバンドルされ、ダウンストリームサービスによって処理されます。 |
| `action` | 作業指示によって実行されるアクション。 消費者による削除の場合、値は `identity-delete`. |
| `createdAt` | 削除指示が作成された際のタイムスタンプ。 |
| `updatedAt` | 削除順序が最後に更新された日時のタイムスタンプ。 |
| `status` | 削除指示の現在のステータス。 |
| `createdBy` | 削除指示を作成したユーザー。 |
| `datasetId` | リクエストの対象となるデータセットの ID。 リクエストがすべてのデータセットに対しての場合、値はに設定されます。 `ALL`. |
| `productStatusDetails` | リクエストに関連するダウンストリームプロセスの現在のステータスをリストする配列。 各配列オブジェクトには、次のプロパティが含まれます。<ul><li>`productName`:ダウンストリームサービスの名前。</li><li>`productStatus`:ダウンストリームサービスからのリクエストの現在の処理ステータス。</li><li>`createdAt`:サービスが最新のステータスを投稿した日時のタイムスタンプ。</li></ul> |

{style=&quot;table-layout:auto&quot;}
