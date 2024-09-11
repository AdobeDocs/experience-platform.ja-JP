---
title: Audiences API エンドポイント
description: Adobe Experience Platform Segmentation Service API のオーディエンスエンドポイントを使用して、組織のオーディエンスをプログラムで作成、管理および更新します。
role: Developer
exl-id: cb1a46e5-3294-4db2-ad46-c5e45f48df15
source-git-commit: 9c50ca0db55ce4b21978273d7b4d1de9b5f9338d
workflow-type: tm+mt
source-wordcount: '1438'
ht-degree: 6%

---

# オーディエンスエンドポイント

オーディエンスは、類似の行動や特徴を共有する人々の集まりです。 これらの人物のコレクションは、Adobe Experience Platformを使用するか、外部ソースから生成できます。 Segmentation API の `/audiences` エンドポイントを使用すると、オーディエンスをプログラムにより取得、作成、更新、削除できます。

## はじめに

このガイドで使用するエンドポイントは、[!DNL Adobe Experience Platform Segmentation Service] API の一部です。 続行する前に、[ はじめる前に ](./getting-started.md) を参照して、必要なヘッダーやサンプル API 呼び出しの読み取り方法など、API の呼び出しを正常に実行するために必要な重要な情報を確認してください。

## オーディエンスのリストの取得 {#list}

`/audiences` エンドポイントに対してGETリクエストを行うことで、組織のすべてのオーディエンスのリストを取得できます。

**API 形式**

`/audiences` エンドポイントは、結果を絞り込むのに役立つ、複数のクエリパラメーターをサポートしています。これらのパラメーターはオプションですが、リソースをリストする際の高価なオーバーヘッドを削減するために、使用することを強くお勧めします。 パラメーターを指定せずにこのエンドポイントを呼び出すと、組織で使用可能なすべてのオーディエンスが取得されます。 複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。

```http
GET /audiences
GET /audiences?{QUERY_PARAMETERS}
```

>[!NOTE]
>
>クエリパラメーターなしでこのエンドポイントを使用すると、非アクティブなオーディエンスは **返されません**。 ただし、このエンドポイントを `property=audienceId` クエリパラメーターと組み合わせて使用すると、非アクティブなオーディエンス **が返され** す。

オーディエンスのリストを取得する際には、次のクエリパラメーターを使用できます。

| クエリーパラメーター | 説明 | 例 |
| --------------- | ----------- | ------- |
| `start` | 返されるオーディエンスの開始オフセットを指定します。 | `start=5` |
| `limit` | ページごとに返されるオーディエンスの最大数を指定します。 | `limit=10` |
| `sort` | 結果の並べ替え順序を指定します。 これは、`attributeName:[desc/asc]` の形式で記述されます。 | `sort=updateTime:desc` |
| `property` | 属性の値に **正確に** 一致するオーディエンスを指定できるフィルター。 これは、`property=` の形式で記述されます | `property=audienceId==test-audience-id` |
| `name` | 指定した値を名前に含む **オーディエンスを指定でき** フィルター。 この値では、大文字と小文字が区別されません。 | `name=Sample` |
| `description` | 指定された値を説明 **含む** オーディエンスを指定できるフィルター。 この値では、大文字と小文字が区別されません。 | `description=Test Description` |

**リクエスト**

次のリクエストでは、組織で作成された最後の 2 つのオーディエンスを取得します。

+++オーディエンスのリストを取得するリクエストのサンプル。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/audiences?limit=2 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答に成功すると、HTTP ステータス 200 が、組織内で JSON として作成されたオーディエンスのリストと共に返されます。

+++組織に属する最後に作成された 2 つのオーディエンスを含む応答のサンプル

