---
title: 作業指示 API エンドポイント
description: Data Whealthy API の/workorder エンドポイントを使用すると、消費者 ID の削除タスクをプログラムで管理できます。
source-git-commit: 931b847761e649696aa8433d53233593efd4d1ee
workflow-type: tm+mt
source-wordcount: '1000'
ht-degree: 5%

---

# 作業指示エンドポイント

>[!IMPORTANT]
>
>現在、Adobe Experience Platformのデータ衛生機能は、医療用Adobeシールドを購入した組織でのみ使用できます。

この `/workorder` Data Whealthy API のエンドポイントを使用すると、Adobe Experience Platformの消費者 ID の削除タスクをプログラムで管理できます。

## はじめに

このガイドで使用するエンドポイントは、データ衛生 API に含まれています。 続行する前に、 [概要](./overview.md) 関連ドキュメントへのリンク、このドキュメントの API 呼び出し例の読み方のガイド、および任意のExperience PlatformAPI を正しく呼び出すために必要な必須ヘッダーに関する重要な情報。

## ID を削除 {#delete-identities}

に対してPOSTリクエストを実行することで、1 つのデータセットまたはすべてのデータセットから 1 つ以上の消費者 ID を削除できます `/workorder` endpoint.

**API 形式**

```http
POST /workorder
```

**リクエスト**

の値に応じて `datasetId` リクエストペイロードで指定された API 呼び出しにより、すべてのデータセット、または指定した単一のデータセットから消費者 ID が削除されます。 次のリクエストでは、特定のデータセットから 3 つの消費者 ID を削除します。

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
| `action` | 実行するアクション。 値はに設定する必要があります `delete_identity` id を削除する際。 |
| `datasetId` | 1 つのデータセットから削除する場合、この値は、該当するデータセットの ID にする必要があります。 すべてのデータセットから削除する場合は、値をに設定します。 `ALL`.<br><br>単一のデータセットを指定する場合、データセットに関連付けられているエクスペリエンスデータモデル (XDM) スキーマには、プライマリ ID が定義されている必要があります。 |
| `identities` | 情報を削除する少なくとも 1 人のユーザーの ID を含む配列。 各 ID は、 [id 名前空間](../../identity-service/namespaces.md) 値：<ul><li>`namespace`:単一の文字列プロパティを含み、 `code`:id 名前空間を表します。 </li><li>`id`：ID 値。</ul>If `datasetId` は 1 つのデータセットを指定します。各エンティティは以下にあります。 `identities` は、スキーマのプライマリ ID と同じ ID 名前空間を使用する必要があります。<br><br>If `datasetId` が `ALL`、 `identities` 各データセットは異なる可能性があるので、配列はどの単一の名前空間にも制限されません。 ただし、で報告されるように、リクエストは、組織で使用可能な名前空間に制約があります。 [ID サービス](https://developer.adobe.com/experience-platform-apis/references/identity-service/#operation/getIdNamespaces). |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、ID 削除の詳細を返します。

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
| `workorderId` | 削除順序の ID。 これは、後で削除のステータスを検索するために使用できます。 |
| `orgId` | 組織の ID。 |
| `batchId` | この削除順序が関連付けられているバッチの ID。デバッグ目的で使用されます。 複数の削除オーダーは、まとめて 1 つのバッチにバンドルされ、ダウンストリームサービスで処理されます。 |
| `bundleOrdinal` | ダウンストリーム処理用のバッチにバンドルされた際に、この削除順序を受け取った順序。 デバッグ目的で使用されます。 |
| `payloadByteSize` | この削除順序を作成したリクエストペイロードで提供された ID のリストのサイズ（バイト単位）。 |
| `operationCount` | この削除順序が適用される ID の数。 |
| `createdAt` | 削除順序が作成された日時のタイムスタンプ。 |
| `responseMessage` | システムが返した最新の応答。 処理中にエラーが発生した場合、この値は JSON 文字列になり、何が問題になったかを理解するのに役立つ詳細なエラー情報が含まれます。 |
| `status` | 削除オーダーの現在のステータス。 |
| `createdBy` | 削除の順序を作成したユーザー。 |

{style=&quot;table-layout:auto&quot;}

## すべての ID 削除のステータスのリスト {#list}

すべての ID 削除のステータスをリストするには、GETリクエストを実行します。

**API 形式**

```http
GET /workorder?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{QUERY_PARAMS}` | リスト呼び出し用のオプションのクエリパラメーターのリストです。複数のパラメーターを区切ります ( `&` 文字。 受け入れられるクエリパラメーターは次のとおりです。<ul><li>`data`  — に設定した場合のブール値 `true`には、削除注文に対して受信したすべての追加の要求および応答データが含まれます。 デフォルト値は `false` です。</li><li>`start`  — 削除注文を検索するための期間の開始時のタイムスタンプ。</li><li>`end`  — 削除注文を検索するためのタイムフレームの終わりのタイムスタンプ。</li><li>`page`  — 返す特定の応答ページ。</li><li>`limit` - 1 ページに表示するレコードの数。</li></ul> |

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

正常な応答は、すべての削除操作の詳細（現在のステータスも含む）を返します。 次の応答の例は、スペースを節約するために切り捨てられています。

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
| `results` | 削除オーダーのリストとその詳細が含まれます。 削除順序のプロパティについて詳しくは、 [削除する注文の検索](#lookup). |
| `total` | 現在のフィルターに基づいて検出された削除注文の合計数です。 |
| `count` | 応答の各ページで見つかった削除注文の合計数。 |
| `_links` | 残りの応答を調べるのに役立つページネーション情報が含まれています。<ul><li>`next`:応答の次のページの URL が含まれます。</li><li>`page`:応答内の別のページにアクセスする、または各ページで返される項目の数を調整する URL テンプレートが含まれます。</li></ul> |

{style=&quot;table-layout:auto&quot;}

## ID 削除のステータスの取得 (#lookup)

にリクエストを送信した後 [id の削除](#delete-identities)の場合は、データリクエストを使用して、ステータスをGETで確認できます。

**API 形式**

```http
GET /workorder/{WORK_ORDER_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{WORK_ORDER_ID}` | この `workorderId` 検索する id 削除の件数です。 |

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

正常な応答は、削除操作の詳細（現在のステータスを含む）を返します。

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
| `workorderId` | 削除順序の ID。 これは、後で削除のステータスを検索するために使用できます。 |
| `orgId` | 組織の ID。 |
| `batchId` | この削除順序が関連付けられているバッチの ID。デバッグ目的で使用されます。 複数の削除オーダーは、まとめて 1 つのバッチにバンドルされ、ダウンストリームサービスで処理されます。 |
| `bundleOrdinal` | ダウンストリーム処理用のバッチにバンドルされた際に、この削除順序を受け取った順序。 デバッグ目的で使用されます。 |
| `payloadByteSize` | この削除順序を作成したリクエストペイロードで提供された ID のリストのサイズ（バイト単位）。 |
| `operationCount` | この削除順序が適用される ID の数。 |
| `createdAt` | 削除順序が作成された日時のタイムスタンプ。 |
| `responseMessage` | システムが返した最新の応答。 処理中にエラーが発生した場合、この値は JSON 文字列になり、何が問題になったかを理解するのに役立つ詳細なエラー情報が含まれます。 |
| `status` | 削除オーダーの現在のステータス。 |
| `createdBy` | 削除の順序を作成したユーザー。 |
