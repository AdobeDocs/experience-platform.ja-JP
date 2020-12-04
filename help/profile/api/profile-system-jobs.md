---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
title: プロファイルシステムジョブ — リアルタイム顧客プロファイルAPI
topic: guide
translation-type: tm+mt
source-git-commit: 8c94d3631296c1c3cc97501ccf1a3ed995ec3cab
workflow-type: tm+mt
source-wordcount: '1420'
ht-degree: 66%

---


# プロファイルシステムジョブエンドポイント（削除要求）

Adobe Experience Platform を使用すると、複数のソースからデータを取得し、個々の顧客に対して堅牢なプロファイルを構築できます。に取り込ま [!DNL Platform] れたデータは、データストアと共 [!DNL Data Lake] に [!DNL Real-time Customer Profile] データストアに保存されます。 不要になった、または誤って追加されたデータを削除するために、プロファイルストアからデータセットまたはバッチを削除する必要が生じる場合があります。 This requires using the [!DNL Real-time Customer Profile] API to create a [!DNL Profile] system job, also known as a &quot;[!DNL delete request]&quot;, that can also be modified, monitored, or removed if required.

>[!NOTE]
>
>If you are trying to delete datasets or batches from the [!DNL Data Lake], please visit the [Catalog Service overview](../../catalog/home.md) for instructions.

## はじめに

