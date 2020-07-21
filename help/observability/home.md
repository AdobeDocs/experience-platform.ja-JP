---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformの観察性インサイト
topic: overview
translation-type: tm+mt
source-git-commit: 73a492ba887ddfe651e0a29aac376d82a7a1dcc4
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 2%

---


# Adobe Experience Platformの観察性インサイトの概要

観察性インサイトは、主要な観察性指標をAdobe Experience Platformに公開できるRESTful APIです。 これらの指標は、使用状況の統計、サー [!DNL Platform] ビスの正常性チェック、各種 [!DNL Platform][!DNL Platform] 機能の過去の傾向、およびパフォーマンス指標に関する洞察を提供します。

このドキュメントは、Obsermability Insights APIの呼び出し例を示します。 監視性エンドポイントの完全なリストについては、『 [Observatibility Insights API reference](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/observability-insights.yaml)』を参照してください。

## はじめに

APIを呼び出すには、まず [!DNL Platform] 認証チュートリアルを完了する必要があり [ます](../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

内のすべてのリソース [!DNL Experience Platform] は、特定の仮想サンドボックスに分離されます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。 のサンドボックスについて詳し [!DNL Platform]くは、 [Sandboxの概要ドキュメントを参照してください](../sandboxes/home.md)。

* x-sandbox-name: `{SANDBOX_NAME}`

## 観察性指標の取得

Obsermability Insights APIでエンドポイントにGETリクエストを行うことで、観察性指標を取得できます。 `/metrics`

**API形式**

エンドポイントを使用する場合は、少なくとも1つの指標リクエストパラメーターを指定する必要があり `/metrics` ます。 その他のクエリパラメーターは、結果をフィルタリングする場合はオプションです。

```http
GET /metrics?metric={METRIC}
GET /metrics?metric={METRIC}&metric={METRIC_2}
GET /metrics?metric={METRIC}&id={ID}
GET /metrics?metric={METRIC}&dateRange={DATE_RANGE}
GET /metrics?metric={METRIC}&metric={METRIC_2}&id={ID}&dateRange={DATE_RANGE}
```

| パラメーター | 説明 |
| --- | --- |
| `{METRIC}` | 公開する指標。 1回の呼び出しで複数の指標を組み合わせる場合、アンパサンド(`&`)を使用して個々の指標を区切る必要があります。 例：`metric={METRIC_1}&metric={METRIC_2}`。 |
| `{ID}` | 指標を公開する特定の [!DNL Platform] リソースの識別子。 このIDは、使用する指標に応じて、オプション、必須、または適用されない場合があります。 使用可能な指標のリスト、および各指標でサポートされるID（必須と任意の両方）については、 [使用可能な指標に関するリファレンスドキュメントを参照してください](metrics.md)。 |
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

成功した応答は、指定された値内のタイムスタンプと、リクエストパスで指定された指標に対応する値を含むオブジェクトのリスト `dateRange` を返します。 リソース `id`[!DNL Platform] の要求が要求パスに含まれる場合、結果はその特定のリソースにのみ適用されます。 を省略 `id` すると、結果はIMS組織内の該当するすべてのリソースに適用されます。

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

このドキュメントでは、Observability Insights APIの概要を提供しました。 APIで使用できる指標の完全なリストについては、 [利用可能な指標](metrics.md) （英語）のドキュメントを参照してください。