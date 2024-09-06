---
solution: Experience Platform
title: セグメントジョブ API エンドポイント
description: Adobe Experience Platform Segmentation Service API のセグメントジョブエンドポイントを使用すると、組織のセグメントジョブをプログラムで管理できます。
role: Developer
exl-id: 105481c2-1c25-4f0e-8fb0-c6577a4616b3
source-git-commit: f35fb6aae6aceb75391b1b615ca067a72918f4cf
workflow-type: tm+mt
source-wordcount: '1648'
ht-degree: 15%

---

# セグメントジョブエンドポイント

セグメントジョブは、オーディエンスセグメントをオンデマンドで作成する非同期プロセスです。 [ セグメント定義 ](./segment-definitions.md) と、[!DNL Real-Time Customer Profile] がプロファイルフラグメント間で重複する属性をどのように結合するかを制御する [ 結合ポリシー ](../../profile/api/merge-policies.md) を参照します。 セグメントジョブが正常に完了すると、処理中に発生した可能性のあるエラーやオーディエンスの最終的なサイズなど、セグメントに関するさまざまな情報を収集できます。

このガイドは、セグメントジョブをよりよく理解するのに役立つ情報を提供し、API を使用して基本的なアクションを実行するためのサンプル API 呼び出しを含みます。

## はじめに

このガイドで使用するエンドポイントは、[!DNL Adobe Experience Platform Segmentation Service] API の一部です。 続行する前に、[ はじめる前に ](./getting-started.md) を参照して、必要なヘッダーやサンプル API 呼び出しの読み取り方法など、API の呼び出しを正常に実行するために必要な重要な情報を確認してください。

## セグメントジョッブリストの取得 {#retrieve-list}

`/segment/jobs` エンドポイントにGETリクエストを行うことで、組織のすべてのセグメントジョブのリストを取得できます。

**API 形式**

`/segment/jobs` エンドポイントは、結果を絞り込むのに役立つ、複数のクエリパラメーターをサポートしています。これらのパラメーターはオプションですが、高価なオーバーヘッドの削減に役立てるため、使用することを強くお勧めします。 パラメーターを指定せずにこのエンドポイントを呼び出すと、組織で使用可能なすべての書き出しジョブが取得されます。 複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。

```http
GET /segment/jobs
GET /segment/jobs?{QUERY_PARAMETERS}
```

**クエリパラメータ**

+++ 使用可能なクエリパラメーターのリスト。

| パラメーター | 説明 | 例 |
| --------- | ----------- | ------- |
| `start` | 返されるセグメントジョブの開始オフセットを指定します。 | `start=1` |
| `limit` | 1 ページに返されるセグメントジョブの数を指定します。 | `limit=20` |
| `status` | ステータスに基づいて結果をフィルターします。サポートされる値は、NEW、QUEUED、PROCESSING、SUCCEEDED、FAILED、CANCELLING、CANCELLED です。 | `status=NEW` |
| `sort` | 返されたセグメントジョブを並べ替えます。`[attributeName]:[desc|asc]` の形式で書き込まれます。 | `sort=creationTime:desc` |
| `property` | セグメントジョブをフィルターし、指定されたフィルターへの完全一致を取得します。次のいずれかの形式で書き込むことができます。 <ul><li>`[jsonObjectPath]==[value]` — オブジェクトキーに対するフィルター</li><li>`[arrayTypeAttributeName]~[objectKey]==[value]` — 配列内のフィルタ－</li></ul> | `property=segments~segmentId==workInUS` |

+++

**リクエスト**

+++ セグメントジョブのリストを表示するリクエストのサンプルです。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/segment/jobs?status=SUCCEEDED \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答に成功すると、HTTP ステータス 200 が、指定した組織のセグメントジョブのリストと共に JSON として返されます。 ただし、応答はセグメントジョブ内のセグメント定義の数によって異なります。

>[!BEGINTABS]

>[!TAB  セグメントジョブの 1,500 個のセグメント定義以下 ]

セグメントジョブで実行されているセグメント定義が 1,500 個未満の場合は、すべてのセグメント定義の完全なリストが `children.segments` 属性内に表示されます。

>[!NOTE]
>
>次の応答はスペースを節約するために切り捨てられており、最初に返されたジョブのみが表示されます。

+++ セグメントジョブのリストを取得する際の応答例です。

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

+++

>[!TAB 1500 を超えるセグメント定義 ]

セグメントジョブで 1500 を超えるセグメント定義が実行されている場合は、`children.segments` 属性が `*` と表示され、すべてのセグメント定義が評価中であることを示します。

>[!NOTE]
>
>次の応答はスペースを節約するために切り捨てられており、最初に返されたジョブのみが表示されます。

