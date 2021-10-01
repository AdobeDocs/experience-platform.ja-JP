---
keywords: Experience Platform;ホーム;人気のトピック;ポリシーの適用;自動適用;API ベースの適用;データガバナンス
solution: Experience Platform
title: ポリシー評価 API エンドポイント
topic-legacy: developer guide
description: マーケティングアクションが作成され、ポリシーが定義されたら、Policy Service API を使用して、特定のアクションによってポリシーが違反したかどうかを評価できます。返される制約は、データ使用ラベルを含む指定されたデータに対してマーケティングアクションを試みることで違反するポリシーのセットの形をとります。
exl-id: f9903939-268b-492c-aca7-63200bfe4179
source-git-commit: 8133804076b1c0adf2eae5b748e86a35f3186d14
workflow-type: ht
source-wordcount: '1540'
ht-degree: 100%

---

# ポリシー評価エンドポイント

マーケティングアクションが作成され、ポリシーが定義されたら、[!DNL Policy Service] API を使用して、特定のアクションがポリシーに違反したかどうかを評価できます。返される制約は、データ使用ラベルを含む指定されたデータに対してマーケティングアクションを試みることで違反するポリシーのセットの形をとります。

デフォルトでは、ステータスが `ENABLED` に設定されたポリシーのみが評価に使用できます。ただし、クエリパラメーター `?includeDraft=true` を使用して、評価に `DRAFT` ポリシーを含めることはできます。

評価のリクエストは、次の 3 つの方法のいずれかでおこなうことができます。

