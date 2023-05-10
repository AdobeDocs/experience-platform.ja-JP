---
keywords: Experience Platform;プロファイル;リアルタイム顧客プロファイル;トラブルシューティング;API
title: プロファイルシステムジョブ API エンドポイント
type: Documentation
description: Adobe Experience Platformでは、不要になった、またはエラーで追加されたリアルタイム顧客プロファイルデータを削除するために、プロファイルストアからデータセットまたはバッチを削除できます。 これには、プロファイル API を使用してプロファイルシステムジョブを作成するか、削除リクエストを作成する必要があります。
exl-id: 75ddbf2f-9a54-424d-8569-d6737e9a590e
source-git-commit: 0f7ef438db5e7141197fb860a5814883d31ca545
workflow-type: tm+mt
source-wordcount: '1316'
ht-degree: 68%

---

# プロファイルシステムジョブエンドポイント（削除リクエスト）

Adobe Experience Platform を使用すると、複数のソースからデータを取得し、個々の顧客に対して堅牢なプロファイルを構築できます。に取り込まれたデータ [!DNL Platform] が [!DNL Data Lake]に保存され、データセットがプロファイルで有効になっている場合、そのデータは [!DNL Real-Time Customer Profile] データストアにも存在します。 不要になったデータや誤って追加されたデータを削除するために、プロファイルストアからデータセットやバッチを削除する必要が生じる場合があります。これには、 [!DNL Real-Time Customer Profile] を作成する API [!DNL Profile] システムジョブ `delete request`（必要に応じて変更、監視または削除も可能）

>[!NOTE]
>
>からデータセットまたはバッチを削除する場合 [!DNL Data Lake]、次を参照してください： [カタログサービスの概要](../../catalog/home.md) を参照してください。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Real-Time Customer Profile API]](https://www.adobe.com/go/profile-apis-en) の一部です。先に進む前に、[はじめる前に](getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## 削除リクエストの表示

削除リクエストは長時間実行される非同期プロセスです。つまり、組織が複数の削除リクエストを一度に実行している場合があります。組織で現在実行中のすべての削除リクエストを表示するには、`/system/jobs` エンドポイントに対して GET リクエストを実行できます。

また、オプションのクエリーパラメーターを使用して、応答で返される削除リクエストのリストをフィルタリングすることもできます。複数のパラメーターを使用する場合は、各パラメーターをアンパサンド (`&`) をクリックします。

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
| `_page.next` | 結果の追加ページが存在する場合は、 [ルックアップリクエスト](#view-a-specific-delete-request) と `"next"` 値を指定します。 |
| `jobType` | 作成されるジョブのタイプ。この場合、常にが返されます。 `"DELETE"`. |
| `status` | 削除リクエストのステータス。指定できる値は次のとおりです。 `"NEW"`, `"PROCESSING"`, `"COMPLETED"`, `"ERROR"`. |
| `metrics` | 処理されたレコード数 (`"recordsProcessed"`) と、リクエストが処理された秒数、またはリクエストが完了するまでに要した時間 (`"timeTakenInSec"`) をクリックします。 |

## 削除リクエストの作成 {#create-a-delete-request}

新しい削除リクエストの開始は、`/systems/jobs` エンドポイントへの POST リクエストを通じて行われます。このエンドポイントでは、削除するデータセットまたはバッチの ID がリクエストの本文に表示されます。 

### データセットの削除

プロファイルストアからデータセットを削除するには、データセット ID をPOSTリクエストの本文に含める必要があります。 この操作により、特定のデータセットのすべてのデータが削除されます。[!DNL Experience Platform] では、レコードスキーマと時系列スキーマの両方に基づいたデータセットを削除できます。

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
> レコードスキーマに基づくデータセットのバッチを削除できないのは、レコードタイプのデータセットバッチが以前のレコードを上書きするため、「取り消し」または削除できないためです。レコードスキーマに基づくデータセットの誤ったバッチの影響を取り除く唯一の方法は、誤ったレコードを上書きするために、正しいデータでバッチを再取り込みすることです。

レコードと時系列の動作の詳細については、 [XDM のデータ動作に関する節](../../xdm/home.md#data-behaviors) 内 [!DNL XDM System] 概要。

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

正常な応答は、新しく作成された削除リクエストの詳細を返します。この詳細には、システムで生成された一意の読み取り専用 ID が含まれます。これは、リクエストを検索し、そのステータスを確認するために使用できます。作成時のリクエストの `"status"` は、処理が開始されるまで `"NEW"` です。この `"batchId"` 応答の値が `"batchId"` リクエストで送信された値。

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

応答には、更新されたステータスを含む、削除リクエストの詳細が表示されます。応答の削除リクエストの ID ( `"id"` の値 ) は、リクエストパスで送信された ID と一致する必要があります。

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
| `jobType` | 作成中のジョブのタイプ。この場合、常に返されます `"DELETE"`. |
| `status` | 削除リクエストのステータス。可能な値： `"NEW"`, `"PROCESSING"`, `"COMPLETED"`, `"ERROR"`. |
| `metrics` | 処理されたレコード数 (`"recordsProcessed"`) と、リクエストが処理された秒数、またはリクエストが完了するまでに要した時間 (`"timeTakenInSec"`) をクリックします。 |

削除リクエストのステータスが `"COMPLETED"` データアクセス API を使用して削除されたデータにアクセスしようとすると、データが削除されたことを確認できます。 データアクセス API を使用してデータセットやバッチにアクセスする手順については、[データアクセスのドキュメント](../../data-access/home.md)を参照してください。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答** 

削除リクエストが成功すると、HTTP ステータス 200（OK）と空の応答本文が返されます。GET リクエストを実行して ID で削除リクエストを表示すると、リクエストが削除されたことを確認できます。削除リクエストが削除されたことを示す HTTP ステータス 404（不検知）が返されます。

## 次の手順

これで、からデータセットとバッチを削除する手順がわかりました。 [!DNL Profile Store] 範囲 [!DNL Experience Platform]を使用すると、誤って追加されたデータや組織で不要になったデータを安全に削除できます。 削除リクエストは元に戻せないので、今は不要で将来は不要になると確信しているデータのみを削除するようにしてください。
