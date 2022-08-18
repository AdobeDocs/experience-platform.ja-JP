---
title: データセット有効期限 API エンドポイント
description: Data Whealthy API の/ttl エンドポイントを使用すると、Adobe Experience Platformでデータセットの有効期限をプログラムでスケジュールできます。
exl-id: fbabc2df-a79e-488c-b06b-cd72d6b9743b
source-git-commit: 49ba5263c6dc8eccac2ffe339476cf316c68e486
workflow-type: tm+mt
source-wordcount: '1375'
ht-degree: 34%

---

# データセット有効期限エンドポイント

>[!IMPORTANT]
>
>Adobe Experience Platform のデータハイジーン機能は、現在、Healthcare Shield を購入した組織のみが利用できます。

この `/ttl` データ衛生 API のエンドポイントを使用すると、Adobe Experience Platformのデータセットの有効期限をスケジュールできます。

データセットの有効期限は、時間差の削除操作のみです。 このデータセットは、暫定的に保護されないので、有効期限に達する前に、別の手段で削除される可能性があります。

>[!NOTE]
>
>有効期限は特定の瞬間として指定されますが、有効期限が切れてから実際に削除が開始されるまで、最大 24 時間の遅延が発生する可能性があります。削除が開始されると、すべてのデータセットの痕跡が Platform システムから削除されるまで、最大 7 日間かかる可能性があります。

データセットの削除が実際に開始される前であればいつでも、有効期限をキャンセルしたり、トリガー時間を変更したりできます。 データセットの有効期限をキャンセルした後、新しい有効期限を設定して再度開くことができます。

データセットの削除が開始されると、その有効期限ジョブは、 `executing`それ以上変更されない場合があります。 データセット自体は、最大 7 日間復元できる可能性がありますが、アドビのサービスリクエストを通じて開始された手動のプロセスによってのみ可能です。リクエストの実行時に、データレイク、ID サービス、リアルタイム顧客プロファイルは別々のプロセスを開始し、各サービスからデータセットの内容を削除します。 3 つのサービスすべてからデータを削除すると、有効期限は `executed`.

## はじめに

