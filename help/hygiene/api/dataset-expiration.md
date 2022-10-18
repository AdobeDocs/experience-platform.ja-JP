---
title: Dataset Expiration API エンドポイント
description: Data Hygiene API の /ttl エンドポイントを使用すると、Adobe Experience Platform のデータセット有効期限をプログラムでスケジュール設定できます。
exl-id: fbabc2df-a79e-488c-b06b-cd72d6b9743b
source-git-commit: 83149c4e6e8ea483133da4766c37886b8ebd7316
workflow-type: ht
source-wordcount: '1451'
ht-degree: 100%

---

# データセットの有効期限エンドポイント

>[!IMPORTANT]
>
>Adobe Experience Platform のデータハイジーン機能は、現在、Adobe Healthcare Shield を購入した組織でのみ利用できます。

Data Hygiene API の `/ttl` エンドポイントを使用すると、Adobe Experience Platform のデータセットの有効期限プロトコルをスケジュール設定できます。

データセットの有効期限は、単なる時間差の削除操作です。このデータセットは、暫定的に保護されないので、有効期限に達する前に、別の手段で削除される可能性があります。

>[!NOTE]
>
>有効期限は特定の瞬間として指定されますが、有効期限が切れてから実際に削除が開始されるまで、最大 24 時間の遅延が発生する可能性があります。削除が開始されると、すべてのデータセットの痕跡が Platform システムから削除されるまで、最大 7 日間かかる可能性があります。

データセット削除が実際に開始される前であれば、いつでも有効期限をキャンセルしたり、そのトリガー時間を変更したりできます。データセット有効期限のキャンセル後は、新しい有効期限を設定することで再開できます。

データセット削除が開始されると、その有効期限ジョブは `executing` とマークされ、それ以上変更できなくなります。データセット自体は、最大 7 日間復元できる可能性がありますが、それにはアドビのサービスリクエストを通じて手動のプロセスを開始する必要があります。リクエストの実行時に、データレイク、ID サービス、リアルタイム顧客プロファイルは、別々のプロセスを開始し、各サービスからデータセットの内容を削除します。 3 つのサービスすべてからデータを削除すると、有効期限は `executed` とマークされます。

