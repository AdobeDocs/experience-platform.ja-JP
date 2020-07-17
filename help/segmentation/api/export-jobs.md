---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 書き出しジョブエンドポイントガイド
topic: developer guide
translation-type: tm+mt
source-git-commit: 3e39333207ef6c94b6d792be33a4605f185ff5ab
workflow-type: tm+mt
source-wordcount: '1469'
ht-degree: 2%

---


# 書き出しジョブエンドポイントガイド

書き出しジョブは、オーディエンスセグメントメンバーをデータセットに永続化するために使用される非同期プロセスです。 Adobe Experience PlatformセグメントAPIの `/export/jobs` エンドポイントを使用すると、プログラムによって、書き出しジョブを取得、作成およびキャンセルできます。

## はじめに

このガイドで使用されるエンドポイントは、 [!DNL Adobe Experience Platform Segmentation Service] APIの一部です。 先に進む前に、 [入門ガイドを参照して](./getting-started.md) 、必要なヘッダーやAPI呼び出し例を読む方法など、APIを正しく呼び出すために必要な重要な情報を確認してください。

## 書き出しジョブのリストの取得 {#retrieve-list}

エンドポイントにGETリクエストを作成して、IMS組織のすべてのエクスポートジョブのリストを取得でき `/export/jobs` ます。

**API形式**

エンドポイントでは、結果のフィルタリングに役立ついくつかのクエリパラメーターが `/export/jobs` サポートされています。 これらのパラメーターはオプションですが、高価なオーバーヘッドを削減するために、このパラメーターの使用を強くお勧めします。 パラメーターを指定せずにこのエンドポイントを呼び出すと、組織で使用可能なすべての書き出しジョブが取得されます。 複数のパラメーターを含める場合は、アンパサンド(`&`)で区切ります。

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
| `{STATUS}` | ステータスに基づいて結果をフィルターします。 サポートされる値は、「NEW」、「SUCCEEDED」および「FAILED」です。 |

**リクエスト**

次のリクエストは、IMS組織内の最後の2つのエクスポートジョブを取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/export/jobs?limit=2 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

次の応答は、要求パスに指定されたクエリパラメーターに基づいて、正常に完了した書き出しジョブのリストを持つHTTPステータス200を返します。

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
| `destination` | エクスポートされたデータのエクスポート先情報：<ul><li>`datasetId`: データがエクスポートされたデータセットのID。</li><li>`segmentPerBatch`: セグメントIDが統合されているかどうかを示すBoolean値。 値が「false」の場合は、すべてのセグメントIDが1つのバッチIDにエクスポートされることを意味します。 値が「true」の場合は、1つのセグメントIDが1つのバッチIDにエクスポートされることを意味します。 **注意：** この値をtrueに設定すると、バッチエクスポートのパフォーマンスに影響する場合があります。</li></ul> |
| `fields` | カンマで区切った、書き出されたフィールドのリスト。 |
| `schema.name` | データをエクスポートするデータセットに関連付けられているスキーマの名前。 |
| `filter.segments` | 書き出されるセグメント。 次のフィールドが含まれます。<ul><li>`segmentId`: プロファイルのエクスポート先のセグメントID。</li><li>`segmentNs`: 指定したのセグメント名前空間 `segmentID`。</li><li>`status`: のステータスフィルターを提供する文字列の配列で `segmentID`す。 デフォルトでは、 `status` は、現在の時間にセグメントに含まれるすべてのプロファイルを表す値 `["realized", "existing"]` を持ちます。 使用できる値は次のとおりです。 「reliated」、「existing」および「exited」。</li></ul> |
| `mergePolicy` | エクスポートしたデータのポリシー情報をマージします。 |
| `metrics.totalTime` | 書き出しジョブの実行に要した合計時間を示すフィールドです。 |
| `metrics.profileExportTime` | プロファイルの書き出しに要した時間を示すフィールドです。 |
| `page` | 要求された書き出しジョブのページ番号に関する情報です。 |
| `link.next` | 書き出しジョブの次のページへのリンク。 |

## 新しい書き出しジョブの作成 {#create}

エンドポイントにPOSTリクエストを作成して、新しい書き出しジョブを作成でき `/export/jobs` ます。

**API形式**

```http
POST /export/jobs
```

**リクエスト**

次のリクエストは、ペイロードで提供されるパラメーターによって設定された、新しい書き出しジョブを作成します。