このガイドで使用するエンドポイントは、Data Hygiene API の一部です。先に進む前に、[概要](./overview.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## データセットの有効期限のリスト {#list}

GETリクエストをおこなうことで、組織のすべてのデータセット有効期限をリストできます。

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

正常な応答は、結果のデータセット有効期限をリストします。 次の例は、スペースを節約するために省略されています。

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
| `results` | 返されたデータセットの有効期限の詳細が含まれます。 データセットの有効期限のプロパティについて詳しくは、 [ルックアップ呼び出し](#lookup). |
| `current_page` | リストされた結果の現在のページ。 |
| `total_pages` | 応答内のページの合計数。 |
| `total_count` | 応答内のデータセット有効期限の合計数。 |

{style=&quot;table-layout:auto&quot;}

## データセットの有効期限の検索 {#lookup}

GETセットの有効期限は、データリクエストを通じて検索できます。

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

正常な応答は、データセットの有効期限の詳細を返します。

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
| `ttlId` | データセットの有効期限の ID。 |
| `datasetId` | この有効期限が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `status` | データセットの有効期限の現在のステータス。 |
| `expiry` | データセットが削除されるようにスケジュール設定された日付および時刻。 |
| `updatedAt` | 有効期限が最後に更新された日時のタイムスタンプ。 |
| `updatedBy` | 有効期限を最後に更新したユーザー。 |

{style=&quot;table-layout:auto&quot;}

## データセットの有効期限の作成 {#create}

データセットの有効期限は、POSTリクエストで作成できます。

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
| `datasetId` | 有効期限をスケジュールするデータセットの ID。 |
| `expiry` | データセットが削除される日時の ISO 8601 タイムスタンプ。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、データセットの有効期限の詳細を返します。既存の有効期限が更新された場合は HTTP ステータス 200(OK)、既存の有効期限がない場合は 201（作成済み）が返されます。

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
| `ttlId` | データセットの有効期限の ID。 |
| `datasetId` | この有効期限が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `status` | データセットの有効期限の現在のステータス。 |
| `expiry` | データセットが削除されるようにスケジュール設定された日付および時刻。 |
| `updatedAt` | 有効期限が最後に更新された日時のタイムスタンプ。 |
| `updatedBy` | 有効期限を最後に更新したユーザー。 |

{style=&quot;table-layout:auto&quot;}

## データセットの有効期限の更新 {#update}

データセットの有効期限は、PUTリクエストで更新できます。

**API 形式**

```http
PUT /ttl/{TTL_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{TTL_ID}` | 変更するデータセット有効期限の ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、データセットの有効期限を更新します `5b020a27e7040801dedbf46e` 2023 年（グリニッジ標準時）末に期限が切れます。

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

正常な応答は、更新された有効期限の詳細を返します。

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
| `ttlId` | データセットの有効期限の ID。 |
| `datasetId` | この有効期限が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `status` | データセットの有効期限の現在のステータス。 |
| `expiry` | データセットが削除されるようにスケジュール設定された日付および時刻。 |
| `updatedAt` | 有効期限が最後に更新された日時のタイムスタンプ。 |
| `updatedBy` | 有効期限を最後に更新したユーザー。 |

{style=&quot;table-layout:auto&quot;}

## データセットの有効期限のキャンセル {#delete}

データセットの有効期限は、リクエストを実行してキャンセルすることがDELETEできます。

**API 形式**

```http
DELETE /ttl/{TTL_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{TTL_ID}` | キャンセルするデータセット有効期限の ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、データセットの有効期限を更新します `5b020a27e7040801dedbf46e` 2023 年（グリニッジ標準時）末に期限が切れます。

```shell
curl -X DELETE \
  https://platform.adobe.io/data/core/hygiene/ttl/5b020a27e7040801dedbf46e \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、データセットの有効期限の詳細と、 `status` 属性が `cancelled`.

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
| `ttlId` | データセットの有効期限の ID。 |
| `datasetId` | この有効期限が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `status` | データセットの有効期限の現在のステータス。 |
| `expiry` | データセットが削除されるようにスケジュール設定された日付および時刻。 |
| `updatedAt` | 有効期限が最後に更新された日時のタイムスタンプ。 |
| `updatedBy` | 有効期限を最後に更新したユーザー。 |

{style=&quot;table-layout:auto&quot;}

## データセット有効期限の履歴の取得

クエリパラメーターを使用して、特定のデータセット有効期限の履歴を調べることができます `include=history` 参照リクエストに含まれます。 結果には、データセットの有効期限の作成、適用された更新、キャンセルまたは実行（該当する場合）に関する情報が含まれます。

**API 形式**

```http
GET /ttl/{TTL_ID}?include=history
```

| パラメーター | 説明 |
| --- | --- |
| `{TTL_ID}` | 履歴を検索するデータセットの有効期限の ID。 |

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

正常な応答は、データセットの有効期限の詳細と `history` 詳細を提供する配列 `status`, `expiry`, `updatedAt`、および `updatedBy` 記録された更新のそれぞれの属性。

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
| `ttlId` | データセットの有効期限の ID。 |
| `datasetId` | この有効期限が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `history` | 有効期限の更新履歴をオブジェクトの配列としてリストします。各オブジェクトには `status`, `expiry`, `updatedAt`、および `updatedBy` 更新時の有効期限の属性。 |

{style=&quot;table-layout:auto&quot;}

## 付録

### 承認されたクエリパラメーター {#query-params}

次の表に、使用可能なクエリパラメーターの概要を示します。 [データセット有効期限のリスト](#list):

| パラメーター | 説明 | 例 |
| --- | --- | --- |
| `size` | 1 ～ 100 の整数で、返す有効期限の最大数を示します。 デフォルトは 25 です。 | `size=50` |
| `page` | 有効期限を返すページを示す整数。 | `page=3` |
| `status` | ステータスのコンマ区切りリスト。この値を含めると、この応答は、リストに示されているステータスの中に現在のステータスを持つデータセット有効期限と一致します。 | `status=pending,cancelled` |
| `author` | の有効期限に一致 `created_by` は、検索文字列に一致します。 検索文字列が `LIKE` または `NOT LIKE` で始まる場合、残りは SQL 検索パターンとして扱われます。それ以外の場合は、検索文字列全体が、`created_by` フィールドのコンテンツ全体に完全に一致する必要があるリテラル文字列として扱われます。 | `author=LIKE %john%` |
| `createdDate` | 指定された時刻から始まる 24 時間のウィンドウで作成された有効期限に一致します。<br><br>時刻を含まない日付（`2021-12-07` など）は、その日の始まりの日時を表していることに注意してください。このように `createdDate=2021-12-07` は、2021 年 12 月 7 日 (PT) に `00:00:00` 経由 `23:59:59.999999999` (UTC) です。 | `createdDate=2021-12-07` |
| `createdFromDate` | 指定された時刻に作成された、またはその後に作成された有効期限に一致します。 | `createdFromDate=2021-12-07T00:00:00Z` |
| `createdToDate` | 指定された時刻またはその前に作成された有効期限に一致します。 | `createdToDate=2021-12-07T23:59:59.999999999Z` |
| `updatedDate`／`updatedToDate`／`updatedFromDate` | 次に類似 `createdDate` / `createdFromDate` / `createdToDate`は抽出されますが、作成時間ではなく、データセットの有効期限の更新時間に一致します。<br><br>有効期限は、編集の作成、キャンセル、実行のタイミングなど、すべての編集時に更新されたと見なされます。 | `updatedDate=2022-01-01` |
| `cancelledDate`／`cancelledToDate`／`cancelledFromDate` | 指定された間隔内の任意の時点でキャンセルされた有効期限に一致します。 これは、（同じデータセットに新しい有効期限を設定することで）後で有効期限が再び開かれた場合でも同じです。 | `updatedDate=2022-01-01` |
| `completedDate`／`completedToDate`／`completedFromDate` | 指定した期間内に完了した有効期限に一致します。 | `completedToDate=2021-11-11-06:00` |
| `expiryDate`／`expiryToDate`／`expiryFromDate` | 指定した期間内に、実行される期限、または既に実行されている期限に一致します。 | `expiryFromDate=2099-01-01&expiryToDate=2100-01-01` |

{style=&quot;table-layout:auto&quot;}
