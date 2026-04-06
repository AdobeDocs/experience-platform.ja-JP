---
title: Audiences API エンドポイント
description: Adobe Experience Platform Segmentation Service APIのオーディエンスエンドポイントを使用して、自社のオーディエンスをプログラムによって作成、管理、更新します。
role: Developer
exl-id: cb1a46e5-3294-4db2-ad46-c5e45f48df15
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '1592'
ht-degree: 6%

---

# オーディエンスエンドポイント

オーディエンスとは、類似の行動や特徴を共有する人々の集まりのことです。 これらの人物のコレクションは、Adobe Experience Platformを使用するか、外部ソースから生成できます。 セグメント APIで`/audiences` エンドポイントを使用できます。これにより、オーディエンスをプログラムで取得、作成、更新、削除できます。

## はじめに

このガイドで使用されているエンドポイントは、[!DNL Adobe Experience Platform Segmentation Service] APIの一部です。 続行する前に、必須ヘッダーやサンプル API呼び出しの読み取り方法など、APIへの呼び出しを正常に行うために知っておく必要がある重要な情報については、[入門ガイド &#x200B;](./getting-started.md)を確認してください。

## オーディエンスのリストの取得 {#list}

`/audiences` エンドポイントに対してGET リクエストを行うことで、組織のすべてのオーディエンスのリストを取得できます。

**API 形式**

`/audiences` エンドポイントは、結果を絞り込むのに役立つ、複数のクエリパラメーターをサポートしています。これらのパラメーターはオプションですが、リソースのリスト時にコストのかかるオーバーヘッドを減らすために、使用を強くお勧めします。 パラメーターを指定せずにこのエンドポイントを呼び出すと、組織で使用可能なすべてのオーディエンスが取得されます。 複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。

```http
GET /audiences
GET /audiences?{QUERY_PARAMETERS}
```

>[!NOTE]
>
>このエンドポイントをクエリパラメーターなしで使用すると、非アクティブなオーディエンスは&#x200B;**not**&#x200B;が返されます。 ただし、このエンドポイントを`property=audienceId` クエリパラメーターと組み合わせて使用すると、非アクティブなオーディエンス **が**&#x200B;返されます。

オーディエンスのリストを取得する際には、次のクエリパラメーターを使用できます。

| クエリパラメーター | 説明 | 例 |
| --------------- | ----------- | ------- |
| `start` | 返されるオーディエンスの開始オフセットを指定します。 | `start=5` |
| `limit` | ページごとに返されるオーディエンスの最大数を指定します。 | `limit=10` |
| `sort` | 結果を並べ替える順序を指定します。 これは`attributeName:[desc/asc]`形式で記述されています。 | `sort=updateTime:desc` |
| `property` | 属性の値に一致する&#x200B;**正確に** オーディエンスを指定できるフィルター。 これは`property=`形式で記述されています | `property=audienceId==test-audience-id` |
| `name` | 指定された値を&#x200B;**含む**&#x200B;という名前のオーディエンスを指定できるフィルター。 この値では大文字と小文字が区別されません。 | `name=Sample` |
| `description` | 説明&#x200B;**に指定された値が**&#x200B;含まれるオーディエンスを指定できるフィルター。 この値では大文字と小文字が区別されません。 | `description=Test Description` |
| `entityType` | 探しているオーディエンスのタイプを指定できるフィルター。 | `entityType=_xdm.context.account` |

**リクエスト**

次のリクエストは、組織で作成された最後の2つのオーディエンスを取得します。

+++オーディエンスのリストを取得するためのサンプルリクエスト。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/audiences?limit=2 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200が返され、組織内でJSONとして作成されたオーディエンスのリストが表示されます。

+++組織に属する最後の2つの作成済みオーディエンスを含むサンプル応答

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
      "totalPages": 21,
      "sortField": "name",
      "sort": "asc", 
      "pageSize": 5,
      "limit": 5,
      "start": "0",
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
| `id` | 両方 | オーディエンスのシステム生成の読み取り専用ID。 |
| `audienceId` | 両方 | オーディエンスがプラットフォームで生成されたオーディエンスの場合、これは`id`と同じ値です。 オーディエンスが外部で生成された場合、この値はクライアントから提供されます。 |
| `schema` | 両方 | オーディエンスのExperience Data Model （XDM）スキーマ。 |
| `imsOrgId` | 両方 | オーディエンスが属する組織のID。 |
| `sandbox` | 両方 | オーディエンスが属するサンドボックスに関する情報。 サンドボックスの詳細については、[&#x200B; サンドボックスの概要](../../sandboxes/home.md)を参照してください。 |
| `name` | 両方 | オーディエンスの名前。 |
| `description` | 両方 | オーディエンスの説明。 |
| `expression` | プラットフォーム生成 | Profile Query Language（PQL）のオーディエンスの表現。 PQL式の詳細については、[PQL式ガイド &#x200B;](../pql/overview.md)を参照してください。 |
| `mergePolicyId` | プラットフォーム生成 | オーディエンスが関連付けられている結合ポリシーのID。 結合ポリシーについて詳しくは、[結合ポリシーガイド](../../profile/api/merge-policies.md)を参照してください。 |
| `evaluationInfo` | プラットフォーム生成 | オーディエンスがどのように評価されるかを示します。 評価方法として、バッチ、同期（ストリーミング）、連続（エッジ）などがあります。 評価方法の詳細については、[&#x200B; セグメント化の概要](../home.md)を参照してください |
| `dependents` | 両方 | 現在のオーディエンスに依存するオーディエンス IDの配列。 これは、セグメントのセグメントであるオーディエンスを作成する場合に使用されます。 |
| `dependencies` | 両方 | オーディエンスが依存するオーディエンス IDの配列。 これは、セグメントのセグメントであるオーディエンスを作成する場合に使用されます。 |
| `type` | 両方 | オーディエンスがプラットフォーム生成のオーディエンスか、外部で生成されたオーディエンスかを表示するシステム生成フィールド。 指定できる値は`SegmentDefinition`と`ExternalSegment`です。 `SegmentDefinition`はPlatformで生成されたオーディエンスを表し、`ExternalSegment`はPlatformで生成されなかったオーディエンスを表します。 |
| `originName` | 両方 | オーディエンスのオリジンの名前を参照するフィールド。 Platformで生成されたオーディエンスの場合、この値は`REAL_TIME_CUSTOMER_PROFILE`になります。 Audience Orchestrationで生成されたオーディエンスの場合、この値は`AUDIENCE_ORCHESTRATION`になります。 Adobe Audience Managerで生成されたオーディエンスの場合、この値は`AUDIENCE_MANAGER`になります。 その他の外部生成オーディエンスの場合、この値は`CUSTOM_UPLOAD`になります。 |
| `createdBy` | 両方 | オーディエンスを作成したユーザーのID |
| `labels` | 両方 | オーディエンスに関連する、オブジェクトレベルのデータ使用ラベルと属性ベースのアクセス制御ラベル。 |
| `namespace` | 両方 | オーディエンスが属する名前空間。 指定できる値は、`AAM`、`AAMSegments`、`AAMTraits`、`AEPSegments`です。 |
| `linkedAudienceRef` | 両方 | 他のオーディエンス関連システムの識別子を含むオブジェクト。 |

+++

## 新しいオーディエンスの作成 {#create}

`/audiences` エンドポイントにPOST リクエストを行うことで、新しいオーディエンスを作成できます。

**API 形式**

```http
POST /audiences
```

**リクエスト**

+++ Platformで生成されたオーディエンスを作成するためのサンプルリクエスト

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
| `type` | オーディエンスがプラットフォーム生成のオーディエンスか、外部で生成されたオーディエンスかを表示するフィールド。 指定できる値は`SegmentDefinition`と`ExternalSegment`です。 `SegmentDefinition`はPlatformで生成されたオーディエンスを表し、`ExternalSegment`はPlatformで生成されなかったオーディエンスを表します。 |
| `expression` | Profile Query Language（PQL）のオーディエンスの表現。 PQL式の詳細については、[PQL式ガイド &#x200B;](../pql/overview.md)を参照してください。 |
| `schema` | オーディエンスのExperience Data Model （XDM）スキーマ。 |
| `labels` | オーディエンスに関連する、オブジェクトレベルのデータ使用ラベルと属性ベースのアクセス制御ラベル。 |

+++

**応答**

応答が成功すると、HTTP ステータス 200が返され、新しく作成したオーディエンスに関する情報が表示されます。

+++Platformで生成されたオーディエンスを作成する際の応答のサンプル。

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

`/audiences` エンドポイントに対してGET リクエストを行い、取得するオーディエンスのIDをリクエストパスで指定することで、特定のオーディエンスに関する詳細な情報を検索できます。

**API 形式**

```http
GET /audiences/{AUDIENCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{AUDIENCE_ID}` | 取得しようとしているオーディエンスのID。 これは`id` フィールドであり、**フィールドの**&#x200B;ではなく`audienceId`であることに注意してください。 |

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

応答が成功すると、HTTP ステータス 200が返され、指定されたオーディエンスに関する情報が返されます。

+++Platformで生成されたオーディエンスを取得する際の応答のサンプル。

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

## オーディエンスの上書き {#put}

特定のオーディエンスを更新（上書き）するには、`/audiences` エンドポイントに対してPUT リクエストを行い、更新するオーディエンスのIDをリクエストパスで指定します。

**API 形式**

```http
PUT /audiences/{AUDIENCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{AUDIENCE_ID}` | 更新するオーディエンスのID。 これは`id` フィールドであり、**フィールドの**&#x200B;ではなく`audienceId`であることに注意してください。 |

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
    "audienceId": "test-platform-audience-id",
    "name": "New Platform audience",
    "namespace": "AEPSegments",
    "description": "Last 30 days",
    "type": "SegmentDefinition",
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "workAddress.country=\"US\""
    }
    "lifecycleState": "published",
    "datasetId": "6254cf3c97f8e31b639fb14d",
    "labels": [
        "core/C1"
    ]
}' 
```

| プロパティ | 説明 |
| -------- | ----------- |
| `audienceId` | オーディエンスのID。 外部で生成されたオーディエンスの場合、この値はユーザーから提供される場合があります。 |
| `name` | オーディエンスの名前。 |
| `namespace` | オーディエンスの名前空間。 |
| `description` | オーディエンスの説明。 |
| `type` | オーディエンスがプラットフォーム生成のオーディエンスか、外部で生成されたオーディエンスかを表示するシステム生成フィールド。 指定できる値は`SegmentDefinition`と`ExternalSegment`です。 `SegmentDefinition`はExperience Platformで生成されたオーディエンスを表し、`ExternalSegment`はExperience Platformで生成されなかったオーディエンスを表します。 |
| `expression` | オーディエンスのPQL式を含むオブジェクト。 |
| `lifecycleState` | オーディエンスのステータス。使用可能な値には、`draft`、`published`および`inactive`が含まれます。 `draft`は、オーディエンスが作成された時点、オーディエンスが公開された時点で`published`、オーディエンスがアクティブでなくなった時点で`inactive`を表します。 |
| `datasetId` | オーディエンスデータが見つかるデータセットのID。 |
| `labels` | オーディエンスに関連する、オブジェクトレベルのデータ使用ラベルと属性ベースのアクセス制御ラベル。 |

+++

**応答**

応答が成功すると、HTTP ステータス 200が返され、新しく更新されたオーディエンスの詳細が表示されます。 オーディエンスの詳細は、Experience Platformで生成されたオーディエンスであるか、外部で生成されたオーディエンスであるかによって異なります。

+++オーディエンス全体を更新する際のサンプル応答。

```json
{
    "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
    "audienceId": "test-platform-audience-id",
    "name": "New Experience Platform audience",
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

## オーディエンスの更新 {#patch}

特定のオーディエンスを更新するには、`/audiences` エンドポイントに対してPATCH リクエストを行い、更新するオーディエンスのIDをリクエストパスで指定します。

**API 形式**

```http
PATCH /audiences/{AUDIENCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{AUDIENCE_ID}` | 更新するオーディエンスのID。 これは`id` フィールドであり、**フィールドの**&#x200B;ではなく`audienceId`であることに注意してください。 |

**リクエスト**

+++ オーディエンスを更新するためのサンプルリクエスト。

```shell
curl -X PATCH https://platform.adobe.io/data/core/ups/audiences/60ccea95-1435-4180-97a5-58af4aa285ab5
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '[
    {
        "op": "add",
        "path": "/lifecycleState",
        "value": "inactive"
    }
 ]'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `op` | 実行されたPATCH操作の種類。 このエンドポイントでは、この値は&#x200B;**常に** `/add`です。 |
| `path` | 更新するフィールドのパス。 `id`、`audienceId`、`namespace`、**などのシステム生成フィールドは編集できません**。 |
| `value` | `path`で指定されたプロパティに割り当てられた新しい値。 |

+++

**応答**

応答が成功すると、更新されたオーディエンスを含むHTTP ステータス 200が返されます。

+++オーディエンスのフィールドにパッチを適用する際の応答のサンプル。

```json
{
    "id": "60ccea95-1435-4180-97a5-58af4aa285ab5",
    "audienceId": "test-platform-audience-id",
    "name": "New Experience Platform audience",
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
    "lifecycleState": "inactive",
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

特定のオーディエンスを削除するには、`/audiences` エンドポイントに対してDELETE リクエストを行い、削除するオーディエンスのIDをリクエストパスで指定します。

**API 形式**

```http
DELETE /audiences/{AUDIENCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{AUDIENCE_ID}` | 削除するオーディエンスのID。 これは`id` フィールドであり、**フィールドの**&#x200B;ではなく`audienceId`であることに注意してください。 |

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

`/audiences/bulk-get` エンドポイントにPOST リクエストを行い、取得するオーディエンスのIDを指定することで、複数のオーディエンスを取得できます。

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

応答が成功すると、HTTP ステータス 207が、リクエストされたオーディエンスの情報とともに返されます。

+++ 複数のオーディエンスを取得する際のサンプル応答。

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

このガイドでは、Adobe Experience Platform APIを使用してオーディエンスを作成、管理、削除する方法について解説します。 UIを使用したオーディエンス管理について詳しくは、[&#x200B; セグメント化UI ガイド &#x200B;](../ui/overview.md)を参照してください。