1. マーケティングアクションとデータ使用ラベルのセットが指定されている場合、そのアクションはポリシーに違反していますか。
1. マーケティングアクションと 1 つ以上のデータセットが指定されている場合、そのアクションはポリシーに違反していますか。
1. マーケティングアクション、1 つ以上のデータセット、およびそれらの各データセット内の 1 つ以上のフィールドのサブセットが指定されている場合、そのアクションはポリシーに違反していますか。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Policy Service]  API](https://www.adobe.io/experience-platform-apis/references/policy-service/) の一部です。先に進む前に、[はじめる前に](./getting-started.md)のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の [!DNL Experience Platform] API の呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## データ使用ラベルを使用してポリシー違反を評価   {#labels}

GET リクエストで `duleLabels` クエリパラメーターを使用すると、特定のデータ使用ラベルのセットの存在に基づいてポリシー違反を評価できます。

**API 形式**

```http
GET /marketingActions/core/{MARKETING_ACTION_NAME}/constraints?duleLabels={LABELS_LIST}
GET /marketingActions/custom/{MARKETING_ACTION_NAME}/constraints?duleLabels={LABELS_LIST}
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | データ使用ラベルのセットに対してテストするマーケティングアクションの名前。[マーケティングアクションのエンドポイントに対して GET リクエスト](./marketing-actions.md#list)をおこなうと、使用可能なマーケティングアクションのリストを取得できます。 |
| `{LABELS_LIST}` | マーケティングアクションをテストするためのデータ使用ラベル名のコンマ区切りリスト。次に例を示します。`duleLabels=C1,C2,C3`<br><br>ラベル名では大文字と小文字が区別されます。`duleLabels` パラメーターにリストする場合は、大文字と小文字を正しく指定していることを確認してください。 |

**リクエスト**

次の例のリクエストは、ラベル C1 および C3 に対するマーケティングアクションを評価します。

>[!IMPORTANT]
>
>ポリシーでの表現における`AND`および`OR`演算子に注意してください。以下の例では、いずれかのラベル（`C1` または `C3`）がリクエスト内で単独で表示された場合、マーケティングアクションはこのポリシーに違反していません。両方のラベル（`C1` と `C3`）を使用して、ポリシー違反を返します。ポリシーを慎重に評価し、ポリシー式を十分に定義します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/sampleMarketingAction/constraints?duleLabels=C1,C3' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答には `violatedPolicies` 配列が含まれます。この配列には、指定されたラベルに対してマーケティングアクションを実行した結果として違反されたポリシーの詳細が含まれます。ポリシーに違反していない場合、`violatedPolicies` 配列は空になります。

```JSON
{
    "timestamp": 1551134846737,
    "clientId": "{CLIENT_ID}",
    "userId": "{USER_ID}",
    "imsOrg": "{IMS_ORG}",
    "marketingActionRef": "https://platform.adobe.io/marketingActions/custom/sampleMarketingAction",
    "duleLabels": [
        "C1",
        "C3"
    ],
    "violatedPolicies": [
        {
            "name": "Export Data to Third Party",
            "status": "ENABLED",
            "marketingActionRefs": [
                "https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/sampleMarketingAction"
            ],
            "description": "NEW content for description.",
            "deny": {
                "operator": "AND",
                "operands": [
                    {
                        "label": "C1"
                    },
                    {
                        "operator": "OR",
                        "operands": [
                            {
                                "label": "C3"
                            },
                            {
                                "label": "C7"
                            }
                        ]
                    }
                ]
            },
            "imsOrg": "{IMS_ORG}",
            "created": 1550703519823,
            "createdClient": "{CREATED_CLIENT}",
            "createdUser": "{CREATED_USER}",
            "updated": 1550714340335,
            "updatedClient": "{UPDATED_CLIENT}",
            "updatedUser": "{UPDATED_USER}",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/dulepolicy/policies/custom/5c6ddb9f5c404513dc2dc454"
                }
            },
            "id": "5c6ddb9f5c404513dc2dc454"
        }
    ]
}
```

## データセットを使用してポリシー違反を評価する   {#datasets}

データ使用ラベルを収集できる 1 つ以上のデータセットのセットに基づいて、ポリシー違反を評価できます。これをおこなうには、特定のマーケティングアクションの `/constraints` エンドポイントに対して POST リクエストを実行し、リクエスト本文内にデータセット ID のリストを提供します。

**API 形式**

```http
POST /marketingActions/core/{MARKETING_ACTION_NAME}/constraints
POST /marketingActions/custom/{MARKETING_ACTION_NAME}/constraints
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | 1 つ以上のデータセットに対してテストするマーケティングアクションの名前。[マーケティングアクションのエンドポイントに対して GET リクエスト](./marketing-actions.md#list)をおこなうと、使用可能なマーケティングアクションのリストを取得できます。 |

**リクエスト**

次のリクエストは、3 つのデータセットのセットに対して `crossSiteTargeting` マーケティングアクションを実行し、ポリシー違反の評価をおこないます。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/crossSiteTargeting/constraints \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '[
        {
            "entityType": "dataSet",
            "entityId": "5c423dc25f2f2e00005e2319"
        },
        {
            "entityType": "dataSet",
            "entityId": "5cc323e15410ef14b749481e"
        },
        {
            "entityType": "dataSet",
            "entityId": "5cc1fb685410ef14b748c55f"
        }
      ]'
