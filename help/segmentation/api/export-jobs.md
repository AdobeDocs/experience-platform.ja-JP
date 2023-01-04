---
keywords: Experience Platform；ホーム；人気の高いトピック；セグメント化；セグメント化；セグメント化サービス；書き出しジョブ；api;
solution: Experience Platform
title: セグメント書き出しジョブ API エンドポイント
topic-legacy: developer guide
description: 書き出しジョブは、オーディエンスセグメントメンバーをデータセットに永続化するために使用される非同期プロセスです。 Adobe Experience Platform Segmentation Service API の/export/jobs エンドポイントを使用すると、書き出しジョブをプログラムで取得、作成およびキャンセルできます。
exl-id: 5b504a4d-291a-4969-93df-c23ff5994553
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '1682'
ht-degree: 35%

---

# セグメント書き出しジョブエンドポイント

書き出しジョブは、オーディエンスセグメントメンバーをデータセットに永続化するために使用される非同期プロセスです。 以下を使用して、 `/export/jobs` Adobe Experience Platform Segmentation API のエンドポイント。このエンドポイントを使用すると、書き出しジョブをプログラムで取得、作成およびキャンセルできます。

>[!NOTE]
>
>このガイドでは、 [!DNL Segmentation API]. の書き出しジョブの管理方法の詳細 [!DNL Real-Time Customer Profile] データについては、 [プロファイル API でのジョブの書き出し](../../profile/api/export-jobs.md)

## はじめに

このガイドで使用する エンドポイントは、[!DNL Adobe Experience Platform Segmentation Service]API の一部です。続行する前に、 [入門ガイド](./getting-started.md) を参照してください。

## 書き出しジョブのリストの取得 {#retrieve-list}

IMS 組織のすべての書き出しジョブのリストを取得するには、`/export/jobs` エンドポイントに GET リクエストを実行します。

**API 形式**

`/export/jobs` エンドポイントは、結果を絞り込むのに役立つ、複数のクエリパラメーターをサポートしています。これらのパラメーターはオプションですが、高価なオーバーヘッドを削減するために、使用を強くお勧めします。 パラメーターを指定しないでこのエンドポイントを呼び出すと、組織で使用可能なすべての書き出しジョブが取得されます。複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。

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

次のリクエストは、IMS 組織内の最後の 2 つの書き出しジョブを取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/export/jobs?limit=2 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

次の応答は、リクエストパスで指定されたクエリパラメーターに基づいて、正常に完了した書き出しジョブのリストと共に HTTP ステータス 200 を返します。

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
                        "status": ["realized", "existing"]
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
| `destination` | エクスポートするデータの宛先情報：<ul><li>`datasetId`:データが書き出されたデータセットの ID。</li><li>`segmentPerBatch`:セグメント ID が統合されているかどうかを示す Boolean 値です。 値が「false」の場合、すべてのセグメント ID が単一のバッチ ID に書き出されます。 値が「true」の場合、1 つのセグメント ID が 1 つのバッチ ID に書き出されます。 **注意：** 値を true に設定すると、バッチエクスポートのパフォーマンスに影響を与える場合があります。</li></ul> |
| `fields` | コンマで区切った、書き出すフィールドのリスト。 |
| `schema.name` | データのエクスポート先のデータセットに関連付けられているスキーマの名前。 |
| `filter.segments` | 書き出されるセグメント。 次のフィールドが含まれます。<ul><li>`segmentId`:プロファイルの書き出し先のセグメント ID。</li><li>`segmentNs`:指定したのセグメント名前空間 `segmentID`.</li><li>`status`:のステータスフィルターを提供する文字列の配列 `segmentID`. デフォルトでは、`status` は、現在の時刻にセグメントに含まれているすべてのプロファイルを表す値 `["realized", "existing"]` を持ちます。次の値を指定できます。「適合」、「既存」および「出口」。 値が「適合」の場合は、プロファイルがセグメントに入っていることを意味します。 値が「existing」の場合、プロファイルは引き続きセグメント内に存在します。 値「exiting」は、プロファイルがセグメントから退出していることを意味します。</li></ul> |
| `mergePolicy` | エクスポートされたデータの結合ポリシー情報。 |
| `metrics.totalTime` | 書き出しジョブの実行に要した合計時間を示すフィールド。 |
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

