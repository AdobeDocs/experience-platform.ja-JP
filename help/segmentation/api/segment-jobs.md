---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: セグメントジョブ
topic: developer guide
translation-type: tm+mt
source-git-commit: 2327ce9a87647fb2416093d4a27eb7d4dc4aa4d7
workflow-type: tm+mt
source-wordcount: '994'
ht-degree: 3%

---


# セグメントジョブエンドポイントガイド

セグメントジョブは、新しいオーディエンスセグメントを作成する非同期プロセスです。 これは [セグメント定義](./segment-definitions.md)、およびプロファイルフラグメント間で重なり合う属性を [](../../profile/api/merge-policies.md)[!DNL Real-time Customer Profile] 結合する方法を制御する結合ポリシーを参照します。 セグメントジョブが正常に完了したら、処理中に発生した可能性のあるエラーやオーディエンスの最大サイズなど、セグメントに関する様々な情報を収集できます。

このガイドは、セグメントのジョブをより深く理解するのに役立つ情報を提供し、APIを使用して基本的なアクションを実行するためのサンプルAPI呼び出しを含みます。

## はじめに

このガイドで使用されるエンドポイントは、 [!DNL Adobe Experience Platform Segmentation Service] APIの一部です。 先に進む前に、 [入門ガイドを参照して](./getting-started.md) 、必要なヘッダーやAPI呼び出し例を読む方法など、APIを正しく呼び出すために必要な重要な情報を確認してください。

## セグメントジョブのリストの取得 {#retrieve-list}

エンドポイントにGETリクエストを作成して、IMS組織のすべてのセグメントジョブのリストを取得でき `/segment/jobs` ます。

**API形式**

エンドポイントでは、結果のフィルタリングに役立ついくつかのクエリパラメーターが `/segment/jobs` サポートされています。 これらのパラメーターはオプションですが、高価なオーバーヘッドを削減するために、このパラメーターの使用を強くお勧めします。 パラメーターを指定せずにこのエンドポイントを呼び出すと、組織で使用可能なすべての書き出しジョブが取得されます。 複数のパラメーターを含める場合は、アンパサンド(`&`)で区切ります。

```http
GET /segment/jobs
GET /segment/jobs?{QUERY_PARAMETERS}
```

**クエリパラメーター**

| パラメーター | 説明 | 例 |
| --------- | ----------- | ------- |
| `start` | 返されるセグメントジョブの開始オフセットを指定します。 | `start=1` |
| `limit` | 1ページに返されるセグメントジョブの数を指定します。 | `limit=20` |
| `status` | ステータスに基づいて結果をフィルターします。 サポートされる値は、NEW、QUEUED、PROCESSING、SUCCEEDEDED、FAILED、CANCELLING、CANCELLEDです | `status=NEW` |
| `sort` | 返されたセグメントジョブの順序。 は、形式で書き込まれ `[attributeName]:[desc|asc]`ます。 | `sort=creationTime:desc` |
| `property` | フィルターは、ジョブをセグメント化し、指定されたフィルターに完全一致を取得します。 次のいずれかの形式で書き込むことができます。 <ul><li>`[jsonObjectPath]==[value]`  — オブジェクトキーに対するフィルタリング</li><li>`[arrayTypeAttributeName]~[objectKey]==[value]`  — 配列内のフィルタ</li></ul> | `property=segments~segmentId==workInUS` |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/segment/jobs?status=SUCCEEDED \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、指定したIMS組織のセグメントジョブのリストを持つHTTPステータス200をJSONとして返します。 次の応答は、IMS組織の成功したすべてのセグメントジョブのリストを返します。

>[!NOTE]
>
>次の応答は領域に対して切り捨てられ、最初に返されたジョブのみを表示します。