```

| プロパティ | 説明 |
| --- | --- |
| `entityType` | ID が兄弟 `entityId` プロパティに示されるエンティティのタイプ。現在、許可されている値は `dataSet` のみです。 |
| `entityId` | マーケティングアクションをテストするデータセットの ID。データセットとそれに対応する ID のリストは、[!DNL Catalog Service] API の `/dataSets` エンドポイントに GET リクエストをおこなうことで取得できます。詳しくは、[ [!DNL Catalog]  オブジェクトの一覧表示](../../catalog/api/list-objects.md)に関するガイドを参照してください。 |

**応答** 

成功した応答には `violatedPolicies` 配列が含まれます。この配列には、指定されたデータセットに対するマーケティングアクションを実行した結果として違反となったポリシーの詳細が含まれます。ポリシーに違反していない場合、`violatedPolicies` 配列は空になります。

```JSON
{
    "timestamp": 1556324277895,
    "clientId": "{CLIENT_ID}",
    "userId": "{USER_ID}",
    "imsOrg": "{IMS_ORG}",
    "marketingActionRef": "https://platform.adobe.io:443/data/foundation/dulepolicy/marketingActions/custom/crossSiteTargeting",
    "duleLabels": [
        "C1",
        "C2",
        "C4",
        "C5",
        "C6"
    ],
    "discoveredLabels": [
        {
            "entityType": "dataSet",
            "entityId": "5c423dc25f2f2e00005e2319",
            "dataSetLabels": {
                "connection": {
                    "labels": []
                },
                "dataSet": {
                    "labels": [
                        "C6"
                    ]
                },
                "fields": [
                    {
                        "labels": [
                            "C2",
                            "C5"
                        ],
                        "path": "/properties/_customer"
                    },
                    {
                        "labels": [
                            "C4",
                            "C5"
                        ],
                        "path": "/properties/geoUnit"
                    },
                    {
                        "labels": [
                            "C4"
                        ],
                        "path": "/properties/identityMap"
                    },
                    {
                        "labels": [
                            "C4"
                        ],
                        "path": "/properties/journeyAI"
                    },
                    {
                        "labels": [
                            "C5"
                        ],
                        "path": "/properties/createdByBatchID"
                    },
                    {
                        "labels": [
                            "C5"
                        ],
                        "path": "/properties/faxPhone"
                    }
                ]
            }
        },
        {
            "entityType": "dataSet",
            "entityId": "5cc323e15410ef14b749481e",
            "dataSetLabels": {
                "connection": {
                    "labels": []
                },
                "dataSet": {
                    "labels": [
                        "C5"
                    ]
                },
                "fields": [
                    {
                        "labels": [
                            "C2",
                        ],
                        "path": "/properties/_customer"
                    },
                    {
                        "labels": [
                            "C5"
                        ],
                        "path": "/properties/geoUnit"
                    },
                    {
                        "labels": [
                            "C1"
                        ],
                        "path": "/properties/identityMap"
                    }
                ]
            }
        },
        {
            "entityType": "dataSet",
            "entityId": "5cc1fb685410ef14b748c55f",
            "dataSetLabels": {
                "connection": {
                    "labels": []
                },
                "dataSet": {
                    "labels": [
                        "C5"
                    ]
                },
                "fields": [
                    {
                        "labels": [
                            "C5"
                        ],
                        "path": "/properties/createdByBatchID"
                    },
                    {
                        "labels": [
                            "C5"
                        ],
                        "path": "/properties/faxPhone"
                    }
                ]
            }
        }
    ],
    "violatedPolicies": [
        {
            "name": "Targeting Ads or Content",
            "status": "ENABLED",
            "marketingActionRefs": [
                "https://platform.adobe.io:443/data/foundation/dulepolicy/marketingActions/custom/crossSiteTargeting"
            ],
            "description": "Data cannot be used for targeting any ads or content, either on-site or cross-site.",
            "deny": {
                "operator": "AND",
                "operands": [
                    {
                        "label": "C4"
                    },
                    {
                        "label": "C6"
                    }
                ]
            },
            "imsOrg": "{IMS_ORG}",
            "created": 1551141210463,
            "createdClient": "{CREATED_CLIENT}",
            "createdUser": "{CREATED_USER}",
            "updated": 1551146178603,
            "updatedClient": "{UPDATED_CLIENT}",
            "updatedUser": "{UPDATED_USER}",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io:443/data/foundation/dulepolicy/policies/custom/5c74895a74744d13dc2d87cc"
                }
            },
            "id": "5c74895a74744d13dc2d87cc"
        }
    ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `duleLabels` | 応答オブジェクトには、指定したデータセット内のすべてのラベルの統合リストを含む`duleLabels`配列が含まれます。このリストには、データセット内のすべてのフィールドにデータセットレベルおよびフィールドレベルのラベルが含まれます。 |
| `discoveredLabels` | この応答には、各データセットのオブジェクトを含む`discoveredLabels`配列も含まれ、`datasetLabels`がデータセットレベルおよびフィールドレベルのラベルに分類されて表示されます。各フィールドレベルのラベルには、そのラベルを持つ特定のフィールドへのパスが表示されます。 |

