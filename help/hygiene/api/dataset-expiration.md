---
title: Dataset Expiration API エンドポイント
description: Data Hygiene API の /ttl エンドポイントを使用すると、Adobe Experience Platform のデータセット有効期限をプログラムでスケジュール設定できます。
role: Developer
exl-id: fbabc2df-a79e-488c-b06b-cd72d6b9743b
source-git-commit: 911089ec641d9fbb436807b04dd38e00fd47eecf
workflow-type: tm+mt
source-wordcount: '1964'
ht-degree: 51%

---

# データセットの有効期限エンドポイント

Data Hygiene API の `/ttl` エンドポイントを使用すると、Adobe Experience Platform のデータセットの有効期限プロトコルをスケジュール設定できます。

データセットの有効期限は、単なる時間差の削除操作です。このデータセットは、暫定的に保護されないので、有効期限に達する前に、別の手段で削除される可能性があります。

>[!NOTE]
>
>有効期限は特定の瞬間として指定されますが、有効期限が切れてから実際に削除が開始されるまで、最大 24 時間の遅延が発生する可能性があります。削除が開始されると、すべてのデータセットの痕跡が Platform システムから削除されるまで、最大 7 日間かかる可能性があります。

データセット削除が実際に開始される前であれば、いつでも有効期限をキャンセルしたり、そのトリガー時間を変更したりできます。データセット有効期限のキャンセル後は、新しい有効期限を設定することで再開できます。

データセット削除が開始されると、その有効期限ジョブは `executing` とマークされ、それ以上変更できなくなります。データセット自体は、最大 7 日間復元できる可能性がありますが、それにはアドビのサービスリクエストを通じて手動のプロセスを開始する必要があります。リクエストの実行時に、データレイク、ID サービス、リアルタイム顧客プロファイルは、別々のプロセスを開始し、各サービスからデータセットの内容を削除します。 3 つのサービスすべてからデータを削除すると、有効期限は `completed` とマークされます。

>[!WARNING]
>
>データセットの有効期限が切れるように設定されている場合、ダウンストリームワークフローに悪影響が及ばないよう、データをそのデータセットに取り込む可能性があるデータフローを手動で変更する必要があります。

Advanced Data Lifecycle Management では、データセット有効期限エンドポイントを介したデータセット削除と、[workorder エンドポイント ](./workorder.md) を介したプライマリ ID を使用した ID 削除（行レベルのデータ）をサポートしています。 また、Platform UI を使用して [ データセットの有効期限 ](../ui/dataset-expiration.md) および [ レコードの削除 ](../ui/record-delete.md) を管理することもできます。 詳しくは、リンクされたドキュメントを参照してください。

>[!NOTE]
>
>データ ライフサイクルはバッチ削除をサポートしていません。

## はじめに

このガイドで使用するエンドポイントは、Data Hygiene API の一部です。続行する前に、[API ガイド ](./overview.md) を参照し、CRUD 操作に必要なヘッダー、エラーメッセージ、Postman コレクション、サンプル API 呼び出しの読み取り方法を確認してください。

>[!IMPORTANT]
>
>Data Hygiene API を呼び出す場合は、-H `x-sandbox-name: {SANDBOX_NAME}` ヘッダーを使用する必要があります。

## データセット有効期限のリスト {#list}

