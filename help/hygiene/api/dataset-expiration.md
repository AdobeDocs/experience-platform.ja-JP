---
title: Dataset Expirations API のエンドポイント
description: Data Hygiene API の /ttl エンドポイントを使用すると、Adobe Experience Platform のデータセット有効期限をプログラムでスケジュール設定できます。
exl-id: fbabc2df-a79e-488c-b06b-cd72d6b9743b
source-git-commit: 5a12c75a54f420b2ca831dbfe05105dfd856dc4d
workflow-type: tm+mt
source-wordcount: '1405'
ht-degree: 97%

---

# データセット有効期限のエンドポイント

>[!IMPORTANT]
>
>Adobe Experience Platform のデータハイジーン機能は、現在、Healthcare Shield を購入した組織のみが利用できます。

Data Hygiene API の `/ttl` エンドポイントを使用すると、Adobe Experience Platform のデータセットの有効期限プロトコルをスケジュール設定できます。

データセットの有効期限は、単なる時間差の削除操作です。このデータセットは、暫定的に保護されないので、有効期限に達する前に、別の手段で削除される可能性があります。

>[!NOTE]
>
>有効期限は特定の瞬間として指定されますが、有効期限が切れてから実際に削除が開始されるまで、最大 24 時間の遅延が発生する可能性があります。削除が開始されると、すべてのデータセットの痕跡が Platform システムから削除されるまで、最大 7 日間かかる可能性があります。

データセット削除が実際に開始される前であれば、いつでも有効期限をキャンセルしたり、そのトリガー時間を変更したりできます。データセット有効期限のキャンセル後は、新しい有効期限を設定することで再開できます。

データセット削除が開始されると、その有効期限ジョブは `executing` とマークされ、それ以上変更できなくなります。データセット自体は、最大 7 日間復元できる可能性がありますが、それにはアドビのサービスリクエストを通じて手動のプロセスを開始する必要があります。リクエストの実行時に、データレイク、ID サービス、リアルタイム顧客プロファイルは、別々のプロセスを開始し、各サービスからデータセットの内容を削除します。 3 つのサービスすべてからデータを削除すると、有効期限は `executed` とマークされます。

>[!WARNING]
>
>データセットの有効期限が切れるように設定されている場合、ダウンストリームワークフローに悪影響が及ばないように、データをそのデータセットに取り込む可能性があるデータフローを手動で変更する必要があります。

## はじめに

