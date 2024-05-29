---
solution: Experience Platform
title: セグメント書き出しジョブ API エンドポイント
description: 書き出しジョブは、オーディエンスセグメントメンバーをデータセットに保持するために使用される非同期プロセスです。 Adobe Experience Platform Segmentation Service API で/export/jobs エンドポイントを使用できます。これにより、書き出しジョブをプログラムによって取得、作成およびキャンセルできます。
role: Developer
exl-id: 5b504a4d-291a-4969-93df-c23ff5994553
source-git-commit: e52eb90b64ae9142e714a46017cfd14156c78f8b
workflow-type: tm+mt
source-wordcount: '1615'
ht-degree: 33%

---

# セグメント書き出しジョブエンドポイント

書き出しジョブは、オーディエンスセグメントメンバーをデータセットに保持するために使用される非同期プロセスです。 を使用できます `/export/jobs` Adobe Experience Platform Segmentation API のエンドポイント。プログラムによって書き出しジョブを取得、作成およびキャンセルできます。

>[!NOTE]
>
>このガイドでは、でのエクスポートジョブの使用について説明します [!DNL Segmentation API]. の書き出しジョブを管理する方法について [!DNL Real-Time Customer Profile] データについては、のガイドを参照してください。 [プロファイル API でのジョブの書き出し](../../profile/api/export-jobs.md)

## はじめに

このガイドで使用するエンドポイントは、 [!DNL Adobe Experience Platform Segmentation Service] API です。 続行する前に、を確認してください [はじめる前に](./getting-started.md) は、必要なヘッダーやサンプル API 呼び出しの読み取り方法など、API に対する呼び出しを正常に実行するために知っておく必要がある重要な情報です。

## 書き出しジョブのリストの取得 {#retrieve-list}

に対してGETリクエストをおこなうと、組織のすべての書き出しジョブのリストを取得できます。 `/export/jobs` エンドポイント。

**API 形式**

`/export/jobs` エンドポイントは、結果を絞り込むのに役立つ、複数のクエリパラメーターをサポートしています。これらのパラメーターはオプションですが、高価なオーバーヘッドの削減に役立てるため、使用することを強くお勧めします。 パラメーターを指定せずにこのエンドポイントを呼び出すと、組織で使用可能なすべての書き出しジョブが取得されます。 複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。

```http
GET /export/jobs
GET /export/jobs?limit={LIMIT}
GET /export/jobs?offset={OFFSET}
GET /export/jobs?status={STATUS}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{LIMIT}` | 返される書き出しジョブの数を指定します。 |
| `{OFFSET}` | 結果のページのオフセットを指定します。 |
| `{STATUS}` | ステータスに基づいて結果をフィルターします。サポートされる値は、「NEW」、「SUCCEEDED」、「FAILED」です。 |

**リクエスト**

次のリクエストでは、組織内の最後の 2 つのエクスポートジョブを取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/export/jobs?limit=2 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

次の応答は、リクエストパスで指定されたクエリパラメーターに基づいて、HTTP ステータス 200 と、正常に完了したエクスポートジョブのリストを返します。

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
| `destination` | 書き出されたデータの宛先情報：<ul><li>`datasetId`：データが書き出されたデータセットの ID。</li><li>`segmentPerBatch`：セグメント ID が統合されるかどうかを示すブール値。 値が「false」の場合、すべてのセグメント ID が単一のバッチ ID に書き出されます。 値が「true」の場合、1 つのセグメント ID が 1 つのバッチ ID に書き出されます。 **注意：** 値を true に設定すると、バッチエクスポートのパフォーマンスに影響を与える可能性があります。</li></ul> |
| `fields` | コンマで区切られた書き出されたフィールドのリスト。 |
| `schema.name` | データを書き出すデータセットに関連付けられたスキーマの名前。 |
| `filter.segments` | 書き出されるセグメント。 次のフィールドが含まれています。<ul><li>`segmentId`：プロファイルの書き出し先のセグメント ID。</li><li>`segmentNs`：特定ののセグメント名前空間 `segmentID`.</li><li>`status`：のステータスフィルターを提供する文字列の配列 `segmentID`. デフォルトでは、`status` は、現在の時刻にセグメントに含まれているすべてのプロファイルを表す値 `["realized"]` を持ちます。使用可能な値は次のとおりです。 `realized` および `exited`. 値 `realized` は、プロファイルがセグメントの対象となることを意味します。 値 `exiting` プロファイルがセグメントから離脱していることを意味します。</li></ul> |
| `mergePolicy` | エクスポートされたデータの結合ポリシー情報。 |
| `metrics.totalTime` | エクスポートジョブの実行にかかった合計時間を示すフィールド。 |
| `metrics.profileExportTime` | プロファイルのエクスポートに要した時間を示すフィールド。 |
| `page` | リクエストされた書き出しジョブのページネーションに関する情報。 |
| `link.next` | 書き出しジョブの次のページへのリンク。 |

