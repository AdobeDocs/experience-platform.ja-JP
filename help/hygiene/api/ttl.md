---
title: データセット有効期間 (TTL)API エンドポイント
description: データ衛生 API の/ttl エンドポイントを使用すると、Adobe Experience Platformでデータセットの TTL をプログラムでスケジュールできます。
source-git-commit: 931b847761e649696aa8433d53233593efd4d1ee
workflow-type: tm+mt
source-wordcount: '1315'
ht-degree: 7%

---

# データセットの有効期間 (TTL) エンドポイント

>[!IMPORTANT]
>
>現在、Adobe Experience Platformのデータ衛生機能は、医療用Adobeシールドを購入した組織でのみ使用できます。

この `/ttl` エンドポイントを使用すると、Adobe Experience Platformのデータセットの有効期間 (TTL) プロトコルをスケジュールできます。

データセットの TTL は、時間遅延削除操作のみです。 データセットは中間的に保護されていないので、有効期限に達する前に他の方法で削除される場合があります。

>[!NOTE]
>
>有効期限は特定の即時インタントとして指定されますが、有効期限から実際の削除が開始されるまで、最大 24 時間の遅延が生じる場合があります。 削除が開始されると、データセットのすべてのトレースが Platform システムから削除されるまで、最大 7 日かかる場合があります。

データセットの削除が実際に開始される前であればいつでも、TTL をキャンセルしたり、トリガー時間を変更したりできます。 TTL をキャンセルした後、新しい有効期限を設定して再度開くことができます。

データセットの削除が開始されると、その TTL は、 `executing`それ以上変更されない場合があります。 データセット自体は最大 7 日間復元できますが、復元リクエストを通じて手動で開始したプロセスによってのみAdobe サービスできます。

## はじめに

このガイドで使用するエンドポイントは、データ衛生 API に含まれています。 続行する前に、 [概要](./overview.md) 関連ドキュメントへのリンク、このドキュメントの API 呼び出し例の読み方のガイド、および任意のExperience PlatformAPI を正しく呼び出すために必要な必須ヘッダーに関する重要な情報。

## データセットの TTL をリストする {#list}

GETリクエストをおこなうことで、組織のすべてのデータセット TTL をリストできます。

**API 形式**