このガイドで使用するエンドポイントは、Data Hygiene API の一部です。先に進む前に、[概要](./overview.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## データセット有効期限のリスト {#list}

GET リクエストを行うことで、組織のすべてのデータセット有効期限をリスト表示できます。

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

応答が成功すると、結果のデータセット有効期限がリスト表示されます。次の例は、スペースを節約するために省略されています。

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
| `results` | 返されるデータセット有効期限の詳細が含まれます。データセット有効期限のプロパティについて詳しくは、[ルックアップ呼び出し](#lookup)を行うための応答の節を参照してください。 |
| `current_page` | リストされた結果の現在のページ。 |
| `total_pages` | 応答内のページの合計数。 |
| `total_count` | 応答内にあるデータセット有効期限の合計数。 |

{style=&quot;table-layout:auto&quot;}

## データセット有効期限の検索 {#lookup}

GET リクエストを通じて、データセットの有効期限を参照できます。

**API 形式**

```http
GET /ttl/{TTL_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{TTL_ID}` | 検索するデータセット有効期限の ID。 |

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

応答が成功すると、データセット有効期限の詳細が返されます。

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
| `ttlId` | データセット有効期限の ID。 |
| `datasetId` | この有効期限が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `status` | データセット有効期限の現在のステータス。 |
| `expiry` | データセットが削除されるようにスケジュール設定された日付および時刻。 |
| `updatedAt` | 有効期限が最後に更新された際のタイムスタンプ。 |
| `updatedBy` | 有効期限を最後に更新したユーザー。 |

{style=&quot;table-layout:auto&quot;}

## データセット有効期限の作成 {#create}

POST リクエストを通じて、データセットの有効期限を作成できます。

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
| `datasetId` | 有効期限をスケジュール設定するデータセットの ID。 |
| `expiry` | データセットが削除される日時の ISO 8601 タイムスタンプ。 |

{style=&quot;table-layout:auto&quot;}

**応答**

応答が成功すると、データセット有効期限の詳細が、HTTP ステータス（既存の有効期限が更新されていた場合は 200（OK）、既存の有効期限がなかった場合は 201（作成済み））と共に返されます。

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
| `ttlId` | データセット有効期限の ID。 |
| `datasetId` | この有効期限が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `status` | データセット有効期限の現在のステータス。 |
| `expiry` | データセットが削除されるようにスケジュール設定された日付および時刻。 |
| `updatedAt` | 有効期限が最後に更新された際のタイムスタンプ。 |
| `updatedBy` | 有効期限を最後に更新したユーザー。 |

{style=&quot;table-layout:auto&quot;}

## データセット有効期限の更新 {#update}

PUT リクエストを使用して、データセットの有効期限を更新できます。

**API 形式**

```http
PUT /ttl/{TTL_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{TTL_ID}` | 変更するデータセット有効期限の ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、データセット `5b020a27e7040801dedbf46e` の有効期限を更新し、2023年末（グリニッジ標準時）に期限が切れるようにします。

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

応答が成功すると、更新された有効期限の詳細が返されます。

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
| `ttlId` | データセット有効期限の ID。 |
| `datasetId` | この有効期限が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `status` | データセット有効期限の現在のステータス。 |
| `expiry` | データセットが削除されるようにスケジュール設定された日付および時刻。 |
| `updatedAt` | 有効期限が最後に更新された際のタイムスタンプ。 |
| `updatedBy` | 有効期限を最後に更新したユーザー。 |

{style=&quot;table-layout:auto&quot;}

## データセット有効期限のキャンセル {#delete}

DELETE リクエストを行うことでデータセット有効期限をキャンセルできます。

**API 形式**

```http
DELETE /ttl/{TTL_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{TTL_ID}` | キャンセルするデータセット有効期限の ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、データセット `5b020a27e7040801dedbf46e` の有効期限を更新し、2023年末（グリニッジ標準時）に有効期限が切れるようにします。

```shell
curl -X DELETE \
  https://platform.adobe.io/data/core/hygiene/ttl/5b020a27e7040801dedbf46e \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、`status` 属性が `cancelled` に設定されたデータセットの有効期限の詳細が返されます。

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
| `ttlId` | データセット有効期限の ID。 |
| `datasetId` | この有効期限が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `status` | データセット有効期限の現在のステータス。 |
| `expiry` | データセットが削除されるようにスケジュール設定された日付および時刻。 |
| `updatedAt` | 有効期限が最後に更新された際のタイムスタンプ。 |
| `updatedBy` | 有効期限を最後に更新したユーザー。 |

{style=&quot;table-layout:auto&quot;}

## データセットの有効期限の履歴の取得

ルックアップリクエストでクエリパラメーター `include=history` を使用することで、特定のデータセットの有効期限の履歴を参照できます。結果には、データセットの有効期限の作成、適用された更新、およびそのキャンセルまたは実行（該当する場合）に関する情報が含まれます。

**API 形式**

```http
GET /ttl/{TTL_ID}?include=history
```

| パラメーター | 説明 |
| --- | --- |
| `{TTL_ID}` | 履歴を参照したいデータセットの有効期限の ID。 |

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

正常な応答では、データセットの有効期限の詳細が、記録された各更新の `status`、`expiry`、`updatedAt` および `updatedBy` 属性の詳細を示す `history` 配列と共に返されます。

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
| `ttlId` | データセット有効期限の ID。 |
| `datasetId` | この有効期限が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `history` | 有効期限の更新の履歴をオブジェクトの配列としてリスト表示し、各オブジェクトには、更新時の有効期限の `status`、`expiry`、`updatedAt` および `updatedBy` 属性が含まれます。 |

{style=&quot;table-layout:auto&quot;}

## 付録

### 承認されたクエリパラメーター {#query-params}

次の表では、[データセット有効期限をリスト表示](#list)する際に使用できるクエリパラメーターを説明します。

| パラメーター | 説明 | 例 |
| --- | --- | --- |
| `size` | 返される有効期限の最大数を示す 1～100 の整数。デフォルトは 25 です。 | `size=50` |
| `page` | 返される有効期限のページを示す整数。 | `page=3` |
| `status` | ステータスのコンマ区切りリスト。含める場合、応答は、現在のステータスがリストに含まれるデータセットの有効期限に一致します。 | `status=pending,cancelled` |
| `author` | `created_by` が検索文字列と一致する有効期限に一致します。検索文字列が `LIKE` または `NOT LIKE` で始まる場合、残りは SQL 検索パターンとして扱われます。それ以外の場合は、検索文字列全体が、`created_by` フィールドのコンテンツ全体に完全に一致する必要があるリテラル文字列として扱われます。 | `author=LIKE %john%` |
| `createdDate` | 指定した時間から 24 時間以内に作成された有効期限に一致します。<br><br>時刻を含まない日付（`2021-12-07` など）は、その日の始まりの日時を表していることに注意してください。したがって、`createdDate=2021-12-07` は、2021年12月7日の `00:00:00`～`23:59:59.999999999`（UTC）に作成されたすべての有効期限を参照します。 | `createdDate=2021-12-07` |
| `createdFromDate` | 示された時間以降に作成された有効期限に一致します。 | `createdFromDate=2021-12-07T00:00:00Z` |
| `createdToDate` | 示された時間以前に作成された有効期限に一致します。 | `createdToDate=2021-12-07T23:59:59.999999999Z` |
| `updatedDate`／`updatedToDate`／`updatedFromDate` | `createdDate`／`createdFromDate`／`createdToDate` に似ていますが、作成時間ではなく、データセットの有効期限の更新時間に対して一致します。<br><br>有効期限は、作成時、キャンセル時、実行時を含め、編集されるたびに更新されたと見なされます。 | `updatedDate=2022-01-01` |
| `cancelledDate`／`cancelledToDate`／`cancelledFromDate` | 示された期間の任意の時間にキャンセルされた有効期限に一致します。これは、有効期限が後で（同じデータセットに対して新しい有効期限を設定することで）再開された場合でも適用されます。 | `updatedDate=2022-01-01` |
| `completedDate`／`completedToDate`／`completedFromDate` | 指定された期間内に完了した有効期限に一致します。 | `completedToDate=2021-11-11-06:00` |
| `expiryDate`／`expiryToDate`／`expiryFromDate` | 指定された期間内に実行される予定の、または既に実行された有効期限に一致します。 | `expiryFromDate=2099-01-01&expiryToDate=2100-01-01` |

{style=&quot;table-layout:auto&quot;}
