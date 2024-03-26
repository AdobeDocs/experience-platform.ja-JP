---
title: Dataset Expiration API エンドポイント
description: Data Hygiene API の /ttl エンドポイントを使用すると、Adobe Experience Platform のデータセット有効期限をプログラムでスケジュール設定できます。
role: Developer
exl-id: fbabc2df-a79e-488c-b06b-cd72d6b9743b
source-git-commit: 04d49282d60b2e886a6d2dae281b98b60e6ce9b3
workflow-type: tm+mt
source-wordcount: '2083'
ht-degree: 63%

---

# データセットの有効期限エンドポイント

Data Hygiene API の `/ttl` エンドポイントを使用すると、Adobe Experience Platform のデータセットの有効期限プロトコルをスケジュール設定できます。

データセットの有効期限は、単なる時間差の削除操作です。このデータセットは、暫定的に保護されないので、有効期限に達する前に、別の手段で削除される可能性があります。

>[!NOTE]
>
>有効期限は特定の瞬間として指定されますが、有効期限が切れてから実際に削除が開始されるまで、最大 24 時間の遅延が発生する可能性があります。削除が開始されると、すべてのデータセットの痕跡が Platform システムから削除されるまで、最大 7 日間かかる可能性があります。

データセット削除が実際に開始される前であれば、いつでも有効期限をキャンセルしたり、そのトリガー時間を変更したりできます。データセット有効期限のキャンセル後は、新しい有効期限を設定することで再開できます。

データセット削除が開始されると、その有効期限ジョブは `executing` とマークされ、それ以上変更できなくなります。データセット自体は、最大 7 日間復元できる可能性がありますが、それにはアドビのサービスリクエストを通じて手動のプロセスを開始する必要があります。リクエストの実行時に、データレイク、ID サービス、リアルタイム顧客プロファイルは別々のプロセスを開始し、各サービスからデータセットの内容を削除します。 3 つのサービスすべてからデータを削除すると、有効期限は `executed` とマークされます。

>[!WARNING]
>
>データセットの有効期限が切れるように設定されている場合、ダウンストリームワークフローに悪影響が及ばないよう、データをそのデータセットに取り込む可能性があるデータフローを手動で変更する必要があります。

## はじめに

このガイドで使用するエンドポイントは、Data Hygiene API の一部です。続行する前に、 [API ガイド](./overview.md) を参照してください。

>[!IMPORTANT]
>
>データ衛生 API を呼び出す場合は、-H を使用する必要があります。 `x-sandbox-name: {SANDBOX_NAME}` ヘッダー。

## データセット有効期限のリスト {#list}

GETリクエストをおこなうことで、組織のすべてのデータセット有効期限をリストできます。 クエリパラメーターを使用して、応答を適切な結果に絞り込むことができます。

**API 形式**

