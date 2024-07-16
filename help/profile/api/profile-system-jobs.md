---
keywords: Experience Platform;プロファイル;リアルタイム顧客プロファイル;トラブルシューティング;API
title: プロファイルシステムジョブ API エンドポイント
type: Documentation
description: Adobe Experience Platformを使用すると、プロファイルストアからデータセットやバッチを削除して、不要になった、または誤って追加されたリアルタイム顧客プロファイルデータを削除できます。 これには、プロファイル API を使用して、プロファイルシステムジョブまたは削除リクエストを作成する必要があります。
role: Developer
exl-id: 75ddbf2f-9a54-424d-8569-d6737e9a590e
source-git-commit: e52eb90b64ae9142e714a46017cfd14156c78f8b
workflow-type: tm+mt
source-wordcount: '1327'
ht-degree: 61%

---

# プロファイルシステムジョブエンドポイント（削除リクエスト）

Adobe Experience Platform を使用すると、複数のソースからデータを取得し、個々の顧客に対して堅牢なプロファイルを構築できます。[!DNL Platform] に取り込まれたデータは [!DNL Data Lake] に格納されます。データセットがプロファイルに対して有効になっている場合、そのデータは [!DNL Real-Time Customer Profile] データストアにも格納されます。 不要になったデータやエラーで追加されたデータを削除するには、プロファイルストアからデータセットに関連付けられたプロファイルデータを削除する必要が生じる場合があります。 これには、[!DNL Real-Time Customer Profile] API を使用して、必要に応じて変更、監視、削除できる [!DNL Profile] システムジョブ（`delete request`）を作成する必要があります。

