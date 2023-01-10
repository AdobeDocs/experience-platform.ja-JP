---
keywords: Experience Platform；ホーム；人気のトピック；セグメント化；セグメント化；セグメント化サービス；オーディエンス；オーディエンス；API;API;
title: Audiences API エンドポイント
description: Adobe Experience Platform Segmentation Service API のオーディエンスエンドポイントを使用すると、組織のオーディエンスをプログラムで管理できます。
exl-id: cb1a46e5-3294-4db2-ad46-c5e45f48df15
hide: true
hidefromtoc: true
source-git-commit: 59dfa862388394a68630a7136dee8e8988d0368c
workflow-type: tm+mt
source-wordcount: '1515'
ht-degree: 9%

---

# オーディエンスエンドポイント

>[!IMPORTANT]
>
>オーディエンスエンドポイントは現在ベータ版で、すべてのユーザーが使用できるわけではありません。 ドキュメントと機能は変更される場合があります。

オーディエンスとは、類似した行動や特性を共有する人々の集まりです。 これらの人々のコレクションは、Adobe Experience Platformを使用して、または外部ソースから生成できます。 以下を使用して、 `/audiences` エンドポイントを使用して、オーディエンスをプログラムで取得、作成、更新および削除できます。

## はじめに

このガイドで使用する エンドポイントは、[!DNL Adobe Experience Platform Segmentation Service]API の一部です。続行する前に、 [入門ガイド](./getting-started.md) を参照してください。

## オーディエンスのリストの取得 {#list}

組織のすべてのオーディエンスのリストを取得するには、 `/audiences` endpoint.

**API 形式**

`/audiences` エンドポイントは、結果を絞り込むのに役立つ、複数のクエリパラメーターをサポートしています。これらのパラメーターはオプションですが、リソースをリストする際の高価なオーバーヘッドを削減するために、パラメーターの使用を強くお勧めします。 パラメーターを指定せずにこのエンドポイントを呼び出すと、組織で使用可能なすべてのオーディエンスが取得されます。 複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。

```http
GET /audiences
GET /audiences?{QUERY_PARAMETERS}
```

オーディエンスのリストを取得する際には、次のクエリパラメーターを使用できます。

| クエリーパラメーター | 説明 | 例 |
| --------------- | ----------- | ------- |
| `start` | 返されるオーディエンスの開始オフセットを指定します。 | `start=5` |
| `limit` | 1 ページに返されるオーディエンスの最大数を指定します。 | `limit=10` |
| `sort` | 結果の並べ替え順を指定します。 これは、 `attributeName:[desc/asc]`. | `sort=updateTime:desc` |
| `property` | オーディエンスを指定できるフィルター **正確に** は属性の値に一致します。 これは、 `property=` | `property=audienceId==test-audience-id` |
| `name` | 名前を持つオーディエンスを指定できるフィルター **次を含む** 指定された値。 この値では大文字と小文字が区別されません。 | `name=Sample` |
| `description` | 説明を持つオーディエンスを指定できるフィルター **次を含む** 指定された値。 この値では大文字と小文字が区別されません。 | `description=Test Description` |
| `withMetrics` | オーディエンスに加えて指標も返すフィルター。 | `property=withMetrics==true` |

>[!IMPORTANT]
>
>オーディエンスの場合、指標は `metrics` 属性。プロファイル数、作成および更新のタイムスタンプに関する情報が含まれます。

**指標がありません**

次のリクエストと応答のペアは、 `withMetrics` クエリパラメーターが存在しません。

**リクエスト**

次のリクエストでは、組織で作成された直近 5 人のオーディエンスを取得します。

```shell
curl -X GET https: //platform.adobe.io/data/core/ups/audiences?limit=5 \
 -H 'Authorization:  Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id:  {IMS_ORG}' \
 -H 'x-api-key:  {API_KEY}' \
 -H 'x-sandbox-name:  {SANDBOX_NAME}'
```