## 特定のデータセットフィールドを使用してポリシー違反を評価する {#fields}

1 つ以上のデータセット内のフィールドのサブセットに基づいてポリシー違反を評価できるので、これらのフィールドに適用されたデータ使用ラベルのみが評価されます。

データセットフィールドを使用してポリシーを評価する場合は、次の点に注意してください。

* **フィールド名では大文字と小文字が区別されます。** フィールドを指定するときは、データセットに表示されるとおりに正確に記述する必要があります（例えば、`firstName` と `firstname`）。
* **データセットラベルの継承**：データセット内の個々のフィールドは、データセットレベルで適用されたすべてのラベルを継承します。ポリシーの評価が期待どおりに返されない場合は、フィールドレベルで適用されるラベルに加え、データセットレベルからフィールドに継承されたラベルがないかどうかを確認してください。

**API 形式**

```http
POST /marketingActions/core/{MARKETING_ACTION_NAME}/constraints
POST /marketingActions/custom/{MARKETING_ACTION_NAME}/constraints
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | データセットフィールドのサブセットに対してテストするマーケティングアクションの名前。[マーケティングアクションのエンドポイントに対して GET リクエスト](./marketing-actions.md#list)をおこなうと、使用可能なマーケティングアクションのリストを取得できます。 |

**リクエスト**

次のリクエストは、3 つのデータセットに属する特定のフィールドセットに対してマーケティングアクション `crossSiteTargeting` をテストします。ペイロードは、[データセットのみを含む評価リクエスト](#datasets)に似ており、ラベルを収集するデータセットごとに特定のフィールドを追加します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/crossSiteTargeting/constraints \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '[
        {
            "entityType": "dataSet",
            "entityId": "5c423dc25f2f2e00005e2319",
            "entityMeta": {
                "fields": [
                    "/properties/_customer",
                    "/properties/faxPhone"
                ]
            }
        },
        {
            "entityType": "dataSet",
            "entityId": "5cc323e15410ef14b749481e",
            "entityMeta": {
                "fields": [
                    "/properties/_customer",
                    "/properties/geoUnit"
                ]
            }
        },
        {
            "entityType": "dataSet",
            "entityId": "5cc1fb685410ef14b748c55f",
            "entityMeta": {
                "fields": [
                    "/properties/faxPhone"
                ]
            }
        }
      ]'
```

