---
keywords: Experience Platform、ホーム、人気のあるトピック、データフローの監視、フローサービスapi、フローサービス
solution: Experience Platform
title: フローサービスAPIを使用したデータフローの監視
topic-legacy: overview
type: Tutorial
description: このチュートリアルでは、フローサービスAPIを使用して、完全性、エラーおよび指標のフロー実行データを監視する手順を説明します。
exl-id: c4b2db97-eba4-460d-8c00-c76c666ed70e
source-git-commit: 5160bc8057a7f71e6b0f7f2d594ba414bae9d8f6
workflow-type: tm+mt
source-wordcount: '718'
ht-degree: 28%

---

# フローサービスAPIを使用したデータフローの監視

Adobe Experience Platformを使用すると、[!DNL Platform]サービスを使用して、受信データの構造化、ラベル付け、拡張を行いながら、外部ソースからデータを取り込むことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。さらに、Experience Platformでは、外部のパートナーに対してデータをアクティブ化することもできます。

[!DNL Flow Service] は、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、サポートされるすべてのソースと宛先が接続可能なユーザーインターフェイスとRESTful APIを提供します。

このチュートリアルでは、[[!DNL Flow Service API]](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して、完全性、エラーおよび指標のフロー実行データを監視する手順を説明します。

## はじめに

このチュートリアルでは、有効なデータフローのID値が必要です。 有効なデータフローIDがない場合は、[ソースの概要](../../sources/home.md)または[宛先の概要](../../destinations/catalog/overview.md)から目的のコネクタを選択し、このチュートリアルを試す前に説明した手順に従います。

また、このチュートリアルでは、Adobe Experience Platformの次のコンポーネントに関する十分な知識が必要です。

- [宛先](../../destinations/home.md):宛先は、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用例に対して、Platformからデータをシームレスにアクティブ化できる、一般的に使用されるアプリケーションとの事前に構築された統合です。
- [ソース](../../sources/home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
- [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用してフローの実行を正しく監視するために必要な追加情報を示します。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- `Authorization: Bearer {ACCESS_TOKEN}`
- `x-api-key: {API_KEY}`
- `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Flow Service]に属するリソースを含む、[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されます。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

- `Content-Type: application/json`

## フロー実行の監視

データフローを作成したら、[!DNL Flow Service] APIに対してGETリクエストを実行します。

**API 形式**

```http
GET /runs?property=flowId=={FLOW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FLOW_ID}` | 監視するデータフローの一意の`id`値。 |

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

正常な応答は、作成日、ソース接続、ターゲット接続に関する情報、およびフロー実行の一意の識別子(`id`)を含む、フロー実行に関する詳細を返します。

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
| `activities` | データの変換方法を表示します。 |
| `durationSummary` | フロー実行の開始時刻と終了時刻。 |
| `sizeSummary` | データのボリューム（バイト単位）。 |
| `recordSummary` | データのレコード数。 |
| `fileSummary` | データのファイル数。 |
| `fileSummary.extensions` | アクティビティに固有の情報が含まれます。 例えば、`manifest`は「プロモーションアクティビティ」の一部に過ぎないので、`extensions`オブジェクトに含まれます。 |
| `statusSummary` | フロー実行が成功か失敗かを示します。 |

## 次の手順

このチュートリアルでは、[!DNL Flow Service] APIを使用してデータフローの指標とエラー情報を取得しました。 これで、取得スケジュールに応じて、データフローを引き続き監視し、ステータスと取得率を追跡できます。 ソースのデータフローを監視する方法の詳細については、ユーザーインターフェイス](../ui/monitor-sources.md)を使用したソースの[データフローの監視に関するチュートリアルを参照してください。 宛先のデータフローを監視する方法の詳細については、ユーザーインターフェイス](../ui/monitor-destinations.md)を使用した宛先の[データフローの監視に関するチュートリアルを参照してください。