**応答** {#no-metrics}

正常な応答は、HTTP ステータス 200 と、組織内で JSON として作成されたオーディエンスのリストを返します。

>[!NOTE]
>
>次の応答はスペースを節約するために切り捨てられ、最初に返されたオーディエンスのみが表示されます。

```json
{
    "children": [
        {
            "id": "60ccea95-1435-4180-97a5-58af4aa285ab",
            "audienceId": "60ccea95-1435-4180-97a5-58af4aa285ab",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "ttlInDays": 60,
            "profileInstanceId": "ups",
            "imsOrgId": "{ORG_ID}",
            "sandbox": {
                "sandboxId": "6ed34f6f-fe21-4a30-934f-6ffe21fa3075",
                "sandboxName": "prod",
                "type": "production",
                "default": true
            },
            "name": "People who ordered in the last 30 days",
            "description": "Last 30 days",
            "expression": {
                "type": "PQL",
                "format": "pql/text",
                "value": "workAddress.country = \"US\""
            },
            "mergePolicyId": "ef006bbe-750e-4e81-85f0-0c6902192dcc",
            "evaluationInfo": {
                "batch": {
                    "enabled": false
                },
                "continuous": {
                    "enabled": true
                },
                "synchronous": {
                    "enabled": false
                }
            },
            "dataGovernancePolicy": {
                "excludeOptOut": true
            },
            "isSystem": false,
            "creationTime": 1650374572000,
            "updateEpoch": 1650374573,
            "updateTime": 1650374573000,
            "createEpoch": 1650374572,
            "_etag": "\"33120d7c-0000-0200-0000-625eb7ad0000\"",
            "dependents": [],
            "definedOn": [
                {
                    "meta: resourceType": "unions",
                    "meta: containerId": "tenant",
                    "$ref": "https: //ns.adobe.com/xdm/context/profile__union"
                }
            ],
            "dependencies": [],
            "type": "SegmentDefinition",
            "overridePerformanceWarnings": false,
            "createdBy": "{CREATED_BY_ID}",
            "lifecycle": "published",
            "labels": [
                "core/C1"
            ],
            "namespace": "AEPSegments"
        }
    ]
}
```

| プロパティ | オーディエンスタイプ | 説明 |
| -------- | ------------- | ----------- | 
| `id` | 両方 | オーディエンスのシステム生成の読み取り専用識別子。 |
| `audienceId` | 両方 | オーディエンスが Platform で生成されたオーディエンスの場合、これは `id`. オーディエンスが外部で生成されている場合、この値はクライアントによって提供されます。 |
| `schema` | 両方 | オーディエンスのエクスペリエンスデータモデル (XDM) スキーマ。 |
| `imsOrgId` | 両方 | オーディエンスが属する組織の ID。 |
| `sandbox` | 両方 | オーディエンスが属するサンドボックスに関する情報です。 サンドボックスの詳細については、 [サンドボックスの概要](../../sandboxes/home.md). |
| `name` | 両方 | オーディエンスの名前。 |
| `description` | 両方 | オーディエンスの説明。 |
| `expression` | Platform が生成した | オーディエンスのプロファイルクエリ言語 (PQL) 式。 PQL 式の詳細については、 [PQL 式ガイド](../pql/overview.md). |
| `mergePolicyId` | Platform が生成した | オーディエンスが関連付けられている結合ポリシーの ID。 結合ポリシーについて詳しくは、[結合ポリシーガイド](../../profile/api/merge-policies.md)を参照してください。 |
| `evaluationInfo` | Platform が生成した | オーディエンスの評価方法を表示します。 使用可能な評価方法は、バッチ、ストリーミング、エッジです。 評価方法の詳細については、 [セグメントの概要](../home.md) |
| `dependents` | 両方 | 現在のオーディエンスに依存するオーディエンス ID の配列。 これは、セグメントのセグメントであるオーディエンスを作成する場合に使用されます。 |
| `dependencies` | 両方 | オーディエンスが依存するオーディエンス ID の配列。 これは、セグメントのセグメントであるオーディエンスを作成する場合に使用されます。 |
| `type` | 両方 | オーディエンスが Platform で生成されたものか、外部で生成されたオーディエンスかを表示する、システムで生成されたフィールド。 以下の値を指定できます。 `SegmentDefinition` および `ExternalAudience`. A `SegmentDefinition` は、Platform で生成されたオーディエンスを指し、 `ExternalAudience` は、Platform で生成されなかったオーディエンスを指します。 |
| `createdBy` | 両方 | オーディエンスを作成したユーザーの ID。 |
| `labels` | 両方 | オーディエンスに関連するオブジェクトレベルのデータ使用および属性ベースのアクセス制御ラベル。 |
| `namespace` | 両方 | オーディエンスが属する名前空間。 以下の値を指定できます。 `AAM`, `AAMSegments`, `AAMTraits`、および `AEPSegments`. |
| `audienceMeta` | 外部 web アプリケーション | 外部で作成されたオーディエンスから外部で作成されたメタデータ。 |

**指標を使用**

次のリクエストと応答のペアは、 `withMetrics` クエリーパラメーターが存在する。

**リクエスト**

次のリクエストでは、組織で作成された指標を使用して、直近 5 人のオーディエンスを取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/audiences?propoerty=withMetrics==true&limit=5&sort=totalProfiles:desc \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、指定された組織の JSON としてのオーディエンスのリストと指標を含む HTTP ステータス 200 を返します。

>[!NOTE]
>
>次の応答はスペースを節約するために切り捨てられ、最初に返されたオーディエンスのみが表示されます。

```json
{
    "children": [
        {
            "id": "60ccea95-1435-4180-97a5-58af4aa285ab",
            "audienceId": "60ccea95-1435-4180-97a5-58af4aa285ab",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "ttlInDays": 60,
            "profileInstanceId": "ups",
            "imsOrgId": "{ORG_ID}",
            "sandbox": {
                "sandboxId": "6ed34f6f-fe21-4a30-934f-6ffe21fa3075",
                "sandboxName": "prod",
                "type": "production",
                "default": true
            },
            "isSystem": false,
            "name": "People who ordered in the last 30 days",
            "description": "Last 30 days",
            "expression": {
                "type": "PQL",
                "format": "pql/text",
                "value": "workAddress.country = \"US\""
            },
            "mergePolicyId": "ef006bbe-750e-4e81-85f0-0c6902192dcc",
            "evaluationInfo": {
                "batch": {
                    "enabled": false
                },
                "continuous": {
                    "enabled": true
                },
                "synchronous": {
                    "enabled": false
                }
            },
            "dataGovernancePolicy": {
                "excludeOptOut": true
            },
            "creationTime": 1650374572000,
            "updateEpoch": 1650374573,
            "updateTime": 1650374573000,
            "createEpoch": 1650374572,
            "_etag": "\"33120d7c-0000-0200-0000-625eb7ad0000\"",
            "dependents": [],
            "definedOn": [
                {
                    "meta: resourceType": "unions",
                    "meta: containerId": "tenant",
                    "$ref": "https: //ns.adobe.com/xdm/context/profile__union"
                }
            ],
            "dependencies": [],
            "metrics": {
                "type": "export",
                "jobId": "test-job-id",
                "id": "32a83b5d-a118-4bd6-b3cb-3aee2f4c30a1",
                "data": {
                    "totalProfiles": 11200769,
                    "totalProfilesByNamespace": {
                        "crmid": 11400769
                    },
                    "totalProfilesByStatus": {
                        "existing": 11400769
                    }
                },
                "createEpoch": 1653583927,
                "updateEpoch": 1653583927
            },
            "type": "SegmentDefinition",
            "overridePerformanceWarnings": false,
            "createdBy": "{CREATED_BY_ID}",
            "lifecycle": "published",
            "labels": [
                "core/C1"
            ],
            "namespace": "AEPSegments"
        }
   ],
   "_page": {
      "totalCount": 111,
      "pageSize": 5,
      "next": "1"
   },
   "_links": {
      "next": {
         "href": "@/audiences?start=1&limit=5&totalCount=111"
      }
   }
}
```

次のリストに、プロパティを示します。 **排他的** から `withMetrics` 応答。 標準のオーディエンスプロパティを知りたい場合は、 [前のセクション](#no-metrics).

| プロパティ | 説明 |
| -------- | ----------- |
| `metrics.imsOrgId` | オーディエンスの組織 ID。 |
| `metrics.sandbox` | オーディエンスに関連するサンドボックス情報。 |
| `metrics.jobId` | オーディエンスを処理するセグメントジョブの ID。 |
| `metrics.type` | セグメントジョブタイプ。 これは、 `export` または `batch_segmentation`. |
| `metrics.id` | オーディエンスの ID。 |
| `metrics.data` | オーディエンスに関連する指標。 これには、オーディエンスに含まれるプロファイルの合計数、名前空間ごとのプロファイルの合計数、ステータスごとのプロファイルの合計数などの情報が含まれます。 |
| `metrics.createEpoch` | オーディエンスが作成された日時を示すタイムスタンプ。 |
| `metrics.updateEpoch` | オーディエンスが最後に更新された日時を示すタイムスタンプ。 |

## 新しいオーディエンスを作成する {#create}

新しいオーディエンスを作成するには、 `/audiences` endpoint.

**API 形式**

```http
POST /audiences
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/core/ups/audiences
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
        "name": "People who ordered in the last 30 days",
        "profileInstanceId": "ups",
        "description": "Last 30 days",
        "type": "SegmentDefinition",
        "expression": {
            "type": "PQL",
            "format": "pql/text",
            "value": "workAddress.country = \"US\""
        },
        "schema": {
            "name": "_xdm.context.profile"
        },
        "labels": [
          "core/C1"
        ]
    }'
```

| プロパティ | 説明 |
| -------- | ----------- | 
| `name` | オーディエンスの名前。 |
| `description` | オーディエンスの説明。 |
| `type` | オーディエンスが Platform で生成されたものか、外部で生成されたオーディエンスかを表示するフィールドです。 以下の値を指定できます。 `SegmentDefinition` および `ExternalAudience`. A `SegmentDefinition` は、Platform で生成されたオーディエンスを指し、 `ExternalAudience` は、Platform で生成されなかったオーディエンスを指します。 |
| `expression` | オーディエンスのプロファイルクエリ言語 (PQL) 式。 PQL 式の詳細については、 [PQL 式ガイド](../pql/overview.md). |
| `schema` | オーディエンスのエクスペリエンスデータモデル (XDM) スキーマ。 |
| `labels` | オーディエンスに関連するオブジェクトレベルのデータ使用および属性ベースのアクセス制御ラベル。 |

**応答**

```json
{
    "id": "60ccea95-1435-4180-97a5-58af4aa285ab",
    "audienceId": "60ccea95-1435-4180-97a5-58af4aa285ab",
     "schema": {
      "name": "_xdm.context.profile"
    },
    "ttlInDays": 60,
    "profileInstanceId": "ups",
    "imsOrgId": "{ORG_ID}",
    "sandbox": {
        "sandboxId": "6ed34f6f-fe21-4a30-934f-6ffe21fa3075",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "isSystem":false,     
    "name": "People who ordered in the last 30 days",
    "description": "Last 30 days",
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "workAddress.country = \"US\""
    },
    "mergePolicyId": "ef006bbe-750e-4e81-85f0-0c6902192dcc",
    "evaluationInfo": {
        "batch": {
          "enabled": false
        },
        "continuous": {
          "enabled": true
        },
        "synchronous": {
          "enabled": false
        }
    },
    "dataGovernancePolicy": {
      "excludeOptOut": true
    },
    "creationTime": 1650374572000,
    "updateEpoch": 1650374573,
    "updateTime": 1650374573000,
    "createEpoch": 1650374572,
    "_etag": "\"33120d7c-0000-0200-0000-625eb7ad0000\"",
    "dependents": [],
    "definedOn": [
        {
          "meta:resourceType": "unions",
          "meta:containerId": "tenant",
          "$ref": "https://ns.adobe.com/xdm/context/profile__union"
        }
    ],
    "dependencies": [],
    "type": "SegmentDefinition",
    "overridePerformanceWarnings": false,
    "createdBy": "{CREATED_BY_ID}",
    "lifecycle": "active",
    "labels": [
      "core/C1"
    ],
    "namespace": "AEPSegments"
}
```

## 指定したオーディエンスの検索 {#get}

特定のオーディエンスに関する詳細な情報を検索するには、 `/audiences` エンドポイントを検索し、リクエストパスで取得するオーディエンスの ID を指定します。

**API 形式**

```http
GET /audiences/{AUDIENCE_ID}
GET /audiences/{AUDIENCE_ID}?property=withmetrics==true
```

| パラメーター | 説明 |
| --------- | ----------- | 
| `{AUDIENCE_ID}` | 取得しようとしているオーディエンスの ID。 |
| `property=withmetrics==true` | オーディエンス指標を使用して指定のオーディエンスを取得する場合に使用できるオプションのクエリパラメーターです。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/audiences/60ccea95-1435-4180-97a5-58af4aa285ab \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、HTTP ステータス 200 と、指定されたオーディエンスに関する情報を返します。 応答は、オーディエンスがAdobe Experience Platformで生成されたか外部ソースで生成されたかに応じて異なります。

**Platform が生成した**

```json
{
    "id": "60ccea95-1435-4180-97a5-58af4aa285ab",
    "audienceId": "60ccea95-1435-4180-97a5-58af4aa285ab",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "ttlInDays": 60,
    "profileInstanceId": "ups",
    "imsOrgId": "{ORG_ID}",
    "sandbox": {
        "sandboxId": "6ed34f6f-fe21-4a30-934f-6ffe21fa3075",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "isSystem": false,
    "name": "People who ordered in the last 30 days",
    "description": "Last 30 days",
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "workAddress.country = \"US\""
    },
    "mergePolicyId": "ef006bbe-750e-4e81-85f0-0c6902192dcc",
    "evaluationInfo": {
        "batch": {
            "enabled": false
        },
        "continuous": {
            "enabled": true
        },
        "synchronous": {
            "enabled": false
        }
    },
    "dataGovernancePolicy": {
        "excludeOptOut": true
    },
    "creationTime": 1650374572000,
    "updateEpoch": 1650374573,
    "updateTime": 1650374573000,
    "createEpoch": 1650374572,
    "_etag": "\"33120d7c-0000-0200-0000-625eb7ad0000\"",
    "dependents": [],
    "definedOn": [
        {
            "meta:resourceType": "unions",
            "meta:containerId": "tenant",
            "$ref": "https://ns.adobe.com/xdm/context/profile__union"
        }
    ],
    "dependencies": [],
    "type": "SegmentDefinition",
    "overridePerformanceWarnings": false,
    "createdBy": "{CREATED_BY_ID}",
    "lifecycle": "active",
    "labels": [
        "core/C1"
    ],
    "namespace": "AEPSegments"
}
```

**外部で生成**

```json
{
    "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
    "audienceId": "test-external-audience-id",
    "name": "externalSegment1",
    "namespace": "aam",
    "imsOrgId": "{ORG_ID}",
    "sandbox": {
        "sandboxId": "6ed34f6f-fe21-4a30-934f-6ffe21fa3075",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "isSystem":false,
    "description": "Last 30 days",
    "type": "ExternalSegment",
    "lifecycle": "active",
    "createdBy": "{CREATED_BY_ID}",
    "datasetId": "6254cf3c97f8e31b639fb14d",
    "labels": [
        "core/C1"
    ],
    "_etag": "\"f4102699-0000-0200-0000-625cd61a0000\"",
    "creationTime": 1650251290000,
    "updateEpoch": 1650251290,
    "updateTime": 1650251290000,
    "createEpoch": 1650251290
}
```

## オーディエンスのフィールドの更新 {#update-field}

特定のオーディエンスのフィールドを更新するには、 `/audiences` エンドポイントを作成し、更新するオーディエンスの ID をリクエストパスで指定します。

**API 形式**

```http
PATCH /audiences/{AUDIENCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{AUDIENCE_ID}` | 更新するオーディエンスの ID。 |

**リクエスト**

```shell
curl -X PATCH https://platform.adobe.io/data/core/ups/audiences/4afe34ae-8c98-4513-8a1d-67ccaa54bc05 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
     [
        {
            "op": "add",
            "path": "/expression",
            "value": {
                "type": "PQL",
                "format": "pql/text",
                "value": "workAddress.country = \"CA\""
            }
        }
      ]'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `op` | オーディエンスを更新する場合、この値は常に `add`. |
| `path` | 更新するフィールドのパス。 |
| `value` | フィールドを更新する値です。 |

**応答**

正常な応答は、HTTP ステータス 200 と、新しく更新されたオーディエンスに関する情報を返します。

```json
{
    "id": "60ccea95-1435-4180-97a5-58af4aa285ab",
    "audienceId": "60ccea95-1435-4180-97a5-58af4aa285ab",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "ttlInDays": 60,
    "profileInstanceId": "ups",
    "imsOrgId": "{ORG_ID}",
    "sandbox": {
        "sandboxId": "6ed34f6f-fe21-4a30-934f-6ffe21fa3075",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "name": "People who ordered in the last 30 days",
    "description": "Last 30 days",
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "workAddress.country = \"CA\""
    },
    "mergePolicyId": "ef006bbe-750e-4e81-85f0-0c6902192dcc",
    "evaluationInfo": {
        "batch": {
          "enabled": false
        },
        "continuous": {
          "enabled": true
        },
        "synchronous": {
          "enabled": false
        }
    },
    "dataGovernancePolicy": {
      "excludeOptOut": true
    },
    "creationTime": 1650374572000,
    "updateEpoch": 1650374573,
    "updateTime": 1650374573000,
    "createEpoch": 1650374572,
    "_etag": "\"33120d7c-0000-0200-0000-625eb7ad0000\"",
    "dependents": [],
    "definedOn": [
        {
          "meta:resourceType": "unions",
          "meta:containerId": "tenant",
          "$ref": "https://ns.adobe.com/xdm/context/profile__union"
        }
    ],
    "dependencies": [],
    "type": "SegmentDefinition",
    "overridePerformanceWarnings": false,
    "createdBy": "{CREATED_BY_ID}",
    "lifecycle": "active",
    "labels": [
      "core/C1"
    ],
    "namespace": "AEPSegments"
}
```

## オーディエンスの更新 {#put}

特定のオーディエンスを更新（上書き）するには、 `/audiences` エンドポイントを作成し、更新するオーディエンスの ID をリクエストパスで指定します。

**API 形式**

```http
PUT /audiences/{AUDIENCE_ID}
```

**リクエスト**

```shell
curl -X PUT https://platform.adobe.io/data/core/ups/audiences/4afe34ae-8c98-4513-8a1d-67ccaa54bc05 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
    "audienceId":"test-external-audience-id",
    "name":"new externalSegment",
    "namespace":"aam",
    "description":"Last 30 days",
    "type":"ExternalSegment",
    "lifecycle":"published",
    "datasetId":"6254cf3c97f8e31b639fb14d",
    "labels":[
        "core/C1"
    ]
}' 
```

| プロパティ | 説明 |
| -------- | ----------- | 
| `audienceId` | オーディエンスの ID。 これは、外部のオーディエンスで使用されます |
| `name` | オーディエンスの名前。 |
| `namespace` |  |
| `description` | オーディエンスの説明。 |
| `type` | オーディエンスが Platform で生成されたものか、外部で生成されたオーディエンスかを表示する、システムで生成されたフィールド。 以下の値を指定できます。 `SegmentDefinition` および `ExternalAudience`. A `SegmentDefinition` は、Platform で生成されたオーディエンスを指し、 `ExternalAudience` は、Platform で生成されなかったオーディエンスを指します。 |
| `lifecycle` | オーディエンスのステータス。 以下の値を指定できます。 `draft`, `published`, `inactive`、および `archived`. `draft` は、オーディエンスが作成される日時を表します。 `published` オーディエンスが公開された時点で、 `inactive` オーディエンスがアクティブでなくなり、 `archived` （オーディエンスが削除された場合） |
| `datasetId` | オーディエンスデータが見つかるデータセットの ID。 |
| `labels` | オーディエンスに関連するオブジェクトレベルのデータ使用および属性ベースのアクセス制御ラベル。 |

**応答**

正常な応答は、HTTP ステータス 200 と、新しく更新されたオーディエンスの詳細を返します。 オーディエンスの詳細は、Platform で生成されたオーディエンスか、外部で生成されたオーディエンスかによって異なることに注意してください。

```json
{
    "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
    "audienceId": "test-external-audience-id",
    "name": "new externalSegment",
    "namespace": "aam",
    "imsOrgId": "{ORG_ID}",
    "sandbox": {
        "sandboxId": "6ed34f6f-fe21-4a30-934f-6ffe21fa3075",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "description": "Last 30 days",
    "type": "ExternalSegment",
    "lifecycle": "published",
    "createdBy": "{CREATED_BY_ID}",
    "datasetId": "6254cf3c97f8e31b639fb14d",
    "_etag": "\"f4102699-0000-0200-0000-625cd61a0000\"",
    "creationTime": 1650251290000,
    "updateEpoch": 1650251290,
    "updateTime": 1650251290000,
    "createEpoch": 1650251290
}
```

## オーディエンスの削除 {#delete}

特定のオーディエンスを削除するには、 `/audiences` エンドポイントを探し、リクエストパスで削除するオーディエンスの ID を指定します。

**API 形式**

```http
DELETE /audiences/{AUDIENCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{AUDIENCE_ID}` | 削除するオーディエンスの ID。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/audiences/60ccea95-1435-4180-97a5-58af4aa285ab5 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合、HTTP ステータス 204 が返され、メッセージは返されません。