+++ セグメントジョブのリストを表示する際の応答例。

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
                    "segmentId": "*",
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
                "totalProfiles": 13146432,
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
| `status` | セグメントジョブの現在のステータス。 ステータスの可能性のある値には、「NEW」、「PROCESSING」、「CANCELLED」、「CANCELLED」、「FAILED」、「SUCCESSFUL」などがあります。 |
| `segments` | セグメントジョブ内で返されるセグメント定義に関する情報を含むオブジェクト。 |
| `segments.segment.id` | セグメント定義の ID。 |
| `segments.segment.expression` | セグメント定義の式に関する情報を含むオブジェクト（PQLで記述）。 |
| `metrics` | セグメントジョブに関する診断情報を含むオブジェクト。 |
| `metrics.totalTime` | セグメント化ジョブの開始時刻と終了時刻および合計所要時間に関する情報を含むオブジェクト。 |
| `metrics.profileSegmentationTime` | セグメント化の評価の開始時刻と終了時刻および合計所要時間に関する情報を含むオブジェクト。 |
| `metrics.segmentProfileCounter` | セグメントごとに選定されたプロファイルの数。 |
| `metrics.segmentedProfileByNamespaceCounter` | セグメント定義ごとに、各 ID 名前空間に適合するプロファイルの数。 |
| `metrics.segmentProfileByStatusCounter` | 各ステータスのプロファイルの数。 次の 3 つのステータスがサポートされています。 <ul><li>「実現済み」 – セグメント定義に適合するプロファイルの数。</li><li>「離脱済み」 – セグメント定義に存在しなくなったプロファイルの数。</li></ul> |
| `metrics.totalProfilesByMergePolicy` | 結合ポリシーごとの結合プロファイルの合計数。 |

+++

>[!ENDTABS]

## 新しいセグメントジョブの作成 {#create}

新しいセグメントジョブを作成するには、`/segment/jobs` エンドポイントにPOSTリクエストを行い、新しいオーディエンスを作成するセグメント定義の ID を本文に含めます。

**API 形式**

```http
POST /segment/jobs
```

新しいセグメントジョブを作成する場合、リクエストと応答は、セグメントジョブ内のセグメント定義の数によって異なります。

>[!BEGINTABS]

>[!TAB  セグメントジョブのセグメント数が 1,500 個以下 ]

**リクエスト**

+++新しいセグメントジョブを作成するためのサンプルリクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/ups/segment/jobs \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '[
    {
        "segmentId": "7863c010-e092-41c8-ae5e-9e533186752e"
    },
    {
        "segmentId": "07d39471-05d1-4083-a310-d96978fd7c85"
    }
 ]'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `segmentId` | セグメントジョブを作成するセグメント定義の ID。 これらのセグメント定義は、異なる結合ポリシーに属することができます。 セグメント定義について詳しくは、[ セグメント定義エンドポイントガイド ](./segment-definitions.md) を参照してください。 |

+++

**応答**

応答に成功すると、HTTP ステータス 200 と、新しく作成されたセグメントジョブに関する情報が返されます。

+++ 新しいセグメントジョブを作成する際のサンプル応答。

```json
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
    "status": "PROCESSING",
    "batchId": "678f53bc-e21d-4c47-a7ec-5ad0064f8e4c",
    "computeJobId": 8811,
    "computeGatewayJobId": "9ea97b25-a0f5-410e-ae87-b2d85e58f399",
    "segments": [
        {
            "segmentId": "7863c010-e092-41c8-ae5e-9e533186752e",
            "segment": {
                "id": "7863c010-e092-41c8-ae5e-9e533186752e",
                "expression": {
                    "type": "PQL",
                    "format": "pql/json",
                    "value": "workAddress.country = \"US\""
                },
                "mergePolicyId": "25c548a0-ca7f-4dcd-81d5-997642f178b9",
                "mergePolicy": {
                    "id": "25c548a0-ca7f-4dcd-81d5-997642f178b9",
                    "version": 1
                }
            }
        },
        {
            "segmentId": "07d39471-05d1-4083-a310-d96978fd7c85",
            "segment": {
                "id": "07d39471-05d1-4083-a310-d96978fd7c85",
                "expression": {
                    "type": "PQL",
                    "format": "pql/json",
                    "value": "workAddress.country = \"US\""
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
        "segmentedProfileCounter":{
            "7863c010-e092-41c8-ae5e-9e533186752e":1033
        },
        "segmentedProfileByNamespaceCounter":{
            "7863c010-e092-41c8-ae5e-9e533186752e":{
                "tenantiduserobjid":1033,
                "campaign_profile_mscom_mkt_prod2":1033
            }
        },
        "segmentedProfileByStatusCounter":{
            "7863c010-e092-41c8-ae5e-9e533186752e":{
                "exited":144646,
                "realized":2056
            }
        },
        "totalProfiles":13146432,
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
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | 新しく作成されたセグメントジョブのシステム生成の読み取り専用識別子。 |
| `status` | セグメントジョブの現在のステータス。 セグメントジョブは新しく作成されるので、ステータスは常に「新規」になります。 |
| `segments` | このセグメントジョブが実行されているセグメント定義に関する情報を含むオブジェクト。 |
| `segments.segment.id` | 指定したセグメント定義の ID。 |
| `segments.segment.expression` | セグメント定義の式に関する情報を含むオブジェクト（PQLで記述）。 |

+++

>[!TAB  セグメントジョブに 1,500 を超えるセグメント定義 ]

**リクエスト**

>[!NOTE]
>
>1,500 個を超えるセグメント定義を持つセグメントジョブを作成できますが、これは **強くお勧めしません**。

+++ セグメントジョブを作成するためのサンプルリクエスト。

```shell
curl -X POST https://platform.adobe.io/data/core/ups/segment/jobs \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
    "schema": {
        "name": "_xdm.context.profile"
    },
    "segments": [
        {
            "segmentId": "*"
        }
    ]
 }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `schema.name` | セグメント定義のスキーマの名前。 |