データセットリクエストを行うことで、組織のすべてのGETセット有効期限をリスト表示できます。 クエリパラメーターを使用すると、適切な結果に対する応答をフィルタリングできます。

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
| `total_count` | リスト呼び出しのパラメーターに一致したデータセット有効期限の数。 |
| `results` | 返されるデータセット有効期限の詳細が含まれます。データセット有効期限のプロパティについて詳しくは、[ルックアップ呼び出し](#lookup)を行うための応答の節を参照してください。 |

{style="table-layout:auto"}

## データセット有効期限の検索 {#lookup}

データセットの有効期限を参照するには、`{DATASET_ID}` または `{DATASET_EXPIRATION_ID}` を使用してGETリクエストを行います。

>[!IMPORTANT]
>
>`{DATASET_EXPIRATION_ID}` は、応答では `ttlId` と呼ばれます。 どちらも、データセットの有効期限の一意の ID を参照します。

**API 形式**

```http
GET /ttl/{DATASET_ID}?include=history
GET /ttl/{DATASET_EXPIRATION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | 有効期限を検索したいデータセットの ID。 |
| `{DATASET_EXPIRATION_ID}` | データセット有効期限の ID。 |

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

指定した期間の後にデータがシステムから削除されるようにするには、データセット ID と有効期限の日時を ISO 8601 形式で指定して、特定のデータセットの有効期限をスケジュールします。

データセットの有効期限を作成するには、次に示すようにPOSTリクエストを実行し、ペイロード内で以下に示す値を指定します。

>[!NOTE]
>
>404 エラーが発生した場合は、リクエストに追加のフォワードスラッシュがないことを確認します。 末尾のスラッシュは、POSTリクエストの失敗の原因になる可能性があります。

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
| `datasetId` | **必須** 有効期限をスケジュール設定するターゲットデータセットの ID。 |
| `expiry` | **必須** ISO 8601 形式の日時。 文字列に明示的なタイムゾーンオフセットがない場合、タイムゾーンは UTC と見なされます。 システム内のデータの存続期間は、指定された有効期限の値に応じて設定される。<br> メモ：<ul><li>データセットの有効期限が既に存在する場合、リクエストは失敗します。</li><li>この日付と時刻は、少なくとも **24 時間後である必要があり** す。</li></ul> |
| `displayName` | データセット有効期限リクエストのオプション表示名。 |
| `description` | 有効期限リクエストのオプション説明。 |

**応答**

応答が成功すると、HTTP 201 （作成済み）ステータスと、データセットの有効期限の新しい状態が返されます。

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

データセットの有効期限が既に存在する場合、400 （無効なリクエスト） HTTP ステータスが発生します。 そのようなデータセットの有効期限が存在しない（またはデータセットへのアクセス権がない）場合、応答は失敗し、404 （見つかりません） HTTP ステータスを返します。

## データセット有効期限の更新 {#update}

データセットの有効期限を更新するには、PUTリクエストと `ttlId` を使用します。 `displayName`、`description`、`expiry` の情報を更新できます。

>[!NOTE]
>
>有効期限の日時を変更する場合は、24 時間以上後にする必要があります。 この強制的な遅延により、有効期限をキャンセルまたは再スケジュールして、誤ってデータを失わないようにすることができます。

**API 形式**

```http
PUT /ttl/{DATASET_EXPIRATION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_EXPIRATION_ID}` | 変更するデータセット有効期限の ID。 メモ：これは、応答内の `ttlId` と呼ばれます。 |

**リクエスト**

次のリクエストでは、データセットの有効期限 `SD-c8c75921-2416-4be7-9cfd-9ab01de66c5f` が 2024 年末（グリニッジ標準時）に発生するように再スケジュールされます。 既存のデータセット有効期限が見つかった場合、その有効期限は新しい `expiry` 値で更新されます。

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
| `expiry` | **必須** ISO 8601 形式の日時。 文字列に明示的なタイムゾーンオフセットがない場合、タイムゾーンは UTC と見なされます。 システム内のデータの存続期間は、指定された有効期限の値に応じて設定される。 同じデータセットの以前の有効期限タイムスタンプは、指定した新しい有効期限値に置き換えられます。 この日付と時刻は、少なくとも **24 時間後である必要があり** す。 |
| `displayName` | 有効期限のリクエストの表示名。 |
| `description` | 有効期限リクエストのオプション説明。 |

{style="table-layout:auto"}

**応答**

応答が成功すると、データセットの有効期限の新しい状態と、既存の有効期限が更新された場合は HTTP ステータス 200 （OK）が返されます。

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

データセットの有効期限が存在しない場合、応答は失敗し、404 （見つかりません） HTTP ステータスを返します。

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

## 付録

### 承認されたクエリパラメーター {#query-params}

次の表では、[データセット有効期限をリスト表示](#list)する際に使用できるクエリパラメーターを説明します。

>[!NOTE]
>
>`description`、`displayName`、`datasetName` の各パラメーターには、LIKE 値で検索する機能が含まれています。 つまり、「Name1」文字列を検索すると、「Name123」、「Name183」、「DisplayName1234」という名前のスケジュールされたデータセット有効期限を見つけることができます。

| パラメーター | 説明 | 例 |
| --- | --- | --- |
| `author` | `author` クエリパラメーターを使用して、データセットの有効期限を最後に更新したユーザーを見つけます。 作成後に更新されていない場合は、有効期限の元の作成者と一致します。 このパラメーターは、`created_by` フィールドが検索文字列に対応する有効期限に一致します。<br> 検索文字列が `LIKE` または `NOT LIKE` で始まる場合、残りは SQL 検索パターンとして扱われます。 それ以外の場合は、検索文字列全体が、`created_by` フィールドのコンテンツ全体に完全に一致する必要があるリテラル文字列として扱われます。 | `author=LIKE %john%`、`author=John Q. Public` |
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