```json
{
    "_page": {
        "totalCount": 14,
        "pageSize": 14
    },
    "children": [
        {
            "id": "b31aed3d-b3b1-4613-98c6-7d3846e8d48f",
            "imsOrgId": "E95186D65A28ABF00A495D82@AdobeOrg",
            "sandbox": {
                "sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
                "sandboxName": "prod",
                "type": "production",
                "default": true
            },
            "profileInstanceId": "ups",
            "source": "scheduler",
            "status": "SUCCEEDED",
            "batchId": "678f53bc-e21d-4c47-a7ec-5ad0064f8e4c",
            "computeJobId": 8811,
            "computeGatewayJobId": "9ea97b25-a0f5-410e-ae87-b2d85e58f399",
            "segments": [
                {
                    "segmentId": "30230300-ccf1-48ad-8012-c5563a007069",
                    "segment": {
                        "id": "30230300-ccf1-48ad-8012-c5563a007069",
                        "expression": {
                            "type": "PQL",
                            "format": "pql/json",
                            "value": "{PQL_EXPRESSION}"
                        },
                        "mergePolicyId": "b83185bb-0bc6-489c-9363-0075eb30b4c8",
                        "mergePolicy": {
                            "id": "b83185bb-0bc6-489c-9363-0075eb30b4c8",
                            "version": 1
                        }
                    }
                }
            ],
            "metrics": {
                "totalTime": {
                    "startTimeInMs": 1573203617195,
                    "endTimeInMs": 1573204395655,
                    "totalTimeInMs": 778460
                },
                "profileSegmentationTime": {
                    "startTimeInMs": 1573204266727,
                    "endTimeInMs": 1573204395655,
                    "totalTimeInMs": 128928
                },
                "totalProfiles": 0,
                "segmentedProfileCounter": {
                    "30230300-ccf1-48ad-8012-c5563a007069": 0,
                    "ca763983-5572-4ea4-809c-b7dff7e0d79b": 0
                },
                "segmentedProfileByNamespaceCounter": {
                    "30230300-ccf1-48ad-8012-c5563a007069": {},
                    "ca763983-5572-4ea4-809c-b7dff7e0d79b": {}
                }
            },
            "requestId": "4e538382-dbd8-449e-988a-4ac639ebe72b-1573203600264",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "properties": {
                "scheduleId": "4e538382-dbd8-449e-988a-4ac639ebe72b",
                "runId": "e6c1308d-0d4b-4246-b2eb-43697b50a149"
            },
            "_links": {
                "cancel": {
                    "href": "/segment/jobs/b31aed3d-b3b1-4613-98c6-7d3846e8d48f",
                    "method": "DELETE"
                },
                "checkStatus": {
                    "href": "/segment/jobs/b31aed3d-b3b1-4613-98c6-7d3846e8d48f",
                    "method": "GET"
                }
            },
            "updateTime": 1573204395000,
            "creationTime": 1573203600535,
            "updateEpoch": 1573204395
        }
    ],
    "_links": {
        "next": {}
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | セグメントジョブに関して、システム生成の読み取り専用識別子。 |
| `status` | セグメントジョブの現在のステータス。 ステータスには、「NEW」、「PROCESSING」、「CANCELLING」、「CANCELLED」、「FAILED」、「SUCCEEDEDED」などの値が考えられます。 |
| `segments` | セグメントジョブ内で返されるセグメント定義に関する情報を含むオブジェクト。 |
| `segments.segment.id` | セグメント定義のID。 |
| `segments.segment.expression` | PQLで記述される、セグメント定義の式に関する情報を含むオブジェクト。 |
| `metrics` | セグメントジョブに関する診断情報を含むオブジェクト。 |

## 新しいセグメントジョブの作成 {#create}

新しいセグメントオーディエンスを作成するセグメント定義のIDを本文に含め、エンドポイントへのPOSTリクエストを作成することで、新しいセグメントジョブを作成でき `/segment/jobs` ます。

**API形式**

```http
POST /segment/jobs
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/core/ups/segment/jobs \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
[
  {
    "segmentId": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
  }
]'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `segmentId` | セグメントジョブを作成する対象のセグメント定義のID。 セグメント定義の詳細については、『 [セグメント定義エンドポイントガイド](./segment-definitions.md)』を参照してください。 |

**応答**

正常に応答すると、新しく作成したセグメントジョブの詳細と共に、HTTPステータス200が返されます。

