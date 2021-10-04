---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント化；セグメント化；セグメント化サービス；セグメントジョブ；セグメントジョブ；API;API;
solution: Experience Platform
title: セグメントジョブ API エンドポイント
topic-legacy: developer guide
description: Adobe Experience Platform Segmentation Service API のセグメントジョブエンドポイントを使用すると、組織のセグメントジョブをプログラムで管理できます。
exl-id: 105481c2-1c25-4f0e-8fb0-c6577a4616b3
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1168'
ht-degree: 34%

---

# セグメントジョブエンドポイント

セグメントジョブは、新しいオーディエンスセグメントを作成する非同期プロセスです。[ セグメント定義 ](./segment-definitions.md) と、[ 結合ポリシー ](../../profile/api/merge-policies.md) を参照し、[!DNL Real-time Customer Profile] がプロファイルフラグメント間で重なり合う属性を結合する方法を制御します。 セグメントジョブが正常に完了すると、処理中に発生した可能性のあるエラーやオーディエンスの最終的なサイズなど、セグメントに関するさまざまな情報を収集できます。

このガイドは、セグメントジョブをよりよく理解するのに役立つ情報を提供し、API を使用して基本的なアクションを実行するためのサンプル API 呼び出しを含みます。

## はじめに

このガイドで使用する エンドポイントは、[!DNL Adobe Experience Platform Segmentation Service]API の一部です。続行する前に、[ はじめに ](./getting-started.md) を参照して、必要なヘッダーやサンプル API 呼び出しを含む API を正しく呼び出すために知っておく必要がある重要な情報を確認してください。

## セグメントジョッブリストの取得 {#retrieve-list}

IMS 組織のすべてのセグメントジョッブリストを取得するには、`/segment/jobs` エンドポイントに GET リクエストをします。

**API 形式**

`/segment/jobs` エンドポイントは、結果を絞り込むのに役立つ、複数のクエリパラメーターをサポートしています。これらのパラメーターはオプションですが、高価なオーバーヘッドを削減するのに役立つように、パラメーターの使用を強くお勧めします。 パラメーターを指定しないでこのエンドポイントを呼び出すと、組織で使用可能なすべての書き出しジョブが取得されます。複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。

```http
GET /segment/jobs
GET /segment/jobs?{QUERY_PARAMETERS}
```

**クエリパラメータ**

| パラメーター | 説明 | 例 |
| --------- | ----------- | ------- |
| `start` | 返されるセグメントジョブの開始オフセットを指定します。 | `start=1` |
| `limit` | 1 ページに返されるセグメントジョブの数を指定します。 | `limit=20` |
| `status` | ステータスに基づいて結果をフィルターします。サポートされる値は、NEW、QUEUED、PROCESSING、SUCCEEDED、FAILED、CANCELLING、CANCELLED です。 | `status=NEW` |
| `sort` | 返されたセグメントジョブを並べ替えます。`[attributeName]:[desc|asc]` の形式で書き込まれます。 | `sort=creationTime:desc` |
| `property` | セグメントジョブをフィルターし、指定されたフィルターへの完全一致を取得します。次のいずれかの形式で書き込むことができます。 <ul><li>`[jsonObjectPath]==[value]` — オブジェクトキーに対するフィルター</li><li>`[arrayTypeAttributeName]~[objectKey]==[value]` — 配列内のフィルタ－</li></ul> | `property=segments~segmentId==workInUS` |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/segment/jobs?status=SUCCEEDED \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、HTTP ステータス 200 と、指定した IMS 組織のセグメントジョブのリストを JSON として返します。次の応答は、IMS 組織の成功したすべてのリストセグメントジョブのジョブを返します。