## 新しい書き出しジョブの作成 {#create}

`/export/jobs` エンドポイントに POST リクエストを実行することで、新しい書き出しジョブを作成できます。

**API 形式**

```http
POST /export/jobs
```

**リクエスト**

次のリクエストは、ペイロード内のパラメーター設定に基づいて、新しいエクスポートジョブを作成します。

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
| `mergePolicy` | 書き出されたデータを制御する結合ポリシーを指定します。 複数のセグメントがエクスポートされる場合は、このパラメーターを含めます。指定されなかった場合、書き出しは指定されたセグメントと同じ結合ポリシーを使用します。 |
| `filter` | 以下に示すサブプロパティに応じて、ID、認定時間、または取り込み時間ごとに書き出しジョブに含めるセグメントを指定するオブジェクト。 空白の場合、すべてのデータが書き出しされます。 |
| `filter.segments` | 書き出すセグメントを指定します。 この値を省略すると、すべてのプロファイルのすべてのデータがエクスポートされます。次のフィールドを含むセグメントオブジェクトの配列を受け入れます。<ul><li>`segmentId`：**（`segments` を使用する場合は必須）**&#x200B;エクスポートするプロファイルのセグメント ID。</li><li>`segmentNs` *（オプション）*&#x200B;指定した `segmentID` のセグメント名前空間。</li><li>`status` *（オプション）*`segmentID` のステータスフィルターを提供する文字列の配列。デフォルトでは、`status` は、現在の時刻にセグメントに含まれているすべてのプロファイルを表す値 `["realized"]` を持ちます。使用可能な値は次のとおりです。 `realized` および `exited`.  値 `realized` は、プロファイルがセグメントの対象となることを意味します。 値 `exiting` プロファイルがセグメントから離脱していることを意味します。</li></ul> |
| `filter.segmentQualificationTime` | セグメントの選定時間に基づいてフィルタリングします。 開始時間および/または終了時間を指定できます。 |
| `filter.segmentQualificationTime.startTime` | 特定のステータスのセグメント ID に対するセグメントの選定の開始時間。 指定されていない場合、セグメント ID 認定の開始時間にフィルターは適用されません。タイムスタンプは [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式で指定する必要があります。 |
| `filter.segmentQualificationTime.endTime` | 特定のステータスのセグメント ID に対するセグメントの選定の終了時間。 指定されていない場合、セグメント ID 認定の終了時間にフィルターは適用されません。タイムスタンプは [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式で指定する必要があります。 |
| `filter.fromIngestTimestamp ` | 書き出されるプロファイルを、このタイムスタンプ以降に更新されたプロファイルのみを含むように制限します。 タイムスタンプは [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式で指定する必要があります。 <ul><li>**プロファイル**&#x200B;の `fromIngestTimestamp`（指定されている場合）：更新された結合タイムスタンプが指定タイムスタンプよりも大きいすべての結合プロファイルが含まれます。`greater_than` オペランドがサポートされます。</li><li>**イベント**&#x200B;の `fromIngestTimestamp`：このタイムスタンプの後に取り込まれるすべてのイベントは、プロファイルの結果に応じてエクスポートされます。これは、イベント時間自体ではなく、イベントの取得時間です。</li> |
| `filter.emptyProfiles` | 空のプロファイルをフィルタリングするかどうかを示すブール値。 プロファイルには、プロファイルレコード、ExperienceEvent レコード、またはその両方を含めることができます。 プロファイルレコードを持たず、ExperienceEvent レコードのみを持つプロファイルは、「emptyProfiles」と呼ばれます。 「emptyProfiles」を含めて、プロファイルストア内のすべてのプロファイルをエクスポートするには、`emptyProfiles` の値を `true` に設定します。次の場合 `emptyProfiles` はに設定されています。 `false`で、ストアにプロファイルレコードがあるプロファイルのみが書き出されます。 デフォルトでは、次の場合： `emptyProfiles` 属性は含まれません。プロファイルレコードを含んだプロファイルのみが書き出されます。 |
| `additionalFields.eventList` | 次の 1 つ以上の設定を指定して、子または関連するオブジェクト用に書き出された時系列イベントフィールドを制御します。<ul><li>`fields`：エクスポートするフィールドを制御します。</li><li>`filter`：関連オブジェクトから取得される結果を制限する基準を指定します。エクスポートに必要な最小値（通常は日付）が基準として予期されます。</li><li>`filter.fromIngestTimestamp`：時系列イベントを、指定されたタイムスタンプ以降に取り込まれたイベントにフィルターします。 これは、イベント時間自体ではなく、イベントの取得時間です。</li><li>`filter.toIngestTimestamp`：タイムスタンプを、指定されたタイムスタンプより前に取り込まれたタイムスタンプにフィルタリングします。 これは、イベント時間自体ではなく、イベントの取得時間です。</li></ul> |
| `destination` | **（必須）** エクスポートされたデータに関する情報：<ul><li>`datasetId`：**（必須）**&#x200B;データのエクスポート先のデータセットの ID。</li><li>`segmentPerBatch`: *（オプション）* ブール値。指定されていない場合は、デフォルトで「false」に設定されます。 値が「false」の場合、すべてのセグメント ID を 1 つのバッチ ID にエクスポートします。 値が「true」の場合、1 つのセグメント ID を 1 つのバッチ ID にエクスポートします。 値を「true」に設定すると、バッチエクスポートのパフォーマンスに影響を与える可能性があります。</li></ul> |
| `schema.name` | **（必須）**&#x200B;データのエクスポート先のデータセットに関連付けられているスキーマの名前。 |
| `evaluationInfo.segmentation` | *（オプション）* ブール値。指定されていない場合は、デフォルトで次のように設定されます `false`. 値 `true` は、書き出しジョブでセグメント化を行う必要があることを示します。 |

**応答**

正常な応答は、HTTP ステータス 200 と、新しく作成された書き出しジョブの詳細を返します。

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
| `id` | 作成されたエクスポートジョブを識別する、システム生成の読み取り専用の値。 |

または、次の場合： `destination.segmentPerBatch` はに設定されていました `true`, `destination` 上記のオブジェクトにはが含まれています `batches` 配列。次に示します。

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

## 特定の書き出しジョブの取得 {#get}

特定のエクスポートジョブに関する詳細な情報を取得するには、に対してGETリクエストを実行します。 `/export/jobs` エンドポイントと、取得するエクスポートジョブの ID をリクエストパスで指定します。

**API 形式**

```http
GET /export/jobs/{EXPORT_JOB_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{EXPORT_JOB_ID}` | アクセスするエクスポートジョブの `id`。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/export/jobs/11037 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、HTTP ステータス 200 と、指定された書き出しジョブに関する詳細情報を返します。

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
| `destination` | 書き出されたデータの宛先情報：<ul><li>`datasetId`：データが書き出されたデータセットの ID。</li><li>`segmentPerBatch`：セグメント ID が統合されるかどうかを示すブール値。 値 `false` は、すべてのセグメント ID が単一のバッチ ID になったことを意味します。 値 `true` は、1 つのセグメント ID が 1 つのバッチ ID に書き出されることを意味します。</li></ul> |
| `fields` | コンマで区切られた書き出されたフィールドのリスト。 |
| `schema.name` | データを書き出すデータセットに関連付けられたスキーマの名前。 |
| `filter.segments` | 書き出されるセグメント。 次のフィールドが含まれています。<ul><li>`segmentId`：書き出すプロファイルのセグメント ID。</li><li>`segmentNs`：特定ののセグメント名前空間 `segmentID`.</li><li>`status`：のステータスフィルターを提供する文字列の配列 `segmentID`. デフォルトでは、`status` は、現在の時刻にセグメントに含まれているすべてのプロファイルを表す値 `["realized"]` を持ちます。使用可能な値は次のとおりです。 `realized` および `exited`.  値 `realized` は、プロファイルがセグメントの対象となることを意味します。 値 `exiting` プロファイルがセグメントから離脱していることを意味します。</li></ul> |
| `mergePolicy` | エクスポートされたデータの結合ポリシー情報。 |
| `metrics.totalTime` | エクスポートジョブの実行にかかった合計時間を示すフィールド。 |
| `metrics.profileExportTime` | プロファイルのエクスポートに要した時間を示すフィールド。 |
| `totalExportedProfileCounter` | すべてのバッチでエクスポートされたプロファイルの合計数。 |

## 特定の書き出しジョブのキャンセルまたは削除 {#delete}

に対してDELETEリクエストを行うことで、指定したエクスポートジョブの削除をリクエストできます。 `/export/jobs` エンドポイントと、リクエストパスで削除するエクスポートジョブの ID を指定します。

**API 形式**

```http
DELETE /export/jobs/{EXPORT_JOB_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{EXPORT_JOB_ID}` | この `id` 削除するエクスポートジョブの。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/export/jobs/{EXPORT_JOB_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、HTTP ステータス 204 と次のメッセージを返します。

```json
{
  "status": true,
  "message": "Export job has been marked for cancelling"
}
```

## 次の手順

このガイドを読むことで、書き出しジョブの仕組みについて理解が深まりました。
