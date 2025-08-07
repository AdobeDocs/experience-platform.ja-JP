---
title: Dataset Expiration API エンドポイント
description: Data Hygiene API の /ttl エンドポイントを使用すると、Adobe Experience Platform のデータセット有効期限をプログラムでスケジュール設定できます。
role: Developer
exl-id: fbabc2df-a79e-488c-b06b-cd72d6b9743b
source-git-commit: ca6d7d257085da65b3f08376f0bd32e51e293533
workflow-type: tm+mt
source-wordcount: '2331'
ht-degree: 19%

---

# データセットの有効期限エンドポイント

Data Hygiene API の `/ttl` エンドポイントを使用して、Adobe Experience Platformのデータセットをいつ削除するかをスケジュールします。

データセットの有効期限は、遅延した削除操作です。 データセットは、暫定的に保護されず、スケジュールされた有効期限の前に他の手段で削除される可能性があります。

>[!NOTE]
>
>有効期限は特定の瞬間として指定されますが、有効期限が切れてから実際に削除が開始されるまで、最大 24 時間の遅延が発生する可能性があります。削除が開始されると、すべてのデータセットの痕跡がExperience Platform システムから削除されるまで、最大 7 日間かかる可能性があります。

削除を開始する前に、有効期限をキャンセルするか、スケジュールされた時間を変更できます。 キャンセルされた有効期限を再オープンするには、新しい有効期限を設定します。

削除が開始されると、有効期限ジョブは `executing` とマークされ、変更できなくなります。 データセットは、最大 7 日間復元できる可能性がありますが、それは手動のAdobe サービスリクエストを通じてのみです。 削除中、データレイク、ID サービス、リアルタイム顧客プロファイルは、それぞれデータセットの内容を個別に削除します。 削除が完了すると、有効期限は `completed` とマークされます。

>[!WARNING]
>
>データセットの有効期限が切れるように設定されている場合、ダウンストリームワークフローに悪影響が及ばないよう、データをそのデータセットに取り込む可能性があるデータフローを手動で変更する必要があります。

Advanced Data Lifecycle Management では、データセット有効期限エンドポイントを介したデータセット削除と、[workorder エンドポイント ](./workorder.md) を介したプライマリ ID を使用した ID 削除（行レベルのデータ）をサポートしています。 また、Experience Platform UI を使用して [ データセットの有効期限 ](../ui/dataset-expiration.md) および [ レコードの削除 ](../ui/record-delete.md) を管理することもできます。 詳しくは、リンクされたドキュメントを参照してください。

>[!NOTE]
>
>データ ライフサイクルはバッチ削除をサポートしていません。

## はじめに

このガイドで使用するエンドポイントは、Data Hygiene API の一部です。続行する前に、[API ガイド ](./overview.md) を参照し、CRUD 操作に必要なヘッダー、エラーメッセージ、Postman コレクション、サンプル API 呼び出しの読み取り方法を確認してください。

>[!IMPORTANT]
>
>Data Hygiene API を呼び出す場合は、-H `x-sandbox-name: {SANDBOX_NAME}` ヘッダーを使用する必要があります。

## データセット有効期限のリスト {#list}

`/ttl` エンドポイントにGET リクエストを行うことで、組織で設定されているすべてのデータセット有効期限をリストできます。

クエリパラメーターを使用して結果をフィルタリングし、条件を満たす有効期限のみを返します。 各結果には、各データセット有効期限のステータスと設定の詳細が含まれます。

**API 形式**

