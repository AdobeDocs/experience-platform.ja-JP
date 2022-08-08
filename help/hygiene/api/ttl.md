---
title: データセット Time-to-Live（TTL）API エンドポイント
description: Data Hygiene API の /ttl エンドポイントを使用すると、Adobe Experience Platform のデータセット TTL をプログラムでスケジュール設定できます。
exl-id: fbabc2df-a79e-488c-b06b-cd72d6b9743b
source-git-commit: 7f1e4bdf54314cab1f69619bcbb34216da94b17e
workflow-type: ht
source-wordcount: '1313'
ht-degree: 100%

---

# データセット Time-to-Live（TTL）エンドポイント

>[!IMPORTANT]
>
>Adobe Experience Platform のデータハイジーン機能は、現在、Healthcare Shield を購入した組織のみが利用できます。

Data Hygiene API の `/ttl` エンドポイントを使用すると、Adobe Experience Platform のデータセットの Time-to-Live（TTL）プロトコルをスケジュール設定できます。

データセット TTL は、単なる時間差の削除操作です。このデータセットは、暫定的に保護されないので、有効期限に達する前に、別の手段で削除される可能性があります。

>[!NOTE]
>
>有効期限は特定の瞬間として指定されますが、有効期限が切れてから実際に削除が開始されるまで、最大 24 時間の遅延が発生する可能性があります。削除が開始されると、すべてのデータセットの痕跡が Platform システムから削除されるまで、最大 7 日間かかる可能性があります。

データセット削除が実際に開始される前であれば、いつでも TTL をキャンセルしたり、そのトリガー時間を変更したりできます。TTL のキャンセル後は、新しい有効期限を設定することで再開できます。

データセット削除が開始されると、その TTL は `executing` とマークされ、それ以上変更できなくなります。データセット自体は、最大 7 日間復元できる可能性がありますが、アドビのサービスリクエストを通じて開始された手動のプロセスによってのみ可能です。

## はじめに

