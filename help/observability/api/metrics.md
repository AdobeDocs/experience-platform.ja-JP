---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: 指標 API エンドポイント
topic-legacy: developer guide
description: 観察性インサイト API を使用して、Experience Platformで観察性指標を取得する方法を説明します。
exl-id: 08d416f0-305a-44e2-a2b7-d563b2bdd2d2
source-git-commit: 5c893d7c8c455c86c94cd311a20ce774abcf65e0
workflow-type: tm+mt
source-wordcount: '1866'
ht-degree: 42%

---

# 指標エンドポイント

観察性指標は、Adobe Experience Platformの様々な機能の使用状況の統計、過去の傾向、パフォーマンス指標に関するインサイトを提供します。 この `/metrics` エンドポイント [!DNL Observability Insights API] を使用すると、で組織のアクティビティの指標データをプログラムによって取得できます。 [!DNL Platform].

>[!NOTE]
>
>以前のバージョンの指標エンドポイント (V1) は非推奨（廃止予定）となりました。 このドキュメントでは、現在のバージョン (V2) にのみ焦点を当てます。 レガシー実装の V1 エンドポイントについて詳しくは、 [API リファレンス](https://www.adobe.io/experience-platform-apis/references/observability-insights/#operation/retrieveMetricsV1).

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Observability Insights] API](https://www.adobe.io/experience-platform-apis/references/observability-insights/) の一部です。先に進む前に、[はじめる前に](./getting-started.md)のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の [!DNL Experience Platform] API の呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## 観察性指標の取得

指標データを取得するには、 `/metrics` エンドポイントで、ペイロードで取得する指標を指定します。

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
| `granularity` | 指標データを除算する時間間隔を示すオプションのフィールドです。 例えば、 `DAY` は、次の間の各日の指標を返します `start` および `end` 日付、値は `MONTH` では、代わりに月別に指標の結果がグループ化されます。 このフィールドを使用する場合、 `downsample` プロパティを指定して、データをグループ化する集計関数を示す必要もあります。 |
| `metrics` | オブジェクトの配列。取得する指標ごとに 1 つずつ。 |
| `name` | 観察性インサイトで認識される指標の名前。 詳しくは、 [付録](#available-metrics) を参照してください。 |
| `filters` | 特定のデータセットで指標をフィルタリングできるオプションのフィールドです。 フィールドはオブジェクトの配列（各フィルターに対して 1 つ）で、各オブジェクトには次のプロパティが含まれます。 <ul><li>`name`:指標をフィルターするエンティティのタイプ。 現在は、`dataSets` のみがサポートされています。</li><li>`value`:1 つ以上のデータセットの ID。 複数のデータセット ID を 1 つの文字列として指定でき、各 ID を縦棒グラフ (`\|`) をクリックします。</li><li>`groupBy`:true に設定した場合、は、対応する `value` は、指標の結果を個別に返す必要がある複数のデータセットを表します。 false に設定した場合、これらのデータセットの指標の結果は 1 つにグループ化されます。</li></ul> |
| `aggregator` | 複数の時系列レコードを単一の結果にグループ化するために使用する集計関数を指定します。 使用可能な集約について詳しくは、 [OpenTSDB ドキュメント](http://opentsdb.net/docs/build/html/user_guide/query/aggregators.html). |
| `downsample` | フィールドを間隔（「グループ」）に並べ替えることで、集計関数を指定して、指標データのサンプリング率を減らすことができるオプションのフィールドです。 ダウンサンプリングの間隔は、 `granularity` プロパティ。 ダウンサンプリングについて詳しくは、 [OpenTSDB ドキュメント](http://opentsdb.net/docs/build/html/user_guide/query/downsampling.html). |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、リクエストで指定された指標およびフィルターに関する結果のデータポイントを返します。

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
| `metricResponses` | リクエストで指定された各指標を表すオブジェクトを持つ配列。 各オブジェクトには、フィルター設定に関する情報と、返された指標データが含まれます。 |
| `metric` | リクエストで提供される指標の 1 つの名前。 |
| `filters` | 指定した指標のフィルター設定。 |
| `datapoints` | 指定した指標とフィルターの結果を表すオブジェクトを持つ配列。 配列内のオブジェクトの数は、リクエストで提供されるフィルターオプションによって異なります。 フィルターが指定されなかった場合、配列にはすべてのデータセットを表す 1 つのオブジェクトのみが含まれます。 |
| `groupBy` | 複数のデータセットが `filter` プロパティを使用して、 `groupBy` オプションがリクエストで true に設定された場合、このオブジェクトには、対応する `dps` プロパティの適用先：<br><br>このオブジェクトが応答で空の場合、 `dps` プロパティは、 `filters` 配列（またはすべてのデータセット） [!DNL Platform] （フィルターが指定されていない場合）。 |
| `dps` | 指定された指標、フィルターおよび時間範囲に対して返されるデータ。 このオブジェクトの各キーは、指定した指標に対応する値を持つタイムスタンプを表します。 各データポイント間の期間は、 `granularity` リクエストで指定された値。 |

{style=&quot;table-layout:auto&quot;}

## 付録

次の節では、 `/metrics` endpoint.

### 使用可能な指標 {#available-metrics}

次の表に、で公開されているすべての指標を示します。 [!DNL Observability Insights]で分類された [!DNL Platform] サービス。 各指標には、説明と受け入れられた ID クエリーパラメーターが含まれます。

>[!NOTE]
>
>リストに表示される ID クエリパラメーターは、特に記載がない限り、すべてオプションです。

#### [!DNL Data Ingestion] {#ingestion}

次の表に、Adobe Experience Platform指標の概要を示します [!DNL Data Ingestion]. **太字**&#x200B;の指標はストリーミング取得指標です。

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
| **timeseries.data.collection.inlet.total.messages.received** | 1 つのデータインレットまたはすべてのデータインレットに対して受信したメッセージの合計数。 | インレット ID |
| **timeseries.data.collection.inlet.total.messages.size.received** | 1 つのデータインレットまたはすべてのデータインレットに対して受信したデータの合計サイズ。 | インレット ID |
| **timeseries.data.collection.inlet.success** | 1 つのデータインレットまたはすべてのデータインレットに対する成功した HTTP 呼び出しの合計数。 | インレット ID |
| **timeseries.data.collection.inlet.failure** | 1 つのデータインレットまたはすべてのデータインレットに対する失敗した HTTP 呼び出しの合計数。 | インレット ID |

{style=&quot;table-layout:auto&quot;}

#### [!DNL Identity Service] {#identity}

次の表に、Adobe Experience Platform指標の概要を示します [!DNL Identity Service].

| インサイト指標 | 説明 | ID クエリーパラメーター |
| ---- | ---- | ---- |
| timeseries.identity.dataset.recordsuccess.count | データソースに書き込まれたレコードの数 [!DNL Identity Service]、1 つのデータセットまたはすべてのデータセット用。 | データセット ID |
| timeseries.identity.dataset.recordfailed.count | 失敗したレコードの数（次の場合） [!DNL Identity Service]（1 つのデータセット、またはすべてのデータセット用）。 | データセット ID |
| timeseries.identity.dataset.namespacecode.recordsuccess.count | ユーザーが正常に取得した ID レコードの名前空間数。 | 名前空間 ID（**必須**） |
| timeseries.identity.dataset.namespacecode.recordfailed.count | 名前空間が失敗した ID レコードの数。 | 名前空間 ID（**必須**） |
| timeseries.identity.dataset.namespacecode.recordskipped.count | 名前空間がスキップした ID レコードの数。 | 名前空間 ID（**必須**） |
| timeseries.identity.graph.imsorg.uniqueidentities.count | IMS 組織の ID グラフに保存される一意の ID の数。 | なし |
| timeseries.identity.graph.imsorg.namespacecode.uniqueidentities.count | 名前空間の ID グラフに保存される一意の ID の数。 | 名前空間 ID（**必須**） |
| timeseries.identity.graph.imsorg.numidgraphs.count | IMS 組織 の ID グラフに保存される一意のグラフ ID の数。 | なし |
| timeseries.identity.graph.imsorg.graphstrength.uniqueidentities.count | 特定のグラフの強さ（「不明」、「弱」または「強」）の IMS 組織の ID グラフに保存される一意の ID の数。 | グラフの強さ（**必須**） |

{style=&quot;table-layout:auto&quot;}

#### [!DNL Privacy Service] {#privacy}

次の表に、Adobe Experience Platform指標の概要を示します [!DNL Privacy Service].

| インサイト指標 | 説明 | ID クエリーパラメーター |
| ---- | ---- | ---- |
| timeseries.gdpr.jobs.totaljobs.count | GDPR から作成されたジョブの合計数。 | ENV(必&#x200B;**須**) |
| timeseries.gdpr.jobs.completedjobs.count | GDPR から完了したジョブの合計数。 | ENV(必&#x200B;**須**) |
| timeseries.gdpr.jobs.errorjobs.count | GDPR からのエラージョブの合計数。 | ENV(必&#x200B;**須**) |

{style=&quot;table-layout:auto&quot;}

#### [!DNL Query Service] {#query}

次の表に、Adobe Experience Platform指標の概要を示します [!DNL Query Service].

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

次の表に、 [!DNL Real-time Customer Profile].

| インサイト指標 | 説明 | ID クエリーパラメーター |
| ---- | ---- | ---- |
| timeseries.profiles.dataset.recordread.count | から読み取られたレコードの数 [!DNL Data Lake] 作成者 [!DNL Profile]（1 つのデータセット、またはすべてのデータセット用）。 | データセット ID |
| timeseries.profiles.dataset.recordsuccess.count | データソースに書き込まれたレコードの数 [!DNL Profile]（1 つのデータセット、またはすべてのデータセット用）。 | データセット ID |
| timeseries.profiles.dataset.recordfailed.count | 失敗したレコードの数（次の場合） [!DNL Profile]（1 つのデータセット、またはすべてのデータセット用）。 | データセット ID |
| timeseries.profiles.dataset.batchsuccess.count | 数 [!DNL Profile] データセットまたはすべてのデータセットに対して取り込まれるバッチ。 | データセット ID |
| timeseries.profiles.dataset.batchfailed.count | 数 [!DNL Profile] 1 つのデータセットまたはすべてのデータセットでバッチが失敗しました。 | データセット ID |
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

次の `/metrics` エンドポイントは、特定の状況でエラーメッセージを返す場合があります。 これらのエラーメッセージは、次の形式で返されます。

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
| `title` | エラーメッセージおよびエラーメッセージが発生した可能性のある理由を含む文字列。 |
| `report` | エラーをトリガーした操作で使用されているサンドボックスや IMS 組織など、エラーに関するコンテキスト情報が含まれます。 |

{style=&quot;table-layout:auto&quot;}

次の表に、API から返される様々なエラーコードを示します。

| エラーコード | タイトル | 説明 |
| --- | --- | --- |
| `INSGHT-1000-400` | 無効なリクエストペイロード | リクエストペイロードに問題が発生しました。 ペイロードの形式が、表示されているとおりに一致していることを確認してください [上](#v2). 考えられる理由のいずれかで、このエラーがトリガーする場合があります。<ul><li>必須フィールド（例： ）がありません `aggregator`</li><li>無効な指標</li><li>リクエストに無効な集計が含まれています</li><li>開始日は、終了日の後に実行されます</li></ul> |
| `INSGHT-1001-400` | 指標のクエリが失敗しました | リクエストが正しくないか、クエリ自体を解析できないため、指標データベースに対してクエリを実行しようとした際にエラーが発生しました。 再試行する前に、リクエストが正しく形式設定されていることを確認してください。 |
| `INSGHT-1001-500` | 指標のクエリが失敗しました | サーバーエラーが原因で、指標データベースに対してクエリを実行しようとした際にエラーが発生しました。 要求を再試行してください。問題が解決しない場合は、Adobeサポートにお問い合わせください。 |
| `INSGHT-1002-500` | サービスエラー | 内部エラーが原因で、リクエストを処理できませんでした。 要求を再試行してください。問題が解決しない場合は、Adobeサポートにお問い合わせください。 |
| `INSGHT-1003-401` | サンドボックス検証エラー | サンドボックス検証エラーが原因で、リクエストを処理できませんでした。 で指定したサンドボックス名が `x-sandbox-name` ヘッダーは、IMS 組織に対して有効な、リクエストを再試行する前に有効なサンドボックスを表します。 |

{style=&quot;table-layout:auto&quot;}