```http
GET /ttl?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
| --- | --- |
| `{QUERY_PARAMETERS}` | `&` 文字で区切られた複数のパラメーターを含む、オプションのクエリパラメーターのリスト。共通のパラメーターには、ページネーション用の `limit` および `page` があります。サポートされるクエリパラメーターの完全なリストについては、[ 付録の節 ](#query-params) サポートされるクエリパラメーターの完全なリストを参照してください。 最もよく使用されるパラメーターは、以下および付録に含まれています。 |
| `author` | データセットの有効期限を最近更新または作成したユーザーでフィルタリングします。 SQL に似たパターン（`LIKE %john%` など）をサポートします。 |
| `datasetId` | 特定のデータセット ID で有効期限をフィルタリングします。 |
| `datasetName` | データセット名の一致に対する、大文字と小文字を区別しないフィルター。 |
| `status` | ステータスのコンマ区切りリスト（`pending`、`executing`、`cancelled`、`completed`）でフィルタリングします。 |
| `expiryDate` | 特定の有効期限を使用して有効期限をフィルタリングします。 |
| `limit` | 返される結果の最大数を指定します（1 ～ 100、デフォルト：25）。 |
| `page` | 0 から始まるインデックスを持つページ番号（デフォルトのページサイズ：50、最大：100）。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、2021 年 8 月 1 日（PT）より前に更新され、名前が「Jane Doe」と一致するユーザーによる最終更新日のすべてのデータセット有効期限を取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/hygiene/ttl?updatedToDate=2021-08-01&author=LIKE%20%25Jane%20Doe%25 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、結果のデータセット有効期限がリスト表示されます。次の例は、スペースを節約するために省略されています。

>[!IMPORTANT]
>
>応答の `ttlId` は `{DATASET_EXPIRATION_ID}` とも呼ばれます。 どちらも、データセットの有効期限の一意の ID を参照します。

```json
{
  "results": [
    {
      "ttlId": "SD-c9f113f2-d751-44bc-bc20-9d5ca0b6ae15",
      "datasetId": "3e9f815ae1194c65b2a4c5ea",
      "datasetName": "Acme_Profile_Engagements",
      "sandboxName": "acme-beta",
      "displayName": "Engagement Data Retention Policy",
      "description": "Scheduled expiry for Acme marketing data",
      "imsOrg": "C9D8E7F6A5B41234567890AB@AcmeOrg",
      "status": "pending",
      "expiry": "2027-01-12T17:15:31.000Z",
      "updatedAt": "2026-12-15T12:40:20.000Z",
      "updatedBy": "t.lannister@acme.com <t.lannister@acme.com> 3E9F815AE1194C65B2A4C5EA@acme.com"
    }
  ],
  "current_page": 0,
  "total_pages": 1,
  "total_count": 1
}
```

| プロパティ | 説明 |
| --- | --- |
| `results` | データセット有効期限設定の配列。 |
| `ttlId` | データセット有効期限設定の一意の ID。 |
| `datasetId` | この設定に関連付けられたデータセットの一意の ID。 |
| `datasetName` | データセットの名前。 |
| `sandboxName` | このデータセット有効期限が設定されているサンドボックス。 |
| `displayName` | 人間が判読できる有効期限設定の名前。 |
| `description` | 有効期限設定の説明。 |
| `imsOrg` | 一意の組織 ID。 |
| `status` | 有効期限の現在のステータス。 次のいずれか：`pending`、`executing`、`cancelled`、`completed`。 |
| `expiry` | スケジュールされた有効期限の日時（ISO 8601 形式）。 |
| `updatedAt` | この設定の最終更新のタイムスタンプ。 |
| `updatedBy` | 設定を最後に更新したユーザーまたはサービスの識別子とメールアドレス。 |
| `current_page` | 現在の結果ページのインデックス（0 から始まります）。 |
| `total_pages` | 使用可能な結果ページの合計数。 |
| `total_count` | 返されたデータセット有効期限設定レコードの合計数です。 |

{style="table-layout:auto"}

## データセット有効期限の検索 {#lookup}

データセット有効期限 ID またはデータセット ID をパスパラメーターとして使用してGET リクエストを実行し、特定のデータセット有効期限設定の詳細を取得します。

>[!IMPORTANT]
>
>パスには、データセット有効期限 ID （例：`SD-xxxxxx-xxxx`）またはデータセット ID を指定できます。 応答の `ttlId` は、データセットの有効期限の一意の ID です。

**API 形式**

```http
GET /ttl/{ID}
GET /ttl/{ID}?include=history
```

| パラメーター | 説明 |
| --- | --- |
| `{ID}` | データセット有効期限設定の一意の ID。 データセット有効期限 ID またはデータセット ID を指定できます。 |
| `include` | （オプション） `history` に設定した場合、応答には設定の変更イベントを含む `history` 配列が含まれます。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストでは、`62759f2ede9e601b63a2ee14` データセットの有効期限の詳細を調べます。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/hygiene/ttl/62759f2ede9e601b63a2ee14 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、データセット有効期限の詳細が返されます。

```json
{
    "ttlId": "SD-c8c75921-2416-4be7-9cfd-9ab01de66c5f",
    "datasetId": "62759f2ede9e601b63a2ee14",
    "datasetName": "XtVRwq9-38734",
    "sandboxName": "prod",
    "displayName": "Delete Acme Data before 2025",
    "description": "The Acme information in this dataset is licensed for our use through the end of 2024.",
    "imsOrg": "885737B25DC460C50A49411B@AdobeOrg",
    "status": "pending",
    "expiry": "2035-09-25T00:00:00Z",
    "updatedAt": "2025-05-01T19:00:55.000Z",
    "updatedBy": "Jane Doe <jdoe@adobe.com> 77A51F696282E48C0A494 012@64d18d6361fae88d49412d.e",
}
```

| プロパティ | 説明 |
| --- | --- |
| `ttlId` | データセット有効期限設定の一意の ID。 |
| `datasetId` | データセットの一意の ID。 |
| `datasetName` | データセットの名前。 |
| `sandboxName` | データセットの有効期限が設定されているサンドボックス。 |
| `displayName` | データセット有効期限設定の、人間が判読できる名前。 |
| `description` | データセット有効期限設定の説明。 |
| `imsOrg` | この設定に関連付けられた一意の組織 ID。 |
| `status` | データセット有効期限設定の現在のステータス。<br>1 つ：`pending`、`executing`、`cancelled`、`completed`。 |
| `expiry` | データセットのスケジュールされた有効期限タイムスタンプ（ISO 8601 形式）。 |
| `updatedAt` | 最新の更新のタイムスタンプ。 |
| `updatedBy` | データセットの有効期限を最後に更新したユーザーまたはサービスの識別子とメール。 |

{style="table-layout:auto"}

### カタログの有効期限タグ

[Catalog API](../../catalog/api/getting-started.md) を使用してデータセットの詳細を検索するには、データセットにアクティブな有効期限がある場合は、そのデータセットが `tags.adobe/hygiene/ttl` の下に表示されます。

次の JSON は、有効期限値が `32503680000000` のデータセットに対して、切り詰められたカタログ API 応答を示しています。 タグは、有効期限を Unix エポックからのミリ秒数としてエンコードします。

```json
{
  "63212313c308d51b997858ba": {
    "name": "Test Dataset",
    "description": "A piecrust promise, made to be broken",
    "imsOrg": "0FCC747E56F59C747F000101@AdobeOrg",
    "sandboxId": "8dc51b90-d0f9-11e9-b164-ed6a398c8b35",
    "tags": {
      "adobe/hygiene/ttl": [ "32503680000000" ],
      ...
    },
    ...
  }
}
```

## データセット有効期限の作成 {#create}

新しいデータセット有効期限設定を作成して、データセットが期限切れになり、削除できるタイミングを定義します。\
データセット ID、有効期限または日時（ISO 8601 形式）、表示名、（オプションで）説明を入力します。

>[!NOTE]
>
>有効期限の値は、日付（YYYY-MM-DD）または日時（YYYY-MM-DDTHH:MM:SSZ）にすることができます。 日付のみを指定した場合、システムはその日の午前 0 時の UTC （00:00:00Z）を使用します。 有効期限は 24 時間以上先である必要があります。

データセットの有効期限を作成するには、次に示すように、POST リクエストを送信します。

>[!TIP]
>
>404 エラーが発生した場合は、リクエストに追加のフォワードスラッシュがないことを確認します。 末尾のスラッシュは、POST リクエストの失敗の原因となる可能性があります。

**API 形式**

```http
POST /ttl
```

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/core/hygiene/ttl \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "datasetId": "3e9f815ae1194c65b2a4c5ea",
        "expiry": "2030-12-31",
        "displayName": "Expiry rule for Acme customers",
        "description": "Set expiration for Acme customer dataset"
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `datasetId` | **必須** 有効期限を適用するデータセットの一意の ID。 |
| `expiry` | **必須。** 有効期限（ISO 8601 形式）。 これは、システム内のデータの存続期間を定義します。 日付のみを指定した場合、デフォルトでは午前 0 時（UTC）（00:00:00Z）になります。 有効期限 **少なくとも 24 時間後にする必要があります**。 <br>**注意**:<ul><li>データセットの有効期限が既に存在する場合、リクエストは失敗します。</li></ul> |
| `displayName` | **必須。** データセットの有効期限設定の、人間が判読できる名前。 |
| `description` | データセットの有効期限設定のオプション説明。 |

**応答**

応答が成功すると、HTTP 201 （作成済み）ステータスと、新しいデータセットの有効期限設定が返されます。

```json
{
  "ttlId": "SD-2aaf113e-3f17-4321-bf29-a2c51152b042",
  "datasetId": "3e9f815ae1194c65b2a4c5ea",
  "datasetName": "Acme_Customer_Data",
  "sandboxName": "acme-prod",
  "displayName": "Expiry rule for Acme customers",
  "description": "Set expiration for Acme customer dataset",
  "imsOrg": "{ORG_ID}",
  "status": "pending",
  "expiry": "2030-12-31T00:00:00Z",
  "updatedAt": "2025-01-02T10:35:45.000Z",
  "updatedBy": "s.stark@acme.com <s.stark@acme.com> 3E9F815AE1194C65B2A4C5EA@acme.com"
}
```

| プロパティ | 説明 |
| --- | --- |
| `ttlId` | 作成したデータセット有効期限設定の一意の ID。 |
| `datasetId` | データセットの一意の ID。 |
| `datasetName` | データセットの名前。 |
| `sandboxName` | このデータセット有効期限が設定されているサンドボックス。 |
| `displayName` | データセット有効期限設定の表示名。 |
| `description` | データセット有効期限設定の説明。 |
| `imsOrg` | この設定に関連付けられた一意の組織 ID。 |
| `status` | データセット有効期限設定の現在のステータス。<br>1 つ：`pending`、`executing`、`cancelled`、`completed`。 |
| `expiry` | データセットに対してスケジュールされた有効期限タイムスタンプ。 |
| `updatedAt` | 最新の更新のタイムスタンプ。 |
| `updatedBy` | データセット有効期限設定を最後に更新したユーザーまたはサービスの識別子とメール。 |

データセットの有効期限が既に存在する場合、400 （無効なリクエスト） HTTP ステータスが発生します。 404 （見つかりません） HTTP ステータスは、そのようなデータセットが存在しない場合や、データセットへのアクセス権がない場合に発生します。

## データセット有効期限設定の更新 {#update}

既存のデータセット有効期限設定を更新するには、`/ttl/DATASET_EXPIRATION_ID` に対してPUT リクエストを行います。 更新できるのは、設定の `displayName`、`description`、`expiry` フィールドのみです。 更新は、有効期限ステータスが `pending` の場合にのみ許可されます。

>[!NOTE]
>
>`expiry` フィールドには日付（YYYY-MM-DD）または日時（YYYY-MM-DDTHH:MM:SSZ）を指定できます。 日付のみを指定した場合、システムはその日の午前 0 時の UTC （00:00:00Z）を使用します。 有効期限 **少なくとも 24 時間後にする必要があります**。

**API 形式**

```http
PUT /ttl/{DATASET_EXPIRATION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_EXPIRATION_ID}` | データセット有効期限設定の一意の ID。 **メモ**：これは、応答では `ttlId` と呼ばれます。 |

**リクエスト**

次のリクエストは、データセットの有効期限 `SD-c1f902aa-57cb-412e-bb2b-c70b8e1a5f45` の有効期限、表示名、説明を更新します。

```shell
curl -X PUT \
  https://platform.adobe.io/data/core/hygiene/ttl/SD-c1f902aa-57cb-412e-bb2b-c70b8e1a5f45 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "displayName": "Customer Dataset Expiry Rule",
        "description": "Updated description for Acme customer dataset",
        "expiry": "2031-06-15"
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `displayName` | （任意）データセットの有効期限設定の、人間が判読できる新しい名前。 |
| `description` | （オプション）データセットの有効期限設定に関する新しい説明。 |
| `expiry` | （任意） ISO 8601 形式の新しい有効期限または日時。 日付のみを指定した場合、デフォルトでは午前 0 時（UTC）になります。 有効期限は **少なくとも 24 時間後** にする必要があります。 |

