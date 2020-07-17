---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: セグメント定義
topic: developer guide
translation-type: tm+mt
source-git-commit: 41a5d816f9dc6e7c26141ff5e9173b1b5631d75e
workflow-type: tm+mt
source-wordcount: '1042'
ht-degree: 4%

---


# セグメント定義エンドポイントガイド

Adobe Experience Platformを使用すると、プロファイルのグループから、特定の属性または行動のグループを定義するセグメントを作成できます。 セグメント定義は、 [!DNL Profile Query Language] (PQL)に書き込まれたクエリをカプセル化するオブジェクトです。 このオブジェクトは、PQL述語とも呼ばれます。 PQL述語は、指定するレコードまたは時系列データに関連する条件に基づいて、セグメントの規則を定義し [!DNL Real-time Customer Profile]ます。 PQLクエリの記述の詳細については、 [PQLガイド](../pql/overview.md) を参照してください。

このガイドは、セグメント定義をより深く理解するのに役立つ情報を提供し、APIを使用して基本的なアクションを実行するためのサンプルAPI呼び出しを含みます。

## はじめに

このガイドで使用されるエンドポイントは、 [!DNL Adobe Experience Platform Segmentation Service] APIの一部です。 先に進む前に、 [入門ガイドを参照して](./getting-started.md) 、必要なヘッダーやAPI呼び出し例を読む方法など、APIを正しく呼び出すために必要な重要な情報を確認してください。

## セグメント定義のリストの取得 {#list}

エンドポイントにGETリクエストを作成して、IMS組織のすべてのセグメント定義のリストを取得でき `/segment/definitions` ます。

**API形式**

エンドポイントでは、結果のフィルタリングに役立ついくつかのクエリパラメーターが `/segment/definitions` サポートされています。 これらのパラメーターはオプションですが、高価なオーバーヘッドを削減するために、このパラメーターの使用を強くお勧めします。 パラメーターを指定しないでこのエンドポイントを呼び出すと、組織で使用可能なすべてのセグメント定義が取得されます。 複数のパラメーターを含める場合は、アンパサンド(`&`)で区切ります。

```http
GET /segment/definitions
GET /segment/definitions?{QUERY_PARAMETERS}
```

**クエリパラメーター**

| パラメーター | 説明 | 例 |
| --------- | ----------- | ------- |
| `start` | 返されるセグメント定義の開始オフセットを指定します。 | `start=4` |
| `limit` | 1ページに返されるセグメント定義の数を指定します。 | `limit=20` |
| `page` | セグメント定義の結果の開始元となるページを指定します。 | `page=5` |
| `sort` | 結果を並べ替える基準となるフィールドを指定します。 は次の形式で書き込まれます。 `[attributeName]:[desc|asc]`. | `sort=updateTime:desc` |
| `evaluationInfo.continuous.enabled` | セグメント定義がストリーミングを有効にするかどうかを指定します。 | `evaluationInfo.continuous.enabled=true` |

**リクエスト**

次のリクエストは、IMS組織内で投稿された最後の2つのセグメント定義を取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/segment/definitions?limit=2 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答が返されると、指定したIMS組織のセグメント定義のリストを持つHTTPステータス200がJSONとして返されます。

```json
{
    "segments": [
        {
            "id": "3da8bad9-29fb-40e0-b39e-f80322e964de",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "ttlInDays": 30,
            "imsOrgId": "{IMS_ORG}",
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
            "ttlInDays": 30,
            "imsOrgId": "{IMS_ORG}",
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

## 新しいセグメント定義の作成 {#create}

エンドポイントにPOSTリクエストを作成して、新しいセグメント定義を作成でき `/segment/definitions` ます。

**API形式**

```http
POST /segment/definitions
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/core/ups/segment/definitions
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
        "payloadSchema": "string",
        "ttlInDays": 60
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | **必須。** セグメントを参照する一意の名前。 |
| `schema` | **必須。** セグメント内のエンティティに関連付けられているスキーマ。 またはフィールドで構成され `id``name` ます。 |
| `expression` | **必須。** セグメント定義に関するフィールド情報を含むエンティティ。 |
| `expression.type` | 式タイプを指定します。 現在、「PQL」のみがサポートされています。 |
| `expression.format` | 式の構造を値で示します。 現在、次の形式がサポートされています。 <ul><li>`pql/text`: パブリッシュされたPQL文法に従った、セグメント定義のテキスト表現。  例：`workAddress.stateProvince = homeAddress.stateProvince`。</li></ul> |
| `expression.value` | に示す種類に適合する式 `expression.format`。 |
| `description` | 定義の人間が読み取り可能な説明。 |

**応答**

正常に応答すると、新しく作成したセグメント定義の詳細を含むHTTPステータス200が返されます。