>[!NOTE]
>
> 次の応答はスペースを節約するために切り捨てられ、最初に返されたジョブのみが表示されます。

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
                        "mergePolicyId": "25c548a0-ca7f-4dcd-81d5-997642f178b9",
                        "mergePolicy": {
                            "id": "25c548a0-ca7f-4dcd-81d5-997642f178b9",
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
                "totalProfiles":13146432,
                "segmentedProfileCounter":{
                    "94509dba-7387-452f-addc-5d8d979f6ae8":1033
                },
                "segmentedProfileByNamespaceCounter":{
                    "94509dba-7387-452f-addc-5d8d979f6ae8":{
                        "tenantiduserobjid":1033,
                        "campaign_profile_mscom_mkt_prod2":1033
                    }
                },
                "segmentedProfileByStatusCounter":{
                    "94509dba-7387-452f-addc-5d8d979f6ae8":{
                        "exited":144646,
                        "existing":10,
                        "realized":2056
                    }
                },
                "totalProfilesByMergePolicy":{
                    "25c548a0-ca7f-4dcd-81d5-997642f178b9":13146432
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
| `id` | セグメントジョブのシステム生成の読み取り専用識別子。 |
| `status` | セグメントジョブの現在のステータス。 ステータスの有効な値は、「NEW」、「PROCESSING」、「CANCELLING」、「CANCELLED」、「FAILED」、「SUCCEEDED」です。 |
| `segments` | セグメントジョブ内で返されるセグメント定義に関する情報を含むオブジェクト。 |
| `segments.segment.id` | セグメント定義の ID。 |
| `segments.segment.expression` | セグメント定義の式に関する情報を含む、PQL で記述されたオブジェクト。 |
| `metrics` | セグメントジョブに関する診断情報を含むオブジェクト。 |
| `metrics.totalTime` | セグメント化ジョブの開始および終了の時間と合計所要時間に関する情報を含むオブジェクト。 |
| `metrics.profileSegmentationTime` | セグメント化評価の開始時刻と終了時刻、および所要時間の合計に関する情報を含むオブジェクト。 |
| `metrics.segmentProfileCounter` | セグメントごとに認定されるプロファイルの数。 |
| `metrics.segmentedProfileByNamespaceCounter` | セグメントごとに各 ID 名前空間で認定されたプロファイルの数。 |
| `metrics.segmentProfileByStatusCounter` | 各ステータスのプロファイルの数。 次の 3 つのステータスがサポートされています。 <ul><li>「認識済み」 — セグメントに入力された新しいプロファイルの数。</li><li>「既存」 — セグメント内に引き続き存在するプロファイルの数。</li><li>「出口」 — セグメントに存在しなくなったプロファイルセグメントの数。</li></ul> |
| `metrics.totalProfilesByMergePolicy` | 結合ポリシーごとの結合済みプロファイルの合計数です。 |

## 新しいセグメントジョブの作成 {#create}

新しいセグメントジョブを作成するには、`/segment/jobs` エンドポイントにPOSTリクエストを送信し、本文に新しいオーディエンスを作成するセグメント定義の ID を含めます。

**API 形式**

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
| `segmentId` | セグメントジョブを作成するセグメント定義の ID。 これらのセグメント定義は、異なる結合ポリシーに属することができます。 セグメント定義の詳細については、『[ セグメント定義エンドポイントガイド ](./segment-definitions.md)』を参照してください。 |

**応答**

正常な応答は、HTTP ステータス 200 と、新しく作成したセグメントジョブの詳細を返します。

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
| `id` | 新しく作成されたセグメントジョブのシステム生成読み取り専用識別子。 |
| `status` | セグメントジョブの現在のステータス。 セグメントジョブは新しく作成されるので、ステータスは常に「NEW」になります。 |
| `segments` | このセグメントジョブが実行されているセグメント定義に関する情報を含むオブジェクト。 |
| `segments.segment.id` | 指定したセグメント定義の ID。 |
| `segments.segment.expression` | セグメント定義の式に関する情報を含む、PQL で記述されたオブジェクト。 |

## 特定のセグメントジョブの取得 {#get}

`/segment/jobs` エンドポイントにGETリクエストを送信し、リクエストパスに取得するセグメントジョブの ID を指定することで、特定のセグメントジョブに関する詳細な情報を取得できます。

**API 形式**

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

正常な応答は、HTTP ステータス 200 と、指定したセグメントジョブに関する詳細情報を返します。

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
| `id` | セグメントジョブのシステム生成の読み取り専用識別子。 |
| `status` | セグメントジョブの現在のステータス。 ステータスの有効な値は、「NEW」、「PROCESSING」、「CANCELLING」、「CANCELLED」、「FAILED」、「SUCCEEDED」です。 |
| `segments` | セグメントジョブ内で返されるセグメント定義に関する情報を含むオブジェクト。 |
| `segments.segment.id` | セグメント定義の ID。 |
| `segments.segment.expression` | セグメント定義の式に関する情報を含む、PQL で記述されたオブジェクト。 |
| `metrics` | セグメントジョブに関する診断情報を含むオブジェクト。 |

## セグメントジョブの一括取得 {#bulk-get}

`/segment/jobs/bulk-get` エンドポイントにPOSTリクエストを送信し、リクエスト本文にセグメントジョブの `id` 値を指定することで、複数のセグメントジョブに関する詳細な情報を取得できます。

**API 形式**

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

正常な応答は、HTTP ステータス 207 と、リクエストされたセグメントジョブを返します。

>[!NOTE]
>
>次の応答はスペースを節約するために切り捨てられ、各セグメントジョブの詳細の一部のみが表示されます。 完全な応答には、リクエストされたセグメントジョブの完全な詳細がリストされます。

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
| `id` | セグメントジョブのシステム生成の読み取り専用識別子。 |
| `status` | セグメントジョブの現在のステータス。 ステータスの有効な値は、「NEW」、「PROCESSING」、「CANCELLING」、「CANCELLED」、「FAILED」、「SUCCEEDED」です。 |
| `segments` | セグメントジョブ内で返されるセグメント定義に関する情報を含むオブジェクト。 |
| `segments.segment.id` | セグメント定義の ID。 |
| `segments.segment.expression` | セグメント定義の式に関する情報を含む、PQL で記述されたオブジェクト。 |

## 特定のセグメントジョブのキャンセルまたは削除 {#delete}

`/segment/jobs` エンドポイントにDELETEリクエストを送信し、リクエストパスに削除するセグメントジョブの ID を指定することで、特定のセグメントジョブを削除できます。

>[!NOTE]
>
>削除リクエストに対する API 応答は直ちに発生します。 ただし、セグメントジョブの実際の削除は非同期的におこなわれます。 つまり、セグメントジョブに対して削除リクエストを実行するタイミングと適用するタイミングに時間差があります。

**API 形式**

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

正常な応答は、HTTP ステータス 204 と次の情報を返します。

```json
{
    "status": true,
    "message": "Segment job with id 'd3b4a50d-dfea-43eb-9fca-557ea53771fd' has been marked for cancelling"
}
```

## 次の手順

このガイドを読むと、セグメントジョブの動作をより深く理解できます。
