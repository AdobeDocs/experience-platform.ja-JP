---
solution: Experience Platform
title: セグメント書き出しジョブ API エンドポイント
description: 書き出しジョブは、オーディエンスセグメントメンバーをデータセットに保持するために使用される非同期プロセスです。 Adobe Experience Platform Segmentation Service APIの/export/jobs エンドポイントを使用すると、プログラムで書き出しジョブを取得、作成、キャンセルできます。
role: Developer
exl-id: 5b504a4d-291a-4969-93df-c23ff5994553
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '1678'
ht-degree: 32%

---

# セグメント書き出しジョブ エンドポイント

書き出しジョブは、オーディエンスセグメントメンバーをデータセットに保持するために使用される非同期プロセスです。 Adobe Experience Platform Segmentation APIで`/export/jobs` エンドポイントを使用できます。これにより、プログラムでエクスポート ジョブを取得、作成、キャンセルできます。

>[!NOTE]
>
>このガイドでは、[!DNL Segmentation API]での書き出しジョブの使用について説明します。 [!DNL Real-Time Customer Profile] データの書き出しジョブを管理する方法について詳しくは、[ プロファイル APIの書き出しジョブ ](../../profile/api/export-jobs.md)に関するガイドを参照してください

## はじめに

このガイドで使用されているエンドポイントは、[!DNL Adobe Experience Platform Segmentation Service] APIの一部です。 続行する前に、必須ヘッダーやサンプル API呼び出しの読み取り方法など、APIへの呼び出しを正常に行うために知っておく必要がある重要な情報については、[入門ガイド ](./getting-started.md)を確認してください。

## 書き出しジョブのリストの取得 {#retrieve-list}

GET リクエストを`/export/jobs` エンドポイントに行うことで、組織のすべての書き出しジョブのリストを取得できます。

**API 形式**

`/export/jobs` エンドポイントは、結果を絞り込むのに役立つ、複数のクエリパラメーターをサポートしています。これらのパラメーターはオプションですが、高価なオーバーヘッドを減らすために使用することを強くお勧めします。 パラメーターを指定せずにこのエンドポイントを呼び出すと、組織で使用可能なすべての書き出しジョブが取得されます。 複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。

```http
GET /export/jobs
GET /export/jobs?{QUERY_PARAMETERS}
```

**クエリパラメータ**

+++ 使用可能なクエリパラメーターのリスト。

| パラメーター | 説明 | 例 |
| --------- | ----------- | ------- |
| `limit` | 返される書き出しジョブの数を指定します。 | `limit=10` |
| `offset` | 結果のページのオフセットを指定します。 | `offset=1540974701302_96` |
| `status` | ステータスに基づいて結果をフィルターします。サポートされる値は、「NEW」、「SUCCEEDED」、「FAILED」です。 | `status=NEW` |

+++

**リクエスト**

次のリクエストは、組織内の最後の2つの書き出しジョブを取得します。

+++ 書き出しジョブを取得するためのサンプルリクエスト。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/export/jobs?limit=2 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

次の応答は、リクエストパスで指定されたクエリパラメーターに基づいて、正常に完了したエクスポートジョブのリストを含むHTTP ステータス 200を返します。

+++ 書き出しジョブを取得する際の応答のサンプル。