```http
GET /ttl?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
| --- | --- |
| `{QUERY_PARAMETERS}` | `&` 文字で区切られた複数のパラメーターを含む、オプションのクエリパラメーターのリスト。共通のパラメーターには、ページネーション用の `limit` および `page` があります。サポートされるクエリパラメーターの完全なリストについては、[付録の節](#query-params)を参照してください。 |

{style="table-layout:auto"}

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/hygiene/ttl?updatedToDate=2021-08-01&author=LIKE%Jane Doe%25 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、結果のデータセット有効期限がリスト表示されます。次の例は、スペースを節約するために省略されています。

```json
{
  "results": [
    {
      "ttlId": "SD-b16c8b48-a15a-45c8-9215-587ea89369bf",
      "datasetId": "629bd9125b31471b2da7645c",
      "datasetName": "Sample Acme dataset",
      "sandboxName": "hygiene-beta",
      "imsOrg": "A2A5*EF06164773A8A49418C@AdobeOrg",
      "status": "pending",
      "expiry": "2050-01-01T00:00:00Z",
      "updatedAt": "2023-06-09T16:52:44.136028Z",
      "updatedBy": "Jane Doe <jdoe@adobe.com> 77A51F696282E48C0A494 012@64d18d6361fae88d49412d.e"
    }
  ],
  "current_page": 0,
  "total_pages": 1,
  "total_count": 1
}
```

| プロパティ | 説明 |
| --- | --- |
| `totalRecords` | リスト呼び出しのパラメーターに一致したデータセット有効期限の数。 |
| `ttlDetails` | 返されるデータセット有効期限の詳細が含まれます。データセット有効期限のプロパティについて詳しくは、[ルックアップ呼び出し](#lookup)を行うための応答の節を参照してください。 |

{style="table-layout:auto"}

## データセット有効期限の検索 {#lookup}

データセットの有効期限を参照するには、 `datasetId` または `ttlId`.

**API 形式**

```http
GET /ttl/{DATASET_ID}?include=history
GET /ttl/{TTL_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | 有効期限を検索したいデータセットの ID。 |
| `{TTL_ID}` | データセット有効期限の ID。 |

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
    "imsOrg": "A2A5*EF06164773A8A49418C@AdobeOrg",
    "status": "pending",
    "expiry": "2024-12-31T23:59:59Z",
    "updatedAt": "2024-05-11T15:12:40.393115Z",
    "updatedBy": "Jane Doe <jdoe@adobe.com> 77A51F696282E48C0A494 012@64d18d6361fae88d49412d.e",
    "displayName": "Delete Acme Data before 2025",
    "description": "The Acme information in this dataset is licensed for our use through the end of 2024."
}
```

| プロパティ | 説明 |
| --- | --- |
| `ttlId` | データセット有効期限の ID。 |
| `datasetId` | この有効期限が適用されるデータセットの ID。 |
| `datasetName` | この有効期限が適用されるデータセットの表示名。 |
| `sandboxName` | ターゲットデータセットが配置されているサンドボックスの名前。 |
| `imsOrg` | 組織の ID。 |
| `status` | データセット有効期限の現在のステータス。 |
| `expiry` | データセットが削除されるようにスケジュール設定された日付および時刻。 |
| `updatedAt` | 有効期限が最後に更新された際のタイムスタンプ。 |
| `updatedBy` | 有効期限を最後に更新したユーザー。 |
| `displayName` | 有効期限リクエストの表示名。 |
| `description` | 有効期限リクエストの説明。 |

{style="table-layout:auto"}

### カタログの有効期限タグ

[Catalog API](../../catalog/api/getting-started.md) を使用してデータセットの詳細を検索するには、データセットに有効期限がある場合は、そのデータセットが `tags.adobe/hygiene/ttl` の下に表示されます。

次の JSON は、カタログのデータセットの詳細に対する切り捨てられた応答を表し、有効期限は `32503680000000` となっています。タグの値は、有効期限を Unix エポック開始からミリ秒の整数値でエンコードします。

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

指定した期間が経過してもデータがシステムから削除されるようにするには、データセット ID と有効期限の日時を ISO 8601 形式で指定して、特定のデータセットの有効期限をスケジュールします。

データセットの有効期限を作成するには、次に示すようにPOSTリクエストを実行し、ペイロード内で以下に示す値を指定します。

**API 形式**

```http
POST /ttl
```

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/core/hygiene/ttl \
  -H `Authorization: Bearer {ACCESS_TOKEN}`
  -H `x-gw-ims-org-id: {ORG_ID}`
  -H `x-api-key: {API_KEY}`
  -H `Accept: application/json`
  -d {
      "datasetId": "5b020a27e7040801dedbf46e",
      "expiry": "2030-12-31T23:59:59Z"
      "displayName": "Delete Acme Data before 2025",
      "description": "The Acme information in this dataset is licensed for our use through the end of 2024."
      }
```