| プロパティ | 説明 |
| --- | --- |
| `entityType` | ID が兄弟 `entityId` プロパティに示されるエンティティのタイプ。現在、許可されている値は `dataSet` のみです。 |
| `entityId` | マーケティングアクションに対してフィールドが評価されるデータセットの ID。データセットとそれに対応する ID のリストは、[!DNL Catalog Service] API の `/dataSets` エンドポイントに GET リクエストをおこなうことで取得できます。詳しくは、[ [!DNL Catalog]  オブジェクトの一覧表示](../../catalog/api/list-objects.md)に関するガイドを参照してください。 |
| `entityMeta.fields` | データセットのスキーマ内の特定のフィールドへのパスの配列。JSON ポインター文字列の形式で提供されます。これらの文字列に許可された構文の詳細については、API 基本ガイドの [JSON ポインター](../../landing/api-fundamentals.md#json-pointer)の節を参照してください。 |

**応答** 

成功した応答には `violatedPolicies` 配列が含まれます。この配列には、指定されたデータセットフィールドに対するマーケティングアクションの実行結果として違反されたポリシーの詳細が含まれます。ポリシーに違反していない場合、`violatedPolicies` 配列は空になります。

以下の例の応答と](#datasets)データセットのみを含む応答[を比較して、収集されたラベルのリストが短いことに注意してください。また、各データセットの `discoveredLabels` も、リクエスト本文で指定されたフィールドのみが含まれるので、削減されています。また、以前に違反したポリシー `Targeting Ads or Content` には、両方の`C4 AND C6` ラベルの提示が必要なので、空の `violatedPolicies` 配列で示されるように違反しなくなりました。

```JSON
{
    "timestamp": 1556325503038,
    "clientId": "{CLIENT_ID}",
    "userId": "{USER_ID}",
    "imsOrg": "{IMS_ORG}",
    "marketingActionRef": "https://platform.adobe.io:443/data/foundation/dulepolicy/marketingActions/custom/crossSiteTargeting",
    "duleLabels": [
        "C2",
        "C5",
        "C6"
    ],
    "discoveredLabels": [
        {
            "entityType": "dataSet",
            "entityId": "5c423dc25f2f2e00005e2319",
            "dataSetLabels": {
                "connection": {
                    "labels": []
                },
                "dataSet": {
                    "labels": [
                        "C6"
                    ]
                },
                "fields": [
                    {
                        "labels": [
                            "C2",
                            "C5"
                        ],
                        "path": "/properties/_customer"
                    },
                    {
                        "labels": [
                            "C5"
                        ],
                        "path": "/properties/faxPhone"
                    }
                ]
            }
        },
        {
            "entityType": "dataSet",
            "entityId": "5cc323e15410ef14b749481e",
            "dataSetLabels": {
                "connection": {
                    "labels": []
                },
                "dataSet": {
                    "labels": [
                        "C5"
                    ]
                },
                "fields": [
                    {
                        "labels": [
                            "C2",
                            "C5"
                        ],
                        "path": "/properties/_customer"
                    },
                    {
                        "labels": [
                            "C5"
                        ],
                        "path": "/properties/geoUnit"
                    }
                ]
            }
        },
        {
            "entityType": "dataSet",
            "entityId": "5cc1fb685410ef14b748c55f",
            "dataSetLabels": {
                "connection": {
                    "labels": []
                },
                "dataSet": {
                    "labels": [
                        "C5"
                    ]
                },
                "fields": [
                    {
                        "labels": [
                            "C5"
                        ],
                        "path": "/properties/faxPhone"
                    }
                ]
            }
        }
    ],
    "violatedPolicies": []
}
```

## ポリシーを一括で評価 {#bulk}

`/bulk-eval` エンドポイントを使用すると、1 回の API 呼び出しで複数の評価ジョブを実行できます。

**API 形式**

```http
POST /bulk-eval
```

**リクエスト**

一括評価要求のペイロードは、実行する評価ジョブごとに 1 つの、オブジェクトの配列でなければなりません。データセットとフィールドに基づいて評価されるジョブの場合は、`entityList` 配列を指定する必要があります。データ使用ラベルに基づいて評価されるジョブの場合は、`labels` 配列を指定する必要があります。

>[!WARNING]
>
>リストにある評価ジョブに `entityList` と `labels` の両方の配列が含まれている場合、エラーが発生します。データセットとラベルの両方に基づいて同じマーケティングアクションを評価する場合は、そのマーケティングアクションに対して別々の評価ジョブを含める必要があります。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/dulepolicy/bulk-eval \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '[
        {
          "evalRef": "https://platform.adobe.io:443/data/foundation/dulepolicy/marketingActions/core/emailTargeting/constraints",
          "includeDraft": false,
          "labels": [
            "C1",
            "C2",
            "C3"
          ]
        },
        {
          "evalRef": "https://platform.adobe.io:443/data/foundation/dulepolicy/marketingActions/core/emailTargeting/constraints",
          "includeDraft": false,
          "entityList": [
            {
              "entityType": "dataSet",
              "entityId": "5b67f4dd9f6e710000ea9da4",
              "entityMeta": {
                "fields": [
                  "address"
                ]
              }
            }
          ]
        }
      ]'
