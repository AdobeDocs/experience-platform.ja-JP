---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformの観察性インサイト
topic: overview
translation-type: tm+mt
source-git-commit: d349ffab7c0de72d38b5195585c14a4a8f80e37c

---


# Adobe Experience Platformの観察性インサイトの概要

観察性インサイトは、Adobe Experience Platformで主要な観察性指標を公開できるRESTful APIです。 これらの指標は、プラットフォームの使用状況の統計、プラットフォームサービスのヘルスチェック、様々なプラットフォーム機能の過去の傾向、パフォーマンス指標に関する洞察を提供します。

このドキュメントでは、Observatibility Insights APIの呼び出し例を示します。 監視性エンドポイントの完全なリストについては、『 [Observatibility Insights APIリファレンス』を参照してください](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/observability-insights.yaml)。

## はじめに

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

* 認証：無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。 プラットフォームのサンドボックスについて詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../sandboxes/home.md)い。

* x-sandbox-name: `{SANDBOX_NAME}`

## 観察性指標の取得

監視性インサイトAPIでエンドポイントにGETリクエストを送信することで、 `/metrics` 監視性指標を取得できます。

**API形式**

エンドポイントを使用す `/metrics` る場合は、1つ以上の指標リクエストパラメーターを指定する必要があります。 その他のクエリパラメーターは、結果をフィルタリングする場合はオプションです。

```http
GET /metrics?metric={METRIC}
GET /metrics?metric={METRIC}&metric={METRIC_2}
GET /metrics?metric={METRIC}&id={ID}
GET /metrics?metric={METRIC}&dateRange={DATE_RANGE}
GET /metrics?metric={METRIC}&metric={METRIC_2}&id={ID}&dateRange={DATE_RANGE}
```

| パラメーター | 説明 |
| --- | --- |
| `{METRIC}` | 表示する指標。 1回の呼び出しで複数の指標を組み合わせる場合、アンパサンド(`&`)を使用して個々の指標を区切る必要があります。 例：`metric={METRIC_1}&metric={METRIC_2}`。 |
| `{ID}` | 指標を公開する特定のプラットフォームリソースの識別子。 このIDは、使用する指標に応じて、オプション、必須、または適用されない場合があります。 使用可能な指標のリスト、および各指標でサポートされるID（必須と任意の両方）については、使用可能な指標に関するリファレンスドキュメントを参 [照してください](metrics.md)。 |
| `{DATE_RANGE}` | ISO 8601形式(例： `2018-10-01T07:00:00.000Z/2018-10-09T07:00:00.000Z`)で公開する指標の日付範囲。 |

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

成功した応答は、オブジェクトのリストを返します。各オブジェクトには、指定された値内にタイムスタ `dateRange` ンプが含まれ、リクエストパスで指定された指標に対応する値が含まれます。 プラットフ `id` ォームリソースのが要求パスに含まれる場合、結果はその特定のリソースにのみ適用されます。 を省略する `id` と、結果はIMS組織内の該当するすべてのリソースに適用されます。

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

## 次の手順

このドキュメントでは、Observatibility Insights APIの概要を提供しました。 APIで使用できる指標 [の完全なリストについては](metrics.md) 、利用可能な指標に関するドキュメントを参照してください。