| プロパティ | 説明 |
| --- | --- |
| `datasetId` | **必須** 有効期限をスケジュールするターゲットデータセットの ID。 |
| `expiry` | **必須** ISO 8601 形式の日時。 文字列に明示的なタイムゾーンオフセットがない場合、タイムゾーンは UTC と見なされます。 システム内のデータの有効期限は、提供された有効期限値に従って設定されます。<br>注意：<ul><li>データセットのデータセット有効期限が既に存在する場合、リクエストは失敗します。</li><li>この日時は少なくとも設定する必要があります **24 時間後**.</li></ul> |
| `displayName` | データセット有効期限リクエストの表示名（オプション）。 |
| `description` | 有効期限リクエストのオプション説明。 |

**応答**

リクエストが成功した場合は、HTTP 201（作成済み）ステータスと、データセットの有効期限の新しい状態（既存のデータセットの有効期限がない場合）が返されます。

```json
{
  "ttlId":       "SD-c8c75921-2416-4be7-9cfd-9ab01de66c5f",
  "datasetId":   "5b020a27e7040801dedbf46e",
  "datasetName": "Acme licensed data",
  "sandboxName": "prod",
  "imsOrg":      "{ORG_ID}",
  "status":      "pending",
  "expiry":      "2030-12-31T23:59:59Z",
  "updatedAt":   "2021-08-19T11:14:16Z",
  "updatedBy":   "Jane Doe <jdoe@adobe.com> 77A51F696282E48C0A494 012@64d18d6361fae88d49412d.e",
  "displayName": "Delete Acme Data before 2031",
  "description": "The Acme information in this dataset is licensed for our use through the end of 2030."
}
```

| プロパティ | 説明 |
| --- | --- |
| `ttlId` | データセット有効期限の ID。 |
| `datasetId` | この有効期限が適用されるデータセットの ID。 |
| `datasetName` | この有効期限が適用されるデータセットの表示名。 |
| `sandboxName` | ターゲットデータセットが配置されているサンドボックスの名前。 |
| `imsOrg` | 組織の ID。 |
| `status` | データセット有効期限の現在のステータス。 |
| `expiry` | データセットが削除されるようにスケジュール設定された日付および時刻。 |
| `updatedAt` | 有効期限が最後に更新された際のタイムスタンプ。 |
| `updatedBy` | 有効期限を最後に更新したユーザー。 |
| `displayName` | 有効期限のリクエストの表示名。 |
| `description` | 有効期限リクエストの説明。 |

データセットのデータセット有効期限が既に存在する場合、400(Bad Request)HTTP ステータスが発生します。 失敗した応答は、そのようなデータセットの有効期限が存在しない（またはアクセス権を持っていない）場合、404（未検出）HTTP ステータスを返します。

## データセット有効期限の更新 {#update}

データセットの有効期限を更新するには、PUTリクエストと `ttlId`. 次の項目を更新して、 `displayName`, `description`，および/または `expiry` 情報。

>[!NOTE]
>
>有効期限の日時を変更する場合は、24 時間以上後にする必要があります。 この強制的な遅延により、期限切れのキャンセルや再スケジュールをおこない、誤ってデータが失われるのを防ぐことができます。

**API 形式**

```http
PUT /ttl/{TTL_ID}
```

<!-- We should be avoiding usage of TTL, Can I change that to {EXPIRY_ID} or {EXPIRATION_ID} instead? -->

| パラメーター | 説明 |
| --- | --- |
| `{TTL_ID}` | 変更するデータセット有効期限の ID。 |

**リクエスト**

次のリクエストは、データセットの有効期限を再スケジュールします。 `SD-c8c75921-2416-4be7-9cfd-9ab01de66c5f` 2024 年末（グリニッジ標準時）に発生する 既存のデータセットの有効期限が見つかった場合、その有効期限は新しい `expiry` の値です。