```json
{
    "records": [
        {
            "id": 100,
            "jobType": "BATCH",
            "destination": {
                "datasetId": "5b7c86968f7b6501e21ba9df",
                "segmentPerBatch": false,
                "batchId": "da5cfb4de32c4b93a09f7e37fa53ad52",
            },
            "fields": "identities.id,personalEmail.address",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "imsOrgId": "1BD6382559DF0C130A49422D@AdobeOrg",
            "status": "SUCCEEDED",
            "filter": {
                "segments": [
                    {
                        "segmentId": "52c26d0d-45f2-47a2-ab30-ed06abc981ff",
                        "segmentNs": "ups",
                        "status": [
                            "realized"
                        ]
                    }
                ]
            },
            "mergePolicy": {
                "id": "timestampOrdered-none-mp",
                "version": 1
            },
            "profileInstanceId": "ups",
            "errors": [
                {
                    "code": "0100000003",
                    "msg": "Error in Export Job",
                    "callStack": "com.adobe.aep.unifiedprofile.common.logging.Logger"
                }
            ],
            "metrics": {
                "totalTime": {
                    "startTimeInMs": 123456789000,
                    "endTimeInMs": 123456799000,
                    "totalTimeInMs": 10000
                },
                "profileExportTime": {
                    "startTimeInMs": 123456789000,
                    "endTimeInMs": 123456799000,
                    "totalTimeInMs": 10000
                },
                "totalExportedProfileCounter": 20,
                "exportedProfileByNamespaceCounter": {
                    "namespace1": 10,
                    "namespace2": 5
                }
            },
            "computeGatewayJobId": {
                "exportJob": "f3058161-7349-4ca9-807d-212cee2c2e94"
            },
            "creationTime": 1538615973895,
            "updateTime": 1538616233239,
            "requestId": "d995479c-8a08-4240-903b-af469c67be1f"
        },
        {
            "profileInstanceId": "test_xdm_latest_profile_20_e2e_1538573005395",
            "errors": [
                {
                    "code": "0090000009",
                    "msg": "Error writing profiles to output path 'adl://va7devprofilesnapshot.azuredatalakestore.net/snapshot/722'",
                    "callStack": "com.adobe.aep.unifiedprofile.common.logging.Logger" 
                },
                {
                    "code": "unknown",
                    "msg": "Job aborted.",
                    "callStack": "org.apache.spark.SparkException: Job aborted."
                }
            ],
            "jobType": "BATCH",
            "filter": {
                "segments": [
                    {
                        "segmentId": "52c26d0d-45f2-47a2-ab30-ed06abc981ff",
                        "segmentNs": "AAM",
                        "status": ["realized"]
                    }
                ]
            },
            "id": 722,
            "schema": {
                "name": "_xdm.context.profile"
            },
            "mergePolicy": {
                "id": "7972e3d6-96ea-4ece-9627-cbfd62709c5d",
                "version": 1
            },
            "status": "FAILED",
            "requestId": "KbOAsV7HXmdg262lc4yZZhoml27UWXPZ",
            "computeGatewayJobId": {
                "exportJob": "15971e0f-317c-4390-9038-1a0498eb356f"
            },
            "metrics": {
                "totalTime": {
                    "startTimeInMs": 1538573416687,
                    "endTimeInMs": 1538573922551,
                    "totalTimeInMs": 505864
                },
                "profileExportTime": {
                    "startTimeInMs": 1538573872211,
                    "endTimeInMs": 1538573918809,
                    "totalTimeInMs": 46598
                }
            },
            "destination": {
                "datasetId": "5bb4c46757920712f924a3eb",
                "segmentPerBatch": false,
                "batchId": "IWEQ6920712f9475762D"
            },
            "updateTime": 1538573922551,
            "imsOrgId": "1BD6382559DF0C130A49422D@AdobeOrg",
            "creationTime": 1538573416687
        }
    ],
    "page":{
        "sortField": "createdTime",
        "sort": "desc",
        "pageOffset": "1540974701302_96",
        "pageSize": 2
    },
    "link":{
        "next": "/export/jobs/?limit=2&offset=1538573416687_722"
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `destination` | 書き出されたデータの宛先情報：<ul><li>`datasetId`: データがエクスポートされたデータセットのID。</li><li>`segmentPerBatch`: セグメント IDが統合されているかどうかを示すブール値。 値「false」は、すべてのセグメント IDが単一のバッチ IDに書き出されることを意味します。 値「true」は、1つのセグメント IDが1つのバッチ IDに書き出されることを意味します。 **メモ：**&#x200B;値をtrueに設定すると、バッチ書き出しのパフォーマンスに影響する場合があります。</li></ul> |
| `fields` | 書き出されたフィールドのリストをコンマで区切ります。 |
| `schema.name` | データをエクスポートするデータセットに関連付けられているスキーマの名前。 |
| `filter.segments` | 書き出されるセグメント。 次のフィールドが含まれています。<ul><li>`segmentId`: プロファイルの書き出し先となるセグメント ID。</li><li>`segmentNs`：指定された`segmentID`のセグメント名前空間。</li><li>`status`: `segmentID`のステータス フィルターを提供する文字列の配列。 デフォルトでは、`status` は、現在の時刻にセグメントに含まれているすべてのプロファイルを表す値 `["realized"]` を持ちます。指定できる値は`realized`と`exited`です。 値`realized`は、プロファイルがセグメントに適格であることを意味します。 値`exiting`は、プロファイルがセグメントから離脱していることを意味します。</li></ul> |
| `mergePolicy` | 書き出されたデータのポリシー情報を結合します。 |
| `metrics.totalTime` | 書き出しジョブの実行にかかった合計時間を示すフィールド。 |
| `metrics.profileExportTime` | プロファイルが書き出されるまでの時間を示すフィールド。 |
| `page` | リクエストされた書き出しジョブのページネーションに関する情報。 |
| `link.next` | 書き出しジョブの次のページへのリンク。 |

+++

## 新しい書き出しジョブの作成 {#create}

`/export/jobs` エンドポイントに POST リクエストを実行することで、新しい書き出しジョブを作成できます。

**API 形式**

```http
POST /export/jobs
```

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターで設定された新しい書き出しジョブを作成します。

+++ 書き出しジョブを作成するためのサンプルリクエスト。

```shell
curl -X POST https://platform.adobe.io/data/core/ups/export/jobs \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
    "fields": "identities.id,personalEmail.address",
    "mergePolicy": {
        "id": "timestampOrdered-none-mp",
        "version": 1
    },
    "filter": {
        "segments": [
            {
                "segmentId": "52c26d0d-45f2-47a2-ab30-ed06abc981ff",
                "segmentNs": "ups",
                "status": [
                    "realized"
                ]
            }
        ],
        "segmentQualificationTime": {
            "startTime": "2018-01-01T00:00:00Z",
            "endTime": "2018-02-01T00:00:00Z"
        },
        "fromIngestTimestamp": "2018-01-01T00:00:00Z",
        "emptyProfiles": true
    },
    "additionalFields": {
        "eventList": {
            "fields": "string",
            "filter": {
                "fromIngestTimestamp": "2018-01-01T00:00:00Z",
                "toIngestTimestamp": "2020-01-01T00:00:00Z"
            }
        }
    },
    "destination":{
        "datasetId": "5b7c86968f7b6501e21ba9df",
        "segmentPerBatch": false
    },
    "schema":{
        "name": "_xdm.context.profile"
    },
    "evaluationInfo": {
        "segmentation": true
    }
}'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `fields` | コンマで区切った、書き出すフィールドのリスト。空白の場合、すべてのフィールドが書き出されます。 |
| `mergePolicy` | 書き出されたデータを管理する結合ポリシーを指定します。 複数のセグメントがエクスポートされる場合は、このパラメーターを含めます。指定されなかった場合、書き出しは指定されたセグメントと同じ結合ポリシーを使用します。 |
| `filter` | 以下に示すサブプロパティに応じて、書き出しジョブに含めるセグメントをID、修飾時間、または取り込み時間で指定するオブジェクト。 空白の場合、すべてのデータが書き出しされます。 |
| `filter.segments` | 書き出すセグメントを指定します。 この値を省略すると、すべてのプロファイルのすべてのデータがエクスポートされます。次のフィールドを含むセグメントオブジェクトの配列を受け入れます。<ul><li>`segmentId`：**（`segments` を使用する場合は必須）**&#x200B;エクスポートするプロファイルのセグメント ID。</li><li>`segmentNs` *（オプション）*&#x200B;指定した `segmentID` のセグメント名前空間。</li><li>`status` *（オプション）*`segmentID` のステータスフィルターを提供する文字列の配列。デフォルトでは、`status` は、現在の時刻にセグメントに含まれているすべてのプロファイルを表す値 `["realized"]` を持ちます。指定できる値は`realized`と`exited`です。  値`realized`は、プロファイルがセグメントに適格であることを意味します。 値`exiting`は、プロファイルがセグメントから離脱していることを意味します。</li></ul> |
| `filter.segmentQualificationTime` | セグメントの適格化時間にもとづいてフィルタリングします。 開始時間および/または終了時間を指定できます。 |
| `filter.segmentQualificationTime.startTime` | 特定のステータスのセグメント IDのセグメント選定の開始時間。 指定されていない場合、セグメント ID 選定の開始時間にフィルターは適用されません。タイムスタンプは [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式で指定する必要があります。 |
| `filter.segmentQualificationTime.endTime` | 特定のステータスのセグメント IDに対するセグメントの選定の終了時間。 指定されていない場合、セグメント ID 選定の終了時間にフィルターは適用されません。タイムスタンプは [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式で指定する必要があります。 |
| `filter.fromIngestTimestamp` | 書き出されたプロファイルには、このタイムスタンプの後に更新されたプロファイルのみが含まれるように制限されます。 タイムスタンプは [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式で指定する必要があります。 <ul><li>**プロファイル**&#x200B;の `fromIngestTimestamp`（指定されている場合）：更新された結合タイムスタンプが指定タイムスタンプよりも大きいすべての結合プロファイルが含まれます。`greater_than` オペランドがサポートされます。</li><li>**イベント**&#x200B;の `fromIngestTimestamp`：このタイムスタンプの後に取り込まれるすべてのイベントは、プロファイルの結果に応じてエクスポートされます。これは、イベント時間自体ではなく、イベントの取り込み時間です。</li> |
| `filter.emptyProfiles` | 空のプロファイルをフィルタリングするかどうかを示すブール値。 プロファイルには、プロファイルレコード、ExperienceEvent レコード、またはその両方を含めることができます。 プロファイルレコードがなく、ExperienceEvent レコードのみを含むプロファイルは、「emptyProfiles」と呼ばれます。 「emptyProfiles」を含めて、プロファイルストア内のすべてのプロファイルをエクスポートするには、`emptyProfiles` の値を `true` に設定します。`emptyProfiles`が`false`に設定されている場合、ストア内のプロファイルレコードを持つプロファイルのみが書き出されます。 デフォルトでは、`emptyProfiles`属性が含まれていない場合、プロファイルレコードを含むプロファイルのみが書き出されます。 |
| `additionalFields.eventList` | 子オブジェクトまたは関連オブジェクトに書き出される時系列イベントフィールドを制御するには、次の設定を1つ以上指定します。<ul><li>`fields`：エクスポートするフィールドを制御します。</li><li>`filter`：関連オブジェクトから取得される結果を制限する基準を指定します。エクスポートに必要な最小値（通常は日付）が基準として予期されます。</li><li>`filter.fromIngestTimestamp`：指定されたタイムスタンプの後に取り込まれたイベントに対して、時系列イベントをフィルタリングします。 これは、イベント時間自体ではなく、イベントの取り込み時間です。</li><li>`filter.toIngestTimestamp`：指定されたタイムスタンプより前に取り込まれたタイムスタンプに対して、タイムスタンプをフィルタリングします。 これは、イベント時間自体ではなく、イベントの取り込み時間です。</li></ul> |
| `destination` | **（必須）**&#x200B;書き出されたデータに関する情報：<ul><li>`datasetId`：**（必須）**&#x200B;データのエクスポート先のデータセットの ID。</li><li>`segmentPerBatch`: *（オプション）*&#x200B;指定しない場合はデフォルトで「false」になるブール値。 値が「false」の場合、すべてのセグメント IDが単一のバッチ IDに書き出されます。 値「true」を指定すると、1つのセグメント IDが1つのバッチ IDに書き出されます。 値を「true」に設定すると、一括書き出しのパフォーマンスに影響する場合があります。</li></ul> |
| `schema.name` | **（必須）**&#x200B;データのエクスポート先のデータセットに関連付けられているスキーマの名前。 |
| `evaluationInfo.segmentation` | *（オプション）*&#x200B;指定しない場合はデフォルトで`false`になるブール値。 値`true`は、書き出しジョブでセグメント化を行う必要があることを示します。 |

+++

**応答**

正常な応答は、HTTP ステータス 200 と、新しく作成された書き出しジョブの詳細を返します。

+++ 書き出しジョブを作成する際の応答のサンプル。

```json
{
    "id": 100,
    "jobType": "BATCH",
    "destination": {
        "datasetId": "5b7c86968f7b6501e21ba9df",
        "segmentPerBatch": false,
        "batchId": "da5cfb4de32c4b93a09f7e37fa53ad52"
    },
    "fields": "identities.id,personalEmail.address",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "imsOrgId": "{ORG_ID}",
    "status": "NEW",
    "filter": {
        "segments": [
            {
                "segmentId": "52c26d0d-45f2-47a2-ab30-ed06abc981ff",
                "segmentNs": "ups",
                "status": [
                    "realized"
                ]
            }
        ],
        "segmentQualificationTime": {
            "startTime": "2018-01-01T00:00:00Z",
            "endTime": "2018-02-01T00:00:00Z"
        },
        "fromIngestTimestamp": "2018-01-01T00:00:00Z",
        "emptyProfiles": true
    },
    "additionalFields": {
        "eventList": {
            "fields": "_id, _experience",
            "filter": {
                "fromIngestTimestamp": "2018-01-01T00:00:00Z"
            }
        }
    },
    "mergePolicy": {
        "id": "timestampOrdered-none-mp",
        "version": 1
    },
    "profileInstanceId": "ups",
    "metrics": {
        "totalTime": {
            "startTimeInMs": 123456789000,
        }
    },
    "computeGatewayJobId": {
        "exportJob": ""    
    },
    "creationTime": 1538615973895,
    "updateTime": 1538616233239,
    "requestId": "d995479c-8a08-4240-903b-af469c67be1f"
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | 作成したばかりの書き出しジョブを識別する、システム生成の読み取り専用の値。 |

または、`destination.segmentPerBatch`が`true`に設定されている場合、上の`destination` オブジェクトには、次に示すように`batches`配列が含まれます。

```json
    "destination": {
        "dataSetId": "{DATASET_ID}",
        "segmentPerBatch": true,
        "batches": [
            {
                "segmentId": "segment1",
                "segmentNs": "ups",
                "status": ["realized"],
                "batchId": "da5cfb4de32c4b93a09f7e37fa53ad52"
            },
            {
                "segmentId": "segment2",
                "segmentNs": "AdCloud",
                "status": "exited",
                "batchId": "df4gssdfb93a09f7e37fa53ad52"
            }
        ]
    }
```

+++

## 特定の書き出しジョブの取得 {#get}

`/export/jobs` エンドポイントに対してGET リクエストを行い、取得するエクスポートジョブのIDをリクエストパスに指定することで、特定のエクスポートジョブに関する詳細な情報を取得できます。

**API 形式**

```http
GET /export/jobs/{EXPORT_JOB_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{EXPORT_JOB_ID}` | アクセスするエクスポートジョブの `id`。 |

**リクエスト**

+++ 書き出しジョブを取得するためのサンプルリクエスト。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/export/jobs/11037 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

正常な応答は、HTTP ステータス 200 と、指定された書き出しジョブに関する詳細情報を返します。

+++ 書き出しジョブを取得する際の応答のサンプル。

```json
{
    "id": 11037,
    "jobType": "BATCH",
    "destination": {
        "datasetId": "5b7c86968f7b6501e21ba9df",
        "segmentPerBatch": false,
        "batchId": "da5cfb4de32c4b93a09f7e37fa53ad52"
    },
    "fields": "identities.id,personalEmail.address",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "imsOrgId": "{ORG_ID}",
    "status": "SUCCEEDED",
    "filter": {
        "segments": [
            {
                "segmentId": "52c26d0d-45f2-47a2-ab30-ed06abc981ff",
                "segmentNs": "ups",
                "status":[
                    "realized"
                ]
            }
        ]
    },
    "mergePolicy": {
        "id": "timestampOrdered-none-mp",
        "version": 1
    },
    "profileInstanceId": "ups",
    "metrics": {
        "totalTime": {
            "startTimeInMs": 123456789000,
            "endTimeInMs": 123456799000,
            "totalTimeInMs": 10000
        },
        "profileExportTime": {
            "startTimeInMs": 123456789000,
            "endTimeInMs": 123456799000,
            "totalTimeInMs": 10000
        },
        "totalExportedProfileCounter": 20,
        "exportedProfileByNamespaceCounter": {
            "namespace1": 10,
            "namespace2": 5
        }
    },
    "computeGatewayJobId": {
        "exportJob": "f3058161-7349-4ca9-807d-212cee2c2e94"
    },
    "creationTime": 1538615973895,
    "updateTime": 1538616233239,
    "requestId": "d995479c-8a08-4240-903b-af469c67be1f"
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `destination` | 書き出されたデータの宛先情報：<ul><li>`datasetId`: データがエクスポートされたデータセットのID。</li><li>`segmentPerBatch`: セグメント IDが統合されているかどうかを示すブール値。 値`false`は、すべてのセグメント IDが単一のバッチ IDになったことを意味します。 値`true`は、1つのセグメント IDが1つのバッチ IDに書き出されることを意味します。</li></ul> |
| `fields` | 書き出されたフィールドのリストをコンマで区切ります。 |
| `schema.name` | データをエクスポートするデータセットに関連付けられているスキーマの名前。 |
| `filter.segments` | 書き出されるセグメント。 次のフィールドが含まれています。<ul><li>`segmentId`: エクスポートするプロファイルのセグメント ID。</li><li>`segmentNs`：指定された`segmentID`のセグメント名前空間。</li><li>`status`: `segmentID`のステータス フィルターを提供する文字列の配列。 デフォルトでは、`status` は、現在の時刻にセグメントに含まれているすべてのプロファイルを表す値 `["realized"]` を持ちます。指定できる値は`realized`と`exited`です。  値`realized`は、プロファイルがセグメントに適格であることを意味します。 値`exiting`は、プロファイルがセグメントから離脱していることを意味します。</li></ul> |
| `mergePolicy` | 書き出されたデータのポリシー情報を結合します。 |
| `metrics.totalTime` | 書き出しジョブの実行にかかった合計時間を示すフィールド。 |
| `metrics.profileExportTime` | プロファイルが書き出されるまでの時間を示すフィールド。 |
| `totalExportedProfileCounter` | すべてのバッチでエクスポートされたプロファイルの合計数です。 |

+++

## 特定の書き出しジョブのキャンセルまたは削除 {#delete}

指定された書き出しジョブの削除をリクエストするには、`/export/jobs` エンドポイントに対してDELETE リクエストを行い、削除する書き出しジョブのIDをリクエストパスに指定します。

**API 形式**

```http
DELETE /export/jobs/{EXPORT_JOB_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{EXPORT_JOB_ID}` | 削除する書き出しジョブの`id`。 |

**リクエスト**

+++ 書き出しジョブを削除するサンプルリクエスト。

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/export/jobs/{EXPORT_JOB_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

正常な応答は、HTTP ステータス 204 と次のメッセージを返します。

```json
{
  "status": true,
  "message": "Export job has been marked for cancelling"
}
```

## 次の手順

このガイドを読むと、書き出しジョブの仕組みがより深く理解できるようになります。