```

| プロパティ | 説明 |
| --- | --- |
| `evalRef` | ポリシー違反のラベルまたはデータセットに対してテストするマーケティングアクションの URI です。 |
| `includeDraft` | デフォルトでは、有効なポリシーのみが評価に関与します。`includeDraft` を `true` に設定すると、`DRAFT` ステータスのポリシーも関与します。 |
| `labels` | マーケティングアクションをテストするためのデータ使用ラベルの配列。<br><br>**重要**：このプロパティを使用する場合は、同じオブジェクトに `entityList` プロパティを含めないでください。データセットやフィールドを使用して同じマーケティングアクションを評価するには、`entityList` 配列を含むリクエストペイロードに別のオブジェクトを含める必要があります。 |
| `entityList` | マーケティングアクションをテストするためのデータセットの配列と（オプションで）それらのデータセット内の特定のフィールド。<br><br>**重要**：このプロパティを使用する場合、同じオブジェクトに `labels` プロパティを含めないでください。特定のデータ使用ラベルを使用して同じマーケティングアクションを評価するには、`labels` 配列を含むリクエストペイロードに別のオブジェクトを含める必要があります。 |
| `entityType` | マーケティングアクションをテストするエンティティのタイプ。現在は、`dataSet` のみがサポートされています。 |
| `entityId` | マーケティングアクションをテストするデータセットの ID。 |
| `entityMeta.fields` | （オプション）マーケティングアクションをテストするデータセット内の特定のフィールドのリスト。 |

**応答** 

応答が成功すると、リクエストで送信されたポリシー評価ジョブごとに 1 つ、評価結果の配列を返します。

```json
[
  {
    "status": 200,
    "body": {
      "timestamp": 1595866566165,
      "clientId": "{CLIENT_ID}",
      "userId": "{USER_ID}",
      "imsOrg": "{IMS_ORG}",
      "sandboxName": "prod",
      "marketingActionRef": "https://platform.adobe.io:443/data/foundation/dulepolicy/marketingActions/core/emailTargeting",
      "duleLabels": [
        "C1",
        "C2",
        "C3"
      ],
      "violatedPolicies": []
    }
  },
  {
    "status": 200,
    "body": {
      "timestamp": 1595866566165,
      "clientId": "{CLIENT_ID}",
      "userId": "{USER_ID}",
      "imsOrg": "{IMS_ORG}",
      "sandboxName": "prod",
      "marketingActionRef": "https://platform.adobe.io:443/data/foundation/dulepolicy/marketingActions/core/emailTargeting",
      "duleLabels": [
        "C1",
        "C2"
      ],
      "discoveredLabels": [
        {
          "entityType": "dataset",
          "entityId": "5b67f4dd9f6e710000ea9da4",
          "dataSetLabels": {
            "connection": {
              "labels": [

              ]
            },
            "dataset": {
              "labels": [
                "C1",
                "C2"
              ]
            },
            "fields": []
          }
        }
      ],
      "violatedPolicies": [
        {
          "name": "Email Policy",
          "status": "DRAFT",
          "marketingActionRefs": [
            "https://platform.adobe.io:443/data/foundation/dulepolicy/marketingActions/core/emailTargeting"
          ],
          "description": "Conditions under which we won't send marketing-based email",
          "deny": {
            "label": "C1",
            "operator": "AND",
            "operands": [
              {
                "label": "C1"
              },
              {
                "label": "C3"
              }
            ]
          },
          "id": "76131228-7654-11e8-adc0-fa7ae01bbebc",
          "imsOrg": "{IMS_ORG}",
          "created": 1529696681413,
          "createdClient": "{CLIENT_ID}",
          "createdUser": "{USER_ID}",
          "updated": 1529697651972,
          "updatedClient": "{CLIENT_ID}",
          "updatedUser": "{USER_ID}",
          "_links": {
            "self": {
              "href": "./76131228-7654-11e8-adc0-fa7ae01bbebc"
            }
          }
        }
      ]
    }
  }
]
```

## [!DNL Real-time Customer Profile] のポリシー評価

[!DNL Policy Service] APIを使用して、[!DNL Real-time Customer Profile] セグメントの使用に関連するポリシー違反をチェックすることもできます。詳しくは、[オーディエンスセグメントのデータ使用に対する準拠の適用](../../segmentation/tutorials/governance.md)に関するチュートリアルを参照してください。
