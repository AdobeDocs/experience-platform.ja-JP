---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: セグメント定義
topic: developer guide
translation-type: tm+mt
source-git-commit: 45a196d13b50031d635ceb7c5c952e42c09bd893

---


# セグメント定義開発ガイド

Adobe Experience Platformを使用すると、セグメントのグループから特定の属性や行動のグループを定義するセグメントを作成できます。プロファイル

この開発者ガイドでは、次のセグメント定義の説明を提供します。

- [セグメント定義のリストの取得](#retrieve-a-list-of-segment-definitions)
- [新しいセグメント定義の作成](#create-a-new-segment-definition)
- [特定のセグメント定義の取得](#retrieve-a-specific-segment-definition)
- [特定のセグメント定義の削除](#delete-a-specific-segment-definition)
- [特定のセグメント定義の更新](#update-a-specific-segment-definition)

## はじめに

このガイドで使用されるAPIエンドポイントは、セグメントAPIの一部です。 先に進む前に、セグメント化開発ガイ [ドを参照してください](./getting-started.md)。

特に、『セグメン [ト開発者](./getting-started.md#getting-started) 』ガイドの「はじめに」の節には、関連トピックへのリンク、ドキュメントでのサンプルAPI呼び出しの読み方のガイド、Experience Platform APIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報が含まれています。

## セグメント定義のリストの取得

エンドポイントにGETリクエストを行うことで、IMS組織のすべてのセグメント定義のリストを取得で `/segment/definitions` きます。

**API形式**

```http
GET /segment/definitions
GET /segment/definitions?{QUERY_PARAMETERS}
```

- `{QUERY_PARAMETERS}`:(オ&#x200B;*プション*)応答に返される結果を設定する要求パスに追加されるパラメーター。 複数のパラメーターを含め、アンパサンド(`&`)で区切ることができます。 使用可能なパラメーターを以下に示します。

**クエリパラメータ**

次に、セグメント定義のリストに使用できるクエリパラメータの一覧を示します。 これらのパラメーターはすべてオプションです。 パラメーターを指定しないでこのエンドポイントを呼び出すと、組織で使用可能なすべてのセグメント定義が取得されます。

| パラメーター | 説明 |
| --------- | ----------- |
| `start` | ??? |
| `limit` | 1ページに返されるセグメント定義の数を指定します。 |
| `page` | セグメント定義の結果の開始元ページ。 |
| `sort` | 結果の並べ替えに使用するフィールドを指定します。 |
| `evaluationInfo.continuous.enabled` | セグメント定義がストリーミングを有効にするかどうかを指定します。 |

**リクエスト**

```shell
cur -X GET https://platform.adobe.io/data/core/ups/segment/definitions?QUERY \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、指定したIMS組織のセグメント定義のリストをJSONとして持つHTTPステータス200を返します。

```json
{
    "segments": [
        {
            "id": "3da8bad9-29fb-40e0-b39e-f80322e964de",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "ttlInDays": 30,
            "imsOrgId": "E95186D65A28ABF00A495D82@AdobeOrg",
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
                "value": "{\"nodeType\":\"select\",\"variables\":[{\"nodeType\":\"varDecl\",\"varName\":\"_Checkouts1\",\"from\":{\"nodeType\":\"fieldLookup\",\"fieldName\":\"xEvent\",\"object\":{\"nodeType\":\"literal\",\"literalType\":\"XDMObject\",\"value\":\"profile\"}},\"where\":{\"nodeType\":\"fnApply\",\"fnName\":\"equals\",\"params\":[{\"nodeType\":\"fieldLookup\",\"fieldName\":\"eventType\",\"object\":{\"nodeType\":\"varRef\",\"varName\":\"_Checkouts1\"}},{\"literalType\":\"String\",\"nodeType\":\"literal\",\"value\":\"commerce.checkouts\"},{\"nodeType\":\"literal\",\"literalType\":\"Boolean\",\"value\":true}]}}]}"
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
        ... ,
        {
            "id": "ca763983-5572-4ea4-809c-b7dff7e0d79b",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "ttlInDays": 30,
            "imsOrgId": "E95186D65A28ABF00A495D82@AdobeOrg",
            "name": "test segment",
            "description": "",
            "expression": {
                "type": "PQL",
                "format": "pql/json",
                "value": "{\"nodeType\":\"fnApply\",\"fnName\":\"and\",\"params\":[{\"nodeType\":\"fnApply\",\"fnName\":\"=\",\"params\":[{\"nodeType\":\"fieldLookup\",\"fieldName\":\"points\",\"object\":{\"nodeType\":\"fieldLookup\",\"fieldName\":\"_acpstardust\",\"object\":{\"nodeType\":\"literal\",\"literalType\":\"XDMObject\",\"value\":\"profile\"}}},{\"literalType\":\"Double\",\"nodeType\":\"literal\",\"value\":2}]},{\"nodeType\":\"fnApply\",\"fnName\":\"equals\",\"params\":[{\"nodeType\":\"fieldLookup\",\"fieldName\":\"testLoyaltyID\",\"object\":{\"nodeType\":\"fieldLookup\",\"fieldName\":\"_acpstardust\",\"object\":{\"nodeType\":\"literal\",\"literalType\":\"XDMObject\",\"value\":\"profile\"}}},{\"literalType\":\"String\",\"nodeType\":\"literal\",\"value\":\"\"},{\"nodeType\":\"literal\",\"literalType\":\"Boolean\",\"value\":false}]}]}"
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
        "totalCount": 4,
        "totalPages": 1,
        "sortField": "creationTime",
        "sort": "desc",
        "pageSize": 4,
        "limit": 100
    },
    "link": {}
}
```

## 新しいセグメント定義の作成

エンドポイントにPOSTリクエストを行うことで、新しいセグメント定義を作成で `/segment/definitions` きます。

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
 -d '
 {
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
  "ttlInDays": 60,
  "creationTime": 0,
  "updateTime": 0,
  "updateEpoch": 0
}
 '
```

説明体

**応答**

正常な応答は、新しく作成したセグメント定義の詳細を含むHTTPステータス200を返します。

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

本文とヘッダーを説明する

## 特定のセグメント定義の取得

特定のセグメント定義に関する詳細な情報を取得するには、エンドポイントにGETリクエストを作成し、 `/segment/definitions` リクエストパスにセグメント定義の値を `id` 指定します。

**API形式**

```http
GET /segment/definitions/{SEGMENT_ID}
```

- `{SEGMENT_ID}`:取得 `id` するセグメント定義の値。

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/segment/definitions/4afe34ae-8c98-4513-8a1d-67ccaa54bc05 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、HTTPステータス200を返し、指定したセグメント定義に関する詳細情報が返されます。

```json
{
    "id": "4afe34ae-8c98-4513-8a1d-67ccaa54bc05",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "ttlInDays": 60,
    "profileInstanceId": "ups",
    "imsOrgId": "E95186D65A28ABF00A495D82@AdobeOrg",
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

## 特定のセグメント定義の削除

エンドポイントにDELETEリクエストを行い、リクエストパスにセグメント定義の値を指定するこ `/segment/definitions` とで、指定したセグメント定義を削 `id` 除するようにリクエストできます。

**API形式**

```http
DELETE /segment/definitions/{SEGMENT_ID}
```

- `{SEGMENT_ID}` 削除 `id` するセグメント定義の値。

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/segment/definitions/4afe34ae-8c98-4513-8a1d-67ccaa54bc05 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、HTTPステータス200をメッセージなしで返します。

## 特定のセグメント定義の更新

エンドポイントにPATCHリクエストを作成し、リクエストパスにセグメント定義の値を `/segment/definitions` 指定することで、指定したセ `id` グメント定義を更新できます。

**API形式**

```http
PATCH /segment/definitions/{SEGMENT_ID}
```

- `{SEGMENT_ID}`:更新 `id` するセグメント定義の値。

**リクエスト**

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
        "value": "workAddress.country = \"US\""
    },
    "schema": {
        "name": "_xdm.context.profile"
    },
    "payloadSchema": "string",
    "ttlInDays": 60,
    "creationTime": 0,
    "updateTime": 0,
    "updateEpoch": 0
}
'
```

説明体

**応答**

正常に応答すると、新たに更新されたセグメント定義の詳細と共にHTTPステータス200が返されます。

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
    "updateEpoch": 1579295340,
    "updateTime": 1579295340000
}
```

本文とヘッダを説明する

## 次の手順
