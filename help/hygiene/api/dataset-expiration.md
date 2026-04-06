---
title: Dataset Expiration API エンドポイント
description: Data Hygiene API の /ttl エンドポイントを使用すると、Adobe Experience Platform のデータセット有効期限をプログラムでスケジュール設定できます。
role: Developer
exl-id: fbabc2df-a79e-488c-b06b-cd72d6b9743b
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '2331'
ht-degree: 19%

---

# データセットの有効期限エンドポイント

Data Hygiene APIの`/ttl` エンドポイントを使用して、Adobe Experience Platformのデータセットを削除するタイミングをスケジュールします。

データセットの有効期限は、遅延削除操作です。 データセットは暫定的に保護されず、スケジュールされた有効期限の前に他の方法で削除される可能性があります。

>[!NOTE]
>
>有効期限は特定の瞬間として指定されますが、有効期限が切れてから実際に削除が開始されるまで、最大 24 時間の遅延が発生する可能性があります。削除が開始されると、データセットのすべてのトレースがExperience Platform システムから削除されるまでに、最大7日かかる場合があります。

削除を開始する前に、有効期限をキャンセルしたり、スケジュールされた時間を変更したりできます。 キャンセルされた有効期限を再度開くには、新しい有効期限を設定します。

削除が開始されると、有効期限ジョブは`executing`としてマークされ、変更できなくなります。 このデータセットは、最大7日間回復可能ですが、手動でのAdobe サービスリクエストによってのみ可能です。 削除中、データレイク、ID サービス、リアルタイム顧客プロファイルは、それぞれデータセットの内容を個別に削除します。 削除が完了すると、有効期限は`completed`としてマークされます。

>[!WARNING]
>
>データセットの有効期限が切れるように設定されている場合、ダウンストリームワークフローに悪影響が及ばないよう、データをそのデータセットに取り込む可能性があるデータフローを手動で変更する必要があります。