次のリクエストは、ペイロードで指定されたパラメーターで設定された新しい書き出しジョブを作成します。

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
| `mergePolicy` | エクスポートされるデータを管理する結合ポリシーを指定します。 複数のセグメントがエクスポートされる場合は、このパラメーターを含めます。指定されなかった場合、書き出しは指定されたセグメントと同じ結合ポリシーを使用します。 |
| `filter` | 以下に示すサブプロパティに応じて、ID、認定時間、または取り込み時間で、書き出しジョブに含めるセグメントを指定するオブジェクト。 空白の場合、すべてのデータが書き出しされます。 |
| `filter.segments` | 書き出すセグメントを指定します。 この値を省略すると、すべてのプロファイルのすべてのデータがエクスポートされます。次のフィールドを含むセグメントオブジェクトの配列を受け入れます。<ul><li>`segmentId`：**（`segments` を使用する場合は必須）**&#x200B;エクスポートするプロファイルのセグメント ID。</li><li>`segmentNs` *（オプション）*&#x200B;指定した `segmentID` のセグメント名前空間。</li><li>`status` *（オプション）*`segmentID` のステータスフィルターを提供する文字列の配列。デフォルトでは、`status` は、現在の時刻にセグメントに含まれているすべてのプロファイルを表す値 `["realized", "existing"]` を持ちます。`"realized"`、`"existing"`、`"exited"` などの値が使用されます。値が「適合」の場合は、プロファイルがセグメントに入っていることを意味します。 値が「existing」の場合、プロファイルは引き続きセグメント内に存在します。 値「exiting」は、プロファイルがセグメントから退出していることを意味します。</li></ul> |
| `filter.segmentQualificationTime` | セグメント認定時間に基づくフィルター。 開始時間および/または終了時間を指定できます。 |
| `filter.segmentQualificationTime.startTime` | 特定のステータスのセグメント ID のセグメント認定開始時間。 指定されていない場合、セグメント ID 認定の開始時間にフィルターは適用されません。タイムスタンプは [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式で指定する必要があります。 |
| `filter.segmentQualificationTime.endTime` | 特定のステータスのセグメント ID のセグメント認定終了時間。 指定されていない場合、セグメント ID 認定の終了時間にフィルターは適用されません。タイムスタンプは [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式で指定する必要があります。 |
| `filter.fromIngestTimestamp ` | 書き出すプロファイルに、このタイムスタンプの後に更新されたプロファイルのみが含まれるように制限します。 タイムスタンプは [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式で指定する必要があります。 <ul><li>**プロファイル**&#x200B;の `fromIngestTimestamp`（指定されている場合）：更新された結合タイムスタンプが指定タイムスタンプよりも大きいすべての結合プロファイルが含まれます。`greater_than` オペランドがサポートされます。</li><li>**イベント**&#x200B;の `fromIngestTimestamp`：このタイムスタンプの後に取り込まれるすべてのイベントは、プロファイルの結果に応じてエクスポートされます。これは、イベント時間自体ではなく、イベントの取得時間です。</li> |
| `filter.emptyProfiles` | 空のプロファイルをフィルタリングするかどうかを示す boolean 値です。 プロファイルには、プロファイルレコードと ExperienceEvent レコードのどちらか、または両方を含めることができます。 プロファイルレコードがなく、ExperienceEvent レコードのみが含まれるプロファイルは、「emptyProfiles」と呼ばれます。 「emptyProfiles」を含め、プロファイルストア内のすべてのプロファイルをエクスポートするには、の値を設定します。 `emptyProfiles` から `true`. If `emptyProfiles` が `false`に設定すると、ストア内のプロファイルレコードを持つプロファイルのみがエクスポートされます。 デフォルトでは、 `emptyProfiles` 属性が含まれていない場合、プロファイルレコードを含むプロファイルのみがエクスポートされます。 |
| `additionalFields.eventList` | 次の 1 つ以上の設定を指定して、子オブジェクトまたは関連オブジェクト用に書き出される時系列イベントフィールドを制御します。<ul><li>`fields`：エクスポートするフィールドを制御します。</li><li>`filter`：関連オブジェクトから取得される結果を制限する基準を指定します。エクスポートに必要な最小値（通常は日付）が基準として予期されます。</li><li>`filter.fromIngestTimestamp`:時系列イベントをフィルター処理して、指定されたタイムスタンプの後に取り込まれたイベントに絞り込みます。 これは、イベント時間自体ではなく、イベントの取得時間です。</li><li>`filter.toIngestTimestamp`:指定されたタイムスタンプの前に取り込まれたタイムスタンプにタイムスタンプをフィルターします。 これは、イベント時間自体ではなく、イベントの取得時間です。</li></ul> |
| `destination` | **（必須）** エクスポートされたデータに関する情報：<ul><li>`datasetId`：**（必須）**&#x200B;データのエクスポート先のデータセットの ID。</li><li>`segmentPerBatch`: *（オプション）* 指定しない場合、ブール値はデフォルトで「false」になります。 値が「false」の場合、すべてのセグメント ID が単一のバッチ ID にエクスポートされます。 値が「true」の場合、1 つのセグメント ID が 1 つのバッチ ID にエクスポートされます。 値を「true」に設定すると、バッチエクスポートのパフォーマンスに影響を与える場合があることに注意してください。</li></ul> |
| `schema.name` | **（必須）**&#x200B;データのエクスポート先のデータセットに関連付けられているスキーマの名前。 |
| `evaluationInfo.segmentation` | *（オプション）* 指定しない場合、デフォルトでになる boolean 値です。 `false`. 値： `true` は、書き出しジョブでセグメント化をおこなう必要があることを示します。 |

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
| `id` | 作成された書き出しジョブを識別する、システムで生成される読み取り専用の値です。 |

または、 `destination.segmentPerBatch` が `true`、 `destination` 上のオブジェクトは `batches` 配列を作成します。

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

特定の書き出しジョブに関する詳細な情報を取得するには、に対してGETリクエストを実行します `/export/jobs` エンドポイントを検索し、リクエストパスで取得する書き出しジョブの ID を指定します。

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
| `destination` | エクスポートするデータの宛先情報：<ul><li>`datasetId`:データが書き出されたデータセットの ID。</li><li>`segmentPerBatch`:セグメント ID が統合されているかどうかを示す Boolean 値です。 値： `false` は、すべてのセグメント ID が単一のバッチ ID に含まれていることを意味します。 値： `true` は、1 つのセグメント ID が 1 つのバッチ ID に書き出されることを意味します。</li></ul> |
| `fields` | コンマで区切った、書き出すフィールドのリスト。 |
| `schema.name` | データのエクスポート先のデータセットに関連付けられているスキーマの名前。 |
| `filter.segments` | 書き出されるセグメント。 次のフィールドが含まれます。<ul><li>`segmentId`:書き出すプロファイルのセグメント ID。</li><li>`segmentNs`:指定したのセグメント名前空間 `segmentID`.</li><li>`status`:のステータスフィルターを提供する文字列の配列 `segmentID`. デフォルトでは、`status` は、現在の時刻にセグメントに含まれているすべてのプロファイルを表す値 `["realized", "existing"]` を持ちます。次の値を指定できます。「適合」、「既存」および「出口」。  値が「適合」の場合は、プロファイルがセグメントに入っていることを意味します。 値が「existing」の場合、プロファイルは引き続きセグメント内に存在します。 値「exiting」は、プロファイルがセグメントから退出していることを意味します。</li></ul> |
| `mergePolicy` | エクスポートされたデータの結合ポリシー情報。 |
| `metrics.totalTime` | 書き出しジョブの実行に要した合計時間を示すフィールド。 |
| `metrics.profileExportTime` | プロファイルのエクスポートに要した時間を示すフィールド。 |
| `totalExportedProfileCounter` | すべてのバッチでエクスポートされたプロファイルの合計数。 |

## 特定の書き出しジョブのキャンセルまたは削除 {#delete}

指定した書き出しジョブの削除をリクエストするには、 `/export/jobs` エンドポイントを作成し、削除する書き出しジョブの ID をリクエストパスに指定します。

**API 形式**

```http
DELETE /export/jobs/{EXPORT_JOB_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{EXPORT_JOB_ID}` | この `id` 削除する書き出しジョブの数を指定します。 |

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

このガイドを読むと、書き出しジョブの動作をより深く理解できます。
