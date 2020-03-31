---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ポリシー
topic: developer guide
translation-type: tm+mt
source-git-commit: 2997243622a7483ae23e21487128cea6badecb80

---


# ポリシー評価

マーケティングアクションが作成され、ポリシーが定義されたら、Policy Service APIを使用して、特定のアクションによってポリシーが違反したかどうかを評価できます。 返される制約は、データ使用ラベルを含む指定されたデータに対してマーケティングアクションを試みることで違反するポリシーのセットの形をとります。

デフォルトでは、 **ステータスが「ENABLED」に設定されたポリシーのみが評価に含まれます**。ただし、クエリパラメーターを使用して `?includeDraft=true` 、評価に「DRAFT」ポリシーを含めることができます。

評価のリクエストは、次の3つの方法のいずれかで行うことができます。

1. 一連のデータ使用ラベルとマーケティングアクションが指定された場合、そのアクションはポリシーに違反していますか。
1. 1つ以上のデータセットとマーケティングアクションが指定された場合、そのアクションはポリシーに違反していますか。
1. 1つ以上のデータセットと、各データセット内の1つ以上のフィールドのサブセットを指定した場合、アクションはポリシーに違反していますか。

## データ使用ラベルとマーケティングアクションを使用してポリシーを評価

Evaluating policy violations based on the presence of data usage labels requires you to specify the set of labels that would be present on the data during the request. This is done through the use of query parameters, where data usage labels are provided as a comma-separated list of values, as shown in the following example.

**API形式**

```http
GET /marketingActions/core/{marketingActionName}/constraints?duleLabels={value1},{value2}
GET /marketingActions/custom/{marketingActionName}/constraints?duleLabels={value1},{value2}
```

**リクエスト**

次の例のリクエストは、ラベルC1およびC3に対するマーケティングアクションを評価します。 When evaluating policies using data usage labels, please keep the following in mind:
* **データ使用ラベルでは大文字と小文字が区別されます。** The request shown above returns a violated policy, whereas making the same request using lowercase labels (e.g. `"c1,c3"`, `"C1,c3"`, `"c1,C3"`) does not.
* **Be aware of the`AND`and`OR`operators in your policy expressions.** In this example, if either label (`C1` or `C3`) had appeared alone in the request, the marketing action would not have violated this policy. 両方のラベル(`C1 AND C3`)を使用して、違反ポリシーを返します。 Ensure you are evaluating policies carefully and defining policy expressions with equal care.

```SHELL
curl -X GET \
  'https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/sampleMarketingAction/constraints?duleLabels=C1,C3' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答オブジェクトには、要 `duleLabels` 求で送信されるラベルと一致する配列が含まれます。 データ使用ラベルに対して指定したマーケティングアクションを実行すると、ポリシーに違反する場合、 `violatedPolicies` アレイには影響を受けるポリシー（複数可）の詳細が含まれます。 ポリシーに違反しない場合は、 `violatedPolicies` アレイは空(`[]`)です。

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

## データセットとマーケティングアクションを使用してポリシーを評価

また、データ使用ラベルを収集できる1つ以上のデータセットのIDを指定することで、ポリシー違反を評価することもできます。 これは、以下に示すように、マーケティングアクションのコアエンドポイントまたはカスタムエンドポイントに対してPOST `/constraints` リクエストを実行し、リクエスト本文内でデータセットIDを指定することで行われます。

**API形式**

```http
POST /marketingActions/core/{marketingActionName}/constraints
POST /marketingActions/custom/{marketingActionName}/constraints
```

**リクエスト**

リクエスト本文には、各データセットIDのオブジェクトを持つ配列が含まれています。 リクエストの本文を送信するので、「Content-Type:application/json&quot;リクエストヘッダーは必須です。次に例を示します。

```SHELL
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

**応答**

応答オブジェクトには、指定し `duleLabels` たデータセット内のすべてのラベルの統合リストを含む配列が含まれます。 このリストには、データセット内のすべてのフィールドにデータセットおよびフィールドレベルのラベルが含まれます。

この応答には、各データセットのオ `discoveredLabels` ブジェクトを含む配列も含まれ、データセッ `datasetLabels` トおよびフィールドレベルのラベルに分類されて表示されます。 各フィールドレベルのラベルには、そのラベルを持つ特定のフィールドへのパスが表示されます。

指定したマーケティングアクションがデータセット内のポリシーに `duleLabels` 違反する場合、影響を受け `violatedPolicies` るポリシー（複数可）の詳細が配列に含まれます。 ポリシーに違反しない場合は、 `violatedPolicies` アレイは空(`[]`)です。

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

## データセット、フィールド、およびマーケティングアクションを使用してポリシーを評価

データセットIDを1つ以上指定する以外に、各データセット内のフィールドのサブセットも指定できます。これらのフィールドのデータ使用ラベルのみが評価されることを示します。 データセットのみを含むPOSTリクエストと同様に、このリクエストは各データセットの特定のフィールドをリクエスト本文に追加します。

データセットフィールドを使用してポリシーを評価する場合は、次の点に注意してください。

* **フィールド名では大文字と小文字が区別されます。** フィールドを指定する場合は、データセットに表示されるとおりに記述する必要があります(例： `firstName` v `firstname`)。
* **データセットのラベルの継承。** データ使用ラベルは複数のレベルで適用でき、下方向に継承されます。 ポリシーの評価が期待どおりの結果を返さない場合は、フィールドレベルで適用されるラベルに加えて、データセットからフィールドに継承されるラベルをチェックしてください。

**API形式**

```http
POST /marketingActions/core/{marketingActionName}/constraints
POST /marketingActions/custom/{marketingActionName}/constraints
```

**リクエスト**

リクエスト本文には、各データセットIDのオブジェクトと、評価に使用するデータセット内のフィールドのサブセットを含む配列が含まれています。 リクエストの本文を送信するので、「Content-Type:application/json&quot;リクエストヘッダーは必須です。次に例を示します。

```SHELL
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

**応答**

応答オブジェクトには、指定 `duleLabels` したフィールドで見つかったラベルの統合リストを含む配列が含まれます。 データセットラベルも含まれ、フィールドに継承されます。

指定されたフィールドのデータに対して指定されたマーケティングアクションを実行することでポリシーに違反した場合、影響を受けるポリシー( `violatedPolicies` またはポリシー)の詳細が配列に含まれます。 ポリシーに違反しない場合は、 `violatedPolicies` アレイは空(`[]`)です。

以下の応答では、のリストが現在は短くなっています。これは、リクエスト本文に指定されたフィールドのみが含まれ `duleLabels``discoveredLabels` るので、各データセットのと同様です。 また、以前に違反したポリシー「広告またはコンテンツのターゲット設定」には両方のラベルが必要なので、違反はなくなり、配列は空になっ `C4 AND C6``violatedPolicies` て表示されます。

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

## リアルタイム顧客プロファイル

Policy Service APIは、リアルタイム顧客セグメントの使用に関連するポリシー違反の有無を確認するためにも使用できます。プロファイル 詳しくは、[オーディエンスセグメントのデータ使用に対する準拠の適用](../../segmentation/tutorials/governance.md)に関するチュートリアルを参照してください。