```json
{
    "id": "d3b4a50d-dfea-43eb-9fca-557ea53771fd",
    "imsOrgId": "{IMS_ORG}",
    "sandbox": {
        "sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "profileInstanceId": "ups",
    "source": "api",
    "status": "NEW",
    "segments": [
        {
            "segmentId": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
            "segment": {
                "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
                "expression": {
                    "type": "PQL",
                    "format": "pql/text",
                    "value": "workAddress.country = \"US\""
                },
                "mergePolicyId": "e161dae9-52f0-4c7f-b264-dc43dd903d56",
                "mergePolicy": {
                    "id": "e161dae9-52f0-4c7f-b264-dc43dd903d56",
                    "version": 1
                }
            }
        }
    ],
    "requestId": "Hw1jdAHeuWHVKVxcAPFrLCbbjkriDl9v",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "_links": {
        "cancel": {
            "href": "/segment/jobs/d3b4a50d-dfea-43eb-9fca-557ea53771fd",
            "method": "DELETE"
        },
        "checkStatus": {
            "href": "/segment/jobs/d3b4a50d-dfea-43eb-9fca-557ea53771fd",
            "method": "GET"
        }
    },
    "updateTime": 1579304260000,
    "creationTime": 1579304260897,
    "updateEpoch": 1579304260
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | 新しく作成されたセグメントジョブに対して、システムによって生成される読み取り専用の識別子。 |
| `status` | セグメントジョブの現在のステータス。 セグメントジョブは新しく作成されるので、ステータスは常に「新規」になります。 |
| `segments` | このセグメントジョブが実行される対象のセグメント定義に関する情報を含むオブジェクト。 |
| `segments.segment.id` | 指定したセグメント定義のID。 |
| `segments.segment.expression` | PQLで記述される、セグメント定義の式に関する情報を含むオブジェクト。 |

## 特定のセグメントジョブの取得 {#get}

エンドポイントにGETリクエストを送信し、取得するセグメントジョブのIDをリクエストパスに指定することで、特定のセグメントジョブに関する詳細な情報を取得でき `/segment/jobs` ます。

**API形式**

```http
GET /segment/jobs/{SEGMENT_JOB_ID}
```

| プロパティ | 説明 |
| -------- | ----------- | 
| `{SEGMENT_JOB_ID}` | 取得するセグメントジョブの `id` 値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/segment/jobs/d3b4a50d-dfea-43eb-9fca-557ea53771fd \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、指定されたセグメントジョブに関する詳細情報と共に、HTTPステータス200を返します。

```json
{
    "id": "d3b4a50d-dfea-43eb-9fca-557ea53771fd",
    "imsOrgId": "{IMS_ORG}",
    "sandbox": {
        "sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "profileInstanceId": "ups",
    "source": "api",
    "status": "SUCCEEDED",
    "batchId": "651fc109-3963-48d2-aa98-9e3cc2003bac",
    "computeJobId": 39312,
    "computeGatewayJobId": "a0099ab6-11ab-4c2b-a0ea-6162e16806bd",
    "segments": [
        {
            "segmentId": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
            "segment": {
                "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
                "expression": {
                    "type": "PQL",
                    "format": "pql/text",
                    "value": "workAddress.country = \"US\""
                },
                "mergePolicyId": "e161dae9-52f0-4c7f-b264-dc43dd903d56",
                "mergePolicy": {
                    "id": "e161dae9-52f0-4c7f-b264-dc43dd903d56",
                    "version": 1
                }
            }
        }
    ],
    "metrics": {
        "totalTime": {
            "startTimeInMs": 1579304313411
        },
        "profileSegmentationTime": {}
    },
    "requestId": "Hw1jdAHeuWHVKVxcAPFrLCbbjkriDl9v",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "_links": {
        "cancel": {
            "href": "/segment/jobs/d3b4a50d-dfea-43eb-9fca-557ea53771fd",
            "method": "DELETE"
        },
        "checkStatus": {
            "href": "/segment/jobs/d3b4a50d-dfea-43eb-9fca-557ea53771fd",
            "method": "GET"
        }
    },
    "updateTime": 1579304339000,
    "creationTime": 1579304260897,
    "updateEpoch": 1579304339
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | セグメントジョブに関して、システム生成の読み取り専用識別子。 |
| `status` | セグメントジョブの現在のステータス。 ステータスには、「NEW」、「PROCESSING」、「CANCELLING」、「CANCELLED」、「FAILED」、「SUCCEEDEDED」などの値が考えられます。 |
| `segments` | セグメントジョブ内で返されるセグメント定義に関する情報を含むオブジェクト。 |
| `segments.segment.id` | セグメント定義のID。 |
| `segments.segment.expression` | PQLで記述される、セグメント定義の式に関する情報を含むオブジェクト。 |
| `metrics` | セグメントジョブに関する診断情報を含むオブジェクト。 |

## セグメントの一括取得ジョブ {#bulk-get}

エンドポイントにPOSTリクエストを送信し、リクエスト本文にセグメントジョブの `/segment/jobs/bulk-get``id` 値を指定することで、複数のセグメントジョブに関する詳細な情報を取得できます。

**API形式**

```http
POST /segment/jobs/bulk-get
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/core/ups/segment/jobs/bulk-get \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "ids": [
            {
                "id": "cc3419d3-0389-47f1-b174-fead6b3c830d"
            },
            {
                "id": "c527dc3f-07fe-4b96-be4e-23f38e734ff8"
            }
        ]
    }'
```

**応答**

正常な応答が返されると、HTTPステータス207が返され、要求されたセグメントジョブが返されます。

>[!NOTE]
>
>次の応答は領域に対して切り捨てられ、各セグメントジョブの詳細の一部のみが表示されました。 完全な回答には、リクエストされたセグメントジョブの完全な詳細がリストされます。

```json
{
    "results": {
        "cc3419d3-0389-47f1-b174-fead6b3c830d": {
            "id": "cc3419d3-0389-47f1-b174-fead6b3c830d",
            "imsOrgId": "{IMS_ORG}",
            "status": "SUCCEEDED",
            "segments": [
                {
                    "segmentId": "30230300-ccf1-48ad-8012-c5563a007069",
                    "segment": {
                        "id": "30230300-ccf1-48ad-8012-c5563a007069",
                        "expression": {
                            "type": "PQL",
                            "format": "pql/json",
                            "value": "{PQL_EXPRESSION}"
                        },
                        "mergePolicyId": "b83185bb-0bc6-489c-9363-0075eb30b4c8",
                        "mergePolicy": {
                            "id": "b83185bb-0bc6-489c-9363-0075eb30b4c8",
                            "version": 1
                        }
                    }
                }
            ],
            "updateTime": 1573204395000,
            "creationTime": 1573203600535,
            "updateEpoch": 1573204395
        },
        "c527dc3f-07fe-4b96-be4e-23f38e734ff8": {
            "id": "c527dc3f-07fe-4b96-be4e-23f38e734ff8",
            "imsOrgId": "{IMS_ORG}",
            "status": "SUCCEEDED",
            "segments": [
                {
                    "segmentId": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
                    "segment": {
                        "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
                        "expression": {
                            "type": "PQL",
                            "format": "pql/json",
                            "value": "{PQL_EXPRESSION}"
                        },
                        "mergePolicyId": "b83185bb-0bc6-489c-9363-0075eb30b4c8",
                        "mergePolicy": {
                            "id": "b83185bb-0bc6-489c-9363-0075eb30b4c8",
                            "version": 1
                        }
                    }
                }
            ],
            "updateTime": 1573204395000,
            "creationTime": 1573203600535,
            "updateEpoch": 1573204395
        }
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | セグメントジョブに関して、システム生成の読み取り専用識別子。 |
| `status` | セグメントジョブの現在のステータス。 ステータスには、「NEW」、「PROCESSING」、「CANCELLING」、「CANCELLED」、「FAILED」、「SUCCEEDEDED」などの値が考えられます。 |
| `segments` | セグメントジョブ内で返されるセグメント定義に関する情報を含むオブジェクト。 |
| `segments.segment.id` | セグメント定義のID。 |
| `segments.segment.expression` | PQLで記述される、セグメント定義の式に関する情報を含むオブジェクト。 |

## 特定のセグメントジョブのキャンセルまたは削除 {#delete}

エンドポイントにDELETEリクエストを送信し、削除するセグメントジョブのIDをリクエストパスに指定することで、特定のセグメントジョブを削除でき `/segment/jobs` ます。

>[!NOTE]
>
>削除リクエストに対するAPIの応答は直ちに行われます。 ただし、実際にセグメントジョブを削除するのは非同期的です。 つまり、セグメントジョブに対する削除リクエストが行われた時点と適用された時点との間に時間差があります。

**API形式**

```http
DELETE /segment/jobs/{SEGMENT_JOB_ID}
```

| プロパティ | 説明 |
| -------- | ----------- | 
| `{SEGMENT_JOB_ID}` | 削除するセグメントジョブの `id` 値。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/segment/jobs/d3b4a50d-dfea-43eb-9fca-557ea53771fd \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常に応答すると、HTTPステータス204が次の情報と共に返されます。

```json
{
    "status": true,
    "message": "Segment job with id 'd3b4a50d-dfea-43eb-9fca-557ea53771fd' has been marked for cancelling"
}
```

## 次の手順

このガイドを読むと、セグメントジョブの動作についての理解が深まります。