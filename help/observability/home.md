---
keywords: Experience Platform;home;popular topics;date range
solution: Experience Platform
title: Adobe Experience Platform 観察性インサイト
topic: overview
description: 観察性インサイトは、Adobe Experience Platform で主要な観察性指標を公開するための RESTful API です。これらの指標は、Platform の使用状況の統計、Platform サービスのヘルスチェック、様々な Platform 機能の過去の傾向とパフォーマンス指標に関する洞察を提供します。
translation-type: tm+mt
source-git-commit: cc81d590f308c7e2677cec000c27e8aca42437f5
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 76%

---


# Adobe Experience Platform 観察性インサイトの概要

観察性インサイトは、Adobe Experience Platform で主要な観察性指標を公開するための RESTful API です。These metrics provide insights into [!DNL Platform] usage statistics, health-checks for [!DNL Platform] services, historical trends, and performance indicators for various [!DNL Platform] functionalities.

このドキュメントでは、観察性インサイト API の呼び出し例を示します。観察性エンドポイントの完全なリストについては、『[観察性インサイト API リファレンス](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/observability-insights.yaml)』を参照してください。

## はじめに

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

All resources in [!DNL Experience Platform] are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in. For more information on sandboxes in [!DNL Platform], see the [sandbox overview documentation](../sandboxes/home.md).

* x-sandbox-name: `{SANDBOX_NAME}`

## 観察性指標の取得

観察性インサイト API で `/metrics` エンドポイントに GET リクエストを送信することで、観察性指標を取得できます。

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
| `{ID}` | The identifier for a particular [!DNL Platform] resource whose metrics you want to expose. この ID は、使用する指標に応じて、オプション/必須/該当しない場合があります。使用可能な指標のリスト、および各指標でサポートされる ID（必須とオプションの両方）については、[使用可能な指標](metrics.md)に関するリファレンスドキュメントを参照してください。 |
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

## 次の手順

このドキュメントでは、観察性インサイト API の概要を提供しました。API で使用できる指標の完全なリストについては、[利用可能な指標](metrics.md)に関するドキュメントを参照してください。