>[!NOTE]
>
>これらのフィールドのうち少なくとも 1 つをリクエストで指定する必要があります。

**応答**

応答が成功すると、HTTP ステータス 200 （OK）と、更新されたデータセットの有効期限設定が返されます。

```json
{
  "ttlId": "SD-c1f902aa-57cb-412e-bb2b-c70b8e1a5f45",
  "datasetId": "3e9f815ae1194c65b2a4c5ea",
  "datasetName": "Acme_Customer_Data",
  "sandboxName": "acme-prod",
  "displayName": "Customer Dataset Expiry Rule",
  "description": "Updated description for Acme customer dataset",
  "imsOrg": "C9D8E7F6A5B41234567890AB@AcmeOrg",
  "status": "pending",
  "expiry": "2031-06-15T00:00:00Z",
  "updatedAt": "2031-05-01T14:11:12.000Z",
  "updatedBy": "b.tarth@acme.com <b.tarth@acme.com> 3E9F815AE1194C65B2A4C5EA@acme.com"
}
```

| プロパティ | 説明 |
| --- | --- |
| `ttlId` | 更新されたデータセット有効期限設定の一意の ID。 |
| `datasetId` | データセットの一意の ID。 |
| `datasetName` | データセットの名前。 |
| `sandboxName` | このデータセット有効期限が設定されているサンドボックス。 |
| `displayName` | データセット有効期限設定の表示名。 |
| `description` | データセット有効期限設定の説明。 |
| `imsOrg` | この設定に関連付けられた組織 ID。 |
| `status` | データセット有効期限設定の現在のステータス。<br>1 つ：`pending`、`executing`、`cancelled`、`completed`。 |
| `expiry` | データセットに対してスケジュールされた有効期限タイムスタンプ。 |
| `updatedAt` | 最新の更新のタイムスタンプ。 |
| `updatedBy` | データセット有効期限設定を最後に更新したユーザーまたはサービスの識別子とメール。 |

