---
solution: Experience Platform
title: セグメント定義 API エンドポイント
description: Adobe Experience Platform Segmentation Service API のセグメント定義エンドポイントを使用すると、組織のセグメント定義をプログラムで管理できます。
role: Developer
exl-id: e7811b96-32bf-4b28-9abb-74c17a71ffab
source-git-commit: 5f19bd0601770115cae859fd6dc85bd9c9f6e92c
workflow-type: tm+mt
source-wordcount: '1571'
ht-degree: 24%

---

# セグメント定義エンドポイント

>[!WARNING]
>
>Segmentation Service API を使用した B2B エンティティを使用したオーディエンスの作成は非推奨（廃止予定）になりました。 アカウント、アカウントと人物の関係、キャンペーン、キャンペーンメンバー、マーケティングリスト、マーケティングリストメンバー、商談、商談と人物の関係などの B2B エンティティを使用してオーディエンスを作成できなくなりました。 詳しくは、[Real-Time CDP B2B edition アーキテクチャのアップグレード ](../../rtcdp/b2b-architecture-upgrade.md) に関するガイドを参照してください。

Adobe Experience Platformでは、プロファイルのグループから特定の属性や動作のグループを定義するセグメント定義を作成できます。 セグメント定義は、[!DNL Profile Query Language] （PQL）で記述されたクエリをカプセル化するオブジェクトです。 セグメント定義は、オーディエンスを作成するためのプロファイルに適用されます。 このオブジェクト（セグメント定義）は、PQL述語とも呼ばれます。 PQLの述語は、[!DNL Real-Time Customer Profile] に提供するレコードまたは時系列データに関連する条件に基づいて、セグメント定義のルールを定義します。 PQL クエリの記述について詳しくは、[PQL ガイド](../pql/overview.md)を参照してください。

このガイドでは、セグメント定義をより深く理解するための情報を提供し、API を使用して基本的なアクションを実行するためのサンプル API 呼び出しが含まれています。

## はじめに

このガイドで使用するエンドポイントは、[!DNL Adobe Experience Platform Segmentation Service] API の一部です。 続行する前に、[ はじめる前に ](./getting-started.md) を参照して、必要なヘッダーやサンプル API 呼び出しの読み取り方法など、API の呼び出しを正常に実行するために必要な重要な情報を確認してください。

## セグメント定義のリストの取得 {#list}

`/segment/definitions` エンドポイントにGET リクエストを実行することで、組織のすべてのセグメント定義のリストを取得できます。

**API 形式**

`/segment/definitions` エンドポイントは、結果を絞り込むのに役立つ、複数のクエリパラメーターをサポートしています。これらのパラメーターはオプションですが、高価なオーバーヘッドの削減に役立てるため、使用することを強くお勧めします。 パラメーターを指定せずにこのエンドポイントを呼び出すと、組織で使用可能なすべてのセグメント定義が取得されます。 複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。

```http
GET /segment/definitions
GET /segment/definitions?{QUERY_PARAMETERS}
```

**クエリパラメータ**

+++ 使用可能なクエリパラメーターのリスト。

| パラメーター | 説明 | 例 |
| --------- | ----------- | ------- |
| `start` | 返されるセグメント定義の開始オフセットを指定します。 | `start=4` |
| `limit` | 返される、1 ページあたりのセグメント定義の数を指定します。 | `limit=20` |
| `page` | どのページからセグメント定義の結果を表示するかを指定します。 | `page=5` |
| `sort` | 結果を並べ替えるフィールドを指定します。 `[attributeName]:[desc/asc]` の形式で記述されます。 | `sort=updateTime:desc` |
| `evaluationInfo.continuous.enabled` | セグメント定義でストリーミングを有効にするかどうかを指定します。 | `evaluationInfo.continuous.enabled=true` |

+++

**リクエスト**

以下のリクエストは、組織内に投稿された最後の 2 つのセグメント定義を取得します。

+++ セグメント定義のリストを取得するリクエストの例です。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/segment/definitions?limit=2 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答に成功すると、HTTP ステータス 200 が、指定した組織のセグメント定義のリストと共に JSON として返されます。

+++ セグメント定義のリストを取得する際の応答例。

