---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: セグメントジョブ
topic: developer guide
translation-type: tm+mt
source-git-commit: b0554d931718bb6a8dd7d4f971daf3652a19a2a8
workflow-type: tm+mt
source-wordcount: '657'
ht-degree: 3%

---


# セグメントジョブ開発ガイド

セグメントジョブは、新しいオーディエンスセグメントを作成する非同期プロセスです。 セグメント定義と、リアルタイム顧客プロファイルがプロファイルフラグメント間で重なり合う属性をマージする方法を制御するマージポリシーを参照します。 セグメントジョブが正常に完了したら、処理中に発生した可能性のあるエラーやオーディエンスの最大サイズなど、セグメントに関する様々な情報を収集できます。

このガイドは、セグメントのジョブをより深く理解するのに役立つ情報を提供し、APIを使用して基本的なアクションを実行するためのサンプルAPI呼び出しを含みます。

## はじめに

このガイドで使用されるAPIエンドポイントは、セグメント化APIの一部です。 先に進む前に、 [セグメント化開発ガイドを参照してください](./getting-started.md)。

特に、セグメント化開発ガイドの「 [はじめに](./getting-started.md#getting-started) 」の節には、関連トピックへのリンク、ドキュメント内のサンプルAPI呼び出しを読むためのガイド、Experience Platform APIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報が含まれています。

## セグメントジョブのリストの取得

エンドポイントにGETリクエストを作成して、IMS組織のすべてのセグメントジョブのリストを取得でき `/segment/jobs` ます。

**API形式**

```http
GET /segment/jobs
GET /segment/jobs?{QUERY_PARAMETERS}
```

- `{QUERY_PARAMETERS}`: (*オプション*)リクエストパスに追加されるパラメーター。応答で返される結果を設定します。 複数のパラメーターを含める場合は、アンパサンド(`&`)で区切ります。 使用可能なパラメーターを以下に示します。

**クエリパラメーター**

次に、セグメントジョブのリスト表示に使用できるクエリパラメータのリストを示します。 これらのパラメーターはすべてオプションです。 パラメーターを指定せずにこのエンドポイントを呼び出すと、組織で使用可能なすべてのセグメントジョブが取得されます。

| パラメーター | 説明 |
| --------- | ----------- |
| `start` | 返されるセグメントジョブの開始オフセットを指定します。 |
| `limit` | 1ページに返されるセグメントジョブの数を指定します。 |
| `status` | ステータスに基づいて結果をフィルターします。 サポートされる値は、NEW、QUEUED、PROCESSING、SUCCEEDEDED、FAILED、CANCELLING、CANCELLEDです |
| `sort` | 返されたセグメントジョブの順序。 は、形式で書き込まれ `[attributeName]:[desc|asc]`ます。 |
| `property` | フィルターは、ジョブをセグメント化し、指定されたフィルターに完全一致を取得します。 次のいずれかの形式で書き込むことができます。 <ul><li>`[jsonObjectPath]==[value]`  — オブジェクトキーに対するフィルタリング</li><li>`[arrayTypeAttributeName]~[objectKey]==[value]`  — 配列内のフィルタ</li></ul> |

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

>[!NOTE] 次の応答は領域に対して切り捨てられ、最初に返されたジョブのみを表示します。

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

## 新しいセグメントジョブの作成

エンドポイントにPOSTリクエストを作成して、新しいセグメントジョブを作成でき `/segment/jobs` ます。

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
]
 '
```

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

## 特定のセグメントジョブの取得

エンドポイントにGETリクエストを送信し、リクエストパスにセグメントジョブの `/segment/jobs``id` 値を指定することで、特定のセグメントジョブに関する詳細な情報を取得できます。

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

## セグメントの一括取得ジョブ

エンドポイントにPOSTリクエストを送信し、リクエスト本文にセグメントジョブの `/segment/jobs/bulk-get``id` 値を指定することで、指定した複数のセグメントジョブに関する詳細な情報を取得できます。

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

>[!NOTE] 次の応答は領域に対して切り捨てられ、各セグメントジョブの詳細の一部のみが表示されました。 完全な回答には、リクエストされたセグメントジョブの完全な詳細がリストされます。

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

## 特定のセグメントジョブのキャンセルまたは削除

エンドポイントにDELETEリクエストを送信し、リクエストパスにセグメントジョブの `/segment/jobs``id` 値を指定することで、指定したセグメントジョブを削除するようにリクエストできます。

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

このガイドを読むと、セグメントジョブの動作についての理解が深まります。 セグメント化について詳しくは、 [セグメント化の概要を参照してください](../home.md)。