>[!NOTE]
>
>[!DNL Data Lake] からデータセットまたはバッチを削除しようとしている場合は、[ カタログサービスの概要 ](../../catalog/home.md) にアクセスして詳細を確認してください。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Real-Time Customer Profile API]](https://www.adobe.com/go/profile-apis-en) の一部です。先に進む前に、[はじめる前に](getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## 削除リクエストの表示

削除リクエストは長時間実行される非同期プロセスです。つまり、組織が複数の削除リクエストを一度に実行している場合があります。組織で現在実行中のすべての削除リクエストを表示するには、`/system/jobs` エンドポイントに対して GET リクエストを実行できます。

また、オプションのクエリーパラメーターを使用して、応答で返される削除リクエストのリストをフィルタリングすることもできます。複数のパラメーターを使用するには、アンパサンド（`&`）を使用して各パラメーターを区切ります。

**API 形式**

```http
GET /system/jobs
GET /system/jobs?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `start` | リクエストの作成時刻に従って、返された結果のページをオフセットします。例：`start=4` |
| `limit` | 返す結果の数を制限します。例：`limit=10` |
| `page` | リクエストの作成時刻に従って、特定のページの結果を返します。例：`page=2` |
| `sort` | 特定のフィールドで結果を昇順（`asc`）または降順（`desc`）で並べ替えます。結果の複数ページを返す場合、並べ替えパラメーターは機能しません。例：`sort=batchId:asc` |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/system/jobs \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
      "imsOrgId": "{ORG_ID}",
      "batchId": "8d075b5a178e48389126b9289dcfd0ac",
      "jobType": "DELETE",
      "status": "COMPLETED",
      "metrics": "{\"recordsProcessed\":5,\"timeTakenInSec\":1}",
      "createEpoch": 1559026134,
      "updateEpoch": 1559026137
    },
    {
      "id": "3f225e7e-ac8c-4904-b1d5-0ce79e03c2ec",
      "imsOrgId": "{ORG_ID}",
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
| `_page.count` | リクエストの合計数。この応答はスペースを節約するために切り捨てられています。 |
| `_page.next` | 結果の追加のページが存在する場合は、[ 参照リクエスト ](#view-a-specific-delete-request) の ID 値を指定された `"next"` 値に置き換えて、結果の次のページを表示します。 |
| `jobType` | 作成されるジョブのタイプ。この場合、常に `"DELETE"` が返されます。 |
| `status` | 削除リクエストのステータス。使用可能な値は、`"NEW"`、`"PROCESSING"`、`"COMPLETED"`、`"ERROR"` です。 |
| `metrics` | 処理されたレコードの数（`"recordsProcessed"`）、リクエストが処理された時間（秒）、またはリクエストが完了するまでにかかった時間（`"timeTakenInSec"`）を含むオブジェクト。 |

## 削除リクエストの作成 {#create-a-delete-request}

新しい削除リクエストの開始は、`/systems/jobs` エンドポイントへの POST リクエストを通じて行われます。このエンドポイントでは、削除するデータセットまたはバッチの ID がリクエストの本文に表示されます。 

### データセットおよび関連するプロファイルデータの削除

データセットとそれに関連付けられたすべてのプロファイルデータをプロファイルストアから削除するには、データセット ID をPOSTリクエストの本文に含める必要があります。 この操作により、特定のデータセットのすべてのデータが削除されます。レコ [!DNL Experience Platform] ドと時系列の両方のスキーマに基づいてデータセットを削除できます。

**API 形式**

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "dataSetId": "5c802d3cd83fc114b741c4b5"
      }'
```

| プロパティ | 説明 |
|---|---|
| `dataSetId` | **（必須）**&#x200B;削除するデータセットの ID。 |

**応答** 

正常な応答は、新しく作成された削除リクエストの詳細を返します。この詳細には、システムで生成された一意の読み取り専用 ID が含まれます。これは、リクエストを検索し、そのステータスを確認するために使用できます。作成時のリクエストの `status` は、処理が開始されるまで `"NEW"` です。応答内の `dataSetId` は、リクエストで送信された `dataSetId` と一致する必要があります。

```json
{
    "id": "3f225e7e-ac8c-4904-b1d5-0ce79e03c2ec",
    "imsOrgId": "{ORG_ID}",
    "dataSetId": "5c802d3cd83fc114b741c4b5",
    "jobType": "DELETE",
    "status": "NEW",
    "createEpoch": 1559025404,
    "updateEpoch": 1559025406
}
```

| プロパティ | 説明 |
|---|---|
| `id` | システムによって生成された、削除リクエストの一意の読み取り専用 ID。 |
| `dataSetId` | POST リクエストで指定されたデータセットの ID。 |

### バッチの削除

バッチを削除するには、バッチ ID を POST リクエストの本文に含める必要があります。レコードスキーマに基づくデータセットのバッチは削除できません。時系列スキーマに基づくデータセットのバッチのみが削除できます。

>[!NOTE]
>
> レコードスキーマに基づくデータセットのバッチを削除できないのは、レコードタイプのデータセットバッチが以前のレコードを上書きするため、「取り消し」または削除できないためです。レコードスキーマに基づくデータセットに対して誤ったバッチの影響を削除する唯一の方法は、間違ったレコードを上書きするために、正しいデータでバッチを再度取り込むことです。

レコードと時系列の動作について詳しくは、[!DNL XDM System] の概要の [XDM データの動作に関する節 ](../../xdm/home.md#data-behaviors) を参照してください。

**API 形式**

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
       "batchId": "8d075b5a178e48389126b9289dcfd0ac"
      }'
```

| プロパティ | 説明 |
|---|---|
| `batchId` | **（必須）**&#x200B;削除するバッチの ID。 |

**応答** 

正常な応答は、新しく作成された削除リクエストの詳細を返します。この詳細には、システムで生成された一意の読み取り専用 ID が含まれます。これは、リクエストを検索し、そのステータスを確認するために使用できます。作成時のリクエストの `"status"` は、処理が開始されるまで `"NEW"` です。応答の `"batchId"` 値は、リクエストで送信された `"batchId"` 値と一致する必要があります。

```json
{
    "id": "9c2018e2-cd04-46a4-b38e-89ef7b1fcdf4",
    "imsOrgId": "{ORG_ID}",
    "batchId": "8d075b5a178e48389126b9289dcfd0ac",
    "jobType": "DELETE",
    "status": "NEW",
    "createEpoch": 1559026131,
    "updateEpoch": 1559026132
}
```

| プロパティ | 説明 |
|---|---|
| `id` | システムによって生成された、削除リクエストの一意の読み取り専用 ID。 |
| `batchId` | POST リクエストで指定されたバッチの ID。 |

レコードデータセットバッチの削除リクエストを開始しようとすると、次のような 400 レベルのエラーが発生します。

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

## 特定の削除リクエストの表示 {#view-a-specific-delete-request}

特定の削除リクエスト（ステータスなどの詳細を含む）を表示するには、`/system/jobs` エンドポイントに対してルックアップ（GET）リクエストを実行し、削除リクエストの ID をパスに含めることができます。

**API 形式**

```http
GET /system/jobs/{DELETE_REQUEST_ID}
```

| パラメーター | 説明 |
|---|---|
| `{DELETE_REQUEST_ID}` | **（必須）**&#x200B;表示する削除リクエストの ID。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/system/jobs/9c2018e2-cd04-46a4-b38e-89ef7b1fcdf4 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答** 

応答には、更新されたステータスを含む、削除リクエストの詳細が表示されます。応答内の削除リクエストの ID （`"id"` 値）は、リクエストパスで送信された ID と一致する必要があります。

```json
{
    "id": "9c2018e2-cd04-46a4-b38e-89ef7b1fcdf4",
    "imsOrgId": "{ORG_ID}",
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
| `jobType` | 作成されるジョブのタイプ。この場合、常に `"DELETE"` を返します。 |
| `status` | 削除リクエストのステータス。使用可能な値：`"NEW"`、`"PROCESSING"`、`"COMPLETED"`、`"ERROR"`。 |
| `metrics` | 処理されたレコードの数（`"recordsProcessed"`）、リクエストが処理された時間（秒）、またはリクエストが完了するまでにかかった時間（`"timeTakenInSec"`）を含む配列。 |

削除リクエストステータスが `"COMPLETED"` になると、Data Access API を使用して削除されたデータにアクセスすることで、データが削除されたことを確認できます。 データアクセス API を使用してデータセットやバッチにアクセスする手順については、[データアクセスのドキュメント](../../data-access/home.md)を参照してください。

## 削除リクエストの削除

以前のリクエストを削除で [!DNL Experience Platform] ます。削除ジョブが完了しなかった場合や、処理ステージで停止した場合など、様々な理由で役立つ場合があります。 削除リクエストを削除するには、`/system/jobs` エンドポイントに対して DELETE リクエストを実行し、削除する削除リクエストの ID をリクエストパスに含めます。

**API 形式**

```http
DELETE /system/jobs/{DELETE_REQUEST_ID}
```

| パラメーター | 説明 |
|---|---|
| {DELETE_REQUEST_ID} | 削除する削除リクエストの ID。 |

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/system/jobs/9c2018e2-cd04-46a4-b38e-89ef7b1fcdf4 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答** 

削除リクエストが成功すると、HTTP ステータス 200（OK）と空の応答本文が返されます。GET リクエストを実行して ID で削除リクエストを表示すると、リクエストが削除されたことを確認できます。削除リクエストが削除されたことを示す HTTP ステータス 404（不検知）が返されます。

## 次の手順

[!DNL Experience Platform] 内の [!DNL Profile store] からデータセットとバッチを削除する手順がわかったので、誤って追加されたデータや、組織が必要としなくなったデータを安全に削除できます。 削除リクエストは元に戻せないので、今は不要で将来は不要になると確信しているデータのみを削除するようにしてください。