>[!WARNING]
>
>データセットの有効期限が切れるように設定されている場合、ダウンストリームワークフローに悪影響が及ばないよう、データをそのデータセットに取り込む可能性があるデータフローを手動で変更する必要があります。

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
  "totalRecords": 3,
  "ttlDetails": [
    {
      "status": "completed",
      "workorderId": "SDc17a9501345c4997878c1383c475a77b",
      "imsOrgId": "885737B25DC460C50A49411B@AdobeOrg",
      "datasetId": "f440ac301c414bf1b6ba419162866346",
      "expiry": "2021-07-07T13:14:15Z",
      "updatedAt": "2021-07-07T13:14:15Z",
      "updatedBy": "Jane Doe <jane.doe@example.com> d741b5b877bf47cf@AdobeId"
    },
    {
      "status": "pending",
      "workorderId": "SD8ef60b33dbed444fb81861cced5da10b",
      "imsOrgId": "885737B25DC460C50A49411B@AdobeOrg",
      "datasetId": "80f0d38820a74879a2c5be82e38b1a94",
      "expiry": "2099-02-02T00:00:00Z",
      "updatedAt": "2021-02-02T13:00:00Z",
      "updatedBy": "John Q. Public <jqp@example.com> 93220281bad34ed0@AdobeId"
    },
    {
      "status": "pending",
      "workorderId": "SD2140ad4eaf1f47a1b24c05cce53e303e",
      "imsOrgId": "885737B25DC460C50A49411B@AdobeOrg",
      "datasetId": "9e63f9b25896416ba811657678b4fcb7",
      "expiry": "2099-01-01T00:00:00Z",
      "updatedAt": "2021-01-01T13:00:00Z",
      "updatedBy": "Jane Doe <jane.doe@example.com> d741b5b877bf47cf@AdobeId"
    }
  ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `totalRecords` | リスト呼び出しのパラメーターに一致したデータセット有効期限の数。 |
| `ttlDetails` | 返されるデータセット有効期限の詳細が含まれます。データセット有効期限のプロパティについて詳しくは、[ルックアップ呼び出し](#lookup)を行うための応答の節を参照してください。 |

{style=&quot;table-layout:auto&quot;}

## データセット有効期限の検索 {#lookup}

GET リクエストを通じて、データセットの有効期限を参照できます。

**API 形式**

```http
GET /ttl/{DATASET_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | 有効期限を検索したいデータセットの ID。 |

{style=&quot;table-layout:auto&quot;}

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
    "workorderId": "SD5cfd7a11b25543a9bcd9ef647db3d8df",
    "datasetId": "62759f2ede9e601b63a2ee14",
    "imsOrg": "{ORG_ID}",
    "status": "pending",
    "expiry": "2023-12-31T23:59:59Z",
    "updatedAt": "2022-05-11T15:12:40.393115Z",
    "updatedBy": "{USER_ID}",
    "displayName": "Example Dataset Expiration Request",
    "description": "A dataset expiration request that will execute at the end of 2023"
}
```

| プロパティ | 説明 |
| --- | --- |
| `workorderId` | データセット有効期限の ID。 |
| `datasetId` | この有効期限が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `status` | データセット有効期限の現在のステータス。 |
| `expiry` | データセットが削除されるようにスケジュール設定された日付および時刻。 |
| `updatedAt` | 有効期限が最後に更新された際のタイムスタンプ。 |
| `updatedBy` | 有効期限を最後に更新したユーザー。 |
| `displayName` | 有効期限リクエストの表示名。 |
| `description` | 有効期限リクエストの説明。 |

{style=&quot;table-layout:auto&quot;}

### カタログの有効期限タグ

[Catalog API](../../catalog/api/getting-started.md) を使用してデータセットの詳細を検索するには、データセットに有効期限がある場合は、そのデータセットが `tags.adobe/hygiene/ttl` の下に表示されます。

次の JSON は、カタログのデータセットの詳細に対する切り捨てられた応答を表し、有効期限は `32503680000000` となっています。タグの値は、有効期限を Unix エポック開始からミリ秒の整数値でエンコードします。

```json
{
  "63212313c308d51b997858ba": {
    "name": "TTL Test Dataset",
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

## データセットの有効期限の作成または更新 {#create-or-update}

PUT リクエストを通じて、データセットの有効期限を作成または更新できます。

**API 形式**

```http
PUT /ttl/{DATASET_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | 有効期限をスケジュール設定するデータセットの ID。 |

**リクエスト**

次のリクエストは、データセット `5b020a27e7040801dedbf46e` の削除を 2022年末（グリニッジ標準時）にスケジュール設定します。データセットに既存の有効期限が見つからない場合は、新しい有効期限が作成されます。データセットに既に保留中の有効期限がある場合、その有効期限は新しい `expiry` の値で更新されます。

```shell
curl -X PUT \
  https://platform.adobe.io/data/core/hygiene/ttl/5b020a27e7040801dedbf46e \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "expiry": "2022-12-31T23:59:59Z",
        "displayName": "Example Expiration Request",
        "description": "Cleanup identities required by JIRA request 12345 across all datasets in the prod sandbox."
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `expiry` | データセットが削除される日時の ISO 8601 タイムスタンプ。 |
| `displayName` | 有効期限のリクエストの表示名。 |
| `description` | 有効期限リクエストのオプション説明。 |

{style=&quot;table-layout:auto&quot;}

**応答**

応答が成功すると、データセット有効期限の詳細が、HTTP ステータス（既存の有効期限が更新されていた場合は 200（OK）、既存の有効期限がなかった場合は 201（作成済み））と共に返されます。

```json
{
    "workorderId": "SD5cfd7a11b25543a9bcd9ef647db3d8df",
    "datasetId": "5b020a27e7040801dedbf46e",
    "imsOrg": "{ORG_ID}",
    "status": "pending",
    "expiry": "2032-12-31T23:59:59Z",
    "updatedAt": "2022-05-09T22:38:40.393115Z",
    "updatedBy": "{USER_ID}",
    "displayName": "Example Expiration Request",
    "description": "Cleanup identities required by JIRA request 12345 across all datasets in the prod sandbox."
}
```

| プロパティ | 説明 |
| --- | --- |
| `workorderId` | データセット有効期限の ID。 |
| `datasetId` | この有効期限が適用されるデータセットの ID。 |
| `imsOrg` | 組織の ID。 |
| `status` | データセット有効期限の現在のステータス。 |
| `expiry` | データセットが削除されるようにスケジュール設定された日付および時刻。 |
| `updatedAt` | 有効期限が最後に更新された際のタイムスタンプ。 |
| `updatedBy` | 有効期限を最後に更新したユーザー。 |

{style=&quot;table-layout:auto&quot;}

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
| `{EXPIRATION_ID}` | キャンセルするデータセット有効期限の `workorderId` |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストでは、ID `SD5cfd7a11b25543a9bcd9ef647db3d8df` を持つデータセットの有効期限がキャンセルされます。

```shell
curl -X DELETE \
  https://platform.adobe.io/data/core/hygiene/ttl/SD5cfd7a11b25543a9bcd9ef647db3d8df \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、HTTP ステータス 204（コンテンツなし）が返され、有効期限の `status` 属性が `cancelled` に設定されます。

## データセットの有効期限ステータス履歴の取得

ルックアップリクエストでクエリパラメーター `include=history` を使用することで、特定のデータセットの有効期限ステータス履歴を検索できます。結果には、データセットの有効期限の作成、適用された更新、およびそのキャンセルまたは実行（該当する場合）に関する情報が含まれます。

**API 形式**

```http
GET /ttl/{DATASET_ID}?include=history
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | 有効期限履歴を検索したいデータセットの ID。 |

{style=&quot;table-layout:auto&quot;}

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
  "workorderId": "SD5cfd7a11b25543a9bcd9ef647db3d8df",
  "datasetId": "62759f2ede9e601b63a2ee14",
  "datasetName": "Example Dataset",
  "sandboxName": "prod",
  "displayName": "Expiration Request 123",
  "description": "Expiration Request 123 Description",
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
| `workorderId` | データセット有効期限の ID。 |
| `datasetId` | この有効期限が適用されるデータセットの ID。 |
| `datasetName` | この有効期限が適用されるデータセットの表示名。 |
| `sandboxName` | ターゲットデータセットが配置されているサンドボックスの名前。 |
| `displayName` | 有効期限リクエストの表示名。 |
| `description` | 有効期限リクエストの説明。 |
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
| `orgId` | 組織 ID がパラメーターと一致するデータセットの有効期限に一致します。この値はデフォルトで `x-gw-ims-org-id` ヘッダーの値となり、リクエストがサービストークンを提供しない限り無視されます。 | `orgId=885737B25DC460C50A49411B@AdobeOrg` |
| `status` | ステータスのコンマ区切りリスト。含める場合、応答は、現在のステータスがリストに含まれるデータセットの有効期限に一致します。 | `status=pending,cancelled` |
| `author` | `created_by` が検索文字列と一致する有効期限に一致します。検索文字列が `LIKE` または `NOT LIKE` で始まる場合、残りは SQL 検索パターンとして扱われます。それ以外の場合は、検索文字列全体が、`created_by` フィールドのコンテンツ全体に完全に一致する必要があるリテラル文字列として扱われます。 | `author=LIKE %john%` |
| `sandboxName` | サンドボックス名が引数と完全に一致するデータセット有効期限に一致します。デフォルトは、リクエストの `x-sandbox-name` ヘッダーにあるサンドボックス名です。`sandboxName=*` を使用して、すべてのサンドボックスからデータセット有効期限を含めます。 | `sandboxName=dev1` |
| `datasetId` | 特定のデータセットに適用する有効期限に一致します。 | `datasetId=62b3925ff20f8e1b990a7434` |
| `createdDate` | 指定した時間から 24 時間以内に作成された有効期限に一致します。<br><br>時刻を含まない日付（`2021-12-07` など）は、その日の始まりの日時を表していることに注意してください。したがって、`createdDate=2021-12-07` は、2021年12月7日の `00:00:00`～`23:59:59.999999999`（UTC）に作成されたすべての有効期限を参照します。 | `createdDate=2021-12-07` |
| `createdFromDate` | 示された時間以降に作成された有効期限に一致します。 | `createdFromDate=2021-12-07T00:00:00Z` |
| `createdToDate` | 示された時間以前に作成された有効期限に一致します。 | `createdToDate=2021-12-07T23:59:59.999999999Z` |
| `updatedDate`／`updatedToDate`／`updatedFromDate` | `createdDate`／`createdFromDate`／`createdToDate` に似ていますが、作成時間ではなく、データセットの有効期限の更新時間に対して一致します。<br><br>有効期限は、作成時、キャンセル時、実行時を含め、編集されるたびに更新されたと見なされます。 | `updatedDate=2022-01-01` |
| `cancelledDate`／`cancelledToDate`／`cancelledFromDate` | 示された期間の任意の時間にキャンセルされた有効期限に一致します。これは、有効期限が後で（同じデータセットに対して新しい有効期限を設定することで）再開された場合でも適用されます。 | `updatedDate=2022-01-01` |
| `completedDate`／`completedToDate`／`completedFromDate` | 指定された期間内に完了した有効期限に一致します。 | `completedToDate=2021-11-11-06:00` |
| `expiryDate`／`expiryToDate`／`expiryFromDate` | 指定された期間内に実行される予定の、または既に実行された有効期限に一致します。 | `expiryFromDate=2099-01-01&expiryToDate=2100-01-01` |

{style=&quot;table-layout:auto&quot;}
