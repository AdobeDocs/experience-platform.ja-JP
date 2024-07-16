---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: 指標 API エンドポイント
description: Observability Insights API を使用してExperience Platformのオブザーバビリティ指標を取得する方法を説明します。
exl-id: 08d416f0-305a-44e2-a2b7-d563b2bdd2d2
source-git-commit: fcd44aef026c1049ccdfe5896e6199d32b4d1114
workflow-type: tm+mt
source-wordcount: '1360'
ht-degree: 25%

---

# 指標エンドポイント

可観測性指標は、Adobe Experience Platformの様々な機能の使用状況の統計、過去の傾向およびパフォーマンス指標に関するインサイトを提供します。 [!DNL Observability Insights API] の `/metrics` エンドポイントを使用すると、[!DNL Platform] での組織のアクティビティの指標データをプログラムによって取得できます。

>[!NOTE]
>
>以前のバージョンの指標エンドポイント（V1）は非推奨（廃止予定）となりました。 このドキュメントでは、現在のバージョン（V2）にのみ焦点を当てています。 レガシー実装の V1 エンドポイントの詳細については、[API リファレンス ](https://www.adobe.io/experience-platform-apis/references/observability-insights/#operation/retrieveMetricsV1) を参照してください。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Observability Insights] API](https://www.adobe.io/experience-platform-apis/references/observability-insights/) の一部です。先に進む前に、[はじめる前に](./getting-started.md)のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の [!DNL Experience Platform] API の呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## 観察性指標の取得

`/metrics` エンドポイントに対してPOSTリクエストを実行し、取得する指標をペイロードで指定することで、指標データを取得できます。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `start` | 指標データを取得する最も古い日時。 |
| `end` | 指標データを取得する最新の日時。 |
| `granularity` | 指標データを除算する時間間隔を示すオプションのフィールド。 例えば、値が `DAY` の場合は、`start` 日から `end` 日までの各日の指標が返されますが、値が `MONTH` の場合は、指標の結果が月でグループ化されます。 このフィールドを使用する場合は、対応する `downsample` プロパティも指定して、データのグループ化基準となる集計関数を示す必要があります。 |
| `metrics` | 取得する指標ごとに 1 つ存在するオブジェクトの配列。 |
| `name` | Observability Insights で認識される指標の名前。 許可された指標名の完全なリストについては、[ 付録 ](#available-metrics) を参照してください。 |
| `filters` | 特定のデータセットで指標をフィルタリングできるオプションのフィールド。 フィールドはオブジェクトの配列（フィルターごとに 1 つ）で、各オブジェクトには次のプロパティが含まれます。 <ul><li>`name`：指標をフィルタリングするエンティティのタイプ。 現在は、`dataSets` のみがサポートされています。</li><li>`value`: 1 つ以上のデータセットの ID。 複数のデータセット ID を 1 つの文字列として指定し、各 ID を縦棒（`\|`）で区切ることができます。</li><li>`groupBy`: true に設定した場合、対応する `value` が、指標の結果を個別に返す必要がある複数のデータセットを表すことを示します。 false に設定した場合、これらのデータセットの指標の結果はグループ化されます。</li></ul> |
| `aggregator` | 複数時系列レコードを 1 つの結果にグループ化するために使用する集計関数を指定します。 使用可能なアグリゲータについて詳しくは、[OpenTSDB ドキュメント ](https://docs.w3cub.com/opentsdb/user_guide/query/aggregators) を参照してください。 |
| `downsample` | フィールドを間隔（または「バケット」）に並べ替えることで、指標データのサンプリングレートを減らす集計関数を指定できるオプションのフィールドです。 ダウンサンプリングの間隔は、`granularity` プロパティによって決まります。 ダウンサンプリングについて詳しくは、[OpenTSDB ドキュメント ](https://docs.w3cub.com/opentsdb/user_guide/query/aggregators) を参照してください。 |

{style="table-layout:auto"}

**応答**

応答が成功すると、リクエストで指定された指標とフィルターの結果のデータポイントが返されます。

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
| `metricResponses` | リクエストで指定された各指標をオブジェクトが表す配列。 各オブジェクトには、フィルター設定に関する情報と返された指標データが含まれます。 |
| `metric` | リクエストで提供される指標の 1 つの名前。 |
| `filters` | 指定した指標のフィルター設定。 |
| `datapoints` | 指定した指標およびフィルターの結果をオブジェクトが表す配列。 配列内のオブジェクトの数は、リクエストで指定されたフィルターオプションによって異なります。 フィルターが指定されていない場合、配列には、すべてのデータセットを表す単一のオブジェクトのみが含まれます。 |
| `groupBy` | 指標の `filter` プロパティで複数のデータセットが指定されていて、リクエストで `groupBy` オプションが true に設定された場合、このオブジェクトには、対応する `dps` プロパティが適用されるデータセットの ID が含まれます。<br><br> 応答でこのオブジェクトが空と表示される場合、対応する `dps` プロパティは、`filters` 配列で提供されるすべてのデータセット（またはフィルターが指定されていない場合は [!DNL Platform] のすべてのデータセット）に適用されます。 |
| `dps` | 指定された指標、フィルターおよび時間範囲で返されるデータ。 このオブジェクトの各キーは、指定された指標に対応する値を持つタイムスタンプを表します。 各データポイント間の期間は、リクエストで指定された `granularity` 値によって異なります。 |

{style="table-layout:auto"}

## 付録

次の節では、`/metrics` エンドポイントの操作に関する追加情報を示します。

### 使用可能な指標 {#available-metrics}

次の表に、[!DNL Observability Insights] によって公開されるすべての指標をサービス別に分類し [!DNL Platform] 示します。 各指標には、説明と受け入れられた ID クエリーパラメーターが含まれます。

>[!NOTE]
>
>リストに表示されるすべての ID クエリパラメーターは、特に断りのない限りオプションです。

#### [!DNL Data Ingestion] {#ingestion}

次の表に、Adobe Experience Platform [!DNL Data Ingestion] の指標の概要を示します。 **太字**&#x200B;の指標はストリーミング取得指標です。

| インサイト指標 | 説明 | ID クエリーパラメーター |
| ---- | ---- | ---- |
| timeseries.ingestion.dataset.size | 1 つまたはすべてのデータセット用に取得されたすべてのデータの累積サイズ。 | データセット ID |
| timeseries.ingestion.dataset.dailysize | 1 つのデータセットまたはすべてのデータセットに対して毎日の使用量ベースで取得されるデータのサイズ。 | データセット ID |
| timeseries.ingestion.dataset.batchfailed.count | 1 つのデータセットまたはすべてのデータセットで失敗したバッチの数。 | データセット ID |
| timeseries.ingestion.dataset.batchsuccess.count | 1 つのデータセットまたはすべてのデータセットに対して取得されたバッチの数。 | データセット ID |
| timeseries.ingestion.dataset.recordsuccess.count | 1 つのデータセットまたはすべてのデータセットに対して取得されたレコードの数。 | データセット ID |
| **timeseries.data.collection.validation.category.presence.count** | 1 つのデータセットまたはすべてのデータセットに対する無効な「存在」メッセージの合計数。 | データセット ID |
| **timeseries.data.collection.inlet.total.messages.received** | 1 つのデータインレットまたはすべてのデータインレットに対して受信したメッセージの合計数。 | インレット ID |
| **timeseries.data.collection.inlet.total.messages.size.received** | 1 つのデータインレットまたはすべてのデータインレットに対して受信したデータの合計サイズ。 | インレット ID |
| **timeseries.data.collection.inlet.success** | 1 つのデータインレットまたはすべてのデータインレットに対する成功した HTTP 呼び出しの合計数。 | インレット ID |
| **timeseries.data.collection.inlet.failure** | 1 つのデータインレットまたはすべてのデータインレットに対する失敗した HTTP 呼び出しの合計数。 | インレット ID |

{style="table-layout:auto"}

#### [!DNL Identity Service] {#identity}

次の表に、Adobe Experience Platform [!DNL Identity Service] の指標の概要を示します。

| インサイト指標 | 説明 | ID クエリーパラメーター |
| ---- | ---- | ---- |
| timeseries.identity.dataset.recordsuccess.count | 1 つのデータセットまたはすべてのデータセットについて、[!DNL Identity Service] によってデータソースに書き込まれたレコードの数。 | データセット ID |
| timeseries.identity.dataset.recordfailed.count | 1 つのデータセットまたはすべてのデータセットに対して、[!DNL Identity Service] で失敗したレコードの数。 | データセット ID |
| timeseries.identity.dataset.namespacecode.recordfailed.count | 名前空間が失敗した ID レコードの数。 | 名前空間 ID（**必須**） |
| timeseries.identity.dataset.namespacecode.recordskipped.count | 名前空間がスキップした ID レコードの数。 | 名前空間 ID（**必須**） |
| timeseries.identity.graph.imsorg.uniqueidentities.count | 組織の ID グラフに保存されている一意の ID の数。 | なし |
| timeseries.identity.graph.imsorg.namespacecode.uniqueidentities.count | 名前空間の ID グラフに保存される一意の ID の数。 | 名前空間 ID（**必須**） |
| timeseries.identity.graph.imsorg.graphstrength.uniqueidentities.count | 特定のグラフ強度（「不明」、「弱い」、「強い」）のために、組織の ID グラフに保存された一意の ID の数。 | グラフの強さ（**必須**） |

{style="table-layout:auto"}

#### [!DNL Real-Time Customer Profile] {#profile}

次の表に、[!DNL Real-Time Customer Profile] の指標の概要を示します。

| インサイト指標 | 説明 | ID クエリーパラメーター |
| ---- | ---- | ---- |
| timeseries.profiles.dataset.recordread.count | 1 つのデータセットまたはすべてのデータセットに対して、[!DNL Profile] で [!DNL Data Lake] から読み取られたレコードの数。 | データセット ID |
| timeseries.profiles.dataset.recordsuccess.count | 1 つのデータセットまたはすべてのデータセットに対して、[!DNL Profile] によってデータソースに書き込まれたレコードの数。 | データセット ID |
| timeseries.profiles.dataset.batchsuccess.count | 1 つのデータセットまたはすべてのデータセットに対して取り込まれた [!DNL Profile] バッチの数。 | データセット ID |

{style="table-layout:auto"}

### エラーメッセージ

`/metrics` エンドポイントからの応答は、特定の条件下でエラーメッセージを返す場合があります。 これらのエラーメッセージは、次の形式で返されます。

```json
{
    "type": "http://ns.adobe.com/aep/errors/INSGHT-1000-400",
    "title": "Bad Request - Start date cannot be after end date.",
    "status": 400,
    "report": {
        "tenantInfo": {
            "sandboxName": "prod",
            "sandboxId": "49f58060-5d47-34rd-aawf-a5384333ff12",
            "imsOrgId": "{ORG_ID}"
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
| `title` | エラーメッセージと、それが発生した可能性がある潜在的な理由を含む文字列。 |
| `report` | エラーに関するコンテキスト情報が含まれています。これには、エラーをトリガーした操作で使用されているサンドボックスや組織が含まれます。 |

{style="table-layout:auto"}

次の表に、API から返される可能性のある様々なエラーコードを示します。

| エラーコード | タイトル | 説明 |
| --- | --- | --- |
| `INSGHT-1000-400` | 無効なリクエストペイロードです | リクエストペイロードに問題が発生しました。 ペイロード形式が [ 上記 ](#v2) と完全に一致することを確認してください。 考えられる理由のいずれかにより、このエラーがトリガーする場合があります。<ul><li>`aggregator` などの必須フィールドがありません</li><li>無効な指標</li><li>リクエストに無効なアグリゲータが含まれています</li><li>開始日は終了日の後に行われます</li></ul> |
| `INSGHT-1001-400` | 指標のクエリに失敗しました | 正しくないリクエストまたはクエリ自体が解析できないことが原因で、指標データベースのクエリを実行しようとしたときにエラーが発生しました。 再試行する前に、リクエストが正しい形式であることを確認してください。 |
| `INSGHT-1001-500` | 指標のクエリに失敗しました | サーバーエラーが原因で、指標データベースのクエリ中にエラーが発生しました。 リクエストを再試行します。問題が解決しない場合は、Adobeサポートにお問い合わせください。 |
| `INSGHT-1002-500` | サービスエラー | 内部エラーが発生したので、リクエストを処理できませんでした。 リクエストを再試行します。問題が解決しない場合は、Adobeサポートにお問い合わせください。 |
| `INSGHT-1003-401` | サンドボックス検証エラー | サンドボックス検証エラーにより、リクエストを処理できませんでした。 リクエストを再試行する前に、`x-sandbox-name` ヘッダーで指定したサンドボックス名が、組織に対して有効な有効なサンドボックスを表していることを確認してください。 |

{style="table-layout:auto"}
