---
title: 作業指示 API エンドポイント
description: Data Hygiene API の /workorder エンドポイントを使用すると、消費者 ID の削除タスクをプログラムで管理できます。
exl-id: f6d9c21e-ca8a-4777-9e5f-f4b2314305bf
hide: true
hidefromtoc: true
source-git-commit: 7f1e4bdf54314cab1f69619bcbb34216da94b17e
workflow-type: tm+mt
source-wordcount: '998'
ht-degree: 98%

---

# 作業指示エンドポイント

>[!IMPORTANT]
>
>現在、Adobe Experience Platformのデータ衛生機能は、Healthcare Shield を購入した組織でのみ利用できます。

Data Hygiene API の `/workorder` エンドポイントを使用すると、Adobe Experience Platform の消費者 ID の削除タスクをプログラムで管理できます。

## はじめに

このガイドで使用するエンドポイントは、Data Hygiene API の一部です。先に進む前に、[概要](./overview.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## ID の削除 {#delete-identities}

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
| `action` | 実行するアクション。ID を削除する場合、値は、`delete_identity` に設定されている必要があります。 |
| `datasetId` | 単一のデータセットから削除する場合、この値は、当該データセットの ID である必要があります。すべてのデータセットから削除する場合、値を `ALL` に設定します。<br><br>単一のデータセットを指定する場合、データセットの関連するエクスペリエンスデータモデル（XDM）スキーマには、プライマリ ID が定義されている必要があります。 |
| `identities` | 削除する情報を持つ少なくとも 1 人のユーザーの ID を含む配列。各 ID は、[ID 名前空間](../../identity-service/namespaces.md)および値で構成されます。<ul><li>`namespace`：ID 名前空間を表す、単一の文字列プロパティ `code` が含まれます。 </li><li>`id`：ID 値。</ul>`datasetId` が単一のデータセットを指定している場合、`identities` 以下の各エンティティは、スキーマのプライマリ ID と同じ ID 名前空間を使用する必要があります。<br><br>`datasetId` が `ALL` に設定されている場合、`identities` 配列は、各データセットが異なる可能性があるので、単一の名前空間に制限されません。ただし、[ID サービス](https://developer.adobe.com/experience-platform-apis/references/identity-service/#operation/getIdNamespaces)でレポートされるように、リクエストは、依然として組織で使用できる名前空間の制約を受けます。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答では、ID 削除の詳細が返されます。

```json
{
  "workorderId": "a15345b8-a2d6-4d6f-b33c-5b593e86439a",
  "orgId": "{ORG_ID}",
  "batchId": "fc0cf8af-a176-4107-a31a-381d6af38cbe",
  "bundleOrdinal": 1,
  "payloadByteSize": 362,
  "operationCount": 3,
  "createdAt": 1652122493242,
  "responseMessage": "received",
  "status": "received",
  "createdBy": "{USER_ID}"
}
```

| プロパティ | 説明 |
| --- | --- |
| `workorderId` | 削除指示の ID。これは、後で削除のステータスを参照するのに使用できます。 |
| `orgId` | 組織の ID。 |
| `batchId` | デバッグ目的で使用される、この削除指示が関連付けられたバッチの ID。複数の削除指示が 1 つのバッチにまとめられて、ダウンストリームサービスによって処理されます。 |
| `bundleOrdinal` | ダウンストリーム処理のためにバッチにまとめられた際にこの削除指示を受け取った順序。デバッグ目的で使用されます。 |
| `payloadByteSize` | この削除指示を作成したリクエストペイロードで提供された ID のリストのサイズ（バイト単位）。 |
| `operationCount` | この削除指示が適用される ID の数。 |
| `createdAt` | 削除指示が作成された際のタイムスタンプ。 |
| `responseMessage` | システムから返された最新の応答。処理中にエラーが発生すると、この値は、何が問題だったのかを把握するのに役立つ詳細なエラー情報を含む JSON 文字列になります。 |
| `status` | 削除指示の現在のステータス。 |
| `createdBy` | 削除指示を作成したユーザー。 |

{style=&quot;table-layout:auto&quot;}

## すべての ID 削除のステータスのリスト表示 {#list}

GET リクエストを行うことで、すべての ID 削除のステータスをリスト表示できます。

**API 形式**

```http
GET /workorder?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{QUERY_PARAMS}` | `&` 文字で区切られた複数のパラメーターを含む、リスト呼び出しのためのオプションのクエリパラメーターのリスト。次に、承認されたクエリパラメーターを示します。<ul><li>`data` - ブール値で、`true` に設定すると、削除指示で受け取ったすべての追加のリクエストおよび応答データを含めます。デフォルト値は `false` です。</li><li>`start` - 削除指示を検索する期間の始まりのタイムスタンプ。</li><li>`end` - 削除指示を検索する期間の終わりのタイムスタンプ。</li><li>`page` - 返される特定の応答ページ。</li><li>`limit` - ページごとに表示されるレコードの数。</li></ul> |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/hygiene/workorder \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答では、現在のステータスを含む、すべての削除操作の詳細が返されます。次の応答の例は、スペースを節約するために省略されています。

```json
{
  "results": [
    {
      "workorderId": "4fe4be4f-47f3-477a-927e-f908452513f6",
      "orgId": "{ORG_ID}",
      "batchId": "e62cd6b6-ce3e-49e0-9221-ba1f286a851c",
      "bundleOrdinal": 1,
      "payloadByteSize": 164,
      "operationCount": 1,
      "createdAt": 1650929265295,
      "responseMessage": "received",
      "status": "received",
      "createdBy": "{USER_ID}"
    },
    {
      "workorderId": "e4a662e8-a5f3-497d-8d6a-d26970d8732b",
      "orgId": "{ORG_ID}",
      "batchId": "74fe4e38-ed42-4ca5-8bee-88bdc03ae786",
      "bundleOrdinal": 1,
      "payloadByteSize": 164,
      "operationCount": 1,
      "createdAt": 1650931057899,
      "responseMessage": "received",
      "status": "received",
      "createdBy": "{USER_ID}"
    }
  ],
  "total": 200,
  "count": 50,
  "_links": {
    "next": {
      "href": "https://platform.adobe.io/workorder?page=1&limit=50",
      "templated": false
    },
    "page": {
      "href": "https://platform.adobe.io/workorder?limit={limit}&page={page}",
      "templated": true
    }
  }
}
```

| プロパティ | 説明 |
| --- | --- |
| `results` | 削除指示およびその詳細のリストが含まれます。削除指示のプロパティについて詳しくは、[削除指示の参照](#lookup)に関する節の応答の例を参照してください。 |
| `total` | 現在のフィルターに基づいて見つかった削除指示の合計数。 |
| `count` | 応答の各ページで見つかった削除指示の合計数。 |
| `_links` | 残りの応答を調査するのに役立つページネーション情報が含まれます。<ul><li>`next`：応答の次のページの URL が含まれます。</li><li>`page`：応答で別のページにアクセスしたり、各ページで返される項目の数を調整したりするための URL テンプレートが含まれます。</li></ul> |

{style=&quot;table-layout:auto&quot;}

## ID 削除のステータスの取得（#lookup）

[ID を削除](#delete-identities)するためのリクエストを送信したら、GET リクエストを使用してそのステータスを確認できます。

**API 形式**

```http
GET /workorder/{WORK_ORDER_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{WORK_ORDER_ID}` | 参照している ID 削除の `workorderId`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/hygiene/workorder/ID6c28e2d2d2b54079aadf7be94568f6d3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答では、現在のステータスを含む、削除操作の詳細が返されます。

```json
{
  "workorderId": "4fe4be4f-47f3-477a-927e-f908452513f6",
  "orgId": "{ORG_ID}",
  "batchId": "e62cd6b6-ce3e-49e0-9221-ba1f286a851c",
  "bundleOrdinal": 1,
  "payloadByteSize": 164,
  "operationCount": 1,
  "createdAt": 1650929265295,
  "responseMessage": "received",
  "status": "received",
  "createdBy": "{USER_ID}"
}
```

| プロパティ | 説明 |
| --- | --- |
| `workorderId` | 削除指示の ID。これは、後で削除のステータスを参照するのに使用できます。 |
| `orgId` | 組織の ID。 |
| `batchId` | デバッグ目的で使用される、この削除指示が関連付けられたバッチの ID。複数の削除指示が 1 つのバッチにまとめられて、ダウンストリームサービスによって処理されます。 |
| `bundleOrdinal` | ダウンストリーム処理のためにバッチにまとめられた際にこの削除指示を受け取った順序。デバッグ目的で使用されます。 |
| `payloadByteSize` | この削除指示を作成したリクエストペイロードで提供された ID のリストのサイズ（バイト単位）。 |
| `operationCount` | この削除指示が適用される ID の数。 |
| `createdAt` | 削除指示が作成された際のタイムスタンプ。 |
| `responseMessage` | システムから返された最新の応答。処理中にエラーが発生すると、この値は、何が問題だったのかを把握するのに役立つ詳細なエラー情報を含む JSON 文字列になります。 |
| `status` | 削除指示の現在のステータス。 |
| `createdBy` | 削除指示を作成したユーザー。 |
