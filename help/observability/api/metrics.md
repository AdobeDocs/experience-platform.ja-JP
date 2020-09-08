---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 使用可能な指標
topic: developer guide
translation-type: tm+mt
source-git-commit: c5455dc0812b251483170ac19506d7c60ad4ecaa
workflow-type: tm+mt
source-wordcount: '1174'
ht-degree: 72%

---


# 指標エンドポイント

観察性指標は、Adobe Experience Platformの様々な機能の使用状況の統計、過去の傾向、およびパフォーマンス指標に関する洞察を提供します。 の `/metrics` エンドポイント [!DNL Observability Insights API] を使用すると、で組織のアクティビティの指標データをプログラムで取得でき [!DNL Platform]ます。

## はじめに

The API endpoint used in this guide is part of the [[!DNL Observability Insights] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/observability-insights.yaml). 先に進む前に、 [はじめに](./getting-started.md)[!DNL Experience Platform] 、関連ドキュメントへのリンク、このドキュメントのサンプルAPI呼び出しを読むためのガイド、APIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報を確認してください。

## 観察性指標の取得

You can retrieve observability metrics by making a GET request to the `/metrics` endpoint in the [!DNL Observability Insights] API.

**API 形式**

`/metrics` エンドポイントを使用する場合は、1 つ以上の指標リクエストパラメーターを指定する必要があります。その他のクエリパラメーターはオプションで、結果をフィルタリングするためのものです。

```http
GET /metrics?metric={METRIC}
GET /metrics?metric={METRIC}&metric={METRIC_2}
GET /metrics?metric={METRIC}&id={ID}
GET /metrics?metric={METRIC}&dateRange={DATE_RANGE}
GET /metrics?metric={METRIC}&metric={METRIC_2}&id={ID}&dateRange={DATE_RANGE}
```

