---
keywords: Experience Platform;ホーム;人気のトピック;データフローのモニター;フローサービス API;フローサービス
solution: Experience Platform
title: Flow Service API を使用したデータフローのモニター
type: Tutorial
description: このチュートリアルでは、Flow Service API を使用して、完全性、エラーおよび指標のフロー実行データをモニタリングする手順を説明します。
exl-id: c4b2db97-eba4-460d-8c00-c76c666ed70e
source-git-commit: 293aa66115ae4579c598e23bf1655d835c8694ae
workflow-type: tm+mt
source-wordcount: '770'
ht-degree: 57%

---

# Flow Service API を使用したデータフローのモニター

Adobe Experience Platform では、外部ソースからデータを取り込むと同時に、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、および拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。 さらに、Experience Platformを使用すれば、データを外部パートナーにアクティベートできます。

[!DNL Flow Service]は、Adobe Experience Platform内の様々な異なるソースから顧客データを収集および一元化するために使用されます。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされているすべてのソースと宛先を接続可能にします。

このチュートリアルでは、[[!DNL Flow Service API]](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して、完全性、エラー、指標に関するフロー実行データを監視する手順について説明します。

## はじめに

このチュートリアルでは、有効なデータフローの ID 値が必要です。 有効なデータフローIDがない場合は、[ ソースの概要](../../sources/home.md)または[宛先の概要](../../destinations/catalog/overview.md)から選択したコネクタを選択し、このチュートリアルを試す前に概説されている手順に従います。

このチュートリアルでは、Adobe Experience Platform の次のコンポーネントについて十分に理解していることを前提にしています。

- [宛先](../../destinations/home.md)：宛先は、一般的に使用されるアプリケーションとの事前定義済みの統合であり、クロスチャネルマーケティング施策、メールキャンペーン、ターゲット広告などの多くのユースケースで、Experience Platformからのデータをシームレスに活用することができます。
- [ソース](../../sources/home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
- [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] APIを使用してフロー実行を正常に監視するために知っておく必要がある追加情報を示します。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。 これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。 また、API レスポンスで返されるサンプル JSON も示されています。 ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Experience Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。 次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- `Authorization: Bearer {ACCESS_TOKEN}`
- `x-api-key: {API_KEY}`
- `x-gw-ims-org-id: {ORG_ID}`

[!DNL Flow Service]に属するリソースを含む、[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されます。 [!DNL Experience Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

- `Content-Type: application/json`

## フロー実行の監視

データフローを作成したら、[!DNL Flow Service] APIに対してGET リクエストを実行します。

**API 形式**

```http
GET /runs?property=flowId=={FLOW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FLOW_ID}` | モニターしたいデータフローの一意の `id` 値。 |

**リクエスト**

次のリクエストは、既存のデータフローの仕様を取得します。

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/runs?property=flowId==c9cef9cb-c934-4467-8ef9-cbc934546741' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、作成日、ソース接続、ターゲット接続に関する情報、フロー実行の一意の識別子（`id`）など、フロー実行に関する詳細を返します。

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
            "imsOrgId": "{ORG_ID}",
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
| `activities` | データがどのように変換されるかを示します。 |
| `durationSummary` | フロー実行の開始時間と終了時間。 |
| `sizeSummary` | データのボリューム（バイト単位）。 |
| `recordSummary` | データのレコード数。 |
| `fileSummary` | データのファイル数。 |
| `fileSummary.extensions` | アクティビティに固有の情報が含まれます。 例えば、`manifest`は「プロモーションアクティビティ」の一部にすぎないため、`extensions` オブジェクトに含まれます。 |
| `statusSummary` | フロー実行が成功か失敗かを示します。 |

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して、データフローの指標とエラー情報を取得しました。 これで、取り込みスケジュールに応じて、データフローを引き続きモニターし、そのステータスと取り込み率をトラックできるようになります。 ソースのデータフローを監視する方法について詳しくは、[ ユーザーインターフェイス ](../ui/monitor-sources.md) チュートリアルを使用したソースのデータフローの監視を参照してください。 宛先のデータフローを監視する方法について詳しくは、[ ユーザーインターフェイス ](../ui/monitor-destinations.md) チュートリアルを使用した宛先のデータフローの監視を参照してください。

複数のXDM エンティティをデータフローに送信するには、HTTP リクエストで`messages`配列を使用するか、複数のレコードを含むファイル（CSV、JSON、Parquet）をアップロードします。 詳細なガイダンスとベストプラクティスについては、[複数のXDM エンティティをデータフローに送信する方法](../../ingestion/tutorials/streaming-multiple-messages.md#send-multiple-xdm-entities-to-a-dataflow)を参照してください。
