---
solution: Experience Platform
title: セグメント定義 API エンドポイント
description: Adobe Experience Platform Segmentation Service API のセグメント定義エンドポイントを使用すると、組織のセグメント定義をプログラムで管理できます。
exl-id: e7811b96-32bf-4b28-9abb-74c17a71ffab
source-git-commit: d47ec6fca05191f532b5a2e94f1943c4337258ed
workflow-type: tm+mt
source-wordcount: '1228'
ht-degree: 29%

---

# セグメント定義エンドポイント

Adobe Experience Platformでは、プロファイルのグループから特定の属性やビヘイビアーのグループを定義するセグメント定義を作成できます。 セグメント定義は、 [!DNL Profile Query Language] (PQL) を参照してください。 セグメント定義は、オーディエンスを作成するためにプロファイルに適用されます。 このオブジェクト（セグメント定義）は、PQL 述語とも呼ばれます。 PQL 述語は、指定するレコードまたは時系列データに関連する条件に基づいて、セグメント定義のルールを定義します [!DNL Real-Time Customer Profile]. PQL クエリの記述について詳しくは、[PQL ガイド](../pql/overview.md)を参照してください。

このガイドには、セグメント定義をより深く理解するのに役立つ情報を提供し、API を使用して基本的なアクションを実行するための API 呼び出しの例を含んでいます。

## はじめに

このガイドで使用されるエンドポイントは、 [!DNL Adobe Experience Platform Segmentation Service] API. 続行する前に、 [入門ガイド](./getting-started.md) を参照してください。

## セグメント定義のリストの取得 {#list}

組織のすべてのセグメント定義のリストを取得するには、に対してGETリクエストを実行します。 `/segment/definitions` endpoint.

**API 形式**

`/segment/definitions` エンドポイントは、結果を絞り込むのに役立つ、複数のクエリパラメーターをサポートしています。これらのパラメーターはオプションですが、高価なオーバーヘッドを削減するために、使用を強くお勧めします。 パラメーターを指定しないでこのエンドポイントを呼び出すと、組織で使用可能なすべてのセグメント定義が取得されます。 複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。

```http
GET /segment/definitions
GET /segment/definitions?{QUERY_PARAMETERS}
```

**クエリパラメータ**

| パラメーター | 説明 | 例 |
| --------- | ----------- | ------- |
| `start` | 返されるセグメント定義の開始オフセットを指定します。 | `start=4` |
| `limit` | 返される、1 ページあたりのセグメント定義の数を指定します。 | `limit=20` |
| `page` | どのページからセグメント定義の結果を表示するかを指定します。 | `page=5` |
| `sort` | 結果の並べ替えに使用するフィールドを指定します。 次の形式で書き込まれます。 `[attributeName]:[desc|asc]`. | `sort=updateTime:desc` |
| `evaluationInfo.continuous.enabled` | セグメント定義でストリーミングを有効にするかどうかを指定します。 | `evaluationInfo.continuous.enabled=true` |

**リクエスト**

次のリクエストでは、組織内で投稿された最後の 2 つのセグメント定義を取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/segment/definitions?limit=2 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、HTTP ステータス 200 と、指定した組織のセグメント定義のリストを JSON として返します。