| パラメーター | 説明 |
| --- | --- |
| `{METRIC}` | 表示する指標。1 回の呼び出しで複数の指標を組み合わせる場合、アンパサンド（`&`）を使用して個々の指標を区切る必要があります。例：`metric={METRIC_1}&metric={METRIC_2}`。 |
| `{ID}` | The identifier for a particular [!DNL Platform] resource whose metrics you want to expose. この ID は、使用する指標に応じて、オプション/必須/該当しない場合があります。使用可能な指標のリスト、および各指標でサポートされるID（必須とオプションの両方）については、 [付録](#available-metrics) を参照してください。 |
| `{DATE_RANGE}` | ISO 8601 形式（例：`2018-10-01T07:00:00.000Z/2018-10-09T07:00:00.000Z`）で公開する指標の日付範囲。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/infrastructure/observability/insights/metrics?metric=timeseries.ingestion.dataset.size&metric=timeseries.ingestion.dataset.recordsuccess.count&id=5cf8ab4ec48aba145214abeb&dateRange=2018-10-01T07:00:00.000Z/2019-06-06T07:00:00.000Z \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、オブジェクトのリストを返します。各オブジェクトには、指定された `dateRange` 内のタイムスタンプと、リクエストパスで指定された指標に対応する値が含まれます。 リソースの `id` がリクエストパスに含まれる場合、結果はその特定のリソースにのみ適用されます。[!DNL Platform]`id` を省略すると、結果は IMS 組織内の該当するすべてのリソースに適用されます。

```json
{
    "id": "5cf8ab4ec48aba145214abeb",
    "imsOrgId": "{IMS_ORG}",
    "timeseries": {
        "granularity": "MONTH",
        "items": [
            {
                "timestamp": "2019-06-01T00:00:00Z",
                "metrics": {
                    "timeseries.ingestion.dataset.recordsuccess.count": 1125,
                    "timeseries.ingestion.dataset.size": 32320
                }
            },
            {
                "timestamp": "2019-05-01T00:00:00Z",
                "metrics": {
                    "timeseries.ingestion.dataset.recordsuccess.count": 1003,
                    "timeseries.ingestion.dataset.size": 31409
                }
            },
            {
                "timestamp": "2019-04-01T00:00:00Z",
                "metrics": {
                    "timeseries.ingestion.dataset.recordsuccess.count": 740,
                    "timeseries.ingestion.dataset.size": 25809
                }
            },
            {
                "timestamp": "2019-03-01T00:00:00Z",
                "metrics": {
                    "timeseries.ingestion.dataset.recordsuccess.count": 740,
                    "timeseries.ingestion.dataset.size": 25809
                }
            },
            {
                "timestamp": "2019-02-01T00:00:00Z",
                "metrics": {
                    "timeseries.ingestion.dataset.recordsuccess.count": 390,
                    "timeseries.ingestion.dataset.size": 16801
                }
            }
        ],
        "_page": null,
        "_links": null
    },
    "stats": {}
}
```

## 付録

次の節では、エンドポイントの操作に関する追加情報について説明し `/metrics` ます。

### 使用可能な指標 {#available-metrics}

The following tables list all of the metrics that are exposed by [!DNL Observability Insights], broken down by [!DNL Platform] service. 各指標には、説明と受け入れられた ID クエリーパラメーターが含まれます。

>[!NOTE]
>
>リストに表示されているIDクエリパラメーターは、別途記述しない限り、すべてオプションです。

#### [!DNL Data Ingestion] {#ingestion}

The following table outlines metrics for Adobe Experience Platform [!DNL Data Ingestion]. **太字**&#x200B;の指標はストリーミング取得指標です。

| インサイト指標 | 説明 | ID クエリーパラメーター |
| ---- | ---- | ---- |
| timeseries.ingestion.dataset.new.count | 作成されたデータセットの合計数。 | なし |
| timeseries.ingestion.dataset.size | 1 つまたはすべてのデータセット用に取得されたすべてのデータの累積サイズ。 | データセット ID |
| timeseries.ingestion.dataset.dailysize | 1 つのデータセットまたはすべてのデータセットに対して毎日の使用量ベースで取得されるデータのサイズ。 | データセット ID |
| timeseries.ingestion.dataset.batchfailed.count | 1 つのデータセットまたはすべてのデータセットで失敗したバッチの数。 | データセット ID |
| timeseries.ingestion.dataset.batchsuccess.count | 1 つのデータセットまたはすべてのデータセットに対して取得されたバッチの数。 | データセット ID |
| timeseries.ingestion.dataset.recordsuccess.count | 1 つのデータセットまたはすべてのデータセットに対して取得されたレコードの数。 | データセット ID |
| **timeseries.data.collection.validation.total.messages.rate** | 1 つのデータセットまたはすべてのデータセットのメッセージの合計数。 | データセット ID |
| **timeseries.data.collection.validation.valid.messages.rate** | 1 つのデータセットまたはすべてのデータセットに対する有効なメッセージの合計数。 | データセット ID |
| **timeseries.data.collection.validation.invalid.messages.rate** | 1 つのデータセットまたはすべてのデータセットに対する無効なメッセージの合計数。 | データセット ID |
| **timeseries.data.collection.validation.category.type.count** | 1 つのデータセットまたはすべてのデータセットに対する無効な「タイプ」メッセージの合計数。 | データセット ID |
| **timeseries.data.collection.validation.category.range.count** | 1 つのデータセットまたはすべてのデータセットに対する無効な「範囲」メッセージの合計数。 | データセット ID |
| **timeseries.data.collection.validation.category.format.count** | 1 つのデータセットまたはすべてのデータセットに対する無効な「形式」メッセージの合計数。 | データセット ID |
| **timeseries.data.collection.validation.category.pattern.count** | 1 つのデータセットまたはすべてのデータセットに対する無効な「パターン」メッセージの合計数。 | データセット ID |
| **timeseries.data.collection.validation.category.presence.count** | 1 つのデータセットまたはすべてのデータセットに対する無効な「存在」メッセージの合計数。 | データセット ID |
| **timeseries.data.collection.validation.category.enum.count** | 1 つのデータセットまたはすべてのデータセットに対する無効な「enum」メッセージの合計数。 | データセット ID |
| **timeseries.data.collection.validation.category.unclassified.count** | 1 つのデータセットまたはすべてのデータセットに対する無効な「未分類」メッセージの合計数。 | データセット ID |
| **timeseries.data.collection.validation.category.unknown.count** | 1 つのデータセットまたはすべてのデータセットに対する無効な「不明」メッセージの合計数。 | データセット ID |
| **timeseries.data.collection.inlet.total.messages.received** | 1 つのデータインレットまたはすべてのデータインレットに対して受信したメッセージの合計数。 | インレットID |
| **timeseries.data.collection.inlet.total.messages.size.received** | 1 つのデータインレットまたはすべてのデータインレットに対して受信したデータの合計サイズ。 | インレットID |
| **timeseries.data.collection.inlet.success** | 1 つのデータインレットまたはすべてのデータインレットに対する成功した HTTP 呼び出しの合計数。 | インレットID |
| **timeseries.data.collection.inlet.failure** | 1 つのデータインレットまたはすべてのデータインレットに対する失敗した HTTP 呼び出しの合計数。 | インレットID |

#### [!DNL Identity Service] {#identity}

The following table outlines metrics for Adobe Experience Platform [!DNL Identity Service].

| インサイト指標 | 説明 | ID クエリーパラメーター |
| ---- | ---- | ---- |
| timeseries.identity.dataset.recordsuccess.count | Number of records written to their data source by [!DNL Identity Service], for one dataset or all datasets. | データセット ID |
| timeseries.identity.dataset.recordfailed.count | Number of records failed by [!DNL Identity Service], for one dataset or for all datasets. | データセット ID |
| timeseries.identity.dataset.namespacecode.recordsuccess.count | ユーザーが正常に取得した ID レコードの名前空間数。 | 名前空間 ID（**必須**） |
| timeseries.identity.dataset.namespacecode.recordfailed.count | 名前空間が失敗した ID レコードの数。 | 名前空間 ID（**必須**） |
| timeseries.identity.dataset.namespacecode.recordskipped.count | 名前空間がスキップした ID レコードの数。 | 名前空間 ID（**必須**） |
| timeseries.identity.graph.imsorg.uniqueidentities.count | IMS 組織の ID グラフに保存される一意の ID の数。 | なし |
| timeseries.identity.graph.imsorg.namespacecode.uniqueidentities.count | 名前空間の ID グラフに保存される一意の ID の数。 | 名前空間 ID（**必須**） |
| timeseries.identity.graph.imsorg.numidgraphs.count | IMS 組織 の ID グラフに保存される一意のグラフ ID の数。 | なし |
| timeseries.identity.graph.imsorg.graphstrength.uniqueidentities.count | 特定のグラフの強さ（「不明」、「弱」または「強」）の IMS 組織の ID グラフに保存される一意の ID の数。 | グラフの強さ（**必須**） |

#### [!DNL Privacy Service] {#privacy}

The following table outlines metrics for Adobe Experience Platform [!DNL Privacy Service].

| インサイト指標 | 説明 | ID クエリーパラメーター |
| ---- | ---- | ---- |
| timeseries.gdpr.jobs.totaljobs.count | GDPR から作成されたジョブの合計数。 | ENV(必&#x200B;**須**) |
| timeseries.gdpr.jobs.completedjobs.count | GDPR から完了したジョブの合計数。 | ENV(必&#x200B;**須**) |
| timeseries.gdpr.jobs.errorjobs.count | GDPR からのエラージョブの合計数。 | ENV(必&#x200B;**須**) |

#### [!DNL Query Service] {#query}

The following table outlines metrics for Adobe Experience Platform [!DNL Query Service].

| インサイト指標 | 説明 | ID クエリーパラメーター |
| ---- | ---- | ---- |
| timeseries.queryservice.query.scheduleonce.count | 定期的でないスケジュール済みクエリの合計数。 | なし |
| timeseries.queryservice.query.scheduledrecurring.count | 定期的なスケジュール済みクエリの合計数。 | なし |
| timeseries.queryservice.query.batchquery.count | 実行されたバッチクエリの合計数。 | なし |
| timeseries.queryservice.query.scheduledquery.count | 実行されたスケジュール済みクエリの合計数。 | なし |
| timeseries.queryservice.query.interactivequery.count | 実行されたインタラクティブクエリの合計数。 | なし |
| timeseries.queryservice.query.batchfrompsqlquery.count | PSQL から実行されたバッチクエリの合計数。 | なし |

#### [!DNL Real-time Customer Profile] {#profile}

The following table outlines metrics for [!DNL Real-time Customer Profile].

| インサイト指標 | 説明 | ID クエリーパラメーター |
| ---- | ---- | ---- |
| timeseries.profiles.dataset.recordread.count | Number of records read from the [!DNL Data Lake] by [!DNL Profile], for one dataset or for all datasets. | データセット ID |
| timeseries.profiles.dataset.recordsuccess.count | Number of records written to their data source by [!DNL Profile], for one dataset or for all datasets. | データセット ID |
| timeseries.profiles.dataset.recordfailed.count | Number of records failed by [!DNL Profile], for one dataset or for all datasets. | データセット ID |
| timeseries.profiles.dataset.batchsuccess.count | Number of [!DNL Profile] batches ingested for a dataset or for all datasets. | データセット ID |
| timeseries.profiles.dataset.batchfailed.count | Number of [!DNL Profile] batches failed for one dataset or for all datasets. | データセット ID |
| platform.ups.ingest.streaming.request.m1_rate | 受信要求の速度。 | IMS 組織 (**必須**) |
| platform.ups.ingest.streaming.access.put.success.m1_rate | 取得成功率。 | IMS 組織 (**必須**) |
| platform.ups.ingest.streaming.records.created.m15_rate | データセットに対して取得された新しいレコードの割合。 | データセット ID (**必須**) |
| platform.ups.ingest.streaming.request.error.created.outOfOrder.m1_rate | データセットの作成要求のタイムスタンプが正しくないレコードの割合。 | データセット ID (**必須**) |
| platform.ups.profile-commons.ingest.streaming.dataSet.record.created.timestamp | データセットに対する最後のレコード作成要求のタイムスタンプ。 | データセット ID (**必須**) |
| platform.ups.ingest.streaming.request.error.updated.outOfOrder.m1_rate | データセットの更新要求のタイムスタンプが正しくないレコードの割合。 | データセット ID (**必須**) |
| platform.ups.profile-commons.ingest.streaming.dataSet.record.updated.timestamp | データセットに対する最後のレコード更新要求のタイムスタンプ。 | データセット ID (**必須**) |
| platform.ups.ingest.streaming.record.size.m1_rate | 平均レコードサイズ。 | IMS 組織 (**必須**) |
| platform.ups.ingest.streaming.records.updated.m15_rate | データセットに対して取得された更新要求の割合。 | データセット ID (**必須**) |