```json
{
    "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "ttlInDays": 60,
    "profileInstanceId": "ups",
    "imsOrgId": "{IMS_ORG}",
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
| `id` | 新しく作成されたセグメント定義のシステム生成ID。 |
| `evaluationInfo` | セグメント定義が受ける評価のタイプを示す、システム生成オブジェクト。 バッチ、連続（ストリーミング）または同期セグメントを使用できます。 |

## 特定のセグメント定義の取得 {#get}

エンドポイントにGETリクエストを送信し、取得するセグメント定義のIDをリクエストパスに指定することで、特定のセグメント定義に関する詳細な情報を取得でき `/segment/definitions` ます。

**API形式**

```http
GET /segment/definitions/{SEGMENT_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SEGMENT_ID}` | 取得するセグメント定義の `id` 値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/segment/definitions/4afe34ae-8c98-4513-8a1d-67ccaa54bc05 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常に応答すると、HTTPステータス200が返され、指定したセグメント定義に関する詳細情報が返されます。

```json
{
    "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "ttlInDays": 60,
    "profileInstanceId": "ups",
    "imsOrgId": "{IMS_ORG}",
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
| `id` | システム生成のセグメント定義の読み取り専用ID。 |
| `name` | セグメントを参照する一意の名前。 |
| `schema` | セグメント内のエンティティに関連付けられているスキーマ。 またはフィールドで構成され `id``name` ます。 |
| `expression` | セグメント定義に関するフィールド情報を含むエンティティ。 |
| `expression.type` | 式タイプを指定します。 現在、「PQL」のみがサポートされています。 |
| `expression.format` | 式の構造を値で示します。 現在、次の形式がサポートされています。 <ul><li>`pql/text`: パブリッシュされたPQL文法に従った、セグメント定義のテキスト表現。  例：`workAddress.stateProvince = homeAddress.stateProvince`。</li></ul> |
| `expression.value` | に示す種類に適合する式 `expression.format`。 |
| `description` | 定義の人間が読み取り可能な説明。 |
| `evaluationInfo` | システム生成オブジェクト。評価、バッチ、連続（ストリーミングとも呼ばれます）または同期のタイプを指定し、セグメント定義が実行されます。 |

## 一括取得セグメント定義 {#bulk-get}

エンドポイントにPOSTリクエストを送信し、リクエスト本文にセグメント定義の `/segment/definitions/bulk-get``id` 値を指定することで、指定した複数のセグメント定義に関する詳細な情報を取得できます。

**API形式**

```http
POST /segment/definitions/bulk-get
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/core/ups/segment/definitions/bulk-get \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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

**応答**

正常な応答が返されると、HTTPステータス207が返され、このステータスには要求されたセグメント定義が含まれます。

```json
{
    "results": {
        "54669488-03ab-4e0d-a694-37fe49e32be8": {
            "id": "54669488-03ab-4e0d-a694-37fe49e32be8",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "ttlInDays": 60,
            "profileInstanceId": "ups",
            "imsOrgId": "{IMS_ORG}",
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
            "ttlInDays": 60,
            "profileInstanceId": "ups",
            "imsOrgId": "{IMS_ORG}",
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
| `id` | システム生成のセグメント定義の読み取り専用ID。 |
| `name` | セグメントを参照する一意の名前。 |
| `schema` | セグメント内のエンティティに関連付けられているスキーマ。 またはフィールドで構成され `id``name` ます。 |
| `expression` | セグメント定義に関するフィールド情報を含むエンティティ。 |
| `expression.type` | 式タイプを指定します。 現在、「PQL」のみがサポートされています。 |
| `expression.format` | 式の構造を値で示します。 現在、次の形式がサポートされています。 <ul><li>`pql/text`: パブリッシュされたPQL文法に従った、セグメント定義のテキスト表現。  例：`workAddress.stateProvince = homeAddress.stateProvince`。</li></ul> |
| `expression.value` | に示す種類に適合する式 `expression.format`。 |
| `description` | 定義の人間が読み取り可能な説明。 |
| `evaluationInfo` | システム生成オブジェクト。評価、バッチ、連続（ストリーミングとも呼ばれます）または同期のタイプを指定し、セグメント定義が実行されます。 |

## 特定のセグメント定義の削除 {#delete}

エンドポイントにDELETEリクエストを送信し、削除するセグメント定義のIDをリクエストパスに指定することで、特定のセグメント定義を削除するようにリクエストでき `/segment/definitions` ます。

**API形式**

```http
DELETE /segment/definitions/{SEGMENT_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SEGMENT_ID}` | 削除するセグメント定義の `id` 値。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/segment/definitions/4afe34ae-8c98-4513-8a1d-67ccaa54bc05 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、メッセージなしのHTTPステータス200が返されます。

## 特定のセグメント定義の更新

特定のセグメント定義を更新するには、エンドポイントにPATCHリクエストを作成し、更新するセグメント定義のIDをリクエストパスに指定します。 `/segment/definitions`

**API形式**

```http
PATCH /segment/definitions/{SEGMENT_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SEGMENT_ID}` | 更新するセグメント定義の `id` 値。 |

**リクエスト**

次のリクエストは、米国からカナダへの勤務先住所の国の更新を求めています。

```shell
curl -X PATCH https://platform.adobe.io/data/core/ups/segment/definitions/4afe34ae-8c98-4513-8a1d-67ccaa54bc05 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
    "ttlInDays": 60,
    "creationTime": 0,
    "updateTime": 0,
    "updateEpoch": 0
}'
```

**応答**

正常に応答すると、HTTPステータス200が返され、新たに更新されたセグメント定義の詳細が返されます。 勤務先住所の国が米国からカナダ(CA)に更新されたことに注目してください。

```json
{
    "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "ttlInDays": 60,
    "profileInstanceId": "ups",
    "imsOrgId": "{IMS_ORG}",
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

## 次の手順

このガイドを読むと、セグメント定義の動作についての理解が深まります。 セグメントの作成の詳細については、「セグメントの [作成](../tutorials/create-a-segment.md) 」チュートリアルを参照してください。