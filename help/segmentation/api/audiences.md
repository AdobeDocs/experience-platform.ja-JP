---
title: Audiences API エンドポイント
description: Adobe Experience Platform Segmentation Service API のオーディエンスエンドポイントを使用して、組織のオーディエンスをプログラムで作成、管理、更新します。
exl-id: cb1a46e5-3294-4db2-ad46-c5e45f48df15
source-git-commit: dbb7e0987521c7a2f6512f05eaa19e0121aa34c6
workflow-type: tm+mt
source-wordcount: '2124'
ht-degree: 8%

---

# オーディエンスエンドポイント

オーディエンスとは、類似した行動や特性を共有する人々の集まりです。 これらの人々のコレクションは、Adobe Experience Platformを使用して、または外部ソースから生成できます。 以下を使用して、 `/audiences` エンドポイントを使用して、オーディエンスをプログラムで取得、作成、更新および削除できます。

## Destination SDK の

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

**リクエスト**

次のリクエストでは、組織で作成された最後の 2 つのオーディエンスを取得します。

+++オーディエンスのリストを取得するためのサンプルリクエスト。

```shell
curl -X GET https: //platform.adobe.io/data/core/ups/audiences?limit=2 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

正常な応答は、HTTP ステータス 200 と、組織内で JSON として作成されたオーディエンスのリストを返します。

+++組織に属する、最後に作成された 2 つのオーディエンスを含むサンプル応答

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
            "originName": "REAL_TIME_CUSTOMER_PROFILE",
            "overridePerformanceWarnings": false,
            "createdBy": "{CREATED_BY_ID}",
            "lifecycleState": "published",
            "labels": [
                "core/C1"
            ],
            "namespace": "AEPSegments"
        },
        {
            "id": "32a83b5d-a118-4bd6-b3cb-3aee2f4c30a1",
            "audienceId": "test-external-audience-id",
            "name": "externalSegment1",
            "namespace": "aam",
            "imsOrgId": "{ORG_ID}",
            "sandbox":{
                "sandboxId": "6ed34f6f-fe21-4a30-934f-6ffe21fa3075",
                "sandboxName": "prod",
                "type": "production",
                "default": true
            },
            "isSystem": false,
            "description": "Last 30 days",
            "type": "ExternalSegment",
            "originName": "CUSTOM_UPLOAD",
            "lifecycleState": "published",
            "createdBy": "{CREATED_BY_ID}",
            "datasetId": "6254cf3c97f8e31b639fb14d",
            "labels":[
                "core/C1"
            ],
            "linkedAudienceRef": {
                "flowId": "4685ea90-d2b6-11ec-9d64-0242ac120002"
            },
            "creationTime": 1642745034000000,
            "updateEpoch": 1649926314,
            "updateTime": 1649926314000,
            "createEpoch": 1642745034
        }
    ],
    "_page":{
      "totalCount": 111,
      "pageSize": 2,
      "next": "1"
   },
   "_links":{
      "next":{
         "href":"@/audiences?start=1&limit=2&totalCount=111"
      }
   }
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
| `evaluationInfo` | Platform が生成した | オーディエンスの評価方法を表示します。 使用可能な評価方法は、バッチ、同期（ストリーミング）、連続（エッジ）です。 評価方法の詳細については、 [セグメントの概要](../home.md) |
| `dependents` | 両方 | 現在のオーディエンスに依存するオーディエンス ID の配列。 これは、セグメントのセグメントであるオーディエンスを作成する場合に使用されます。 |
| `dependencies` | 両方 | オーディエンスが依存するオーディエンス ID の配列。 これは、セグメントのセグメントであるオーディエンスを作成する場合に使用されます。 |
| `type` | 両方 | オーディエンスが Platform で生成されたものか、外部で生成されたオーディエンスかを表示する、システムで生成されたフィールド。 以下の値を指定できます。 `SegmentDefinition` および `ExternalSegment`. A `SegmentDefinition` は、Platform で生成されたオーディエンスを指し、 `ExternalSegment` は、Platform で生成されなかったオーディエンスを指します。 |
| `originName` | 両方 | オーディエンスの接触チャネルの名前を参照するフィールド。 Platform が生成するオーディエンスの場合、この値は `REAL_TIME_CUSTOMER_PROFILE`. Audience Orchestration で生成されたオーディエンスの場合、この値は次のようになります。 `AUDIENCE_ORCHESTRATION`. Adobe Audience Managerで生成されるオーディエンスの場合、この値は `AUDIENCE_MANAGER`. 他の外部で生成されたオーディエンスの場合、この値は `CUSTOM_UPLOAD`. |
| `createdBy` | 両方 | オーディエンスを作成したユーザーの ID。 |
| `labels` | 両方 | オーディエンスに関連するオブジェクトレベルのデータ使用および属性ベースのアクセス制御ラベル。 |
| `namespace` | 両方 | オーディエンスが属する名前空間。 以下の値を指定できます。 `AAM`, `AAMSegments`, `AAMTraits`、および `AEPSegments`. |
| `linkedAudienceRef` | 両方 | 他のオーディエンス関連システムへの識別子を含むオブジェクト。 |

+++

## 新しいオーディエンスを作成する {#create}

新しいオーディエンスを作成するには、 `/audiences` endpoint.

**API 形式**

```http
POST /audiences
```

**リクエスト**

>[!BEGINTABS]

>[!TAB Platform で生成されたオーディエンス]

+++ Platform で生成されたオーディエンスを作成するためのサンプルリクエスト

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
        ],
        "ttlInDays": 60
    }'
```