```json
{
    "segments": [
        {
            "id": "3da8bad9-29fb-40e0-b39e-f80322e964de",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "imsOrgId": "{ORG_ID}",
            "sandbox": {
                "sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
                "sandboxName": "prod",
                "type": "production",
                "default": true
            },
            "name": "segment",
            "description": "",
            "expression": {
                "type": "PQL",
                "format": "pql/json",
                "value": "{PQL_EXPRESSION}"
            },
            "mergePolicyId": "b83185bb-0bc6-489c-9363-0075eb30b4c8",
            "evaluationInfo": {
                "batch": {
                    "enabled": true
                },
                "continuous": {
                    "enabled": false
                },
                "synchronous": {
                    "enabled": false
                }
            },
            "dataGovernancePolicy": {
                "excludeOptOut": true
            },
            "creationTime": 1573253640000,
            "baselineTime": 1574327114,
            "updateEpoch": 1575588309,
            "updateTime": 1575588309000
        },
        {
            "id": "ca763983-5572-4ea4-809c-b7dff7e0d79b",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "imsOrgId": "{ORG_ID}",
            "name": "test segment",
            "description": "",
            "expression": {
                "type": "PQL",
                "format": "pql/json",
                "value": "{PQL_EXPRESSION}"
            },
            "mergePolicyId": "b83185bb-0bc6-489c-9363-0075eb30b4c8",
            "evaluationInfo": {
                "batch": {
                    "enabled": true
                },
                "continuous": {
                    "enabled": false
                },
                "synchronous": {
                    "enabled": false
                }
            },
            "dataGovernancePolicy": {
                "excludeOptOut": true
            },
            "creationTime": 1561073779000,
            "baselineTime": 1574327114,
            "updateEpoch": 1574327114,
            "updateTime": 1574327114000
        }
    ],
    "page": {
        "totalCount": 2,
        "totalPages": 1,
        "sortField": "creationTime",
        "sort": "desc",
        "pageSize": 2,
        "limit": 100
    },
    "link": {}
}
```

+++

## 新しいセグメント定義の作成 {#create}

新しいセグメント定義を作成するには、`/segment/definitions` エンドポイントに POST リクエストを実行します。

>[!IMPORTANT]
>
>API を使用して作成されたセグメント定義 **編集できません** は、セグメントビルダーを使用します。

**API 形式**

```http
POST /segment/definitions
```

**リクエスト**

新しいセグメント定義を作成する際は、`pql/text` 形式または `pql/json` 形式で作成できます。

>[!BEGINTABS]

>[!TAB pql/text の使用 ]

+++ セグメント定義を作成するリクエストの例。

```shell
curl -X POST https://platform.adobe.io/data/core/ups/segment/definitions
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
        "name": "People who ordered in the last 30 days",
        "description": "Last 30 days",
        "expression": {
            "type": "PQL",
            "format": "pql/text",
            "value": "workAddress.country = \"US\""
        },
        "evaluationInfo": {
            "batch": {
                "enabled": true
            },
            "continuous": {
                "enabled": false
            },
            "synchronous": {
                "enabled": false
            }
        },
        "schema": {
            "name": "_xdm.context.profile"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | セグメント定義を参照するための一意の名前。 |
| `description` | （任意）作成するセグメント定義の説明。 |
| `expression` | セグメント定義に関するフィールド情報を含んだエンティティ。 |
| `expression.type` | 式タイプを指定します。現時点では、「PQL」のみサポートされています。 |
| `expression.format` | 値内の式の構造を示します。サポートされる値は `pql/text` と `pql/json` です。 |
| `expression.value` | `expression.format` に指定されたタイプに適合する式です。 |
| `evaluationInfo` | （任意）作成しているセグメント定義のタイプ。 バッチセグメントを作成する場合は、`evaluationInfo.batch.enabled` を true に設定します。 ストリーミングセグメントを作成する場合は、`evaluationInfo.continuous.enabled` を true に設定します。 エッジセグメントを作成する場合は、`evaluationInfo.synchronous.enabled` を true に設定します。 空のままにすると、セグメント定義は **バッチ** セグメントとして作成されます。 |
| `schema` | セグメント内のエンティティに関連付けられているスキーマ。 `id` か `name` のどちらかのフィールドで構成されます。 |

+++

>[!TAB pql/json の使用 ]

+++ セグメント定義を作成するリクエストの例。

```shell
curl -X POST https://platform.adobe.io/data/core/ups/segment/definitions
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
        "name": "People who ordered in the last 30 days",
        "profileInstanceId": "ups",
        "description": "Last 30 days",
        "expression": {
            "type": "PQL",
            "format": "pql/json",
            "value": "{\"nodeType\":\"fnApply\",\"fnName\":\"=\",\"params\":[{\"nodeType\":\"fieldLookup\",\"fieldName\":\"a\",\"object\":{\"nodeType\":\"parameterReference\",\"position\":1}},{\"nodeType\":\"fieldLookup\",\"fieldName\":\"b\",\"object\":{\"nodeType\":\"parameterReference\",\"position\":1}}]}"
        },
        "evaluationInfo": {
            "batch": {
                "enabled": true
            },
            "continuous": {
                "enabled": false
            },
            "synchronous": {
                "enabled": false
            }
        },
        "schema": {
            "name": "_xdm.context.profile"
        },
        "payloadSchema": "string"
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | セグメント定義を参照するための一意の名前。 |
| `description` | （任意）作成するセグメント定義の説明。 |
| `evaluationInfo` | （任意）作成しているセグメント定義のタイプ。 バッチセグメントを作成する場合は、`evaluationInfo.batch.enabled` を true に設定します。 ストリーミングセグメントを作成する場合は、`evaluationInfo.continuous.enabled` を true に設定します。 エッジセグメントを作成する場合は、`evaluationInfo.synchronous.enabled` を true に設定します。 空のままにすると、セグメント定義は **バッチ** セグメントとして作成されます。 |
| `schema` | セグメント内のエンティティに関連付けられているスキーマ。 `id` か `name` のどちらかのフィールドで構成されます。 |
| `expression` | セグメント定義に関するフィールド情報を含んだエンティティ。 |
| `expression.type` | 式タイプを指定します。現時点では、「PQL」のみサポートされています。 |
| `expression.format` | 値における式の構造を示します。 |
| `expression.value` | `expression.format` に指定されたタイプに適合する式です。 |