高度なデータライフサイクル管理では、データセットの有効期限エンドポイントを介したデータセットの削除と、[workorder エンドポイント &#x200B;](./workorder.md)を介したプライマリ IDを使用したIDの削除（行レベルのデータ）をサポートしています。 また、Experience Platform UIを使用して、[&#x200B; データセットの有効期限](../ui/dataset-expiration.md)および[&#x200B; レコードの削除](../ui/record-delete.md)を管理することもできます。 詳しくは、リンクされたドキュメントを参照してください。

>[!NOTE]
>
>データライフサイクルはバッチ削除をサポートしていません。

## はじめに

このガイドで使用するエンドポイントは、Data Hygiene API の一部です。続行する前に、CRUD操作に必要なヘッダー、エラーメッセージ、Postman コレクション、サンプル API呼び出しの読み取り方法について、[API ガイド &#x200B;](./overview.md)を確認してください。

>[!IMPORTANT]
>
>Data Hygiene APIを呼び出す場合は、-H `x-sandbox-name: {SANDBOX_NAME}` ヘッダーを使用する必要があります。

## データセット有効期限のリスト {#list}

`/ttl` エンドポイントに対してGET リクエストを行うことで、組織に設定されたすべてのデータセット有効期限を一覧表示できます。

クエリパラメーターを使用して結果をフィルタリングし、条件を満たす有効期限のみを返します。 各結果には、各データセットの有効期限のステータスと設定の詳細が含まれます。

**API 形式**

```http
GET /ttl?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
| --- | --- |
| `{QUERY_PARAMETERS}` | `&` 文字で区切られた複数のパラメーターを含む、オプションのクエリパラメーターのリスト。共通のパラメーターには、ページネーション用の `limit` および `page` があります。サポートされているクエリパラメーターの完全なリストについては、[付録セクション &#x200B;](#query-params) サポートされているクエリパラメーターの完全なリストを参照してください。 最も一般的に使用されるパラメーターは、以下に含まれています。また、付録にも含まれています。 |
| `author` | データセットの有効期限を最近更新または作成したユーザーでフィルタリングします。 SQLのようなパターンをサポートしています（例：`LIKE %john%`）。 |
| `datasetId` | 特定のデータセット IDで有効期限をフィルタリングします。 |
| `datasetName` | データセット名が一致する場合は、大文字と小文字を区別しないフィルターを使用します。 |
| `status` | ステータスのコンマ区切りリストでフィルタリングします：`pending`、`executing`、`cancelled`、`completed`。 |
| `expiryDate` | 特定の有効期限を持つ有効期限をフィルタリングします。 |
| `limit` | 返す結果の最大数を指定します（1 ～ 100、デフォルト：25）。 |
| `page` | 結果をゼロ ベースのインデックスでページネーションします（デフォルトのページサイズ：50、最大：100）。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、2021年8月1日より前に更新され、名前が「Jane Doe」に一致するユーザーによって最後に更新されたすべてのデータセット有効期限を取得します。

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
>応答の`ttlId`は`{DATASET_EXPIRATION_ID}`とも呼ばれます。 どちらも、データセットの有効期限の一意の識別子を参照します。

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
| `results` | データセットの有効期限の設定の配列。 |
| `ttlId` | データセット有効期限の設定の一意のID。 |
| `datasetId` | この設定に関連付けられているデータセットの一意の識別子。 |
| `datasetName` | データセットの名前。 |
| `sandboxName` | このデータセットの有効期限が設定されているサンドボックス。 |
| `displayName` | 有効期限の設定に関する人間が読み取れる名前。 |
| `description` | 有効期限の設定の説明。 |
| `imsOrg` | 一意の組織ID。 |
| `status` | 有効期限の現在のステータス。 `pending`、`executing`、`cancelled`、`completed`のいずれか。 |
| `expiry` | スケジュールされた有効期限（ISO 8601形式）。 |
| `updatedAt` | この設定に対する最後の更新のタイムスタンプ。 |
| `updatedBy` | 設定を最後に更新したユーザーまたはサービスのIDと電子メール。 |
| `current_page` | 現在の結果ページのインデックス （ゼロ ベース）。 |
| `total_pages` | 使用可能な結果ページの合計数。 |
| `total_count` | 返されたデータセット有効期限コンフィギュレーション レコードの合計数です。 |

{style="table-layout:auto"}

## データセット有効期限の検索 {#lookup}

データセットの有効期限IDまたはデータセット IDをパスパラメーターとして使用してGET リクエストを行うことで、特定のデータセットの有効期限の設定の詳細を取得します。

>[!IMPORTANT]
>
>データセットの有効期限ID （例：`SD-xxxxxx-xxxx`）またはデータセット IDをパスに指定できます。 応答の`ttlId`は、データセットの有効期限の一意の識別子です。

**API 形式**

```http
GET /ttl/{ID}
GET /ttl/{ID}?include=history
```

| パラメーター | 説明 |
| --- | --- |
| `{ID}` | データセット有効期限の設定の一意のID。 データセットの有効期限IDまたはデータセット IDを指定できます。 |
| `include` | （オプション） `history`に設定されている場合、応答には、設定の変更イベントを含む`history`配列が含まれます。 |

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
| `ttlId` | データセット有効期限の設定の一意のID。 |
| `datasetId` | データセットの一意のID。 |
| `datasetName` | データセットの名前。 |
| `sandboxName` | データセットの有効期限が設定されているサンドボックス。 |
| `displayName` | データセットの有効期限の設定に関する、人間が読み取れる名前。 |
| `description` | データセットの有効期限の設定の説明。 |
| `imsOrg` | この設定に関連付けられている一意の組織ID。 |
| `status` | データセット有効期限の設定の現在のステータス。<br>いずれかの：`pending`、`executing`、`cancelled`、`completed`。 |
| `expiry` | データセットのスケジュールされた有効期限タイムスタンプ（ISO 8601形式）。 |
| `updatedAt` | 最新の更新のタイムスタンプ。 |
| `updatedBy` | データセットの有効期限を最後に更新したユーザーまたはサービスの識別子と電子メール。 |

{style="table-layout:auto"}

### カタログの有効期限タグ

[Catalog API](../../catalog/api/getting-started.md) を使用してデータセットの詳細を検索するには、データセットにアクティブな有効期限がある場合は、そのデータセットが `tags.adobe/hygiene/ttl` の下に表示されます。

次のJSONは、有効期限が`32503680000000`のデータセットに対する切り捨てられたカタログ API応答を示しています。 タグは、有効期限をUnix エポックからのミリ秒数としてエンコードします。

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

新しいデータセット有効期限の設定を作成して、データセットがいつ期限切れになり、削除の対象になるかを定義します。\
データセット ID、有効期限または日時（ISO 8601形式）、表示名、および（オプションで）説明を入力します。

>[!NOTE]
>
>有効期限は、日付（YYYY-MM-DD）または日時（YYYY-MM-DDTHH:MM:SSZ）です。 日付のみを指定した場合、システムはその日の午前0時（00:00:00Z）を使用します。 有効期限は24時間以上である必要があります。

データセットの有効期限を作成するには、次に示すようにPOST リクエストを送信します。

>[!TIP]
>
>404 エラーが表示された場合は、リクエストにフォワードスラッシュが追加されていないことを確認します。 末尾のスラッシュを使用すると、POST リクエストが失敗する可能性があります。

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
| `datasetId` | **必須**&#x200B;有効期限を適用するデータセットの一意のID。 |
| `expiry` | **必須。** ISO 8601形式の有効期限の日時。 これにより、システム内のデータの有効期間が定義されます。 日付のみが指定されている場合、デフォルトはUTCの午前0時（00:00:00Z）になります。 有効期限&#x200B;**は、今後24時間以上である必要があります**。 <br>**メモ**:<ul><li>データセットの有効期限が既に存在する場合、リクエストは失敗します。</li></ul> |
| `displayName` | **必須。** データセットの有効期限の設定に使用できる、人間が読み取れる名前。 |
| `description` | データセットの有効期限の設定に関するオプションの説明。 |

**応答**

応答が成功すると、HTTP 201 （作成済み）ステータスと新しいデータセット有効期限の設定が返されます。

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
| `ttlId` | 作成されたデータセット有効期限の設定の一意のID。 |
| `datasetId` | データセットの一意のID。 |
| `datasetName` | データセットの名前。 |
| `sandboxName` | このデータセットの有効期限が設定されているサンドボックス。 |
| `displayName` | データセット有効期限の設定の表示名。 |
| `description` | データセットの有効期限の設定の説明。 |
| `imsOrg` | この設定に関連付けられている一意の組織ID。 |
| `status` | データセット有効期限の設定の現在のステータス。<br>いずれかの：`pending`、`executing`、`cancelled`、`completed`。 |
| `expiry` | データセットのスケジュールされた有効期限タイムスタンプ。 |
| `updatedAt` | 最新の更新のタイムスタンプ。 |
| `updatedBy` | データセットの有効期限の設定を最後に更新したユーザーまたはサービスの識別子と電子メール。 |

データセットの有効期限が既に存在する場合、400 （不正なリクエスト） HTTP ステータスが発生します。 404 （見つかりません） HTTP ステータスは、そのようなデータセットが存在しないか、データセットにアクセスできない場合に発生します。

## データセットの有効期限の設定を更新する {#update}

既存のデータセット有効期限の設定を更新するには、PUT リクエストを`/ttl/DATASET_EXPIRATION_ID`に行います。 設定の`displayName`、`description`および`expiry` フィールドのみを更新できます。 更新は、有効期限ステータスが`pending`の場合にのみ許可されます。

>[!NOTE]
>
>`expiry` フィールドには、日付（YYYY-MM-DD）または日時（YYYY-MM-DDTHH:MM:SSZ）を入力できます。 日付のみが指定された場合、システムはその日の午前0時UTC （00:00:00Z）を使用します。 有効期限&#x200B;**は、今後24時間以上である必要があります**。

**API 形式**

```http
PUT /ttl/{DATASET_EXPIRATION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_EXPIRATION_ID}` | データセット有効期限の設定の一意のID。 **メモ**：応答では`ttlId`と呼ばれます。 |

**リクエスト**

次のリクエストは、データセットの有効期限`SD-c1f902aa-57cb-412e-bb2b-c70b8e1a5f45`の有効期限、表示名、説明を更新します。

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
| `displayName` | （オプション）データセットの有効期限の設定に使用する、人間が読み取れる新しい名前。 |
| `description` | （オプション）データセットの有効期限の設定に関する新しい説明。 |
| `expiry` | （オプション） ISO 8601形式の新しい有効期限または日時。 日付のみが指定されている場合、デフォルトは午前0時（UTC）になります。 有効期限は&#x200B;**24時間以上**&#x200B;である必要があります。 |

>[!NOTE]
>
>これらのフィールドの少なくとも1つは、リクエストで指定する必要があります。

**応答**

応答が成功すると、HTTP ステータス 200 （OK）と更新されたデータセット有効期限の設定が返されます。

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
| `ttlId` | 更新されたデータセット有効期限の設定の一意のID。 |
| `datasetId` | データセットの一意のID。 |
| `datasetName` | データセットの名前。 |
| `sandboxName` | このデータセットの有効期限が設定されているサンドボックス。 |
| `displayName` | データセット有効期限の設定の表示名。 |
| `description` | データセットの有効期限の設定の説明。 |
| `imsOrg` | この設定に関連付けられている組織ID。 |
| `status` | データセット有効期限の設定の現在のステータス。<br>いずれかの：`pending`、`executing`、`cancelled`、`completed`。 |
| `expiry` | データセットのスケジュールされた有効期限タイムスタンプ。 |
| `updatedAt` | 最新の更新のタイムスタンプ。 |
| `updatedBy` | データセットの有効期限の設定を最後に更新したユーザーまたはサービスの識別子と電子メール。 |

{style="table-layout:auto"}

このようなデータセットの有効期限が存在しない場合、応答が失敗すると、404 （見つかりません） HTTP ステータスが返されます。

## データセット有効期限のキャンセル {#delete}

保留中のデータセットの有効期限の設定をキャンセルするには、`/ttl/{ID}`に対してDELETE リクエストを行います。

>[!NOTE]
>
>キャンセルできるのは、`pending` ステータスのデータセット有効期限のみです。 既に`executing`、`completed`または`cancelled`にある有効期限をキャンセルしようとすると、HTTP 400 （不正なリクエスト）が返されます。

**API 形式**

```http
DELETE /ttl/{ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{ID}` | データセット有効期限の設定の一意のID。 データセットの有効期限IDまたはデータセット IDを指定できます。 |

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

応答が成功すると、HTTP ステータス 200 （OK）とキャンセルされたデータセットの有効期限の設定が返されます。 有効期限の`status`属性が`cancelled`に設定されているわけではありません。

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
| `ttlId` | 削除されたデータセット有効期限の設定の一意のID。 |
| `datasetId` | データセットの一意のID。 |
| `datasetName` | データセットの名前。 |
| `sandboxName` | このデータセットの有効期限が設定されているサンドボックス。 |
| `displayName` | データセット有効期限の設定の表示名。 |
| `description` | データセットの有効期限の設定の説明。 |
| `imsOrg` | この設定に関連付けられている一意の組織ID。 |
| `status` | データセット有効期限の設定の現在のステータス。<br>いずれかの：`pending`、`executing`、`cancelled`、`completed`。 |
| `expiry` | データセットのスケジュールされた有効期限タイムスタンプ。 |
| `updatedAt` | 最新の更新のタイムスタンプ。 |
| `updatedBy` | データセットの有効期限の設定を最後に更新したユーザーまたはサービスの識別子と電子メール。 |

**400 （不正なリクエスト）応答の例**

有効期限が`executing`、`completed`、または`cancelled`の設定を持つデータセットをキャンセルしようとすると、400 エラーが発生します。

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
>既に`completed`または`cancelled`にあるデータセットの有効期限をキャンセルしようとすると、404 エラーが発生します。

## 付録

### 承認されたクエリパラメーター {#query-params}

次の表では、[データセット有効期限をリスト表示](#list)する際に使用できるクエリパラメーターを説明します。

>[!NOTE]
>
>`description`、`displayName`および`datasetName`のパラメーターには、すべてLIKE値で検索する機能が含まれています。 つまり、「Name1」という文字列を検索すると、「Name123」、「Name183」、「DisplayName1234」という名前のスケジュールされたデータセットの有効期限を見つけることができます。

| パラメーター | 説明 | 例 |
| --- | --- | --- |
| `author` | `author` クエリパラメーターを使用して、データセットの有効期限を最近更新したユーザーを検索します。 作成後に更新が行われていない場合は、有効期限の元の作成者と一致します。 このパラメーターは、`created_by` フィールドが検索文字列に対応する有効期限と一致します。<br>検索文字列が`LIKE`または`NOT LIKE`で始まる場合、残りはSQL検索パターンとして扱われます。 それ以外の場合は、検索文字列全体が、`created_by` フィールドのコンテンツ全体に完全に一致する必要があるリテラル文字列として扱われます。 | `author=LIKE %john%`, `author=John Q. Public` |
| `datasetId` | 特定のデータセットに適用する有効期限に一致します。 | `datasetId=62b3925ff20f8e1b990a7434` |
| `datasetName` | 指定された検索文字列を含むデータセット名の有効期限と一致します。 一致は大文字と小文字を区別しません。 | `datasetName=Acme` |
| `description` |   | `description=Handle expiration of Acme information through the end of 2024.` |
| `displayName` | 指定された検索文字列を含む表示名の有効期限と一致します。 一致は大文字と小文字を区別しません。 | `displayName=License Expiry` |
| `executedDate`／`executedFromDate`／`executedToDate` | 正確な実行日、実行終了日、実行開始日に基づいて結果をフィルタリングします。 特定の日付、特定の日付より前、または特定の日付より後の操作の実行に関連するデータまたはレコードを取得するために使用されます。 | `executedDate=2023-02-05T19:34:40.383615Z` |
| `expiryDate` | 指定した日付の24時間ウィンドウで発生した有効期限と一致します。 | `2024-01-01` |
| `expiryToDate` または `expiryFromDate` | 指定された期間内に実行される予定の、または既に実行された有効期限に一致します。 | `expiryFromDate=2099-01-01&expiryToDate=2100-01-01` |
| `limit` | 返される有効期限の最大数を示す 1～100 の整数。デフォルトは 25 です。 | `limit=50` |
| `orderBy` | `orderBy` クエリパラメーターは、APIによって返される結果の並べ替え順序を指定します。 ASC （昇順）またはDESC （降順）の1つ以上のフィールドに基づいてデータを配置する場合に使用します。 ASCとDESCをそれぞれ表すには、「+」または「 – 」プレフィックスを使用します。 次の値を使用できます：`displayName`、`description`、`datasetName`、`id`、`updatedBy`、`updatedAt`、`expiry`、`status`。 | `-datasetName` |
| `orgId` | 組織 ID がパラメーターと一致するデータセットの有効期限に一致します。この値はデフォルトで `x-gw-ims-org-id` ヘッダーの値となり、リクエストがサービストークンを提供しない限り無視されます。 | `orgId=885737B25DC460C50A49411B@AdobeOrg` |
| `page` | 返される有効期限のページを示す整数。 | `page=3` |
| `sandboxName` | サンドボックス名が引数と完全に一致するデータセット有効期限に一致します。デフォルトは、リクエストの `x-sandbox-name` ヘッダーにあるサンドボックス名です。`sandboxName=*` を使用して、すべてのサンドボックスからデータセット有効期限を含めます。 | `sandboxName=dev1` |
| `search` | 指定された文字列が有効期限IDと完全に一致するか、次のいずれかのフィールドに&#x200B;**contained**&#x200B;が含まれている場合、有効期限に一致します：<br><ul><li>作成者</li><li>表示名</li><li>description</li><li>表示名</li><li>データセット名</li></ul> | `search=TESTING` |
| `status` | ステータスのコンマ区切りリスト。含める場合、応答は、現在のステータスがリストに含まれるデータセットの有効期限に一致します。 | `status=pending,cancelled` |
| `ttlId` | 指定されたIDに有効期限リクエストを一致させます。 | `ttlID=SD-c8c75921-2416-4be7-9cfd-9ab01de66c5f` |
| `updatedDate` | 指定した日付の24時間ウィンドウで更新された有効期限と一致します。 | `2024-01-01` |
| `updatedToDate` または `updatedFromDate` | 指定された時間から24時間のウィンドウで更新された有効期限に一致します。<br><br>有効期限は、作成時、キャンセル時、実行時を含め、編集されるたびに更新されたと見なされます。 | `updatedDate=2022-01-01` |

{style="table-layout:auto"}