```json
{
    "segments": [
        {
            "id": "3da8bad9-29fb-40e0-b39e-f80322e964de",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "ttlInDays": 30,
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
            "ttlInDays": 30,
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

## 新しいセグメント定義の作成 {#create}

新しいセグメント定義を作成するには、`/segment/definitions` エンドポイントに POST リクエストを実行します。

>[!IMPORTANT]
>
>API を使用して作成されたセグメント定義 **できません** を編集する場合は、セグメントビルダーを使用します。

**API 形式**

```http
POST /segment/definitions
```

**リクエスト**

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
        },
        "payloadSchema": "string",
        "ttlInDays": 60
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | セグメント定義を参照する際に使用される一意の名前。 |
| `description` | （オプション） 作成するセグメント定義の説明。 |
| `evaluationInfo` | （オプション） 作成するセグメント定義のタイプ。 バッチセグメントを作成する場合は、 `evaluationInfo.batch.enabled` を真にする。 ストリーミングセグメントを作成する場合は、 `evaluationInfo.continuous.enabled` を真にする。 エッジセグメントを作成する場合は、 `evaluationInfo.synchronous.enabled` を真にする。 空のままにすると、セグメント定義は **バッチ** セグメント。 |
| `schema` | セグメント内のエンティティに関連付けられたスキーマ。 `id` か `name` のどちらかのフィールドで構成されます。 |
| `expression` | セグメント定義に関するフィールド情報を含むエンティティ。 |
| `expression.type` | 式タイプを指定します。現時点では、「PQL」のみサポートされています。 |
| `expression.format` | 値内の式の構造を示します。現時点では、次の形式がサポートされています。 <ul><li>`pql/text`：セグメント定義のテキスト表現で、公開された PQL 文法に従っている必要があります。例：`workAddress.stateProvince = homeAddress.stateProvince`</li></ul> |
| `expression.value` | `expression.format` に指定されたタイプに適合する式です。 |

<!-- >[!NOTE]
>
>A segment definition expression may also reference a computed attribute. To learn more, please refer to the [computed attribute API endpoint guide](../../profile/computed-attributes/ca-api.md)
>
>Computed attribute functionality is in alpha and is not available to all users. Documentation and functionality are subject to change. -->

**応答**

リクエストが成功した場合は、新しく作成したセグメント定義の詳細と HTTP ステータス 200 が返されます。

```json
{
    "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "ttlInDays": 60,
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
| `evaluationInfo` | セグメント定義が受ける評価のタイプを示すオブジェクト。 バッチ、ストリーミング（連続とも呼ばれます）、エッジ（同期）セグメント化を指定できます。 |

## 特定のセグメント定義の取得 {#get}

特定のセグメント定義に関する詳細な情報を取得するには、 `/segment/definitions` エンドポイントを検索し、リクエストパスで取得するセグメント定義の ID を指定します。

**API 形式**

```http
GET /segment/definitions/{SEGMENT_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SEGMENT_ID}` | The `id` 取得するセグメント定義の値。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/segment/definitions/4afe34ae-8c98-4513-8a1d-67ccaa54bc05 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、特定のセグメント定義についての詳細情報と HTTP ステータス 200 が返されます。

```json
{
    "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "ttlInDays": 60,
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
| `id` | システムで生成された、セグメント定義の読み取り専用 ID。 |
| `name` | セグメント定義を参照する際に使用される一意の名前。 |
| `schema` | セグメント内のエンティティに関連付けられたスキーマ。 `id` か `name` のどちらかのフィールドで構成されます。 |
| `expression` | セグメント定義に関するフィールド情報を含むエンティティ。 |
| `expression.type` | 式タイプを指定します。現時点では、「PQL」のみサポートされています。 |
| `expression.format` | 値内の式の構造を示します。現時点では、次の形式がサポートされています。 <ul><li>`pql/text`：セグメント定義のテキスト表現で、公開された PQL 文法に従っている必要があります。例：`workAddress.stateProvince = homeAddress.stateProvince`</li></ul> |
| `expression.value` | `expression.format` に指定されたタイプに適合する式です。 |
| `description` | 人間にとってわかりやすい、定義の説明。 |
| `evaluationInfo` | セグメント定義が受ける評価のタイプ、バッチ、ストリーミング（連続）、エッジ（同期）を示すオブジェクト。 |

## セグメント定義の一括取得 {#bulk-get}

指定された複数のセグメント定義に関する詳細な情報を取得するには、 `/segment/definitions/bulk-get` エンドポイントと `id` リクエスト本文のセグメント定義の値。

**API 形式**

```http
POST /segment/definitions/bulk-get
```

**リクエスト**

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

**応答**

正常な応答は、HTTP ステータス 207 と、リクエストされたセグメント定義を返します。

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
            "ttlInDays": 60,
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
| `id` | システムで生成された、セグメント定義の読み取り専用 ID。 |
| `name` | セグメント定義を参照する際に使用される一意の名前。 |
| `schema` | セグメント内のエンティティに関連付けられたスキーマ。 `id` か `name` のどちらかのフィールドで構成されます。 |
| `expression` | セグメント定義に関するフィールド情報を含むエンティティ。 |
| `expression.type` | 式タイプを指定します。現時点では、「PQL」のみサポートされています。 |
| `expression.format` | 値内の式の構造を示します。現時点では、次の形式がサポートされています。 <ul><li>`pql/text`：セグメント定義のテキスト表現で、公開された PQL 文法に従っている必要があります。例：`workAddress.stateProvince = homeAddress.stateProvince`</li></ul> |
| `expression.value` | `expression.format` に指定されたタイプに適合する式です。 |
| `description` | 人間にとってわかりやすい、定義の説明。 |
| `evaluationInfo` | セグメント定義が受ける評価のタイプ、バッチ、ストリーミング（連続）、エッジ（同期）を示すオブジェクト。 |

## 特定のセグメント定義の削除 {#delete}

特定のセグメント定義の削除をリクエストするには、 `/segment/definitions` エンドポイントを作成し、リクエストパスで削除するセグメント定義の ID を指定します。

>[!NOTE]
>
> 宛先のアクティベーションで使用されるセグメント定義 **できません** を削除します。

**API 形式**

```http
DELETE /segment/definitions/{SEGMENT_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SEGMENT_ID}` | The `id` 削除するセグメント定義の値。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/segment/definitions/4afe34ae-8c98-4513-8a1d-67ccaa54bc05 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合、HTTP ステータス 200 が返され、メッセージは返されません。

## 特定のセグメント定義の更新

特定のセグメント定義を更新するには、 `/segment/definitions` エンドポイントを作成し、更新するセグメント定義の ID をリクエストパスで指定します。

**API 形式**

```http
PATCH /segment/definitions/{SEGMENT_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{SEGMENT_ID}` | The `id` 更新するセグメント定義の値。 |

**リクエスト**

次のリクエストでは、米国からカナダへの勤務先の国を更新します。

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
    "ttlInDays": 60,
    "creationTime": 0,
    "updateTime": 0,
    "updateEpoch": 0
}'
```

**応答**

正常な応答は、HTTP ステータス 200 と、新しく更新されたセグメント定義の詳細を返します。 勤務先住所の国が米国（米国）からカナダ (CA) に更新されたことに注意してください。

```json
{
    "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "ttlInDays": 60,
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

## セグメント定義を変換

セグメント定義は、 `pql/text` および `pql/json` または `pql/json` から `pql/text` に対してPOSTリクエストを行う `/segment/conversion` endpoint.

**API 形式**

```http
POST /segment/conversion
```

**リクエスト**

次のリクエストでは、セグメント定義の形式が `pql/text` から `pql/json`.

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
        "payloadSchema": "string",
        "ttlInDays": 60
    }'
```

**応答**

正常な応答は、HTTP ステータス 200 と、新しく変換されたセグメント定義の詳細を返します。

```json
{
    "ttlInDays": 60,
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

## 次の手順

このガイドを読むと、セグメント定義の動作をより深く理解できます。 セグメントの作成について詳しくは、 [セグメントの作成](../tutorials/create-a-segment.md) チュートリアル