```http
GET /ttl?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
| --- | --- |
| `{QUERY_PARAMETERS}` | オプションのクエリパラメーターのリストです。複数のパラメーターを区切ります ( `&` 文字。 一般的なパラメーターは次のとおりです `size` および `page` ページネーション用に。 サポートされるクエリパラメーターの完全なリストについては、 [付録の節](#query-params). |

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

正常な応答では、結果の TTL がリストされます。 次の例は、スペースを節約するために切り捨てられています。

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
| `results` | 返される TTL の詳細が含まれます。 TTL エンティティのプロパティについて詳しくは、 [ルックアップ呼び出し](#lookup). |
| `current_page` | リストに表示された結果の現在のページ。 |
| `total_pages` | 応答のページの合計数。 |
| `total_count` | 応答内の TTL エンティティの合計数。 |

{style=&quot;table-layout:auto&quot;}

## TTL の検索 {#lookup}

データセットの TTL は、データリクエストを通じてGETセットを参照できます。

**API 形式**

```http
GET /ttl/{TTL_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{TTL_ID}` | 検索する TTL の ID。 |

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

正常な応答は、TTL の詳細を返します。

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
| `ttlId` | データセットの TTL の ID。 |
| `datasetId` | この TTL が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `status` | TTL の現在のステータス。 |
| `expiry` | データセットを削除する予定日時。 |
| `updatedAt` | TTL が最後に更新された日時のタイムスタンプ。 |
| `updatedBy` | TTL を最後に更新したユーザー。 |

{style=&quot;table-layout:auto&quot;}

## TTL を作成 {#create}

データセットの TTL は、POSTリクエストを通じて追加できます。

**API 形式**

```http
POST /ttl
```

**リクエスト**

次のリクエストは、データセットのスケジュールを設定します `5b020a27e7040801dedbf46e` 2022 年（グリニッジ標準時）末に削除するためのツール。

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
| `datasetId` | TTL をスケジュールするデータセットの ID。 |
| `expiry` | データセットを削除する際のの ISO 8601 タイムスタンプ。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、TTL の詳細を返します。既存の TTL が更新された場合は HTTP ステータス 200(OK)、既存の TTL がない場合は 201（作成済み）が返されます。

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
| `ttlId` | データセットの TTL の ID。 |
| `datasetId` | この TTL が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `status` | TTL の現在のステータス。 |
| `expiry` | データセットを削除する予定日時。 |
| `updatedAt` | TTL が最後に更新された日時のタイムスタンプ。 |
| `updatedBy` | TTL を最後に更新したユーザー。 |

{style=&quot;table-layout:auto&quot;}

## TTL の更新 {#update}

データセットの TTL は、更新リクエストを通じてPUTできます。

**API 形式**

```http
PUT /ttl/{TTL_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{TTL_ID}` | 変更する TTL の ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、データセットの TTL を更新します `5b020a27e7040801dedbf46e` 2023 年（グリニッジ標準時）末に期限が切れます。

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
| `expiry` | データセットを削除する際のの ISO 8601 タイムスタンプ。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、更新された TTL の詳細を返します。

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
| `ttlId` | データセットの TTL の ID。 |
| `datasetId` | この TTL が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `status` | TTL の現在のステータス。 |
| `expiry` | データセットを削除する予定日時。 |
| `updatedAt` | TTL が最後に更新された日時のタイムスタンプ。 |
| `updatedBy` | TTL を最後に更新したユーザー。 |

{style=&quot;table-layout:auto&quot;}

## TTL をキャンセル {#delete}

TTL をキャンセルするには、DELETEリクエストを実行します。

**API 形式**

```http
DELETE /ttl/{TTL_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{TTL_ID}` | キャンセルする TTL の ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、データセットの TTL を更新します `5b020a27e7040801dedbf46e` 2023 年（グリニッジ標準時）末に期限が切れます。

```shell
curl -X DELETE \
  https://platform.adobe.io/data/core/hygiene/ttl/5b020a27e7040801dedbf46e \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、TTL の詳細と共にを返します `status` 属性が `cancelled`.

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
| `ttlId` | データセットの TTL の ID。 |
| `datasetId` | この TTL が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `status` | TTL の現在のステータス。 |
| `expiry` | データセットを削除する予定日時。 |
| `updatedAt` | TTL が最後に更新された日時のタイムスタンプ。 |
| `updatedBy` | TTL を最後に更新したユーザー。 |

{style=&quot;table-layout:auto&quot;}

## TTL の履歴の取得

クエリパラメーターを使用して、特定の TTL の履歴を調べることができます `include=history` 参照リクエストに含まれます。 結果には、TTL の作成に関する情報、適用された更新、キャンセルまたは実行（該当する場合）に関する情報が含まれます。

**API 形式**

```http
GET /ttl/{TTL_ID}?include=history
```

| パラメーター | 説明 |
| --- | --- |
| `{TTL_ID}` | 履歴を検索する TTL の ID。 |

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

正常な応答は、TTL の詳細をと共に返します。 `history` 詳細を提供する配列 `status`, `expiry`, `updatedAt`、および `updatedBy` 記録された更新のそれぞれの属性。

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
| `ttlId` | データセットの TTL の ID。 |
| `datasetId` | この TTL が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `history` | TTL の更新履歴をオブジェクトの配列としてリストします。各オブジェクトには `status`, `expiry`, `updatedAt`、および `updatedBy` 属性（更新時の TTL の属性）。 |

{style=&quot;table-layout:auto&quot;}

## 付録

### 受け入れられたクエリパラメーター {#query-params}

次の表に、使用可能なクエリパラメーターの概要を示します。 [データセットの TTL のリスト](#list):

| パラメーター | 説明 | 例 |
| --- | --- | --- |
| `size` | 1 ～ 100 の整数で、返される TTL の最大数を示します。 デフォルトは 25 です。 | `size=50` |
| `page` | 返される TTL のページを示す整数。 | `page=3` |
| `status` | ステータスのコンマ区切りリスト。 この値が含まれる場合、応答は、リストに示されている TTL の中で現在のステータスを持つ TTL と一致します。 | `status=pending,cancelled` |
| `author` | の TTL に一致 `created_by` は、検索文字列に一致します。 検索文字列が `LIKE` または `NOT LIKE`の場合、残りの部分は SQL 検索パターンとして扱われます。 それ以外の場合、検索文字列全体は、 `created_by` フィールドに入力します。 | `author=LIKE %john%` |
| `createdDate` | 指定された時刻から始まる 24 時間のウィンドウで作成された TTL に一致します。<br><br>日付に時刻が含まれていない ( `2021-12-07`) は、その日の初めの日時を表します。 このように `createdDate=2021-12-07` は、2021 年 12 月 7 日に作成された任意の TTL( `00:00:00` 経由 `23:59:59.999999999` (UTC) です。 | `createdDate=2021-12-07` |
| `createdFromDate` | 指定された時刻以降に作成された TTL に一致します。 | `createdFromDate=2021-12-07T00:00:00Z` |
| `createdToDate` | 指定された時刻またはその前に作成された TTL に一致します。 | `createdToDate=2021-12-07T23:59:59.999999999Z` |
| `updatedDate`／`updatedToDate`／`updatedFromDate` | 次に類似 `createdDate` / `createdFromDate` / `createdToDate`が生成されますが、作成時間ではなく TTL の更新時間に一致します。<br><br>TTL は、作成、キャンセル、実行のタイミングなど、編集のたびに更新されたと見なされます。 | `updatedDate=2022-01-01` |
| `cancelledDate`／`cancelledToDate`／`cancelledFromDate` | 指定された間隔内のいつでもキャンセルされた TTL に一致します。 これは、TTL が後で（同じデータセットに対して新しい有効期限を設定することで）再び開かれた場合でも同じです。 | `updatedDate=2022-01-01` |
| `completedDate`／`completedToDate`／`completedFromDate` | 指定した期間に完了した TTL に一致します。 | `completedToDate=2021-11-11-06:00` |
| `expiryDate`／`expiryToDate`／`expiryFromDate` | 指定された期間に実行される、または既に実行された TTL に一致します。 | `expiryFromDate=2099-01-01&expiryToDate=2100-01-01` |

{style=&quot;table-layout:auto&quot;}