```shell
curl -X PUT \
  https://platform.adobe.io/data/core/hygiene/ttl/SD-c8c75921-2416-4be7-9cfd-9ab01de66c5f \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "expiry": "2024-12-31T23:59:59Z",
        "displayName": "Delete Acme Data before 2025",
        "description": "The Acme information in this dataset is licensed for our use through the end of 2024."
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `expiry` | **必須** ISO 8601 形式の日時。 文字列に明示的なタイムゾーンオフセットがない場合、タイムゾーンは UTC と見なされます。 システム内のデータの有効期限は、提供された有効期限値に従って設定されます。 同じデータセットの以前の有効期限のタイムスタンプは、指定した新しい有効期限値に置き換えられます。 この日時は少なくとも設定する必要があります **24 時間後**. |
| `displayName` | 有効期限のリクエストの表示名。 |
| `description` | 有効期限リクエストのオプション説明。 |

{style="table-layout:auto"}

**応答**

正常な応答は、データセットの有効期限の新しい状態と、既存の有効期限が更新された場合は HTTP ステータス 200(OK) を返します。

```json
{
    "ttlId": "SD-c8c75921-2416-4be7-9cfd-9ab01de66c5f",
    "datasetId": "5b020a27e7040801dedbf46e",
    "imsOrg": "A2A5*EF06164773A8A49418C@AdobeOrg",
    "status": "pending",
    "expiry": "2024-12-31T23:59:59Z",
    "updatedAt": "2022-05-09T22:38:40.393115Z",
    "updatedBy": "Jane Doe <jdoe@adobe.com> 77A51F696282E48C0A494 012@64d18d6361fae88d49412d.e",
    "displayName": "Delete Acme Data before 2025",
    "description": "The Acme information in this dataset is licensed for our use through the end of 2024."
}
```

| プロパティ | 説明 |
| --- | --- |
| `ttlId` | データセット有効期限の ID。 |
| `datasetId` | この有効期限が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `status` | データセット有効期限の現在のステータス。 |
| `expiry` | データセットが削除されるようにスケジュール設定された日付および時刻。 |
| `updatedAt` | 有効期限が最後に更新された際のタイムスタンプ。 |
| `updatedBy` | 有効期限を最後に更新したユーザー。 |

{style="table-layout:auto"}

データセットの有効期限が存在しない場合、失敗した応答は 404（未検出）HTTP ステータスを返します。

## データセット有効期限のキャンセル {#delete}

DELETE リクエストを行うことでデータセット有効期限をキャンセルできます。

>[!NOTE]
>
>ステータスが `pending` のデータセット有効期限のみをキャンセルできます。実行された有効期限または既にキャンセルされた有効期限をキャンセルしようとすると、HTTP 404 エラーが返されます。

**API 形式**

```http
DELETE /ttl/{EXPIRATION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{EXPIRATION_ID}` | キャンセルするデータセット有効期限の `ttlId` |

{style="table-layout:auto"}

**リクエスト**

次のリクエストでは、ID `SD-b16c8b48-a15a-45c8-9215-587ea89369bf` を持つデータセットの有効期限がキャンセルされます。

```shell
curl -X DELETE \
  https://platform.adobe.io/data/core/hygiene/ttl/SD-b16c8b48-a15a-45c8-9215-587ea89369bf \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、HTTP ステータス 204（コンテンツなし）が返され、有効期限の `status` 属性が `cancelled` に設定されます。

## データセットの有効期限ステータス履歴の取得 {#retrieve-expiration-history}

ルックアップリクエストでクエリパラメーター `include=history` を使用することで、特定のデータセットの有効期限ステータス履歴を検索できます。結果には、データセットの有効期限の作成、適用された更新、キャンセルまたは実行（該当する場合）に関する情報が含まれます。 また、 `ttlId` 」と入力します。

**API 形式**

```http
GET /ttl/{DATASET_ID}?include=history
GET /ttl/{TTL_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | 有効期限履歴を検索したいデータセットの ID。 |
| `{TTL_ID}` | データセット有効期限の ID。 |

{style="table-layout:auto"}

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/hygiene/ttl/62759f2ede9e601b63a2ee14?include=history \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答では、データセットの有効期限の詳細が、記録された各更新の `status`、`expiry`、`updatedAt` および `updatedBy` 属性の詳細を示す `history` 配列と共に返されます。

```json
{
  "ttlId": "SD-b16c8b48-a15a-45c8-9215-587ea89369bf",
  "datasetId": "62759f2ede9e601b63a2ee14",
  "datasetName": "Example Dataset",
  "sandboxName": "prod",
  "displayName": "Expiration Request 123",
  "description": "Expiration Request 123 Description",
  "imsOrg": "0FCC747E56F59C747F000101@AdobeOrg",
  "status": "cancelled",
  "expiry": "2022-05-09T23:47:30.071186Z",
  "updatedAt": "2022-05-09T23:47:30.071186Z",
  "updatedBy": "Jane Doe <jdoe@adobe.com> 77A51F696282E48C0A494 012@64d18d6361fae88d49412d.e",
  "history": [
    {
      "status": "created",
      "expiry": "2032-12-31T23:59:59Z",
      "updatedAt": "2022-05-09T22:38:40.393115Z",
      "updatedBy": "Jane Doe <jdoe@adobe.com> 77A51F696282E48C0A494 012@64d18d6361fae88d49412d.e"
    },
    {
      "status": "updated",
      "expiry": "2032-12-31T23:59:59Z",
      "updatedAt": "2022-05-09T22:41:46.731002Z",
      "updatedBy": "Jane Doe <jdoe@adobe.com> 77A51F696282E48C0A494 012@64d18d6361fae88d49412d.e"
    },
    {
      "status": "cancelled",
      "expiry": "2022-05-09T23:47:30.071186Z",
      "updatedAt": "2022-05-09T23:47:30.071186Z",
      "updatedBy": "Jane Doe <jdoe@adobe.com> 77A51F696282E48C0A494 012@64d18d6361fae88d49412d.e"
    }
  ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `ttlId` | データセット有効期限の ID。 |
| `datasetId` | この有効期限が適用されるデータセットの ID。 |
| `datasetName` | この有効期限が適用されるデータセットの表示名。 |
| `sandboxName` | ターゲットデータセットが配置されているサンドボックスの名前。 |
| `displayName` | 有効期限リクエストの表示名。 |
| `description` | 有効期限リクエストの説明。 |
| `imsOrg` | 組織の ID。 |
| `history` | 有効期限の更新の履歴をオブジェクトの配列としてリスト表示し、各オブジェクトには、更新時の有効期限の `status`、`expiry`、`updatedAt` および `updatedBy` 属性が含まれます。 |

{style="table-layout:auto"}

## 付録

### 承認されたクエリパラメーター {#query-params}

次の表では、[データセット有効期限をリスト表示](#list)する際に使用できるクエリパラメーターを説明します。

>[!NOTE]
>
>The `description`, `displayName`、および `datasetName` パラメーターはすべて、LIKE 値で検索する機能を含んでいます。 つまり、「Name123」、「Name183」、「DisplayName1234」という名前のスケジュール済みデータセット有効期限を検索するには、文字列「Name1」を検索します。

| パラメーター | 説明 | 例 |
| --- | --- | --- |
| `author` | `created_by` が検索文字列と一致する有効期限に一致します。検索文字列が `LIKE` または `NOT LIKE` で始まる場合、残りは SQL 検索パターンとして扱われます。それ以外の場合は、検索文字列全体が、`created_by` フィールドのコンテンツ全体に完全に一致する必要があるリテラル文字列として扱われます。 | `author=LIKE %john%`、`author=John Q. Public` |
| `cancelledDate`／`cancelledToDate`／`cancelledFromDate` | 示された期間の任意の時間にキャンセルされた有効期限に一致します。これは、有効期限が後で（同じデータセットに対して新しい有効期限を設定することで）再開された場合でも適用されます。 | `updatedDate=2022-01-01` |
| `completedDate`／`completedToDate`／`completedFromDate` | 指定された期間内に完了した有効期限に一致します。 | `completedToDate=2021-11-11-06:00` |
| `createdDate` | 指定した時間から 24 時間以内に作成された有効期限に一致します。<br><br>時刻を含まない日付（`2021-12-07` など）は、その日の始まりの日時を表していることに注意してください。したがって、`createdDate=2021-12-07` は、2021年12月7日の `00:00:00`～`23:59:59.999999999`（UTC）に作成されたすべての有効期限を参照します。 | `createdDate=2021-12-07` |
| `createdFromDate` | 示された時間以降に作成された有効期限に一致します。 | `createdFromDate=2021-12-07T00:00:00Z` |
| `createdToDate` | 示された時間以前に作成された有効期限に一致します。 | `createdToDate=2021-12-07T23:59:59.999999999Z` |
| `datasetId` | 特定のデータセットに適用する有効期限に一致します。 | `datasetId=62b3925ff20f8e1b990a7434` |
| `datasetName` | 指定した検索文字列がデータセット名に含まれる有効期限に一致します。 一致では、大文字と小文字が区別されません。 | `datasetName=Acme` |
| `description` |   | `description=Handle expiration of Acme information through the end of 2024.` |
| `displayName` | 指定した検索文字列が表示名に含まれる有効期限に一致します。 一致では、大文字と小文字が区別されません。 | `displayName=License Expiry` |
| `executedDate`／`executedFromDate`／`executedToDate` | フィルターは、正確な実行日、実行終了日、または実行の開始日に基づいて結果を表示します。 特定の日付、特定の日付の前、または特定の日付の後に、操作の実行に関連付けられたデータまたはレコードを取得するために使用されます。 | `executedDate=2023-02-05T19:34:40.383615Z` |
| `expiryDate`／`expiryToDate`／`expiryFromDate` | 指定された期間内に実行される予定の、または既に実行された有効期限に一致します。 | `expiryFromDate=2099-01-01&expiryToDate=2100-01-01` |
| `limit` | 返される有効期限の最大数を示す 1～100 の整数。デフォルトは 25 です。 | `limit=50` |
| `orderBy` | The `orderBy` クエリパラメーターは、API から返される結果の並べ替え順を指定します。 昇順 (ASC) または降順 (DESC) の 1 つ以上のフィールドに基づいてデータを並べ替える場合に使用します。 ASC と DESC をそれぞれ示すには、 +または — プレフィックスを使用します。 次の値を使用できます。 `displayName`, `description`, `datasetName`, `id`, `updatedBy`, `updatedAt`, `expiry`, `status`. | `-datasetName` |
| `orgId` | 組織 ID がパラメーターと一致するデータセットの有効期限に一致します。この値はデフォルトで `x-gw-ims-org-id` ヘッダーの値となり、リクエストがサービストークンを提供しない限り無視されます。 | `orgId=885737B25DC460C50A49411B@AdobeOrg` |
| `page` | 返される有効期限のページを示す整数。 | `page=3` |
| `sandboxName` | サンドボックス名が引数と完全に一致するデータセット有効期限に一致します。デフォルトは、リクエストの `x-sandbox-name` ヘッダーにあるサンドボックス名です。`sandboxName=*` を使用して、すべてのサンドボックスからデータセット有効期限を含めます。 | `sandboxName=dev1` |
| `search` | 指定した文字列が有効期限 ID と完全に一致する、またはがの有効期限に一致します **含む** 次のいずれかのフィールドに入力します。<br><ul><li>author</li><li>表示名</li><li>説明</li><li>表示名</li><li>データセット名</li></ul> | `search=TESTING` |
| `status` | ステータスのコンマ区切りリスト。含める場合、応答は、現在のステータスがリストに含まれるデータセットの有効期限に一致します。 | `status=pending,cancelled` |
| `ttlId` | 指定された ID を持つ有効期限要求に一致します。 | `ttlID=SD-c8c75921-2416-4be7-9cfd-9ab01de66c5f` |
| `updatedDate`／`updatedToDate`／`updatedFromDate` | `createdDate`／`createdFromDate`／`createdToDate` に似ていますが、作成時間ではなく、データセットの有効期限の更新時間に対して一致します。<br><br>有効期限は、作成時、キャンセル時、実行時を含め、編集されるたびに更新されたと見なされます。 | `updatedDate=2022-01-01` |

{style="table-layout:auto"}