{style="table-layout:auto"}

データセットの有効期限が存在しない場合、応答は失敗し、404 （見つかりません） HTTP ステータスを返します。

## データセット有効期限のキャンセル {#delete}

`/ttl/{ID}` に対してDELETE リクエストを実行することで、保留中のデータセットの有効期限設定をキャンセルします。

>[!NOTE]
>
>`pending` ステータスのデータセット有効期限のみをキャンセルできます。 既に `executing`、`completed` または `cancelled` である有効期限をキャンセルしようとすると、HTTP 400 （無効なリクエスト）が返されます。

**API 形式**

```http
DELETE /ttl/{ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{ID}` | データセット有効期限設定の一意の ID。 データセット有効期限 ID またはデータセット ID を指定できます。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストでは、ID `SD-d4a7d918-283b-41fd-bfe1-4e730a613d21` を持つデータセットの有効期限がキャンセルされます。

```shell
curl -X DELETE \
  https://platform.adobe.io/data/core/hygiene/ttl/SD-d4a7d918-283b-41fd-bfe1-4e730a613d21 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、HTTP ステータス 200 （OK）と、キャンセルされたデータセットの有効期限設定が返されます。 有効期限の `status` 属性が `cancelled` に設定されているわけではありません。

```json
{
  "ttlId": "SD-d4a7d918-283b-41fd-bfe1-4e730a613d21",
  "datasetId": "5a9e2c68d3b24f03b55a91ce",
  "datasetName": "Acme_Customer_Data",
  "sandboxName": "acme-prod",
  "displayName": "Customer Dataset Expiry Rule",
  "description": "Cancelled expiry configuration for Acme customer dataset",
  "imsOrg": "C9D8E7F6A5B41234567890AB@AcmeOrg",
  "status": "cancelled",
  "expiry": "2032-02-28T00:00:00Z",
  "updatedAt": "2032-01-15T08:27:31.000Z",
  "updatedBy": "s.clegane@acme.com <s.clegane@acme.com> 5A9E2C68D3B24F03B55A91CE@acme.com"
}
```

| プロパティ | 説明 |
|---|---|
| `ttlId` | 削除されたデータセットの有効期限設定の一意の ID。 |
| `datasetId` | データセットの一意の ID。 |
| `datasetName` | データセットの名前。 |
| `sandboxName` | このデータセットの有効期限が設定されているサンドボックス。 |
| `displayName` | データセット有効期限設定の表示名。 |
| `description` | データセット有効期限設定の説明。 |
| `imsOrg` | この設定に関連付けられた一意の組織 ID。 |
| `status` | データセット有効期限設定の現在のステータス。<br>1 つ：`pending`、`executing`、`cancelled`、`completed`。 |
| `expiry` | データセットに対してスケジュールされた有効期限タイムスタンプ。 |
| `updatedAt` | 最新の更新のタイムスタンプ。 |
| `updatedBy` | データセット有効期限設定を最後に更新したユーザーまたはサービスの識別子とメール。 |

**例 400 （無効なリクエスト）応答**

`executing`、`completed` または `cancelled` の有効期限設定を持つデータセットをキャンセルしようとすると、400 エラーが発生します。

```json
{
  "type": "http://ns.adobe.com/aep/errors/HYGN-3102-400",
  "title": "The requested dataset already has an existing expiration. Additional detail: A TTL already exists for datasetId=686e9ca25ef7462aefe72c93",
  "status": 400,
  "report": {
    "tenantInfo": {
      "sandboxName": "prod",
      "sandboxId": "not-applicable",
      "imsOrgId": "{IMS_ORG_ID}"
    },
    "additionalContext": {
      "Invoking Client ID": "acp_privacy_hygiene"
    }
  },
  "error-chain": [
    {
      "serviceId": "HYGN",
      "errorCode": "HYGN-3102-400",
      "invokingServiceId": "acp_privacy_hygiene",
      "unixTimeStampMs": 1754408150394
    }
  ]
}
```

>[!NOTE]
>
>既に `completed` または `cancelled` になっているデータセットの有効期限をキャンセルしようとすると、404 エラーが発生します。

## 付録

### 承認されたクエリパラメーター {#query-params}

次の表では、[データセット有効期限をリスト表示](#list)する際に使用できるクエリパラメーターを説明します。

>[!NOTE]
>
>`description`、`displayName`、`datasetName` の各パラメーターには、LIKE 値で検索する機能が含まれています。 つまり、「Name1」文字列を検索すると、「Name123」、「Name183」、「DisplayName1234」という名前のスケジュールされたデータセット有効期限を見つけることができます。

| パラメーター | 説明 | 例 |
| --- | --- | --- |
| `author` | `author` クエリパラメーターを使用して、データセットの有効期限を最後に更新したユーザーを見つけます。 作成後に更新されていない場合は、有効期限の元の作成者と一致します。 このパラメーターは、`created_by` フィールドが検索文字列に対応する有効期限に一致します。<br> 検索文字列が `LIKE` または `NOT LIKE` で始まる場合、残りは SQL 検索パターンとして扱われます。 それ以外の場合は、検索文字列全体が、`created_by` フィールドのコンテンツ全体に完全に一致する必要があるリテラル文字列として扱われます。 | `author=LIKE %john%`, `author=John Q. Public` |
| `datasetId` | 特定のデータセットに適用する有効期限に一致します。 | `datasetId=62b3925ff20f8e1b990a7434` |
| `datasetName` | 指定された検索文字列をデータセット名に含む有効期限に一致します。 一致する文字列では、大文字と小文字が区別されません。 | `datasetName=Acme` |
| `description` |   | `description=Handle expiration of Acme information through the end of 2024.` |
| `displayName` | 指定された検索文字列を表示名に含む有効期限に一致します。 一致する文字列では、大文字と小文字が区別されません。 | `displayName=License Expiry` |
| `executedDate`／`executedFromDate`／`executedToDate` | 正確な実行日、実行終了日または実行開始日に基づいて結果をフィルタリングします。 これらは、特定の日付、特定の日付の前、特定の日付の後の操作の実行に関連するデータまたはレコードを取得するために使用されます。 | `executedDate=2023-02-05T19:34:40.383615Z` |
| `expiryDate` | 指定された日付の 24 時間枠内に発生した有効期限に一致します。 | `2024-01-01` |
| `expiryToDate` または `expiryFromDate` | 指定された期間内に実行される予定の、または既に実行された有効期限に一致します。 | `expiryFromDate=2099-01-01&expiryToDate=2100-01-01` |
| `limit` | 返される有効期限の最大数を示す 1～100 の整数。デフォルトは 25 です。 | `limit=50` |
| `orderBy` | `orderBy` クエリパラメーターでは、API から返される結果の並べ替え順を指定します。 これを使用すると、1 つ以上のフィールドに基づいて、昇順（ASC）または降順（DESC）のいずれかでデータを並べ替えることができます。 ASC および DESC を示すには、それぞれ+または – の接頭辞を使用します。 指定できる値は、`displayName`、`description`、`datasetName`、`id`、`updatedBy`、`updatedAt`、`expiry`、`status` です。 | `-datasetName` |
| `orgId` | 組織 ID がパラメーターと一致するデータセットの有効期限に一致します。この値はデフォルトで `x-gw-ims-org-id` ヘッダーの値となり、リクエストがサービストークンを提供しない限り無視されます。 | `orgId=885737B25DC460C50A49411B@AdobeOrg` |
| `page` | 返される有効期限のページを示す整数。 | `page=3` |
| `sandboxName` | サンドボックス名が引数と完全に一致するデータセット有効期限に一致します。デフォルトは、リクエストの `x-sandbox-name` ヘッダーにあるサンドボックス名です。`sandboxName=*` を使用して、すべてのサンドボックスからデータセット有効期限を含めます。 | `sandboxName=dev1` |
| `search` | 指定された文字列が有効期限 ID と完全に一致する、または次のいずれかのフィールドに **含まれる** 有効期限に一致します：<br><ul><li>作成者</li><li>表示名</li><li>説明</li><li>表示名</li><li>データセット名</li></ul> | `search=TESTING` |
| `status` | ステータスのコンマ区切りリスト。含める場合、応答は、現在のステータスがリストに含まれるデータセットの有効期限に一致します。 | `status=pending,cancelled` |
| `ttlId` | 指定された ID で有効期限リクエストに一致します。 | `ttlID=SD-c8c75921-2416-4be7-9cfd-9ab01de66c5f` |
| `updatedDate` | 指定された日付の 24 時間以内に更新された有効期限に一致します。 | `2024-01-01` |
| `updatedToDate` または `updatedFromDate` | 指定した時間から 24 時間以内に更新された有効期限に一致します。<br><br>有効期限は、作成時、キャンセル時、実行時を含め、編集されるたびに更新されたと見なされます。 | `updatedDate=2022-01-01` |

{style="table-layout:auto"}

