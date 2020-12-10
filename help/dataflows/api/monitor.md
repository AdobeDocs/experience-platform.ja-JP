---
keywords: Experience Platform;home;popular topics;monitor dataflows;flow service api;Flow Service
solution: Experience Platform
title: フローと実行の監視
topic: overview
type: Tutorial
description: このチュートリアルでは、Flow Service APIを使用して、完全性、エラーおよび指標に関するフロー実行データを監視する手順を説明します。
translation-type: tm+mt
source-git-commit: 3fb5879ea636d4059a6b42e2f98742ff7df0397c
workflow-type: tm+mt
source-wordcount: '703'
ht-degree: 16%

---


# Flow Service APIを使用してデータフローを監視する

Adobe Experience Platform allows data to be ingested from external sources while providing you with the ability to structure, label, and enhance incoming data using [!DNL Platform] services. アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。さらに、Experience Platformでは、データを外部パートナーに対してアクティブ化できます。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。 このサービスは、サポートされるすべてのソースと宛先が接続可能なユーザーインターフェイスおよびRESTful APIを提供します。

このチュートリアルでは、を使用して、完全性、エラー、指標に関するフロー実行データを監視する手順について説明し [[!DNL Flow Service API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml)ます。

## はじめに

このチュートリアルでは、有効なデータフローのID値が必要です。 有効なデータフローIDがない場合は、 [ソースの概要](../../sources/home.md) または [](../../destinations/catalog/overview.md) 宛先の概要から選択したコネクタを選択し、このチュートリアルを試行する前に概要を説明した手順に従ってください。

また、このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

- [宛先](../../destinations/home.md):宛先は、一般的に使用されるアプリケーションとの統合が事前に構築されており、チャネル間のマーケティングキャンペーン、電子メールキャンペーン、ターゲットを絞った広告など、様々な用途で、プラットフォームのデータをシームレスにアクティベーションできます。
- [ソース](../../sources/home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
- [サンドボックス](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

The following sections provide additional information that you will need to know in order to successfully monitor flow runs using the [!DNL Flow Service] API.

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

- `Authorization: Bearer {ACCESS_TOKEN}`
- `x-api-key: {API_KEY}`
- `x-gw-ims-org-id: {IMS_ORG}`

All resources in [!DNL Experience Platform], including those belonging to [!DNL Flow Service], are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

- `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

- `Content-Type: application/json`

## フローの実行の監視

データフローを作成したら、 [!DNL Flow Service] APIに対してGETリクエストを実行します。

**API 形式**

```http
GET /runs?property=flowId=={FLOW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FLOW_ID}` | 監視するデータフローの固有 `id` 値。 |

**リクエスト**

次のリクエストは、既存のデータフローの仕様を取得します。

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/runs?property=flowId==c9cef9cb-c934-4467-8ef9-cbc934546741' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

成功した応答は、作成日、ソース接続とターゲット接続、およびフロー実行の固有な識別子(`id`)に関する情報を含む、フロー実行に関する詳細を返します。

```json
{
    "items": [
        {
            "id": "65b7cfcc-6b2e-47c8-8194-13005b792752",
            "createdAt": 1607520228894,
            "updatedAt": 1607520276948,
            "createdBy": "{CREATED_BY}",
            "updatedBy": "{UPDATED_BY}",
            "createdClient": "{CREATED_CLIENT}",
            "updatedClient": "{UPDATED_CLIENT}",
            "sandboxId": "{SANDBOX_ID}",
            "sandboxName": "prod",
            "imsOrgId": "{IMS_ORG_ID}",
            "flowId": "f00c8762-df2f-432b-a7d7-38999fef5e8e",
            "etag": "\"560266dc-0000-0200-0000-5fd0d0140000\"",
            "metrics": {
                "durationSummary": {
                    "startedAtUTC": 1607520205922,
                    "completedAtUTC": 1607520262413
                },
                "sizeSummary": {
                    "inputBytes": 87885,
                    "outputBytes": 56730
                },
                "recordSummary": {
                    "inputRecordCount": 26758,
                    "outputRecordCount": 26758,
                    "failedRecordCount": 0
                },
                "fileSummary": {
                    "inputFileCount": 11,
                    "outputFileCount": 11,
                    "activityRefs": [
                        "37b34f84-1ada-11eb-adc1-0242ac120002"
                    ]
                },
                "statusSummary": {
                    "status": "success"
                }
            },
            "activities": [
                {
                    "id": "4f008a00-3a04-11eb-adc1-0242ac120002",
                    "name": "Copy Activity",
                    "updatedAtUTC": 0,
                    "durationSummary": {
                        "startedAtUTC": 1607520205922,
                        "completedAtUTC": 1607520236968
                    },
                    "sizeSummary": {
                        "inputBytes": 87885,
                        "outputBytes": 87885
                    },
                    "recordSummary": {
                        "inputRecordCount": 26758,
                        "outputRecordCount": 26758
                    },
                    "fileSummary": {
                        "inputFileCount": 11,
                        "outputFileCount": 11
                    },
                    "statusSummary": {
                        "status": "success"
                    }
                },
                {
                    "id": "37b34f84-1ada-11eb-adc1-0242ac120002",
                    "name": "Promotion Activity",
                    "updatedAtUTC": 0,
                    "durationSummary": {
                        "startedAtUTC": 1607520244985,
                        "completedAtUTC": 1607520262413
                    },
                    "sizeSummary": {
                        "inputBytes": 26758,
                        "outputBytes": 56730
                    },
                    "recordSummary": {
                        "inputRecordCount": 26758,
                        "outputRecordCount": 26758,
                        "failedRecordCount": 0
                    },
                    "fileSummary": {
                        "inputFileCount": 11,
                        "outputFileCount": 2,
                        "extensions": {
                            "manifest": {
                                "fileInfo": "https://platform.adobe.io/data/foundation/export/batches/01ES3TRN69E9W2DZ770XCGYAH1/meta?path=input_files",
                                "pathPrefix": "bucket1/csv_test/"
                            }
                        }
                    },
                    "statusSummary": {
                        "status": "success"
                    }
                }
            ]
        }
    ]
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `items` | 特定のフロー実行に関連付けられたメタデータの単一のペイロードが含まれます。 |
| `metrics` | フロー実行のデータの特性。 |
| `activities` | データの変換方法を示します。 |
| `durationSummary` | フロー実行の開始と終了時間。 |
| `sizeSummary` | データのボリューム（バイト単位）。 |
| `recordSummary` | データのレコード数。 |
| `fileSummary` | データのファイル数。 |
| `fileSummary.extensions` | アクティビティに固有の情報が含まれます。 例えば、 `manifest` は「プロモーションアクティビティ」の一部に過ぎず、オブジェクトに含まれ `extensions` ます。 |
| `statusSummary` | フローの実行が成功か失敗かを示します。 |

## 次の手順

このチュートリアルに従うと、 [!DNL Flow Service] APIを使用してデータフローの指標とエラー情報を取得できます。 これで、取り込みスケジュールに応じてデータフローを監視し続け、ステータスと取り込み率を追跡できます。 ソースのデータフローを監視する方法の詳細は、ユーザー・インタフェースのチュートリアルを使用して、ソースの [監視データフローを読んでください](../ui/monitor-sources.md) 。 宛先のデータ・フローを監視する方法の詳細は、ユーザー・インタフェース・チュートリアルを使用して、宛先の [監視データ・フローを参照してください](../ui/monitor-destinations.md) 。