The API endpoint used in this guide is part of the [[!DNL Real-time Customer Profile API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/real-time-customer-profile.yaml). 先に進む前に、 [はじめに](getting-started.md)[!DNL Experience Platform] 、関連ドキュメントへのリンク、このドキュメントのサンプルAPI呼び出しを読むためのガイド、APIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報を確認してください。

## 削除リクエストの表示

削除リクエストは長時間実行される非同期プロセスです。つまり、組織が複数の削除リクエストを一度に実行している場合があります。組織で現在実行中のすべての削除リクエストを表示するには、`/system/jobs` エンドポイントに対して GET リクエストを実行できます。

また、オプションのクエリーパラメーターを使用して、応答で返される削除リクエストのリストをフィルタリングすることもできます。複数のパラメーターを使用する場合は、各パラメーターをアンパサンド（&amp;）で区切ります。

**API 形式**

```http
GET /system/jobs
GET /system/jobs?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `start` | リクエストの作成時刻に従って、返された結果のページをオフセットします。例：*`start=4`* |
| `limit` | 返す結果の数を制限します。例：*`limit=10`* |
| `page` | リクエストの作成時刻に従って、特定のページの結果を返します。例: ***`page=2`*** |
| `sort` | 特定のフィールドで結果を昇順（*`asc`*）または降順（**`desc`**）で並べ替えます。結果の複数ページを返す場合、並べ替えパラメーターは機能しません。例：`sort=batchId:asc` |

**リクエスト**

```shell
curl -X GET \
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
| `_page.count` | リクエストの合計数。この応答はスペースを節約するために切り捨てられています。 |
| `_page.next` | If an additional page of results exists, view the next page of results by replacing the ID value in a [lookup request](#view-a-specific-delete-request) with the `"next"` value provided. |
| `jobType` | 作成されるジョブのタイプ。In this case, it will always return `"DELETE"`. |
| `status` | 削除リクエストのステータス。Possible values are `"NEW"`, `"PROCESSING"`, `"COMPLETED"`, `"ERROR"`. |
| `metrics` | An object that includes the number of records that have been processed (`"recordsProcessed"`) and the time in seconds that the request has been processing, or how long the request took to complete (`"timeTakenInSec"`). |

## 削除リクエストの作成 {#create-a-delete-request}

新しい削除リクエストの開始は、`/systems/jobs` エンドポイントへの POST リクエストを通じて行われます。このエンドポイントでは、削除するデータセットまたはバッチの ID がリクエストの本文に表示されます。 

### データセットの削除

データセットを削除するには、データセット ID を POST リクエストの本文に含める必要があります。この操作により、特定のデータセットのすべてのデータが削除されます。[!DNL Experience Platform] では、レコードスキーマと時系列スキーマの両方に基づいたデータセットを削除できます。

>[!CAUTION]
>
> When attempting to delete a [!DNL Profile]-enabled dataset using the [!DNL Experience Platform] UI, the dataset is disabled for ingestion but will not be deleted until a delete request is created using the API. 詳しくは、このドキュメントの[付録](#appendix)を参照してください。

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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "dataSetId": "5c802d3cd83fc114b741c4b5"
      }'
```

| プロパティ | 説明 |
|---|---|
| `dataSetId` | **（必須）**&#x200B;削除するデータセットの ID。 |

**応答** 

正常な応答は、新しく作成された削除リクエストの詳細を返します。この詳細には、システムで生成された一意の読み取り専用 ID が含まれます。これは、リクエストを検索し、そのステータスを確認するために使用できます。作成時のリクエストの **`status`** は、処理が開始されるまで *`"NEW"`* です。応答内の **`dataSetId`** は、リクエストで送信された ***`dataSetId`*** と一致する必要があります。

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
| `id` | システムによって生成された、削除リクエストの一意の読み取り専用 ID。 |
| `dataSetId` | POST リクエストで指定されたデータセットの ID。 |

### バッチの削除

バッチを削除するには、バッチ ID を POST リクエストの本文に含める必要があります。レコードスキーマに基づくデータセットのバッチは削除できません。時系列スキーマに基づくデータセットのバッチのみが削除できます。

>[!NOTE]
>
> レコードスキーマに基づくデータセットのバッチを削除できないのは、レコードタイプのデータセットバッチが以前のレコードを上書きするため、「取り消し」または削除できないためです。レコードのスキーマに基づいてデータセットに誤ったバッチが及ぼす影響を取り除く唯一の方法は、誤ったレコードを上書きするために、バッチを正しいデータで再取り込みすることです。

For more information on record and time series behavior, please review the [section on XDM data behaviors](../../xdm/home.md#data-behaviors) in the [!DNL XDM System] overview.

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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
       "batchId": "8d075b5a178e48389126b9289dcfd0ac"
      }'
```

| プロパティ | 説明 |
|---|---|
| `batchId` | **（必須）**&#x200B;削除するバッチの ID。 |

**応答** 

正常な応答は、新しく作成された削除リクエストの詳細を返します。この詳細には、システムで生成された一意の読み取り専用 ID が含まれます。これは、リクエストを検索し、そのステータスを確認するために使用できます。作成時のリクエストの `"status"` は、処理が開始されるまで `"NEW"` です。応答内の `"batchId"` は、リクエストで送信された `"batchId"` と一致する必要があります。

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
curl -X POST \
  https://platform.adobe.io/data/core/ups/system/jobs/9c2018e2-cd04-46a4-b38e-89ef7b1fcdf4 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答** 

応答には、更新されたステータスを含む、削除リクエストの詳細が表示されます。応答の削除リクエストの ID は、リクエストパスで送信された ID と一致する必要があります。

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
| `jobType` | The type of job being created, in this case it will always return `"DELETE"`. |
| `status` | 削除リクエストのステータス。Possible values: `"NEW"`, `"PROCESSING"`, `"COMPLETED"`, `"ERROR"`. |
| `metrics` | An array that includes the number of records that have been processed (`"recordsProcessed"`) and the time in seconds that the request has been processing, or how long the request took to complete (`"timeTakenInSec"`). |

Once the delete request status is `"COMPLETED"` you can confirm that the data has been deleted by attempting to access the deleted data using the Data Access API. データアクセス API を使用してデータセットやバッチにアクセスする手順については、[データアクセスのドキュメント](../../data-access/home.md)を参照してください。

## 削除リクエストの削除

[!DNL Experience Platform] では、以前のリクエストを削除できます。これは、削除ジョブが完了しなかった、処理段階で停止したなど、様々な理由で役立つ場合があります。削除リクエストを削除するには、`/system/jobs` エンドポイントに対して DELETE リクエストを実行し、削除する削除リクエストの ID をリクエストパスに含めます。

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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答** 

削除リクエストが成功すると、HTTP ステータス 200（OK）と空の応答本文が返されます。GET リクエストを実行して ID で削除リクエストを表示すると、リクエストが削除されたことを確認できます。削除リクエストが削除されたことを示す HTTP ステータス 404（不検知）が返されます。

## 次の手順

Now that you know the steps involved in deleting datasets and batches from the [!DNL Profile Store] within [!DNL Experience Platform], you can safely delete data that has been added erroneously or that your organization no longer needs. 削除リクエストは元に戻せないので、今は不要で将来は不要になると確信しているデータのみを削除するようにしてください。

## 付録 {#appendix}

The following information is supplemental to the act of deleting a dataset from the [!DNL Profile Store].

### Deleting a dataset using the [!DNL Experience Platform] UI

When using the [!DNL Experience Platform] user interface to delete a dataset that has been enabled for [!DNL Profile], a dialog opens asking, &quot;Are you sure you want to delete this dataset from the [!DNL Experience Data Lake]? &#39;p[!DNL rofile systems jobs]&#39; APIを使用して、このデータセットをから削除 [!DNL Profile Service]します。」

UI の「**[!UICONTROL 削除]**」をクリックすると、データセットの取得は無効になりますが、バックエンドのデータセットは自動的には削除されません。データセットを完全に削除するには、このガイドにある[削除リクエストの作成](#create-a-delete-request)手順を使用して、削除リクエストを手動で作成する必要があります。

The following image shows the warning when attempting to delete a [!DNL Profile]-enabled dataset using the UI.

![](../images/delete-profile-dataset.png)

データセットの操作の詳細については、まず「[データセットの概要](../../catalog/datasets/overview.md)」を読んでください。