| `segments.segmentId` | 1500 個を超えるセグメントを含むセグメントジョブを実行する場合、すべてのセグメントを使用してセグメント化ジョブを実行することを示すために、`*` をセグメント ID として渡す必要があります。 |

+++

**応答**

正常な応答は、HTTP ステータス 200 と、新しく作成したセグメントジョブの詳細を返します。

+++ セグメントジョブ作成時の応答例

```json
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
    "status": "PROCESSING",
    "batchId": "678f53bc-e21d-4c47-a7ec-5ad0064f8e4c",
    "computeJobId": 8811,
    "computeGatewayJobId": "9ea97b25-a0f5-410e-ae87-b2d85e58f399",
    "segments": [
        {
            "segmentId": "*"
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
        "segmentedProfileCounter":{
            "7863c010-e092-41c8-ae5e-9e533186752e":1033
        },
        "segmentedProfileByNamespaceCounter":{
            "7863c010-e092-41c8-ae5e-9e533186752e":{
                "tenantiduserobjid":1033,
                "campaign_profile_mscom_mkt_prod2":1033
            }
        },
        "segmentedProfileByStatusCounter":{
            "7863c010-e092-41c8-ae5e-9e533186752e":{
                "exited":144646,
                "realized":2056
            }
        },
        "totalProfiles":13146432,
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
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | 新しく作成されたセグメントジョブのシステム生成の読み取り専用識別子。 |
| `status` | セグメントジョブの現在のステータス。 セグメントジョブが新しく作成されるので、ステータスは常に `NEW` になります。 |
| `segments` | このセグメントジョブが実行されているセグメント定義に関する情報を含むオブジェクト。 |
| `segments.segment.id` | `*` は、このセグメントジョブが組織内のすべてのセグメント定義に対して実行されていることを意味します。 |

+++

>[!ENDTABS]


## 特定のセグメントジョブの取得 {#get}

特定のセグメントジョブに関する詳細な情報を取得するには、`/segment/jobs` エンドポイントにGETリクエストを実行し、取得するセグメントジョブの ID をリクエストパスで指定します。

**API 形式**

```http
GET /segment/jobs/{SEGMENT_JOB_ID}
```

| プロパティ | 説明 |
| -------- | ----------- | 
| `{SEGMENT_JOB_ID}` | 取得するセグメントジョブの `id` 値。 |

**リクエスト**

+++ セグメントジョブを取得するためのリクエストの例です。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/segment/jobs/d3b4a50d-dfea-43eb-9fca-557ea53771fd \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

リクエストが成功した場合は、指定されたセグメントジョブの詳細情報とともに HTTP ステータス 200 が返されます。  ただし、応答はセグメントジョブ内のセグメント定義の数によって異なります。

>[!BEGINTABS]

>[!TAB  セグメントジョブの 1,500 個のセグメント定義以下 ]

セグメントジョブで実行されているセグメント定義が 1,500 個未満の場合は、すべてのセグメント定義の完全なリストが `children.segments` 属性内に表示されます。

+++ セグメントジョブを取得するためのサンプル応答。

```json
{
    "id": "d3b4a50d-dfea-43eb-9fca-557ea53771fd",
    "imsOrgId": "{ORG_ID}",
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

+++

>[!TAB 1500 を超えるセグメント定義 ]

セグメントジョブで 1500 を超えるセグメント定義が実行されている場合は、`children.segments` 属性が `*` と表示され、すべてのセグメント定義が評価中であることを示します。

+++ セグメントジョブを取得するためのサンプル応答。

```json
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
            "segmentId": "*"
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
        "segmentedProfileCounter":{
            "7863c010-e092-41c8-ae5e-9e533186752e":1033
        },
        "segmentedProfileByNamespaceCounter":{
            "7863c010-e092-41c8-ae5e-9e533186752e":{
                "tenantiduserobjid":1033,
                "campaign_profile_mscom_mkt_prod2":1033
            }
        },
        "segmentedProfileByStatusCounter":{
            "7863c010-e092-41c8-ae5e-9e533186752e":{
                "exited":144646,
                "realized":2056
            }
        },
        "totalProfiles":13146432,
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
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | セグメントジョブのシステム生成の読み取り専用識別子。 |
| `status` | セグメントジョブの現在のステータス。 ステータスの可能性のある値には、「NEW」、「PROCESSING」、「CANCELLED」、「CANCELLED」、「FAILED」、「SUCCESSFUL」などがあります。 |
| `segments` | セグメントジョブ内で返されるセグメント定義に関する情報を含むオブジェクト。 |
| `segments.segment.id` | セグメント定義の ID。 |
| `segments.segment.expression` | セグメント定義の式に関する情報を含むオブジェクト（PQLで記述）。 |
| `metrics` | セグメントジョブに関する診断情報を含むオブジェクト。 |

+++

>[!ENDTABS]

## セグメントジョブの一括取得 {#bulk-get}

POST `/segment/jobs/bulk-get` エンドポイントに対してセグメントリクエストを実行し、リクエスト本文にセグメントジョブの `id` 値を指定することで、複数のセグメントジョブに関する詳細な情報を取得できます。

**API 形式**

```http
POST /segment/jobs/bulk-get
```

**リクエスト**

+++ 一括取得エンドポイントを使用するためのサンプルリクエスト。

```shell
curl -X POST https://platform.adobe.io/data/core/ups/segment/jobs/bulk-get \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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

+++

**応答**

応答に成功すると、HTTP ステータス 207 とリクエストされたセグメントジョブが返されます。 ただし、`children.segments` 属性の値は、セグメントジョブが 1500 を超えるセグメント定義で実行されているかどうかによって異なります。

>[!NOTE]
>
>次の応答はスペースを節約するために切り捨てられており、各セグメントジョブの一部の詳細のみを表示しています。 完全な応答には、リクエストされたセグメントジョブの詳細がリストされます。

+++ 一括取得応答を使用する場合の応答例。

```json
{
    "results": {
        "cc3419d3-0389-47f1-b174-fead6b3c830d": {
            "id": "cc3419d3-0389-47f1-b174-fead6b3c830d",
            "imsOrgId": "{ORG_ID}",
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
            "imsOrgId": "{ORG_ID}",
            "status": "SUCCEEDED",
            "segments": [
                {
                    "segmentId": "*"
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
| `status` | セグメントジョブの現在のステータス。 ステータスの可能性のある値には、「NEW」、「PROCESSING」、「CANCELLED」、「CANCELLED」、「FAILED」、「SUCCESSFUL」などがあります。 |
| `segments` | セグメントジョブ内で返されるセグメント定義に関する情報を含むオブジェクト。 |
| `segments.segment.id` | セグメント定義の ID。 |
| `segments.segment.expression` | セグメント定義の式に関する情報を含むオブジェクト（PQLで記述）。 |

+++

## 特定のセグメントジョブのキャンセルまたは削除 {#delete}

特定のセグメントジョブを削除するには、`/segment/jobs` エンドポイントにDELETEリクエストを実行し、リクエストパスで削除するセグメントジョブの ID を指定します。

>[!NOTE]
>
>削除リクエストに対する API 応答は即座に行われます。 ただし、セグメントジョブの実際の削除は非同期です。 つまり、セグメントジョブに対する削除リクエストが行われた時間と、削除リクエストが適用された時間には時間差があります。

**API 形式**

```http
DELETE /segment/jobs/{SEGMENT_JOB_ID}
```

| プロパティ | 説明 |
| -------- | ----------- | 
| `{SEGMENT_JOB_ID}` | 削除するセグメントジョブの `id` 値。 |

**リクエスト**

+++ セグメントジョブを削除するリクエストの例。

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/segment/jobs/d3b4a50d-dfea-43eb-9fca-557ea53771fd \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 204 が、空の応答本文と共に返されます。

## 次の手順

このガイドを読むことで、セグメントジョブの仕組みについて理解が深まりました。
