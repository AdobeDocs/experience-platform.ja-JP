---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: 指標APIエンドポイント
topic-legacy: developer guide
description: 観察性インサイトAPIを使用して、Experience Platformの観察性指標を取得する方法を説明します。
exl-id: 08d416f0-305a-44e2-a2b7-d563b2bdd2d2
source-git-commit: 8133804076b1c0adf2eae5b748e86a35f3186d14
workflow-type: tm+mt
source-wordcount: '2050'
ht-degree: 45%

---

# 指標エンドポイント

観察性指標は、Adobe Experience Platformの様々な機能の使用状況統計、過去の傾向、パフォーマンス指標に関するインサイトを提供します。 [!DNL Observability Insights API]の`/metrics`エンドポイントを使用すると、[!DNL Platform]内の組織のアクティビティの指標データをプログラムで取得できます。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Observability Insights] API](https://www.adobe.io/experience-platform-apis/references/observability-insights/) の一部です。先に進む前に、[はじめる前に](./getting-started.md)のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の [!DNL Experience Platform] API の呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## 観察性指標の取得

APIを使用して指標データを取得する方法は、次の2つがサポートされています。

* [バージョン1](#v1):クエリパラメーターを使用して指標を指定します。
* [バージョン2](#v2):JSONペイロードを使用して指標に対してフィルターを指定し、適用する。

### バージョン 1 {#v1}

`/metrics`エンドポイントにGETリクエストを送信し、クエリパラメーターを使用して指標を指定することで、指標データを取得できます。

**API 形式**

`metric`パラメーターには、少なくとも1つの指標を指定する必要があります。 その他のクエリパラメーターはオプションで、結果をフィルタリングするためのものです。

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
| `{ID}` | 指標を表示する特定の[!DNL Platform]リソースの識別子。 この ID は、使用する指標に応じて、オプション/必須/該当しない場合があります。使用可能な指標のリストについては、[付録](#available-metrics)を参照してください。各指標でサポートされるID（必須とオプションの両方）も含まれます。 |
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

### バージョン 2 {#v2}

`/metrics`エンドポイントにPOSTリクエストを送信し、ペイロードで取得する指標を指定することで、指標データを取得できます。

**API 形式**

```http
POST /metrics
```

**リクエスト**

```sh
curl -X POST \
  https://platform.adobe.io/data/infrastructure/observability/insights/metrics \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "start": "2020-07-14T00:00:00.000Z",
        "end": "2020-07-22T00:00:00.000Z",
        "granularity": "day",
        "metrics": [
          {
            "name": "timeseries.ingestion.dataset.recordsuccess.count",
            "filters": [
              {
                "name": "dataSetId",
                "value": "5edcfb2fbb642119194c7d94|5eddb21420f516191b7a8dad",
                "groupBy": true
              }
            ],
            "aggregator": "sum",
            "downsample": "sum"
          },
          {
            "name": "timeseries.ingestion.dataset.dailysize",
            "filters": [
              {
                "name": "dataSetId",
                "value": "5eddb21420f516191b7a8dad",
                "groupBy": false
              }
            ],
            "aggregator": "sum",
            "downsample": "sum"
          }
        ]
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `start` | 指標データを取得する最も早い日時。 |
| `end` | 指標データを取得する最新の日時。 |
| `granularity` | 指標データを除算する時間間隔を示すオプションのフィールド。 例えば、値`DAY`は`start`から`end`の間の各日の指標を返しますが、値`MONTH`は指標の結果を月別にグループ化します。 このフィールドを使用する場合、データをグループ化する集計機能を示す`downsample`プロパティも指定する必要があります。 |
| `metrics` | オブジェクトの配列。取得する各指標に対して1つ。 |
| `name` | 観察性インサイトで認識される指標の名前。 使用可能な指標名の完全なリストについては、[付録](#available-metrics)を参照してください。 |
| `filters` | 特定のデータセットで指標をフィルタリングできるオプションのフィールド。 フィールドはオブジェクトの配列（各フィルターに対して1つ）で、各オブジェクトには次のプロパティが含まれます。 <ul><li>`name`:指標をフィルターするエンティティのタイプ。現在は、`dataSets` のみがサポートされています。</li><li>`value`:1つ以上のデータセットのID。複数のデータセットIDを1つの文字列として指定でき、各IDを縦棒文字(`|`)で区切ります。</li><li>`groupBy`:trueに設定した場合、は、対応するが、指標の結果を別 `value` 々に返す必要がある複数のデータセットを表すことを示します。falseに設定した場合、これらのデータセットの指標の結果はグループ化されます。</li></ul> |
| `aggregator` | 複数の時系列レコードを1つの結果にグループ化する場合に使用する集計関数を指定します。 利用可能な集積の詳細については、[OpenTSDBのドキュメント](http://opentsdb.net/docs/build/html/user_guide/query/aggregators.html)を参照してください。 |
| `downsample` | フィールドを間隔（「グループ」）に並べ替えることで、集計関数を指定して、指標データのサンプリング率を減らすことができるオプションのフィールドです。 ダウンサンプリングの間隔は`granularity`プロパティで決まります。 ダウンサンプリングについて詳しくは、[OpenTSDBのドキュメント](http://opentsdb.net/docs/build/html/user_guide/query/downsampling.html)を参照してください。 |

{style=&quot;table-layout:auto&quot;}

**応答**

リクエストが成功した場合は、リクエストで指定された指標およびフィルターの結果のデータポイントが返されます。

```json
{
  "metricResponses": [
    {
      "metric": "timeseries.ingestion.dataset.recordsuccess.count",
      "filters": [
        {
          "name": "dataSetId",
          "value": "5edcfb2fbb642119194c7d94|5eddb21420f516191b7a8dad",
          "groupBy": true
        }
      ],
      "datapoints": [
        {
          "groupBy": {
            "dataSetId": "5edcfb2fbb642119194c7d94"
          },
          "dps": {
            "2020-07-14T00:00:00Z": 44.0,
            "2020-07-15T00:00:00Z": 46.0,
            "2020-07-16T00:00:00Z": 36.0,
            "2020-07-17T00:00:00Z": 50.0,
            "2020-07-18T00:00:00Z": 38.0,
            "2020-07-19T00:00:00Z": 40.0,
            "2020-07-20T00:00:00Z": 42.0,
            "2020-07-21T00:00:00Z": 42.0,
            "2020-07-22T00:00:00Z": 50.0
          }
        },
        {
          "groupBy": {
            "dataSetId": "5eddb21420f516191b7a8dad"
          },
          "dps": {
            "2020-07-14T00:00:00Z": 44.0,
            "2020-07-15T00:00:00Z": 46.0,
            "2020-07-16T00:00:00Z": 36.0,
            "2020-07-17T00:00:00Z": 50.0,
            "2020-07-18T00:00:00Z": 38.0,
            "2020-07-19T00:00:00Z": 40.0,
            "2020-07-20T00:00:00Z": 42.0,
            "2020-07-21T00:00:00Z": 42.0,
            "2020-07-22T00:00:00Z": 50.0
          }
        }
      ],
      "granularity": "DAY"
    },
    {
      "metric": "timeseries.ingestion.dataset.dailysize",
      "filters": [
        {
          "name": "dataSetId",
          "value": "5eddb21420f516191b7a8dad",
          "groupBy": false
        }
      ],
      "datapoints": [
        {
          "groupBy": {},
          "dps": {
            "2020-07-14T00:00:00Z": 38455.0,
            "2020-07-15T00:00:00Z": 40213.0,
            "2020-07-16T00:00:00Z": 31476.0,
            "2020-07-17T00:00:00Z": 43705.0,
            "2020-07-18T00:00:00Z": 33227.0,
            "2020-07-19T00:00:00Z": 34977.0,
            "2020-07-20T00:00:00Z": 36735.0,
            "2020-07-21T00:00:00Z": 36737.0,
            "2020-07-22T00:00:00Z": 43715.0
          }
        }
      ],
      "granularity": "DAY"
    }
  ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `metricResponses` | リクエストで指定された各指標を表すオブジェクトを持つ配列。 各オブジェクトには、フィルター設定と返される指標データに関する情報が含まれます。 |
| `metric` | リクエストで提供される指標の1つの名前。 |
| `filters` | 指定した指標のフィルター設定。 |
| `datapoints` | 指定した指標とフィルターの結果を表すオブジェクトを持つ配列。 配列内のオブジェクトの数は、リクエストで指定したフィルターオプションによって異なります。 フィルターが指定されなかった場合、配列にはすべてのデータセットを表す1つのオブジェクトのみが含まれます。 |
| `groupBy` | 1つの指標の`filter`プロパティで複数のデータセットが指定され、リクエストで`groupBy`オプションがtrueに設定された場合、このオブジェクトには、対応する`dps`プロパティが適用されるデータセットのIDが含まれます。<br><br>応答でこのオブジェクトが空の場合、対応するプロパティ `dps` は、配列で指定されたすべてのデータセット（またはフィルターが指定されていない場合はのすべてのデータセット）に適用されま `filters`  [!DNL Platform] す。 |
| `dps` | 指定された指標、フィルターおよび時間範囲の返されたデータ。 このオブジェクトの各キーは、指定した指標に対応する値を持つタイムスタンプを表します。 各データポイント間の期間は、リクエストで指定された`granularity`値に応じて異なります。 |

{style=&quot;table-layout:auto&quot;}

## 付録

次の節では、`/metrics`エンドポイントの使用に関する追加情報を示します。

### 使用可能な指標 {#available-metrics}

次の表に、[!DNL Observability Insights]によって公開されるすべての指標を[!DNL Platform]サービス別に示します。 各指標には、説明と受け入れられた ID クエリーパラメーターが含まれます。

>[!NOTE]
>
>特に指定のない限り、リストに表示されるIDクエリパラメーターはすべてオプションです。

#### [!DNL Data Ingestion] {#ingestion}

次の表に、Adobe Experience Platform [!DNL Data Ingestion]の指標の概要を示します。 **太字**&#x200B;の指標はストリーミング取得指標です。

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

{style=&quot;table-layout:auto&quot;}

#### [!DNL Identity Service] {#identity}

次の表に、Adobe Experience Platform [!DNL Identity Service]の指標の概要を示します。

| インサイト指標 | 説明 | ID クエリーパラメーター |
| ---- | ---- | ---- |
| timeseries.identity.dataset.recordsuccess.count | 1つのデータセットまたはすべてのデータセットに対して、[!DNL Identity Service]によってデータソースに書き込まれたレコードの数。 | データセット ID |
| timeseries.identity.dataset.recordfailed.count | 1つのデータセットまたはすべてのデータセットに対して、[!DNL Identity Service]が失敗したレコードの数。 | データセット ID |
| timeseries.identity.dataset.namespacecode.recordsuccess.count | ユーザーが正常に取得した ID レコードの名前空間数。 | 名前空間 ID（**必須**） |
| timeseries.identity.dataset.namespacecode.recordfailed.count | 名前空間が失敗した ID レコードの数。 | 名前空間 ID（**必須**） |
| timeseries.identity.dataset.namespacecode.recordskipped.count | 名前空間がスキップした ID レコードの数。 | 名前空間 ID（**必須**） |
| timeseries.identity.graph.imsorg.uniqueidentities.count | IMS 組織の ID グラフに保存される一意の ID の数。 | なし |
| timeseries.identity.graph.imsorg.namespacecode.uniqueidentities.count | 名前空間の ID グラフに保存される一意の ID の数。 | 名前空間 ID（**必須**） |
| timeseries.identity.graph.imsorg.numidgraphs.count | IMS 組織 の ID グラフに保存される一意のグラフ ID の数。 | なし |
| timeseries.identity.graph.imsorg.graphstrength.uniqueidentities.count | 特定のグラフの強さ（「不明」、「弱」または「強」）の IMS 組織の ID グラフに保存される一意の ID の数。 | グラフの強さ（**必須**） |

{style=&quot;table-layout:auto&quot;}

#### [!DNL Privacy Service] {#privacy}

次の表に、Adobe Experience Platform [!DNL Privacy Service]の指標の概要を示します。

| インサイト指標 | 説明 | ID クエリーパラメーター |
| ---- | ---- | ---- |
| timeseries.gdpr.jobs.totaljobs.count | GDPR から作成されたジョブの合計数。 | ENV(必&#x200B;**須**) |
| timeseries.gdpr.jobs.completedjobs.count | GDPR から完了したジョブの合計数。 | ENV(必&#x200B;**須**) |
| timeseries.gdpr.jobs.errorjobs.count | GDPR からのエラージョブの合計数。 | ENV(必&#x200B;**須**) |

{style=&quot;table-layout:auto&quot;}

#### [!DNL Query Service] {#query}

次の表に、Adobe Experience Platform [!DNL Query Service]の指標の概要を示します。

| インサイト指標 | 説明 | ID クエリーパラメーター |
| ---- | ---- | ---- |
| timeseries.queryservice.query.scheduleonce.count | 定期的でないスケジュール済みクエリの合計数。 | なし |
| timeseries.queryservice.query.scheduledrecurring.count | 定期的なスケジュール済みクエリの合計数。 | なし |
| timeseries.queryservice.query.batchquery.count | 実行されたバッチクエリの合計数。 | なし |
| timeseries.queryservice.query.scheduledquery.count | 実行されたスケジュール済みクエリの合計数。 | なし |
| timeseries.queryservice.query.interactivequery.count | 実行されたインタラクティブクエリの合計数。 | なし |
| timeseries.queryservice.query.batchfrompsqlquery.count | PSQL から実行されたバッチクエリの合計数。 | なし |

{style=&quot;table-layout:auto&quot;}

#### [!DNL Real-time Customer Profile] {#profile}

次の表に、[!DNL Real-time Customer Profile]の指標の概要を示します。

| インサイト指標 | 説明 | ID クエリーパラメーター |
| ---- | ---- | ---- |
| timeseries.profiles.dataset.recordread.count | 1つのデータセット、またはすべてのデータセットに対して、[!DNL Data Lake]から[!DNL Profile]によって読み取られたレコードの数。 | データセット ID |
| timeseries.profiles.dataset.recordsuccess.count | 1つのデータセットまたはすべてのデータセットに対して、[!DNL Profile]によってデータソースに書き込まれたレコードの数。 | データセット ID |
| timeseries.profiles.dataset.recordfailed.count | 1つのデータセットまたはすべてのデータセットに対して、[!DNL Profile]が失敗したレコードの数。 | データセット ID |
| timeseries.profiles.dataset.batchsuccess.count | 1つのデータセットまたはすべてのデータセットに対して取り込まれた[!DNL Profile]バッチの数。 | データセット ID |
| timeseries.profiles.dataset.batchfailed.count | 1つのデータセットまたはすべてのデータセットで失敗した[!DNL Profile]バッチの数。 | データセット ID |
| platform.ups.ingest.streaming.request.m1_rate | 受信要求の速度。 | IMS 組織 (**必須**) |
| platform.ups.ingest.streaming.access.put.success.m1_rate | 取得成功率。 | IMS 組織 (**必須**) |
| platform.ups.ingest.streaming.records.created.m15_rate | データセットに対して取得された新しいレコードの割合。 | データセット ID (**必須**) |
| platform.ups.ingest.streaming.request.error.created.outOfOrder.m1_rate | データセットの作成要求のタイムスタンプが正しくないレコードの割合。 | データセット ID (**必須**) |
| platform.ups.profile-commons.ingest.streaming.dataSet.record.created.timestamp | データセットに対する最後のレコード作成要求のタイムスタンプ。 | データセット ID (**必須**) |
| platform.ups.ingest.streaming.request.error.updated.outOfOrder.m1_rate | データセットの更新要求のタイムスタンプが正しくないレコードの割合。 | データセット ID (**必須**) |
| platform.ups.profile-commons.ingest.streaming.dataSet.record.updated.timestamp | データセットに対する最後のレコード更新要求のタイムスタンプ。 | データセット ID (**必須**) |
| platform.ups.ingest.streaming.record.size.m1_rate | 平均レコードサイズ。 | IMS 組織 (**必須**) |
| platform.ups.ingest.streaming.records.updated.m15_rate | データセットに対して取得された更新要求の割合。 | データセット ID (**必須**) |

{style=&quot;table-layout:auto&quot;}

### エラーメッセージ

`/metrics`エンドポイントからの応答は、特定の条件下でエラーメッセージを返す場合があります。 これらのエラーメッセージは次の形式で返されます。

```json
{
    "type": "http://ns.adobe.com/aep/errors/INSGHT-1000-400",
    "title": "Bad Request - Start date cannot be after end date.",
    "status": 400,
    "report": {
        "tenantInfo": {
            "sandboxName": "prod",
            "sandboxId": "49f58060-5d47-34rd-aawf-a5384333ff12",
            "imsOrgId": "{IMS_ORG}"
        },
        "additionalContext": null
    },
    "error-chain": [
        {
            "serviceId": "INSGHT",
            "errorCode": "INSGHT-1000-400",
            "invokingServiceId": "INSGHT",
            "unixTimeStampMs": 1602095177129
        }
    ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `title` | エラーメッセージと、エラーが発生した可能性のある理由を含む文字列。 |
| `report` | エラーをトリガーした操作で使用されているサンドボックスやIMS組織など、エラーに関するコンテキスト情報が含まれます。 |

{style=&quot;table-layout:auto&quot;}

次の表に、APIから返される様々なエラーコードを示します。

| エラーコード | タイトル | 説明 |
| --- | --- | --- |
| `INSGHT-1000-400` | 不正なリクエストペイロード | リクエストペイロードに問題がありました。 [上記](#v2)に示すように、ペイロードの形式が完全に一致していることを確認してください。 考えられる理由のいずれかで、次のエラーがトリガーする場合があります。<ul><li>`aggregator`などの必須フィールドがありません</li><li>無効な指標</li><li>リクエストに無効な集計が含まれています</li><li>開始日は、終了日の後に設定されます</li></ul> |
| `INSGHT-1001-400` | 指標のクエリに失敗しました | リクエストが正しくないか、クエリ自体を解析できないため、指標データベースをクエリしようとしたときにエラーが発生しました。 再試行する前に、リクエストが正しい形式になっていることを確認してください。 |
| `INSGHT-1001-500` | 指標のクエリに失敗しました | サーバーエラーが原因で、指標データベースに対してクエリを実行しようとしたときにエラーが発生しました。 要求をもう一度試してみてください。問題が解決しない場合は、Adobeサポートにお問い合わせください。 |
| `INSGHT-1002-500` | サービスエラー | 内部エラーが原因で、要求を処理できませんでした。 要求をもう一度試してみてください。問題が解決しない場合は、Adobeサポートにお問い合わせください。 |
| `INSGHT-1003-401` | サンドボックス検証エラー | サンドボックス検証エラーが原因で、要求を処理できませんでした。 要求を再試行する前に、`x-sandbox-name`ヘッダーで指定したサンドボックス名が、IMS組織に対して有効な有効なサンドボックスを表していることを確認してください。 |

{style=&quot;table-layout:auto&quot;}