```json
{
    "children": [
        {
            "id": "60ccea95-1435-4180-97a5-58af4aa285ab",
            "audienceId": "60ccea95-1435-4180-97a5-58af4aa285ab",
            "schema": {
                "name": "_xdm.context.profile"
            },
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
| `audienceId` | 両方 | オーディエンスが Platform で生成されたオーディエンスの場合、これは `id` と同じ値です。 オーディエンスが外部で生成される場合、この値はクライアントから提供されます。 |
| `schema` | 両方 | オーディエンスのエクスペリエンスデータモデル（XDM）スキーマ。 |
| `imsOrgId` | 両方 | オーディエンスが属する組織の ID。 |
| `sandbox` | 両方 | オーディエンスが属するサンドボックスに関する情報。 サンドボックスについて詳しくは、[ サンドボックスの概要 ](../../sandboxes/home.md) を参照してください。 |
| `name` | 両方 | オーディエンスの名前。 |
| `description` | 両方 | オーディエンスの説明。 |
| `expression` | Platform で生成 | オーディエンスのProfile Query Language（PQL）式。 PQL式について詳しくは、[PQL式ガイド ](../pql/overview.md) を参照してください。 |
| `mergePolicyId` | Platform で生成 | オーディエンスが関連付けられている結合ポリシーの ID。 結合ポリシーについて詳しくは、[結合ポリシーガイド](../../profile/api/merge-policies.md)を参照してください。 |
| `evaluationInfo` | Platform で生成 | オーディエンスの評価方法を表示します。 考えられる評価方法には、バッチ、同期（ストリーミング）または連続（エッジ）があります。 評価方法について詳しくは、[ セグメント化の概要 ](../home.md) を参照してください。 |
| `dependents` | 両方 | 現在のオーディエンスに依存するオーディエンス ID の配列。 これは、セグメントのセグメントであるオーディエンスを作成する場合に使用されます。 |
| `dependencies` | 両方 | オーディエンスが依存するオーディエンス ID の配列。 これは、セグメントのセグメントであるオーディエンスを作成する場合に使用されます。 |
| `type` | 両方 | オーディエンスが Platform で生成されたものか、外部で生成されたオーディエンスかを表示する、システムで生成されたフィールド。 使用可能な値は `SegmentDefinition` および `ExternalSegment` です。 `SegmentDefinition` は、Platform で生成されたオーディエンスを指し、`ExternalSegment` は、Platform で生成されなかったオーディエンスを指します。 |
| `originName` | 両方 | オーディエンスの接触チャネルの名前を参照するフィールド。 Platform で生成されたオーディエンスの場合、この値は `REAL_TIME_CUSTOMER_PROFILE` になります。 Audience Orchestration で生成されたオーディエンスの場合、この値は `AUDIENCE_ORCHESTRATION` になります。 Adobe Audience Managerで生成されたオーディエンスの場合、この値は `AUDIENCE_MANAGER` になります。 外部で生成されたその他のオーディエンスの場合、この値は `CUSTOM_UPLOAD` になります。 |
| `createdBy` | 両方 | オーディエンスを作成したユーザーの ID。 |
| `labels` | 両方 | オーディエンスに関連するオブジェクトレベルのデータ使用と属性ベースのアクセス制御ラベル。 |
| `namespace` | 両方 | オーディエンスが属する名前空間。 使用可能な値は、`AAM`、`AAMSegments`、`AAMTraits`、`AEPSegments` などです。 |
| `linkedAudienceRef` | 両方 | 他のオーディエンス関連システムへの識別子を含むオブジェクト。 |

+++

## 新しいオーディエンスの作成 {#create}

`/audiences` エンドポイントにPOSTリクエストを実行することで、新しいオーディエンスを作成できます。

**API 形式**

```http
POST /audiences
```

**リクエスト**

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
        "profileInstanceId": "AEPSegments",
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
| `type` | オーディエンスが Platform で生成されたものか、外部で生成されたものかを表示するフィールド。 使用可能な値は `SegmentDefinition` および `ExternalSegment` です。 `SegmentDefinition` は、Platform で生成されたオーディエンスを指し、`ExternalSegment` は、Platform で生成されなかったオーディエンスを指します。 |
| `expression` | オーディエンスのProfile Query Language（PQL）式。 PQL式について詳しくは、[PQL式ガイド ](../pql/overview.md) を参照してください。 |
| `schema` | オーディエンスのエクスペリエンスデータモデル（XDM）スキーマ。 |
| `labels` | オーディエンスに関連するオブジェクトレベルのデータ使用と属性ベースのアクセス制御ラベル。 |

+++

**応答**

応答に成功すると、HTTP ステータス 200 と、新しく作成されたオーディエンスに関する情報が返されます。

+++Platform で生成されたオーディエンスを作成する際の応答のサンプル。

```json
{
    "id": "60ccea95-1435-4180-97a5-58af4aa285ab",
    "audienceId": "60ccea95-1435-4180-97a5-58af4aa285ab",
     "schema": {
      "name": "_xdm.context.profile"
    },
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

## 指定したオーディエンスの検索 {#get}

`/audiences` エンドポイントに対してGETリクエストを実行し、取得するオーディエンスの ID をリクエストパスで指定することで、特定のオーディエンスに関する詳細な情報を検索できます。

**API 形式**

```http
GET /audiences/{AUDIENCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- | 
| `{AUDIENCE_ID}` | 取得しようとしているオーディエンスの ID。 これは `id` のフィールドであり、`audienceId` のフィールドでは **ありません** 注意してください。 |

**リクエスト**

+++オーディエンスを取得するためのサンプルリクエスト

```shell
curl -X GET https://platform.adobe.io/data/core/ups/audiences/60ccea95-1435-4180-97a5-58af4aa285ab \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答に成功すると、HTTP ステータス 200 と、指定されたオーディエンスに関する情報が返されます。

+++Platform で生成されたオーディエンスを取得する際の応答のサンプル。

```json
{
    "id": "60ccea95-1435-4180-97a5-58af4aa285ab",
    "audienceId": "60ccea95-1435-4180-97a5-58af4aa285ab",
    "schema": {
        "name": "_xdm.context.profile"
    },
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

## オーディエンスの更新 {#put}

特定のエンドポイントに対してPUTリクエストを実行し、リクエストパスで更新するオーディエンスの ID を指定することで、`/audiences` 定のオーディエンスを更新（上書き）できます。

**API 形式**

```http
PUT /audiences/{AUDIENCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{AUDIENCE_ID}` | 更新するオーディエンスの ID。 これは `id` のフィールドであり、`audienceId` のフィールドでは **ありません** 注意してください。 |

**リクエスト**

+++オーディエンス全体を更新するリクエストのサンプル。

```shell
curl -X PUT https://platform.adobe.io/data/core/ups/audiences/4afe34ae-8c98-4513-8a1d-67ccaa54bc05 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
    "audienceId": "test-platform-audience-id",
    "name": "New Platform audience",
    "namespace": "AEPSegments",
    "description": "Last 30 days",
    "type": "SegmentDefinition",
    "lifecycleState": "published",
    "datasetId": "6254cf3c97f8e31b639fb14d",
    "labels": [
        "core/C1"
    ]
}' 
```

| プロパティ | 説明 |
| -------- | ----------- | 
| `audienceId` | オーディエンスの ID。 外部で生成されたオーディエンスの場合、この値はユーザーによって指定される場合があります。 |
| `name` | オーディエンスの名前。 |
| `namespace` | オーディエンスの名前空間。 |
| `description` | オーディエンスの説明。 |
| `type` | オーディエンスが Platform で生成されたものか、外部で生成されたオーディエンスかを表示する、システムで生成されたフィールド。 使用可能な値は `SegmentDefinition` および `ExternalSegment` です。 `SegmentDefinition` は、Platform で生成されたオーディエンスを指し、`ExternalSegment` は、Platform で生成されなかったオーディエンスを指します。 |
| `lifecycleState` | オーディエンスのステータス。使用可能な値は、`draft`、`published`、`inactive` です。 `draft` は、オーディエンスが作成されたとき、オーディエンスが公開されたと `published`、オーディエンスがアクティブでなくなった `inactive` を表します。 |
| `datasetId` | オーディエンスデータを検索できるデータセットの ID。 |
| `labels` | オーディエンスに関連するオブジェクトレベルのデータ使用と属性ベースのアクセス制御ラベル。 |

+++

**応答**

応答が成功すると、HTTP ステータス 200 が、新しく更新されたオーディエンスの詳細と共に返されます。 オーディエンスの詳細は、Platform で生成されたオーディエンスか、外部で生成されたオーディエンスかによって異なることに注意してください。

+++オーディエンス全体を更新する際の応答のサンプル。

```json
{
    "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
    "audienceId": "test-platform-audience-id",
    "name": "New Platform audience",
    "namespace": "AEPSegments",
    "imsOrgId": "{ORG_ID}",
    "sandbox": {
        "sandboxId": "6ed34f6f-fe21-4a30-934f-6ffe21fa3075",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "description": "Last 30 days",
    "type": "SegmentDefinition",
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

特定のオーディエンスを削除するには、`/audiences` エンドポイントにDELETEリクエストを実行し、リクエストパスで削除するオーディエンスの ID を指定します。

**API 形式**

```http
DELETE /audiences/{AUDIENCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{AUDIENCE_ID}` | 削除するオーディエンスの ID。 これは `id` のフィールドであり、`audienceId` のフィールドでは **ありません** 注意してください。 |

**リクエスト**

+++ オーディエンスの削除リクエストのサンプル。

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

`/audiences/bulk-get` エンドポイントに対してPOSTリクエストを行い、取得するオーディエンスの ID を指定することで、複数のオーディエンスを取得できます。

**API 形式**

```http
POST /audiences/bulk-get
```

**リクエスト**

+++ 複数のオーディエンスを取得するリクエストのサンプル。

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

応答に成功すると、HTTP ステータス 207 と、リクエストされたオーディエンスの情報が返されます。

+++ 複数のオーディエンスを取得する場合の応答例。

```json
{
   "results":{
      "72c393ea-caed-441a-9eb6-5f66bb1bd6cd":{
         "id": "72c393ea-caed-441a-9eb6-5f66bb1bd6cd",
         "audienceId": "72c393ea-caed-441a-9eb6-5f66bb1bd6cd",
         "schema": {
            "name": "_xdm.context.profile"
         },
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


## 次の手順

このガイドを読むことで、Adobe Experience Platform API を使用してオーディエンスを作成、管理および削除する方法について、理解が深まりました。 UI を使用した Audience Management について詳しくは、[ セグメント化 UI ガイド ](../ui/overview.md) を参照してください。