| プロパティ | 説明 |
| -------- | ----------- | 
| `name` | オーディエンスの名前。 |
| `description` | オーディエンスの説明。 |
| `type` | オーディエンスが Platform で生成されたものか、外部で生成されたオーディエンスかを表示するフィールドです。 以下の値を指定できます。 `SegmentDefinition` および `ExternalSegment`. A `SegmentDefinition` は、Platform で生成されたオーディエンスを指し、 `ExternalSegment` は、Platform で生成されなかったオーディエンスを指します。 |
| `expression` | オーディエンスのプロファイルクエリ言語 (PQL) 式。 PQL 式の詳細については、 [PQL 式ガイド](../pql/overview.md). |
| `schema` | オーディエンスのエクスペリエンスデータモデル (XDM) スキーマ。 |
| `labels` | オーディエンスに関連するオブジェクトレベルのデータ使用および属性ベースのアクセス制御ラベル。 |
| `ttlInDays` | オーディエンスのデータ有効期限の値を日数で表します。 |

+++

>[!TAB 外部で生成されたオーディエンス]

+++ 外部で生成されたオーディエンスを作成するためのサンプルリクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/ups/audiences
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
        "audienceId":"test-external-audience-id",
        "name":"externalAudience",
        "namespace":"aam",
        "description":"Last 30 days",
        "type":"ExternalSegment",
        "originName":"CUSTOM_UPLOAD",
        "lifecycleState":"published",
        "datasetId":"6254cf3c97f8e31b639fb14d",
        "labels":[
            "core/C1"
        ],
        "linkedAudienceRef":{
            "flowId": "4685ea90-d2b6-11ec-9d64-0242ac120002"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- | 
| `audienceId` | オーディエンスのユーザー指定 ID。 |
| `name` | オーディエンスの名前。 |
| `namespace` | オーディエンスの名前空間。 |
| `description` | オーディエンスの説明。 |
| `type` | オーディエンスが Platform で生成されたものか、外部で生成されたオーディエンスかを表示するフィールドです。 以下の値を指定できます。 `SegmentDefinition` および `ExternalSegment`. A `SegmentDefinition` は、Platform で生成されたオーディエンスを指し、 `ExternalSegment` は、Platform で生成されなかったオーディエンスを指します。 |
| `originName` | オーディエンスの起源の名前。 外部で生成されたオーディエンスの場合、こののデフォルト値は `CUSTOM_UPLOAD`. その他のサポートされている値は次のとおりです。 `REAL_TIME_CUSTOMER_PROFILE`, `CUSTOM_UPLOAD`, `AUDIENCE_ORCHESTRATION`、および `AUDIENCE_MATCH`. |
| `lifecycleState` | 作成しようとしているオーディエンスの初期状態を決定するオプションのフィールドです。 次の値がサポートされています。 `draft`, `published`、および `inactive`. |
| `datasetId` | オーディエンスを構成するデータが見つかるデータセットの ID。 |
| `labels` | オーディエンスに関連するオブジェクトレベルのデータ使用および属性ベースのアクセス制御ラベル。 |
| `audienceMeta` | 外部で生成されたオーディエンスに属するメタデータ。 |
| `linkedAudienceRef` | 他のオーディエンス関連システムの ID を含むオブジェクト。 これには、次のものが含まれます。 <ul><li>`flowId`:この ID は、オーディエンスデータを取り込むために使用されたデータフローにオーディエンスを接続するために使用されます。 必要な ID について詳しくは、 [データフローガイドの作成](../../sources/tutorials/api/collect/cloud-storage.md).</li><li>`aoWorkflowId`:この ID は、オーディエンスを関連する Audience Orchestration 構成に接続するために使用されます。&lt;/li/> <li>`payloadFieldGroupRef`:この ID は、オーディエンスの構造を記述する XDM フィールドグループスキーマを参照するために使用されます。 このフィールドの値の詳細については、 [XDM フィールドグループエンドポイントガイド](../../xdm/api/field-groups.md).</li><li>`audienceFolderId`:この ID は、オーディエンスのAdobe Audience Managerでフォルダー ID を参照するために使用されます。 この API について詳しくは、 [Adobe Audience Manager API ガイド](https://bank.demdex.com/portal/swagger/index.html#/Segment%20Folder%20API).</ul> |

+++

>[!ENDTABS]

**応答**

正常な応答は、HTTP ステータス 200 と、新しく作成されたオーディエンスに関する情報を返します。

>[!BEGINTABS]

>[!TAB Platform で生成されたオーディエンス]

+++Platform で生成されたオーディエンスを作成する際のレスポンスの例。

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
    "originName": "REAL_TIME_CUSTOMER_PROFILE",
    "overridePerformanceWarnings": false,
    "createdBy": "{CREATED_BY_ID}",
    "lifecycleState": "active",
    "labels": [
      "core/C1"
    ],
    "namespace": "AEPSegments"
}
```

+++

>[!TAB 外部で生成されたオーディエンス]

+++外部で生成されたオーディエンスを作成する場合のレスポンスのサンプルです。

```json
{
   "id": "322f9f62-cd27-11ec-9d64-0242ac120002",
   "audienceId": "test-external-audience-id",
   "name": "externalAudience",
   "namespace": "aam",
   "imsOrgId": "{ORG_ID}",
   "sandbox":{
      "sandboxId": "6ed34f6f-fe21-4a30-934f-6ffe21fa3075",
      "sandboxName": "prod",
      "type": "production",
      "default": true
   },
   "isSystem": false,
   "description": "Last 30 days",
   "type": "ExternalSegment",
   "originName": "CUSTOM_UPLOAD",
   "lifecycleState": "published",
   "createdBy": "{CREATED_BY_ID}",
   "datasetId": "6254cf3c97f8e31b639fb14d",
   "labels": [
      "core/C1"
   ],
   "linkedAudienceRef": {
      "flowId": "4685ea90-d2b6-11ec-9d64-0242ac120002"
   },
   "_etag": "\"f4102699-0000-0200-0000-625cd61a0000\"",
   "creationTime": 1650251290000,
   "updateEpoch": 1650251290,
   "updateTime": 1650251290000,
   "createEpoch": 1650251290
}
```

+++

## 指定したオーディエンスの検索 {#get}

特定のオーディエンスに関する詳細な情報を検索するには、 `/audiences` エンドポイントを検索し、リクエストパスで取得するオーディエンスの ID を指定します。

**API 形式**

```http
GET /audiences/{AUDIENCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- | 
| `{AUDIENCE_ID}` | 取得しようとしているオーディエンスの ID。 これは `id` フィールドとは **not** の `audienceId` フィールドに入力します。 |

**リクエスト**

+++オーディエンス取得のサンプルリクエスト

```shell
curl -X GET https://platform.adobe.io/data/core/ups/audiences/60ccea95-1435-4180-97a5-58af4aa285ab \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

正常な応答は、HTTP ステータス 200 と、指定されたオーディエンスに関する情報を返します。 応答は、オーディエンスがAdobe Experience Platformで生成されたか外部ソースで生成されたかに応じて異なります。

>[!BEGINTABS]

>[!TAB Platform で生成されたオーディエンス]

+++Platform で生成されたオーディエンスを取得する際のレスポンスのサンプルです。

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
    "lifecycleState": "active",
    "labels": [
        "core/C1"
    ],
    "namespace": "AEPSegments"
}
```

+++

>[!TAB 外部で生成されたオーディエンス]

+++外部で生成されたオーディエンスを取得する際のレスポンスのサンプルです。

```json
{
    "id": "60ccea95-1435-4180-97a5-58af4aa285ab",
    "audienceId": "test-external-audience-id",
    "name": "externalAudience",
    "namespace": "aam",
    "imsOrgId": "{ORG_ID}",
    "sandbox": {
        "sandboxId": "6ed34f6f-fe21-4a30-934f-6ffe21fa3075",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "isSystem": false,
    "description": "Last 30 days",
    "type": "ExternalSegment",
    "lifecycleState": "active",
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

+++

>[!ENDTABS]

## オーディエンスのフィールドの更新 {#update-field}

特定のオーディエンスのフィールドを更新するには、 `/audiences` エンドポイントを作成し、更新するオーディエンスの ID をリクエストパスで指定します。

**API 形式**

```http
PATCH /audiences/{AUDIENCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{AUDIENCE_ID}` | 更新するオーディエンスの ID。 これは `id` フィールドとは **not** の `audienceId` フィールドに入力します。 |

**リクエスト**

+++オーディエンスのフィールドを更新するためのサンプルリクエスト。

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

+++

**応答**

正常な応答は、HTTP ステータス 200 と、新しく更新されたオーディエンスに関する情報を返します。

+++オーディエンスのフィールドを更新する際のレスポンスの例。

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
    "lifecycleState": "active",
    "labels": [
      "core/C1"
    ],
    "namespace": "AEPSegments"
}
```

+++

## オーディエンスの更新 {#put}

特定のオーディエンスを更新（上書き）するには、 `/audiences` エンドポイントを作成し、更新するオーディエンスの ID をリクエストパスで指定します。

**API 形式**

```http
PUT /audiences/{AUDIENCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{AUDIENCE_ID}` | 更新するオーディエンスの ID。 これは `id` フィールドとは **not** の `audienceId` フィールドに入力します。 |

**リクエスト**

+++オーディエンス全体を更新するためのサンプルリクエスト。

```shell
curl -X PUT https://platform.adobe.io/data/core/ups/audiences/4afe34ae-8c98-4513-8a1d-67ccaa54bc05 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
    "audienceId": "test-external-audience-id",
    "name": "New external audience",
    "namespace": "aam",
    "description": "Last 30 days",
    "type": "ExternalSegment",
    "lifecycleState": "published",
    "datasetId": "6254cf3c97f8e31b639fb14d",
    "labels": [
        "core/C1"
    ]
}' 
```

| プロパティ | 説明 |
| -------- | ----------- | 
| `audienceId` | オーディエンスの ID。 外部で生成されたオーディエンスの場合、この値はユーザーによって提供される可能性があります。 |
| `name` | オーディエンスの名前。 |
| `namespace` | オーディエンスの名前空間。 |
| `description` | オーディエンスの説明。 |
| `type` | オーディエンスが Platform で生成されたものか、外部で生成されたオーディエンスかを表示する、システムで生成されたフィールド。 以下の値を指定できます。 `SegmentDefinition` および `ExternalSegment`. A `SegmentDefinition` は、Platform で生成されたオーディエンスを指し、 `ExternalSegment` は、Platform で生成されなかったオーディエンスを指します。 |
| `lifecycleState` | オーディエンスのステータス。以下の値を指定できます。 `draft`, `published`、および `inactive`. `draft` は、オーディエンスが作成される日時を表します。 `published` オーディエンスが公開された時点 `inactive` オーディエンスがアクティブでなくなったとき。 |
| `datasetId` | オーディエンスデータが見つかるデータセットの ID。 |
| `labels` | オーディエンスに関連するオブジェクトレベルのデータ使用および属性ベースのアクセス制御ラベル。 |

+++

**応答**

正常な応答は、HTTP ステータス 200 と、新しく更新されたオーディエンスの詳細を返します。 オーディエンスの詳細は、Platform で生成されたオーディエンスか、外部で生成されたオーディエンスかによって異なることに注意してください。

+++オーディエンス全体を更新する際のレスポンスの例。

```json
{
    "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
    "audienceId": "test-external-audience-id",
    "name": "New external audience",
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
    "lifecycleState": "published",
    "createdBy": "{CREATED_BY_ID}",
    "datasetId": "6254cf3c97f8e31b639fb14d",
    "_etag": "\"f4102699-0000-0200-0000-625cd61a0000\"",
    "creationTime": 1650251290000,
    "updateEpoch": 1650251290,
    "updateTime": 1650251290000,
    "createEpoch": 1650251290
}
```

+++

## オーディエンスの削除 {#delete}

特定のオーディエンスを削除するには、 `/audiences` エンドポイントを探し、リクエストパスで削除するオーディエンスの ID を指定します。

**API 形式**

```http
DELETE /audiences/{AUDIENCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{AUDIENCE_ID}` | 削除するオーディエンスの ID。 これは `id` フィールドとは **not** の `audienceId` フィールドに入力します。 |

**リクエスト**

+++ オーディエンスを削除するためのサンプルリクエスト。

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/audiences/60ccea95-1435-4180-97a5-58af4aa285ab5 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

リクエストが成功した場合、HTTP ステータス 204 が返され、メッセージは返されません。

## 複数のオーディエンスの取得 {#bulk-get}

複数のオーディエンスを取得するには、 `/audiences/bulk-get` エンドポイントを作成し、取得するオーディエンスの ID を指定します。

**API 形式**

```http
POST /audiences/bulk-get
```

**リクエスト**

+++ 複数のオーディエンスを取得するためのサンプルリクエスト。

```shell
curl -X POST https://platform.adobe.io/data/core/ups/audiences/bulk-get
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d ' {
    "ids": [
        {
            "id": "72c393ea-caed-441a-9eb6-5f66bb1bd6cd"
        },
        {
            "id": "QU9fLTEzOTgzNTE0MzY0NzY0NDg5NzkyOTkx_6ed34f6f-fe21-4a30-934f-6ffe21fa3075"
        }
    ]
 }
```

+++

**応答**

正常な応答は、HTTP ステータス 207 と、リクエストされたオーディエンスの情報を返します。

+++ 複数のオーディエンスを取得する際のレスポンスのサンプルです。

```json
{
   "results":{
      "72c393ea-caed-441a-9eb6-5f66bb1bd6cd":{
         "id": "72c393ea-caed-441a-9eb6-5f66bb1bd6cd",
         "audienceId": "72c393ea-caed-441a-9eb6-5f66bb1bd6cd",
         "schema": {
            "name": "_xdm.context.profile"
         },
         "ttlInDays": 30,
         "imsOrgId": "{ORG_ID}",
         "sandbox": {
            "sandboxId": "6ed34f6f-fe21-4a30-934f-6ffe21fa3075",
            "sandboxName": "prod",
            "type": "production",
            "default": true
         },
         "name": "Sample audience",
         "expression": {
            "type": "pql",
            "format": "pql/text",
            "value": "_id = \"abc\""         
        },
         "mergePolicyId": "87c94d51-239c-4391-932c-29c2412100e5",
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
         "ansibleUiEnabled": false,
         "dataGovernancePolicy": {
            "excludeOptOut": true
         },
         "creationTime": 1623889553000000,
         "updateEpoch": 1674646369,
         "updateTime": 1674646369000,
         "createEpoch": 1623889552,
         "_etag": "\"61030ec7-0000-0200-0000-63d113610000\"",
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
         "state": "enabled",
         "overridePerformanceWarnings": false,
         "lastModifiedBy": "{CREATED_ID}",
         "lifecycleState": "published",
         "namespace": "AEPSegments",
         "isSystem": false,
         "saveSegmentMembership": true,
         "originName": "REAL_TIME_CUSTOMER_PROFILE"
      },
      "QU9fLTEzOTgzNTE0MzY0NzY0NDg5NzkyOTkx_6ed34f6f-fe21-4a30-934f-6ffe21fa3075":{
         "id": "QU9fLTEzOTgzNTE0MzY0NzY0NDg5NzkyOTkx_6ed34f6f-fe21-4a30-934f-6ffe21fa3075",
         "name": "label test24764489707692",
         "namespace": "AO",
         "imsOrgId": "{ORG_ID}",
         "sandbox":{
            "sandboxId": "6ed34f6f-fe21-4a30-934f-6ffe21fa3075",
            "sandboxName": "prod",
            "type": "production",
            "default": true
         },
         "type": "ExternalSegment",
         "lifecycleState": "published",
         "sourceId": "source-id",
         "createdBy": "{USER_ID}",
         "datasetId": "62bf31a105e9891b63525c92",
         "_etag": "\"3100da6d-0000-0200-0000-62bf31a10000\"",
         "creationTime": 1656697249000,
         "updateEpoch": 1656697249,
         "updateTime": 1656697249000,
         "createEpoch": 1656697249,
         "audienceId": "test-audience-id",
         "isSystem": false,
         "saveSegmentMembership": true,
         "linkedAudienceRef": {
            "aoWorkflowId": "62bf31858e87e34c8364befa"
         },
         "originName": "AUDIENCE_ORCHESTRATION"
      }
   }
}
```

+++

## 複数のオーディエンスを更新 {#bulk-patch}

POSTリクエストを `/audiences/bulk-patch-metric` エンドポイントを作成し、更新するオーディエンスの ID を指定します。

**API 形式**

```http
POST /audiences/bulk-patch-metric
```

**リクエスト**

+++ 複数のオーディエンスを更新するためのサンプルリクエスト。

```shell
curl -X POST https://platform.adobe.io/data/core/ups/audiences/bulk-patch-metric
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d ' {
    "jobId": "12345",
    "jobType": "AO",
    "resources": [
        {
            "audienceId": "QUFNVHJhaXRzX2V4dGVybmFsU2VnbWVudC1hdWRpZW5jZS1pZA_6ed34f6f-fe21-4a30-934f-6ffe21fa3075",
            "namespace": "AAMTraits",
            "operations": [
                {
                    "op": "add",
                    "path": "/metrics/data",
                    "value": {
                        "totalProfiles": 11037
                    }
                },
            ]
        },
        {
            "audienceId": "QUFNVHJhaXRzX2V4dGVybmFsU2VnbWVudC1hdWRpZW5jZS1pZA_6ed34f6f-fe21-4a30-934f-6ffe21fa3075",
            "namespace": "AAMTraits",
            "operations": [
                {
                    "op": "add",
                    "path": "/metrics/data",
                    "value": {
                        "totalProfiles": 523
                    }
                }
            ]
        }
    ]
    }
```

<table>
<thead>
<tr>
<th>パラメーター</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>jobId</code></td>
<td>更新を実行するジョブの ID。</td>
</tr>
<tr>
<td><code>jobType</code></td>
<td>更新を実行するジョブのタイプ。 この値は、 <code>export</code> または <code>AO</code>.</td>
</tr>
<tr>
<td><code>audienceId</code></td>
<td>更新するオーディエンスの ID。 これは <code>audienceId</code> 値と <strong>not</strong> の <code>id</code> オーディエンスの値。</td>
</tr>
<tr>
<td><code>namespace</code></td>
<td>更新するオーディエンスの名前空間。</td>
</tr>
<tr>
<td><code>operations</code></td>
<td>オーディエンスの更新に使用される情報を含むオブジェクト。</td>
</tr>
<tr>
<td><code>operations.op</code></td>
<td>パッチに使用する操作。 複数のオーディエンスを更新する場合、この値は <strong>常に</strong> <code>add</code>.</td>
</tr>
<tr>
<td><code>operations.path</code></td>
<td>更新するフィールドのパス。 現在、サポートされているパスは 2 つだけです。 <code>/metrics/data</code> 更新時 <strong>profile</strong> カウントと <code>/recordMetrics/data</code> 更新時 <strong>レコード</strong> 数</td>
</tr>
<tr>
<td><code>operations.value</code></td>
<td>
更新するフィールドの値。 プロファイル数を更新する場合、この値は次のようになります。 
<pre>
{ "totalProfiles":123456 }
</pre>
レコード数を更新する場合、この値は次のようになります。 
<pre>
{ "recordCount":123456 }
</pre>
</td>
</tr>
</tbody>
</table>

+++

**応答**

正常な応答は、HTTP ステータス 207 と、更新されたオーディエンスに関する詳細を返します。

+++ 複数のオーディエンスを更新するためのレスポンスの例です。

```json
{
   "resources":[
      {
         "audienceId":"QUFNVHJhaXRzX2V4dGVybmFsU2VnbWVudC1hdWRpZW5jZS1pZA_6ed34f6f-fe21-4a30-934f-6ffe21fa3075",

         "namespace": "AAMTraits",
         "status":200
      },
      {
         "audienceId":"QUFNVHJhaXRzX2V4dGVybmFsU2VnbWVudC1vcmlnaW4tdGVzdDE_6ed34f6f-fe21-4a30-934f-6ffe21fa3075",

         "namespace": "AAMTraits",
         "status":200
      }
   ]
}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `status` | 更新されたオーディエンスのステータス。 返されたステータスが 200 の場合、オーディエンスは正常に更新されました。 オーディエンスを更新できなかった場合は、オーディエンスが更新されなかった理由を示すエラーが返されます。 |

+++

## 次の手順

このガイドを読むと、Adobe Experience Platform API を使用したオーディエンスの作成、管理、削除の方法についての理解が深まりました。 UI を使用した Audience Management について詳しくは、 [セグメント化 UI ガイド](../ui/overview.md).
