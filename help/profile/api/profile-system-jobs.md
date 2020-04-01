---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: リアルタイム顧客プロファイルAPI開発ガイド
topic: guide
translation-type: tm+mt
source-git-commit: d0ccaa5511375253a2eca8f1235c2f953b734709

---


# プロファイルシステムジョブ（削除要求）

Adobe Experience Platformを使用すると、複数のソースからデータを取り込み、個々の顧客に対して堅牢なプロファイルを構築できます。 Platformに取り込まれたデータは、Data Lakeおよびリアルタイム顧客データストアにプロファイルされます。 不要になったデータやエラーで追加されたデータを削除するために、プロファイルストアからデータセットやバッチを削除する必要が生じる場合があります。 これには、「削除リクエスト」とも呼ばれるプロファイルシステムプロファイルジョブを作成するために、リアルタイム顧客ジョブAPIを使用する必要があります。このジョブは、必要に応じて変更、監視または削除することもできます。

>[!NOTE]
>Data Lakeからデータセットまたはバッチを削除する場合は、 [Catalog Serviceの概要を参照し](../../catalog/home.md) てください。

## はじめに

このガイドで使用されるAPIエンドポイントは、リアルタイム顧客プロファイルAPIの一部です。 先に進む前に、リアルタイム顧客 [プロファイルAPI開発ガイドを参照してください](getting-started.md)。 特に、プロファイル開発ガ [イドの](getting-started.md#getting-started) 「はじめに」の節には、関連トピックへのリンク、このドキュメントのサンプルAPI呼び出しの読み方のガイド、Experience Platform APIを正常に呼び出すために必要なヘッダーに関する重要な情報が含まれています。

## 表示の削除要求

削除リクエストは、長時間実行される非同期プロセスです。つまり、組織で複数の削除リクエストを一度に実行している可能性があります。 組織で現在実行中のすべての削除要求を表示するには、エンドポイントに対してGET要求を実行で `/system/jobs` きます。

また、オプションのクエリパラメーターを使用して、応答で返される削除要求のリストをフィルタリングすることもできます。 複数のパラメーターを使用する場合は、各パラメーターをアンパサンド(&amp;)で区切ります。

**API形式**

```http
GET /system/jobs
GET /system/jobs?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `start` | リクエストの作成時刻に従って、返された結果のページをオフセットします。 例: `start=4` |
| `limit` | 返す結果の数を制限する。 例: `limit=10` |
| `page` | リクエストの作成時刻に従って、特定のページの結果を返します。 例: `page=2` |
| `sort` | 特定のフィールドで結果を昇順(`asc`)または降順(`desc`)で並べ替えます。 結果の複数ページを返す場合、並べ替えパラメーターは機能しません。 例: `sort=batchId:asc` |

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/system/jobs \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

応答には、各削除リクエストのオブジェクトを持つ「子」配列が含まれ、そのリクエストの詳細が含まれます。

```json
{
  "_page": {
    "count": 100,
    "next": "K1JJRDpFaWc5QUwyZFgtMEpBQUFBQUFBQUFBPT0jUlQ6MSNUUkM6MiNGUEM6QWdFQUFBQVFBQWZBQUg0Ly9yL25PcmpmZndEZUR3QT0="
  },
  "children": [
    {
      "id": "9c2018e2-cd04-46a4-b38e-89ef7b1fcdf4",
      "imsOrgId": "{IMS_ORG}",
      "batchId": "8d075b5a178e48389126b9289dcfd0ac",
      "jobType": "DELETE",
      "status": "COMPLETED",
      "metrics": "{\"recordsProcessed\":5,\"timeTakenInSec\":1}",
      "createEpoch": 1559026134,
      "updateEpoch": 1559026137
    },
    {
      "id": "3f225e7e-ac8c-4904-b1d5-0ce79e03c2ec",
      "imsOrgId": "{IMS_ORG}",
      "dataSetId": "5c802d3cd83fc114b741c4b5",
      "jobType": "DELETE",
      "status": "PROCESSING",
      "metrics": "{\"recordsProcessed\":0,\"timeTakenInSec\":15}",
      "createEpoch": 1559025404,
      "updateEpoch": 1559025406
    }
  ]
}
```

| プロパティ | 説明 |
|---|---|
| _page.count | 要求の合計数。 この応答は領域のために切り捨てられました。 |
| _page.next | 検索結果の追加ページが存在する場合は、参照リクエストのID値を指定された「次 [の](#view-a-specific-delete-request) 」値に置き換えて、次の検索結果ページを表示します。 |
| jobType | 作成されるジョブのタイプ。 この場合、常に「DELETE」が返されます。 |
| status | 削除要求のステータス。 指定可能な値は、「NEW」、「PROCESSING」、「COMPLETED」、「ERROR」です。 |
| 指標 | 処理されたレコードの数(「recordsProcessed」)と要求の処理秒数、または要求の完了に要した時間(「timeTakedInSec」)を含むオブジェクト。 |

## 削除リクエストの作成 {#create-a-delete-request}

新しい削除リクエストの開始は、エンドポイントへのPOSTリクエストを通じて行われます。このエンドポイントでは、削除するデータセットまたはバッチのIDがリクエストの本文に表示されます。 `/systems/jobs`

### データセットの削除

データセットを削除するには、データセットIDをPOSTリクエストの本文に含める必要があります。 この操作により、特定のデータセットのALLデータが削除されます。 Experience Platformでは、レコードと時系列の両方のデータセットに基づいてデータセットを削除できます。スキーマ

>[!CAUTION]
> Experience Platform UIを使用してプロファイル対応のデータセットを削除しようとすると、データセットの取り込みは無効になりますが、APIを使用して削除リクエストが作成されるまで削除されません。 詳しくは、このドキュメントの付録 [を参照](#appendix) 。

**API形式**

```http
POST /system/jobs
```

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/system/jobs \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "dataSetId": "5c802d3cd83fc114b741c4b5"
      }'
```

| プロパティ | 説明 |
|---|---|
| dataSetId | **（必須）** 、削除するデータセットのID。 |

**応答**

成功した応答は、新しく作成された削除要求の詳細を返します。この詳細には、システムで生成された一意の読み取り専用IDが含まれます。 これは、リクエストを検索し、そのステータスを確認するために使用できます。 作成 `status` 時のリクエストは、処理が開始さ `"NEW"` れるまでです。 応答内 `dataSetId` のが、要求で送信されたのと `dataSetId` 一致する必要があります。

```json
{
    "id": "3f225e7e-ac8c-4904-b1d5-0ce79e03c2ec",
    "imsOrgId": "{IMS_ORG}",
    "dataSetId": "5c802d3cd83fc114b741c4b5",
    "jobType": "DELETE",
    "status": "NEW",
    "createEpoch": 1559025404,
    "updateEpoch": 1559025406
}
```

| プロパティ | 説明 |
|---|---|
| id | 削除要求の一意のシステム生成読み取り専用ID。 |
| dataSetId | POSTリクエストで指定された、データセットのID。 |

### バッチの削除

バッチを削除するには、バッチIDをPOSTリクエストの本文に含める必要があります。 レコードスキーマに基づくデータセットのバッチは削除できません。 時系列データに基づくデータセットのバッチのみがスキーマできます。

>[!NOTE]
> レコードのスキーマに基づいてデータセットのバッチを削除できないのは、レコードタイプのデータセットバッチが以前のレコードを上書きするので、「元に戻す」または「削除」できないからです。 レコードのスキーマに基づいてデータセットに対する誤ったバッチの影響を除去する唯一の方法は、誤ったレコードを上書きするために、バッチを正しいデータで再取り込みすることです。

レコードと時系列の動作の詳細については、「XDM System Overview」の「XDM [データの動作](../../xdm/home.md#data-behaviors) 」の節を参照してください。

**API形式**

```http
POST /system/jobs
```

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/system/jobs \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
       "batchId": "8d075b5a178e48389126b9289dcfd0ac"
      }'
```

| プロパティ | 説明 |
|---|---|
| batchId | **（必須）** 、削除するバッチのID。 |

**応答**

成功した応答は、新しく作成された削除要求の詳細を返します。この詳細には、システムで生成された一意の読み取り専用IDが含まれます。 これは、リクエストを検索し、そのステータスを確認するために使用できます。 作成時のリクエストの「ステータス」は、処理が開始されるまで「新規」です。 応答の「batchId」は、リクエストで送信される「batchId」と一致する必要があります。

```json
{
    "id": "9c2018e2-cd04-46a4-b38e-89ef7b1fcdf4",
    "imsOrgId": "{IMS_ORG}",
    "batchId": "8d075b5a178e48389126b9289dcfd0ac",
    "jobType": "DELETE",
    "status": "NEW",
    "createEpoch": 1559026131,
    "updateEpoch": 1559026132
}
```

| プロパティ | 説明 |
|---|---|
| id | 削除要求の一意のシステム生成読み取り専用ID。 |
| batchId | POSTリクエストで指定されたバッチのID。 |

Recordデータセットバッチの削除リクエストを開始しようとすると、次のような400レベルのエラーが発生します。

```json
{
    "requestId": "bc4eb29f-63a8-4653-9133-71238884bb81",
    "errors": {
        "400": [
            {
                "code": "500",
                "message": "Batch can only be specified for EE type 'a294e36d382649dab2cc6ad64a41b674'"
            }
        ]
    }
}
```

## 表示特定の削除要求 {#view-a-specific-delete-request}

特定の削除リクエスト（ステータスなどの詳細を含む）を表示するには、エンドポイントに対してルックアップ(GET)リクエストを実行し、削除リクエストのIDをパスに含め `/system/jobs` ることができます。

**API形式**

```http
GET /system/jobs/{DELETE_REQUEST_ID}
```

| パラメーター | 説明 |
|---|---|
| {DELETE_REQUEST_ID} | **（必須）** 表示する削除要求のID。 |

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/system/jobs/9c2018e2-cd04-46a4-b38e-89ef7b1fcdf4 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

応答には、更新されたステータスを含む、削除リクエストの詳細が表示されます。 応答内の削除要求のIDは、要求パスで送信されたIDと一致する必要があります。

```json
{
    "id": "9c2018e2-cd04-46a4-b38e-89ef7b1fcdf4",
    "imsOrgId": "{IMS_ORG}",
    "batchId": "8d075b5a178e48389126b9289dcfd0ac",
    "jobType": "DELETE",
    "status": "COMPLETED",
    "metrics": "{\"recordsProcessed\":5,\"timeTakenInSec\":1}",
    "createEpoch": 1559026134,
    "updateEpoch": 1559026137
}
```

| プロパティ | 説明 |
|---|---|
| jobType | 作成中のジョブのタイプ。この場合、常に「DELETE」が返されます。 |
| status | 削除要求のステータス。 可能な値：&quot;NEW&quot;、&quot;PROCESSING&quot;、&quot;COMPLETED&quot;、&quot;ERROR&quot;。 |
| 指標 | 処理されたレコードの数(「recordsProcessed」)と、リクエストの処理秒数、またはリクエストの完了に要した時間(「timeTakenInSec」)を含む配列。 |

削除要求のステータスが「完了」になったら、データアクセスAPIを使用して削除されたデータにアクセスしようとすることで、データが削除されたことを確認できます。 データアクセスAPIを使用してデータセットやバッチにアクセスする手順については、データアクセスのドキュメントを参 [照してください](../../data-access/home.md)。

## 削除要求の削除

エクスペリエンスプラットフォームでは、以前のリクエストを削除できます。削除ジョブが完了しなかったか、処理段階で停止したなど、様々な理由で役立つ場合があります。 削除リクエストを削除するには、エンドポイントに対してDELETEリクエストを実行し、削除する削除リクエストの `/system/jobs` IDをリクエストパスに含めます。

**API形式**

```http
DELETE /system/jobs/{DELETE_REQUEST_ID}
```

| パラメーター | 説明 |
|---|---|
| {DELETE_REQUEST_ID} | 削除する削除要求のID。 |

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/system/jobs/9c2018e2-cd04-46a4-b38e-89ef7b1fcdf4 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

削除要求が成功すると、HTTPステータス200(OK)と空の応答本文が返されます。 IDで削除要求を表示に対してGET要求を実行すると、要求が削除されたことを確認できます。 削除要求が削除されたことを示すHTTPステータス404 （見つかりません）が返されます。

## 次の手順

Experience Platform内のプロファイルストアからデータセットやバッチを削除する手順がわかったので、誤って追加されたデータや組織で不要になったデータを安全に削除できます。 削除リクエストは元に戻せないので、今は不要で将来は不要になると確信しているデータのみを削除するようにしてください。

## 付録 {#appendix}

次の情報は、データセットストアからデータセットを削除する操作の補足プロファイルです。

### Experience Platform UIを使用したデータセットの削除

Experience Platformユーザーインターフェイスを使用してプロファイルが有効になっているデータセットを削除すると、「このデータセットをExperience Data Lakeから削除しますか？ 「プロファイルシステムジョブ」 APIを使用して、このデータセットをプロファイルサービスから削除します。」

UIの「 **Delete** 」をクリックすると、取り込みのデータセットは無効になりますが、バックエンドのデータセットは自動的には削除されません。 データセットを完全に削除するには、削除リクエストを作成する手順を使用して、手動で作成する [必要があります](#create-a-delete-request)。

次の図は、UIを使用してプロファイル対応データセットを削除しようとした場合の警告を示しています。

![](../images/delete-profile-dataset.png)

データセットの操作の詳細については、まずデータセットの概要を読ん [でください](../../catalog/datasets/overview.md)。