このガイドで使用するエンドポイントは、Data Hygiene API の一部です。先に進む前に、[概要](./overview.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## データセット TTL のリスト表示 {#list}

GET リクエストを行うことで、組織のすべてのデータセット TTL をリスト表示できます。

**API 形式**

```http
GET /ttl?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
| --- | --- |
| `{QUERY_PARAMETERS}` | `&` 文字で区切られた複数のパラメーターを含む、オプションのクエリパラメーターのリスト。共通のパラメーターには、ページネーション用の `size` および `page` があります。サポートされるクエリパラメーターの完全なリストについては、[付録の節](#query-params)を参照してください。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/hygiene/ttl?page=1&size=50 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答では、結果の TTL がリスト表示されます。次の例は、スペースを節約するために省略されています。

```json
{
  "results": [
    {
      "ttlId": "SDfba908e9fb2e427ab4275d20465631d7",
      "datasetId": "62799c3e1151781b63ccaa28",
      "imsOrg": "{ORG_ID}",
      "status": "cancelled",
      "expiry": "2022-05-09T22:57:05.531024Z",
      "updatedAt": "2022-05-09T22:57:05.531025Z",
      "updatedBy": "{USER_ID}"
    },
    {
      "ttlId": "SD5cfd7a11b25543a9bcd9ef647db3d8df",
      "datasetId": "62759f2ede9e601b63a2ee14",
      "imsOrg": "{ORG_ID}",
      "status": "pending",
      "expiry": "2032-12-31T23:59:59Z",
      "updatedAt": "2022-05-09T22:41:46.731002Z",
      "updatedBy": "{USER_ID}"
    }
  ],
  "current_page": 1,
  "total_pages": 36,
  "total_count": 886
}
```

| プロパティ | 説明 |
| --- | --- |
| `results` | 返される TTL の詳細が含まれます。TTL エンティティのプロパティについて詳しくは、[ルックアップ呼び出し](#lookup)を行うための応答の節を参照してください。 |
| `current_page` | リストされた結果の現在のページ。 |
| `total_pages` | 応答内のページの合計数。 |
| `total_count` | 応答内の TTL エンティティの合計数。 |

{style=&quot;table-layout:auto&quot;}

## TTL の参照 {#lookup}

GET リクエストを使用してデータセット TTL を参照できます。

**API 形式**

```http
GET /ttl/{TTL_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{TTL_ID}` | 参照したい TTL の ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/hygiene/ttl/5b020a27e7040801dedbf46e \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答では、TTL の詳細が返されます。

```json
{
    "ttlId": "SD5cfd7a11b25543a9bcd9ef647db3d8df",
    "datasetId": "62759f2ede9e601b63a2ee14",
    "imsOrg": "{ORG_ID}",
    "status": "pending",
    "expiry": "2023-12-31T23:59:59Z",
    "updatedAt": "2022-05-11T15:12:40.393115Z",
    "updatedBy": "{USER_ID}"
}
```

| プロパティ | 説明 |
| --- | --- |
| `ttlId` | データセット TTL の ID。 |
| `datasetId` | この TTL が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `status` | TTL の現在のステータス。 |
| `expiry` | データセットが削除されるようにスケジュール設定された日付および時刻。 |
| `updatedAt` | TTL が最後に更新された際のタイムスタンプ。 |
| `updatedBy` | TTL を最後に更新したユーザー。 |

{style=&quot;table-layout:auto&quot;}

## TTL の作成 {#create}

POST リクエストを使用してデータセットの TTL を追加できます。

**API 形式**

```http
POST /ttl
```

**リクエスト**

次のリクエストは、データセット `5b020a27e7040801dedbf46e` の削除を 2022年末（グリニッジ標準時）にスケジュール設定します。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/hygiene/ttl \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "datasetId": "5b020a27e7040801dedbf46e",
        "expiry": "2022-12-31T23:59:59Z"
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `datasetId` | TTL をスケジュール設定するデータセットの ID。 |
| `expiry` | データセットが削除される日時の ISO 8601 タイムスタンプ。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答では、TTL の詳細が、HTTP ステータス（既存の TTL が更新されていた場合は 200（OK）、既存の TTL がなかった場合は 201（Created））と共に返されます。

```json
{
    "ttlId": "SD5cfd7a11b25543a9bcd9ef647db3d8df",
    "datasetId": "5b020a27e7040801dedbf46e",
    "imsOrg": "{ORG_ID}",
    "status": "pending",
    "expiry": "2032-12-31T23:59:59Z",
    "updatedAt": "2022-05-09T22:38:40.393115Z",
    "updatedBy": "{USER_ID}"
}
```

| プロパティ | 説明 |
| --- | --- |
| `ttlId` | データセット TTL の ID。 |
| `datasetId` | この TTL が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `status` | TTL の現在のステータス。 |
| `expiry` | データセットが削除されるようにスケジュール設定された日付および時刻。 |
| `updatedAt` | TTL が最後に更新された際のタイムスタンプ。 |
| `updatedBy` | TTL を最後に更新したユーザー。 |

{style=&quot;table-layout:auto&quot;}

## TTL の更新 {#update}

PUT リクエストを使用してデータセットの TTL を更新できます。

**API 形式**

```http
PUT /ttl/{TTL_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{TTL_ID}` | 変更したい TTL の ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、データセット `5b020a27e7040801dedbf46e` の TTL を更新し、2023年末（グリニッジ標準時）に期限が切れるようにします。

```shell
curl -X PUT \
  https://platform.adobe.io/data/core/hygiene/ttl/5b020a27e7040801dedbf46e \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "expiry": "2023-12-31T23:59:59Z"
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `expiry` | データセットが削除される日時の ISO 8601 タイムスタンプ。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答では、更新された TTL の詳細が返されます。

```json
{
    "ttlId": "SD5cfd7a11b25543a9bcd9ef647db3d8df",
    "datasetId": "62759f2ede9e601b63a2ee14",
    "imsOrg": "{ORG_ID}",
    "status": "pending",
    "expiry": "2023-12-31T23:59:59Z",
    "updatedAt": "2022-05-11T15:12:40.393115Z",
    "updatedBy": "{USER_ID}"
}
```

| プロパティ | 説明 |
| --- | --- |
| `ttlId` | データセット TTL の ID。 |
| `datasetId` | この TTL が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `status` | TTL の現在のステータス。 |
| `expiry` | データセットが削除されるようにスケジュール設定された日付および時刻。 |
| `updatedAt` | TTL が最後に更新された際のタイムスタンプ。 |
| `updatedBy` | TTL を最後に更新したユーザー。 |

{style=&quot;table-layout:auto&quot;}

## TTL のキャンセル {#delete}

DELETE リクエストを行うことで TTL をキャンセルできます。

**API 形式**

```http
DELETE /ttl/{TTL_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{TTL_ID}` | キャンセルしたい TTL の ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、データセット `5b020a27e7040801dedbf46e` の TTL を更新し、2023年末（グリニッジ標準時）に期限が切れるようにします。

```shell
curl -X DELETE \
  https://platform.adobe.io/data/core/hygiene/ttl/5b020a27e7040801dedbf46e \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答では、`status` 属性が `cancelled` に設定された TTL の詳細が返されます。

```json
{
    "ttlId": "SD5cfd7a11b25543a9bcd9ef647db3d8df",
    "datasetId": "62759f2ede9e601b63a2ee14",
    "imsOrg": "{ORG_ID}",
    "status": "cancelled",
    "expiry": "2023-12-31T23:59:59Z",
    "updatedAt": "2022-05-09T23:47:30.071186Z",
    "updatedBy": "{USER_ID}"
}
```

| プロパティ | 説明 |
| --- | --- |
| `ttlId` | データセット TTL の ID。 |
| `datasetId` | この TTL が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `status` | TTL の現在のステータス。 |
| `expiry` | データセットが削除されるようにスケジュール設定された日付および時刻。 |
| `updatedAt` | TTL が最後に更新された際のタイムスタンプ。 |
| `updatedBy` | TTL を最後に更新したユーザー。 |

{style=&quot;table-layout:auto&quot;}

## TTL の履歴の取得

ルックアップリクエストでクエリパラメーター `include=history` を使用することで、特定の TTL の履歴を参照できます。結果には、TTL の作成、適用された更新およびそのキャンセルまたは実行（該当する場合）に関する情報が含まれます。

**API 形式**

```http
GET /ttl/{TTL_ID}?include=history
```

| パラメーター | 説明 |
| --- | --- |
| `{TTL_ID}` | 履歴を参照したい TTL の ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/hygiene/ttl/5b020a27e7040801dedbf46e?include=history \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答では、TTL の詳細が、記録された各更新の `status`、`expiry`、`updatedAt` および `updatedBy` 属性の詳細を示す `history` 配列と共に返されます。

```json
{
  "ttlId": "SD5cfd7a11b25543a9bcd9ef647db3d8df",
  "datasetId": "62759f2ede9e601b63a2ee14",
  "imsOrg": "{ORG_ID}",
  "status": "cancelled",
  "expiry": "2022-05-09T23:47:30.071186Z",
  "updatedAt": "2022-05-09T23:47:30.071186Z",
  "updatedBy": "{USER_ID}",
  "history": [
    {
      "status": "created",
      "expiry": "2032-12-31T23:59:59Z",
      "updatedAt": "2022-05-09T22:38:40.393115Z",
      "updatedBy": "{USER_ID}"
    },
    {
      "status": "updated",
      "expiry": "2032-12-31T23:59:59Z",
      "updatedAt": "2022-05-09T22:41:46.731002Z",
      "updatedBy": "{USER_ID}"
    },
    {
      "status": "cancelled",
      "expiry": "2022-05-09T23:47:30.071186Z",
      "updatedAt": "2022-05-09T23:47:30.071186Z",
      "updatedBy": "{USER_ID}"
    }
  ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `ttlId` | データセット TTL の ID。 |
| `datasetId` | この TTL が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `history` | TTL の更新の履歴をオブジェクトの配列としてリスト表示し、各オブジェクトには、更新時の TTL の `status`、`expiry`、`updatedAt` および `updatedBy` 属性が含まれます。 |

{style=&quot;table-layout:auto&quot;}

## 付録

### 承認されたクエリパラメーター {#query-params}

次の表では、[データセット TTL をリスト表示](#list)する際に使用できるクエリパラメーターを説明します。

| パラメーター | 説明 | 例 |
| --- | --- | --- |
| `size` | 返される TTL の最大数を示す 1 ～ 100 の整数。デフォルトは 25 です。 | `size=50` |
| `page` | 返される TTL のページを示す整数。 | `page=3` |
| `status` | ステータスのコンマ区切りリスト。含まれる場合、応答は、現在のステータスがリストに含まれる TTL に一致します。 | `status=pending,cancelled` |
| `author` | `created_by` が検索文字列と一致する TTL に一致します。検索文字列が `LIKE` または `NOT LIKE` で始まる場合、残りは SQL 検索パターンとして扱われます。それ以外の場合は、検索文字列全体が、`created_by` フィールドのコンテンツ全体に完全に一致する必要があるリテラル文字列として扱われます。 | `author=LIKE %john%` |
| `createdDate` | 指定した時間から 24 時間以内に作成された TTL に一致します。<br><br>時刻を含まない日付（`2021-12-07` など）は、その日の始まりの日時を表していることに注意してください。したがって、`createdDate=2021-12-07` は、2021年12月7日 `00:00:00` ～ `23:59:59.999999999`（UTC）に作成されたすべての TTL を参照します。 | `createdDate=2021-12-07` |
| `createdFromDate` | 示された時間以降に作成された TTL に一致します。 | `createdFromDate=2021-12-07T00:00:00Z` |
| `createdToDate` | 示された時間以前に作成された TTL に一致します。 | `createdToDate=2021-12-07T23:59:59.999999999Z` |
| `updatedDate`／`updatedToDate`／`updatedFromDate` | `createdDate`／`createdFromDate`／`createdToDate` に似ていますが、作成時間ではなく、TTL の更新時間に対して一致します。<br><br>TTL は、作成時、キャンセル時、実行時を含め、すべての編集で更新されたと見なされます。 | `updatedDate=2022-01-01` |
| `cancelledDate`／`cancelledToDate`／`cancelledFromDate` | 示された期間の任意の時間にキャンセルされた TTL に一致します。これは、TTL が後で（同じデータセットに対して新しい有効期限を設定することで）再開された場合でも適用されます。 | `updatedDate=2022-01-01` |
| `completedDate`／`completedToDate`／`completedFromDate` | 指定された期間内に完了した TTL に一致します。 | `completedToDate=2021-11-11-06:00` |
| `expiryDate`／`expiryToDate`／`expiryFromDate` | 指定された期間内に実行される予定の、または既に実行された TTL に一致します。 | `expiryFromDate=2099-01-01&expiryToDate=2100-01-01` |

{style=&quot;table-layout:auto&quot;}