```shell
curl -X POST https://platform.adobe.io/data/core/ups/export/jobs \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
    }
}'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `fields` | カンマで区切った、書き出されたフィールドのリスト。 空白の場合、すべてのフィールドが書き出されます。 |
| `mergePolicy` | 書き出されたデータを管理するマージポリシーを指定します。 複数のセグメントを書き出す場合は、このパラメーターを含めます。 指定しなかった場合、エクスポートは指定したセグメントと同じマージポリシーを使用します。 |
| `filter` | 以下に示すサブプロパティに応じて、ID、認定時間、または取り込み時間で、書き出しジョブに含めるセグメントを指定するオブジェクト。 空白の場合、すべてのデータがエクスポートされます。 |
| `filter.segments` | 書き出すセグメントを指定します。 この値を省略すると、すべてのプロファイルのすべてのデータがエクスポートされます。 次のフィールドを含むセグメントオブジェクトの配列を受け入れます。<ul><li>`segmentId`: **(使用する場合は必須`segments`)** プロファイルのセグメントIDをエクスポートします。</li><li>`segmentNs` *（オプション）* 指定したのセグメント名前空間 `segmentID`。</li><li>`status` *（オプション）* 、のステータスフィルタを提供する文字列の配列 `segmentID`。 デフォルトでは、 `status` は、現在の時間にセグメントに含まれるすべてのプロファイルを表す値 `["realized", "existing"]` を持ちます。 使用できる値は次のとおりです。 `"realized"`、 `"existing"`および `"exited"`。</li></ul> |
| `filter.segmentQualificationTime` | セグメントクオリフィケーション時間に基づいてフィルターします。 開始時間及び/又は終了時間を提供することができる。 |
| `filter.segmentQualificationTime.startTime` | 特定のステータスのセグメントIDに対するセグメントクオリフィケーション開始時間。 これは指定されていません。セグメントID資格の開始時間に対するフィルターはありません。 タイムスタンプは、 [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式で指定する必要があります。 |
| `filter.segmentQualificationTime.endTime` | 特定のステータスのセグメントIDに対するセグメントクオリフィケーションの終了時間。 これは指定されていません。セグメントID資格の終了時間にフィルターは適用されません。 タイムスタンプは、 [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式で指定する必要があります。 |
| `filter.fromIngestTimestamp ` | 書き出したプロファイルを、このタイムスタンプの後に更新されたアイテムのみに制限します。 タイムスタンプは、 [RFC 3339](https://tools.ietf.org/html/rfc3339) 形式で指定する必要があります。 <ul><li>`fromIngestTimestamp` を **プロファイルに対して設定する場合**、次の条件を満たします。 マージされた更新されたプロファイルが指定されたタイムスタンプより大きい場合に、マージされたすべてのタイムスタンプを含めます。 オペランドをサポート `greater_than` します。</li><li>`fromIngestTimestamp` の **イベント**: このタイムスタンプの後に取り込まれるすべてのイベントは、結果のプロファイル結果に応じて書き出されます。 これは、イベント時間そのものではなく、イベントの取り込み時間です。</li> |
| `filter.emptyProfiles` | 空のプロファイルをフィルタリングするかどうかを示すboolean値です。 プロファイルには、プロファイルレコード、ExperienceEventレコード、またはその両方を含めることができます。 プロファイルレコードがなく、ExperienceEventレコードのみのプロファイルは、「emptyProfiles」と呼ばれます。 プロファイルストア内のすべてのプロファイル（「emptyProfiles」も含む）を書き出すには、の値をに設定 `emptyProfiles` し `true`ます。 をに設定 `emptyProfiles``false`すると、ストア内のプロファイルレコードを持つプロファイルのみがエクスポートされます。 デフォルトでは、 `emptyProfiles` 属性が含まれていない場合、プロファイルレコードを含むプロファイルのみがエクスポートされます。 |
| `additionalFields.eventList` | 次の1つ以上の設定を指定して、子オブジェクトまたは関連オブジェクト用に書き出す時系列イベントフィールドを制御します。<ul><li>`fields`: 書き出すフィールドを制御します。</li><li>`filter`: 関連するオブジェクトから含まれる結果を制限する基準を指定します。 エクスポートに必要な最小値（通常は日付）。</li><li>`filter.fromIngestTimestamp`: 指定されたタイムスタンプの後に取り込まれたものへの時系列イベントのフィルター。 これは、イベント時間そのものではなく、イベントの取り込み時間です。</li><li>`filter.toIngestTimestamp`: 指定したタイムスタンプより前に取り込まれたタイムスタンプにフィルターします。 これは、イベント時間そのものではなく、イベントの取り込み時間です。</li></ul> |
| `destination` | **（必須）** 、書き出されたデータに関する情報：<ul><li>`datasetId`: **（必須）** 、データをエクスポートするデータセットのID。</li><li>`segmentPerBatch`: *（オプション）* Boolean値。指定しなかった場合のデフォルト値は「false」です。 値が「false」の場合、すべてのセグメントIDが1つのバッチIDにエクスポートされます。 値が「true」の場合、1つのセグメントIDが1つのバッチIDにエクスポートされます。 値を「true」に設定すると、バッチエクスポートのパフォーマンスに影響する場合があります。</li></ul> |
| `schema.name` | **（必須）** 、データをエクスポートするデータセットに関連付けられているスキーマの名前。 |

**応答**

正常に応答すると、新しく作成した書き出しジョブの詳細と共に、HTTPステータス200が返されます。

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
    "imsOrgId": "{IMS_ORG}",
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
| `id` | 作成されたばかりの書き出しジョブを識別する、システム生成の読み取り専用の値です。 |

または、をに設定 `destination.segmentPerBatch` した場合 `true`、上の `destination` オブジェクトは `batches` 配列になり、次のようになります。

```json
    "destination": {
        "dataSetId" : "{DATASET_ID}",
        "segmentPerBatch": true,
        "batches" : [
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

エンドポイントにGETリクエストを送信し、取得するエクスポートジョブのIDをリクエストパスに指定することで、特定のエクスポートジョブに関する詳細な情報を取得でき `/export/jobs` ます。

**API形式**

```http
GET /export/jobs/{EXPORT_JOB_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{EXPORT_JOB_ID}` | アクセス `id` する書き出しジョブの名前。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/export/jobs/11037 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常に応答すると、指定された書き出しジョブに関する詳細情報と共に、HTTPステータス200が返されます。

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
    "imsOrgId": "{IMS_ORG}",
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
| `destination` | エクスポートされたデータのエクスポート先情報：<ul><li>`datasetId`: データがエクスポートされたデータセットのID。</li><li>`segmentPerBatch`: セグメントIDが統合されているかどうかを示すBoolean値。 値がの場合は、すべてのセグメントIDが単一のバッチIDになったことを `false` 意味します。 値がの場合、1つのセグメントIDが1つのバッチIDにエクスポートされます。 `true`</li></ul> |
| `fields` | カンマで区切った、書き出されたフィールドのリスト。 |
| `schema.name` | データをエクスポートするデータセットに関連付けられているスキーマの名前。 |
| `filter.segments` | 書き出されるセグメント。 次のフィールドが含まれます。<ul><li>`segmentId`: エクスポートするプロファイルのセグメントID。</li><li>`segmentNs`: 指定したのセグメント名前空間 `segmentID`。</li><li>`status`: のステータスフィルターを提供する文字列の配列で `segmentID`す。 デフォルトでは、 `status` は、現在の時間にセグメントに含まれるすべてのプロファイルを表す値 `["realized", "existing"]` を持ちます。 使用できる値は次のとおりです。 「reliated」、「existing」および「exited」。</li></ul> |
| `mergePolicy` | エクスポートしたデータのポリシー情報をマージします。 |
| `metrics.totalTime` | 書き出しジョブの実行に要した合計時間を示すフィールドです。 |
| `metrics.profileExportTime` | プロファイルの書き出しに要した時間を示すフィールドです。 |
| `totalExportedProfileCounter` | すべてのバッチにエクスポートされたプロファイルの合計数です。 |

## 特定の書き出しジョブのキャンセルまたは削除 {#delete}

エンドポイントにDELETEリクエストを送信し、削除するエクスポートジョブのIDをリクエストパスに指定することで、指定したエクスポートジョブを削除するようにリクエストでき `/export/jobs` ます。

**API形式**

```http
DELETE /export/jobs/{EXPORT_JOB_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{EXPORT_JOB_ID}` | 削除する書き出しジョブ `id` の名前。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/export/jobs/{EXPORT_JOB_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常に応答すると、HTTPステータス204が返され、次のメッセージが表示されます。

```json
{
  "status": true,
  "message": "Export job has been marked for cancelling"
}
```

## 次の手順

このガイドを読むと、書き出しジョブの動作についての理解が深まります。