---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント化；セグメント化；セグメント化サービス；セグメントジョブ；セグメントジョブ；API;API;
solution: Experience Platform
title: セグメントジョブAPIエンドポイント
topic-legacy: developer guide
description: Adobe Experience PlatformセグメントサービスAPIのセグメントジョブエンドポイントは、組織のセグメントジョブをプログラムで管理できるようにします。
exl-id: 105481c2-1c25-4f0e-8fb0-c6577a4616b3
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1168'
ht-degree: 32%

---

# セグメントジョブエンドポイント

セグメントジョブは、新しいオーディエンスセグメントを作成する非同期プロセスです。[セグメント定義](./segment-definitions.md)と、[!DNL Real-time Customer Profile]がプロファイルフラグメント間で重なり合う属性をどのように結合するかを制御する[結合ポリシー](../../profile/api/merge-policies.md)を参照します。 セグメントジョブが正常に完了すると、処理中に発生した可能性のあるエラーやオーディエンスの最終的なサイズなど、セグメントに関するさまざまな情報を収集できます。

このガイドは、セグメントジョブをよりよく理解するのに役立つ情報を提供し、API を使用して基本的なアクションを実行するためのサンプル API 呼び出しを含みます。

## はじめに

このガイドで使用されるエンドポイントは、[!DNL Adobe Experience Platform Segmentation Service] APIの一部です。 先に進む前に、[はじめにガイド](./getting-started.md)を見て、必要なヘッダーやAPI呼び出し例の読み方など、APIを正しく呼び出すために必要な重要な情報を確認してください。

## セグメントジョッブリストの取得 {#retrieve-list}

IMS 組織のすべてのセグメントジョッブリストを取得するには、`/segment/jobs` エンドポイントに GET リクエストをします。

**API 形式**

`/segment/jobs`エンドポイントは、結果のフィルタリングに役立ついくつかのクエリパラメーターをサポートしています。 これらのパラメーターはオプションですが、高価なオーバーヘッドを削減するために、このパラメーターの使用を強くお勧めします。 パラメーターを指定しないでこのエンドポイントを呼び出すと、組織で使用可能なすべての書き出しジョブが取得されます。複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。

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
| `id` | セグメントジョブに関して、システム生成の読み取り専用識別子。 |
| `status` | セグメントジョブの現在のステータス。 ステータスには、「NEW」、「PROCESSING」、「CANCELLING」、「CANCELLED」、「FAILED」、「SUCCEEDED」などの値が考えられます。 |
| `segments` | セグメントジョブ内で返されるセグメント定義に関する情報を含むオブジェクト。 |
| `segments.segment.id` | セグメント定義のID。 |
| `segments.segment.expression` | PQLで記述される、セグメント定義の式に関する情報を含むオブジェクト。 |
| `metrics` | セグメントジョブに関する診断情報を含むオブジェクト。 |
| `metrics.totalTime` | セグメント化ジョブの開始および終了の時間と合計所要時間に関する情報を含むオブジェクトです。 |
| `metrics.profileSegmentationTime` | セグメントの評価が開始および終了した時間と合計所要時間に関する情報を含むオブジェクトです。 |
| `metrics.segmentProfileCounter` | セグメントごとに資格を得たプロファイルの数。 |
| `metrics.segmentedProfileByNamespaceCounter` | 各ID名前空間に対してセグメント単位で資格を持つプロファイルの数。 |
| `metrics.segmentProfileByStatusCounter` | 各ステータスのプロファイル数。 次の3つのステータスがサポートされています。 <ul><li>「realized」 — セグメントに入力された新しいプロファイルの数。</li><li>「existing」 — セグメント内に存在し続けるプロファイルの数。</li><li>「出口」 — セグメント内に存在しなくなったプロファイルセグメントの数。</li></ul> |
| `metrics.totalProfilesByMergePolicy` | マージポリシーごとのマージされたプロファイルの合計数です。 |

## 新しいセグメントジョブの作成 {#create}

`/segment/jobs`エンドポイントへのPOSTリクエストを作成し、本文に新しいオーディエンスの作成元となるセグメント定義のIDを含めることで、新しいセグメントジョブを作成できます。

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
| `segmentId` | セグメントジョブを作成する対象のセグメント定義のID。 これらのセグメント定義は、異なる結合ポリシーに属することができます。 セグメント定義の詳細については、『[セグメント定義エンドポイントガイド](./segment-definitions.md)』を参照してください。 |

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
| `id` | 新しく作成されたセグメントジョブに対して、システムによって生成される読み取り専用の識別子。 |
| `status` | セグメントジョブの現在のステータス。 セグメントジョブは新しく作成されるので、ステータスは常に「新規」になります。 |
| `segments` | このセグメントジョブが実行される対象のセグメント定義に関する情報を含むオブジェクト。 |
| `segments.segment.id` | 指定したセグメント定義のID。 |
| `segments.segment.expression` | PQLで記述される、セグメント定義の式に関する情報を含むオブジェクト。 |

## 特定のセグメントジョブの取得 {#get}

`/segment/jobs`エンドポイントにGETリクエストを送信し、取得するセグメントジョブのIDをリクエストパスに指定することで、特定のセグメントジョブに関する詳細な情報を取得できます。

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
| `id` | セグメントジョブに関して、システム生成の読み取り専用識別子。 |
| `status` | セグメントジョブの現在のステータス。 ステータスには、「NEW」、「PROCESSING」、「CANCELLING」、「CANCELLED」、「FAILED」、「SUCCEEDED」などの値が考えられます。 |
| `segments` | セグメントジョブ内で返されるセグメント定義に関する情報を含むオブジェクト。 |
| `segments.segment.id` | セグメント定義のID。 |
| `segments.segment.expression` | PQLで記述される、セグメント定義の式に関する情報を含むオブジェクト。 |
| `metrics` | セグメントジョブに関する診断情報を含むオブジェクト。 |

## セグメントの一括取得ジョブ{#bulk-get}

`/segment/jobs/bulk-get`エンドポイントにPOSTリクエストを送信し、リクエスト本文にセグメントジョブの`id`値を指定することで、複数のセグメントジョブに関する詳細な情報を取得できます。

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
| `status` | セグメントジョブの現在のステータス。 ステータスには、「NEW」、「PROCESSING」、「CANCELLING」、「CANCELLED」、「FAILED」、「SUCCEEDED」などの値が考えられます。 |
| `segments` | セグメントジョブ内で返されるセグメント定義に関する情報を含むオブジェクト。 |
| `segments.segment.id` | セグメント定義のID。 |
| `segments.segment.expression` | PQLで記述される、セグメント定義の式に関する情報を含むオブジェクト。 |

## 特定のセグメントジョブのキャンセルまたは削除 {#delete}

`/segment/jobs`エンドポイントにDELETEリクエストを送信し、削除するセグメントジョブのIDをリクエストパスに指定することで、特定のセグメントジョブを削除できます。

>[!NOTE]
>
>削除リクエストに対するAPIの応答は直ちに行われます。 ただし、実際にセグメントジョブを削除するのは非同期的です。 つまり、セグメントジョブに対する削除リクエストが行われた時点と適用された時点との間に時間差があります。

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