+++

>[!ENDTABS]

**応答**

リクエストが成功した場合は、新しく作成したセグメント定義の詳細と HTTP ステータス 200 が返されます。

+++ セグメント定義を作成する際のサンプルレスポンス。

```json
{
    "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "profileInstanceId": "ups",
    "imsOrgId": "{ORG_ID}",
    "sandbox": {
        "sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
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
    "evaluationInfo": {
        "batch": {
            "enabled": true
        },
        "continuous": {
            "enabled": false
        },
        "synchronous": {
            "enabled": false
        }
    },
    "dataGovernancePolicy": {
        "excludeOptOut": true
    },
    "creationTime": 0,
    "updateEpoch": 1579292094,
    "updateTime": 1579292094000
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | 新しく作成したセグメント定義のシステム生成 ID。 |
| `evaluationInfo` | セグメント定義が受ける評価のタイプを示すオブジェクト。 バッチ、ストリーミング（連続とも呼ばれます）、エッジ（同期とも呼ばれます）のセグメント化があります。 |

+++

## 特定のセグメント定義の取得 {#get}

リクエストエンドポイントにGET リクエストを実行し、取得するセグメント定義の ID をリクエストパスで指定することで、特定のセグメント `/segment/definitions` 義に関する詳細な情報を取得できます。

**API 形式**

```http
GET /segment/definitions/{SEGMENT_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SEGMENT_ID}` | 取得するセグメント定義の `id` 値。 |

**リクエスト**

+++ セグメント定義を取得するリクエストの例です。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/segment/definitions/4afe34ae-8c98-4513-8a1d-67ccaa54bc05 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

リクエストが成功した場合は、特定のセグメント定義についての詳細情報と HTTP ステータス 200 が返されます。

+++ セグメント定義を取得する際の応答例。

```json
{
    "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "profileInstanceId": "ups",
    "imsOrgId": "{ORG_ID}",
    "sandbox": {
        "sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
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
    "evaluationInfo": {
        "batch": {
            "enabled": true
        },
        "continuous": {
            "enabled": false
        },
        "synchronous": {
            "enabled": false
        }
    },
    "dataGovernancePolicy": {
        "excludeOptOut": true
    },
    "creationTime": 0,
    "updateEpoch": 1579292094,
    "updateTime": 1579292094000
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | セグメント定義のシステム生成の読み取り専用 ID。 |
| `name` | セグメント定義を参照するための一意の名前。 |
| `schema` | セグメント内のエンティティに関連付けられているスキーマ。 `id` か `name` のどちらかのフィールドで構成されます。 |
| `expression` | セグメント定義に関するフィールド情報を含んだエンティティ。 |
| `expression.type` | 式タイプを指定します。現時点では、「PQL」のみサポートされています。 |
| `expression.format` | 値内の式の構造を示します。現時点では、次の形式がサポートされています。 <ul><li>`pql/text`：セグメント定義のテキスト表現で、公開された PQL 文法に従っている必要があります。例：`workAddress.stateProvince = homeAddress.stateProvince`</li></ul> |
| `expression.value` | `expression.format` に指定されたタイプに適合する式です。 |
| `description` | 人間にとってわかりやすい、定義の説明。 |
| `evaluationInfo` | セグメント定義が受ける評価のタイプ、バッチ、ストリーミング（連続とも呼ばれます）、またはエッジ（同期とも呼ばれます）を示すオブジェクト。 |

+++

## セグメント定義の一括取得 {#bulk-get}

指定した複数のセグメント定義に関する詳細な情報を取得するには、`/segment/definitions/bulk-get` エンドポイントに POST リクエストを実行し、リクエスト本文でセグメント定義の `id` の値を指定します。

**API 形式**

```http
POST /segment/definitions/bulk-get
```

**リクエスト**

+++ 一括取得エンドポイントを使用する際のサンプルリクエスト。

```shell
curl -X POST https://platform.adobe.io/data/core/ups/segment/definitions/bulk-get \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "ids": [
            {
                "id": "54669488-03ab-4e0d-a694-37fe49e32be8"
            },
            {
                "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05"
            }
        ]
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `ids` | 取得するセグメント定義の ID を格納するオブジェクトを含む配列。 |

+++

**応答**

応答に成功すると、HTTP ステータス 207 と、リクエストされたセグメント定義が返されます。

+++ 一括取得エンドポイントを使用する場合のサンプル応答。

```json
{
    "results": {
        "54669488-03ab-4e0d-a694-37fe49e32be8": {
            "id": "54669488-03ab-4e0d-a694-37fe49e32be8",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "profileInstanceId": "ups",
            "imsOrgId": "{ORG_ID}",
            "sandbox": {
                "sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
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
            "evaluationInfo": {
                "batch": {
                    "enabled": true
                },
                "continuous": {
                    "enabled": false
                },
                "synchronous": {
                    "enabled": false
                }
            },
            "dataGovernancePolicy": {
                "excludeOptOut": true
            },
            "creationTime": 0,
            "updateEpoch": 1579292094,
            "updateTime": 1579292094000
        },
        "4afe34ae-8c98-4513-8a1d-67ccaa54bc05": {
            "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "profileInstanceId": "ups",
            "imsOrgId": "{ORG_ID}",
            "sandbox": {
                "sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
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
            "evaluationInfo": {
                "batch": {
                    "enabled": true
                },
                "continuous": {
                    "enabled": false
                },
                "synchronous": {
                    "enabled": false
                }
            },
            "dataGovernancePolicy": {
                "excludeOptOut": true
            },
            "creationTime": 0,
            "updateEpoch": 1579292094,
            "updateTime": 1579292094000
        }

    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | セグメント定義のシステム生成の読み取り専用 ID。 |
| `name` | セグメント定義を参照するための一意の名前。 |
| `schema` | セグメント内のエンティティに関連付けられているスキーマ。 `id` か `name` のどちらかのフィールドで構成されます。 |
| `expression` | セグメント定義に関するフィールド情報を含んだエンティティ。 |
| `expression.type` | 式タイプを指定します。現時点では、「PQL」のみサポートされています。 |
| `expression.format` | 値内の式の構造を示します。現時点では、次の形式がサポートされています。 <ul><li>`pql/text`：セグメント定義のテキスト表現で、公開された PQL 文法に従っている必要があります。例：`workAddress.stateProvince = homeAddress.stateProvince`</li></ul> |
| `expression.value` | `expression.format` に指定されたタイプに適合する式です。 |
| `description` | 人間にとってわかりやすい、定義の説明。 |
| `evaluationInfo` | セグメント定義が受ける評価のタイプ、バッチ、ストリーミング（連続とも呼ばれます）、またはエッジ（同期とも呼ばれます）を示すオブジェクト。 |

+++

## 特定のセグメント定義の削除 {#delete}

`/segment/definitions` エンドポイントに対してDELETE リクエストを実行し、リクエストパスで削除するセグメント定義の ID を指定することで、特定のセグメント定義を削除するようにリクエストできます。

>[!NOTE]
>
> 宛先のアクティベーションで使用されているセグメント定義 **削除できません**。

**API 形式**

```http
DELETE /segment/definitions/{SEGMENT_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SEGMENT_ID}` | 削除するセグメント定義の `id` の値。 |

**リクエスト**

+++ セグメント定義を削除するリクエストの例。

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/segment/definitions/4afe34ae-8c98-4513-8a1d-67ccaa54bc05 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

リクエストが成功した場合、HTTP ステータス 200 が返され、メッセージは返されません。

## 特定のセグメント定義の更新

特定のセグメント定義を更新するには、`/segment/definitions` エンドポイントにPATCH リクエストを実行し、リクエストパスで更新するセグメント定義の ID を指定します。

**API 形式**

```http
PATCH /segment/definitions/{SEGMENT_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SEGMENT_ID}` | 更新するセグメント定義の `id` の値。 |

**リクエスト**

次のリクエストは、勤務先住所の国を米国からカナダに更新します。

>[!NOTE]
>
>セグメント定義のコンテンツのこの API 呼び出し **置き換え** なので、保持するフィールド **すべて** がリクエスト本文の一部として含まれていることを確認してください。

+++ セグメント定義を更新するリクエストの例。

```shell
curl -X PATCH https://platform.adobe.io/data/core/ups/segment/definitions/4afe34ae-8c98-4513-8a1d-67ccaa54bc05 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
    "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
    "name": "Updated people who ordered in the last 30 days",
    "profileInstanceId": "ups",
    "description": "Last 30 days",
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "workAddress.country = \"CA\""
    },
    "schema": {
        "name": "_xdm.context.profile"
    },
    "payloadSchema": "string",
    "creationTime": 0,
    "updateTime": 0,
    "updateEpoch": 0
}'
```

+++

**応答**

リクエストが成功した場合は、更新したセグメント定義の詳細と HTTP ステータス 200 が返されます。

+++ セグメント定義を更新する際の応答例。

```json
{
    "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "profileInstanceId": "ups",
    "imsOrgId": "{ORG_ID}",
    "sandbox": {
        "sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "name": "Updated people who ordered in the last 30 days",
    "description": "Last 30 days",
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "workAddress.country = \"CA\""
    },
    "evaluationInfo": {
        "batch": {
            "enabled": true
        },
        "continuous": {
            "enabled": false
        },
        "synchronous": {
            "enabled": false
        }
    },
    "dataGovernancePolicy": {
        "excludeOptOut": true
    },
    "creationTime": 0,
    "updateEpoch": 1579295340,
    "updateTime": 1579295340000
}
```

+++

## セグメント定義を変換

`pql/text` エンドポイントに POST リクエストをおこなうと、`pql/json` と `pql/json` または `pql/text` の間でセグメント定義を `/segment/conversion` に変換できます。

**API 形式**

```http
POST /segment/conversion
```

**リクエスト**

次のリクエストは、セグメント定義の形式を `pql/text` から `pql/json` に変更します。

+++ セグメント定義を変換するリクエストの例。

```shell
curl -X POST https://platform.adobe.io/data/core/ups/segment/conversion \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
        "name": "People who ordered in the last 30 days",
        "profileInstanceId": "ups",
        "description": "Last 30 days",
        "expression": {
            "type": "PQL",
            "format": "pql/text",
            "value": "workAddress.country = \"US\""
        },
        "schema": {
            "name": "_xdm.context.profile"
        },
        "payloadSchema": "string"
    }'
```

+++

**応答**

応答に成功すると、HTTP ステータス 200 が、新しく変換されたセグメント定義の詳細と共に返されます。

+++ セグメント定義を変換する際の応答例。

```json
{
    "imsOrgId": "6A29340459CA8D350A49413A@AdobeOrg",
    "sandbox": {
        "sandboxId": "ff0f6870-c46d-11e9-8ca3-036939a64204",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "description": "Last 30 days",
    "expression": {
        "type": "PQL",
        "format": "pql/json",
        "value": "{\"nodeType\":\"fnApply\",\"fnName\":\"=\",\"params\":[{\"nodeType\":\"fieldLookup\",\"fieldName\":\"country\",\"object\":{\"nodeType\":\"fieldLookup\",\"fieldName\":\"workAddress\",\"object\":{\"nodeType\":\"parameterReference\",\"position\":1}}},{\"nodeType\":\"literal\",\"literalType\":\"String\",\"value\":\"US\"}]}"
    }
}
```

+++

## 次の手順

このガイドを読むことで、セグメント定義の仕組みについて理解が深まりました。 セグメントの作成について詳しくは、「セグメントの作成 [ チュートリアルを参照し ](../tutorials/create-a-segment.md) ください。
