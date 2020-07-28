---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ポリシー
topic: developer guide
translation-type: tm+mt
source-git-commit: 0534fe8dcc11741ddc74749d231e732163adf5b0
workflow-type: tm+mt
source-wordcount: '938'
ht-degree: 95%

---


# ポリシー評価

Once marketing actions have been created and policies have been defined, you can use the [!DNL Policy Service] API to evaluate if any policies are violated by certain actions. 返される制約は、データ使用ラベルを含む指定されたデータに対してマーケティングアクションを試みることで違反するポリシーのセットの形をとります。

デフォルトでは、**ステータスが「ENABLED」に設定されたポリシーのみが評価に含まれます**。ただし、`?includeDraft=true`クエリーパラメーターを使用して、評価に「DRAFT」ポリシーを含めることができます。

評価のリクエストは、次の 3 つの方法のいずれかでおこなうことができます。

1. 一連のデータ使用ラベルとマーケティングアクションが指定された場合、アクションはポリシーに違反していますか。
1. 1 つ以上のデータセットとマーケティングアクションが指定された場合、そのアクションはポリシーに違反していますか。
1. 1 つ以上のデータセットと、各データセット内の 1 つ以上のフィールドのサブセットを指定した場合、アクションはポリシーに違反していますか。

## データ使用ラベルとマーケティングアクションを使用したポリシーの評価

データ使用ラベルの存在に基づいてポリシー違反を評価するには、リクエスト時にデータに存在するラベルのセットを指定する必要があります。これは、次の例に示すように、クエリーパラメーターを使用しておこないます。このパラメーターでは、データ使用ラベルは値のコンマ区切りリストとして提供されます。

**API 形式**

```http
GET /marketingActions/core/{marketingActionName}/constraints?duleLabels={value1},{value2}
GET /marketingActions/custom/{marketingActionName}/constraints?duleLabels={value1},{value2}
```

**リクエスト**

次の例のリクエストは、ラベル C1 および C3 に対するマーケティングアクションを評価します。データ使用ラベルを使用してポリシーを評価する場合は、次の点に注意してください。
* **データ使用ラベルでは大文字と小文字が区別されます。** 上に示したリクエストは違反ポリシーを返しますが、同じリクエストを小文字のラベル（`"c1,c3"`、`"C1,c3"`、`"c1,C3"`）は含まれません。
* **ポリシーでの表現における`AND`および`OR`演算子に注意してください。**&#x200B;この例では、いずれかのラベル（`C1`または`C3`）がリクエスト内で単独で表示された場合、マーケティングアクションはこのポリシーに違反していません。両方のラベル（`C1 AND C3`）を使用して、違反ポリシーを返します。ポリシーを慎重に評価し、ポリシー式を十分に定義します。

```SHELL
curl -X GET \
  'https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/sampleMarketingAction/constraints?duleLabels=C1,C3' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答オブジェクトには、リクエストで送信されるラベルと一致する`duleLabels`配列が含まれます。データ使用ラベルに対して指定したマーケティングアクションを実行すると、ポリシーに違反する場合、`violatedPolicies`配列には影響を受けるポリシー（複数可）の詳細が含まれます。ポリシーに違反しない場合は、`violatedPolicies` 配列は空（`[]`）です。

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

## データセットとマーケティングアクションを使用したポリシーの評価

また、データ使用ラベルを収集できる 1 つ以上のデータセットの ID を指定することで、ポリシー違反を評価することもできます。これは、以下に示すように、マーケティングアクションのコアエンドポイントまたはカスタム `/constraints` エンドポイントに対して POST リクエストを実行し、リクエスト本文内でデータセット ID を指定することで行われます。

**API 形式**

```http
POST /marketingActions/core/{marketingActionName}/constraints
POST /marketingActions/custom/{marketingActionName}/constraints
```

**リクエスト**

リクエスト本文には、各データセット ID のオブジェクトを持つ配列が含まれています。リクエストの本文を送信するので、「Content-Type:application/json」リクエストヘッダーは必須です。次に例を示します。

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

応答オブジェクトには、指定したデータセット内のすべてのラベルの統合リストを含む`duleLabels`配列が含まれます。このリストには、データセット内のすべてのフィールドにデータセットレベルおよびフィールドレベルのラベルが含まれます。

この応答には、各データセットのオブジェクトを含む`discoveredLabels`配列も含まれ、`datasetLabels`がデータセットレベルおよびフィールドレベルのラベルに分類されて表示されます。各フィールドレベルのラベルには、そのラベルを持つ特定のフィールドへのパスが表示されます。

指定したマーケティングアクションがデータセット内の`duleLabels`のポリシーに違反する場合、影響を受けるポリシー（複数可）の詳細が`violatedPolicies`配列に含まれます。ポリシーに違反しない場合は、`violatedPolicies` 配列は空（`[]`）です。

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

## データセット、フィールド、およびマーケティングアクションを使用したポリシーの評価

データセット ID を 1 つ以上指定する以外に、各データセット内のフィールドのサブセットも指定できます。これは、これらのフィールドのデータ使用ラベルのみが評価されることを示します。データセットのみを含む POST リクエストと同様に、このリクエストは各データセットの特定のフィールドをリクエスト本文に追加します。

データセットフィールドを使用してポリシーを評価する場合は、次の点に注意してください。

* **フィールド名では大文字と小文字が区別されます。**&#x200B;フィールドを指定する場合は、データセットに表示されるとおりに記述する必要があります（例：`firstName` vs `firstname`）。
* **データセットラベルの継承。**&#x200B;データ使用ラベルは複数のレベルで適用でき、下方向に継承されます。ポリシーの評価が期待どおりの結果を返さない場合は、フィールドレベルで適用されるラベルに加えて、データセットからフィールドに継承されるラベルを確認してください。

**API 形式**

```http
POST /marketingActions/core/{marketingActionName}/constraints
POST /marketingActions/custom/{marketingActionName}/constraints
```

**リクエスト**

リクエスト本文には、各データセット ID オブジェクトと、評価に使用するデータセット内のフィールドのサブセットを含む配列が含まれています。リクエストの本文を送信するので、「Content-Type:application/json」リクエストヘッダーは必須です。次に例を示します。

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

応答オブジェクトには、指定したフィールドで見つかったラベルの統合リストを含む`duleLabels`配列が含まれます。データセットラベルも含まれ、フィールドに継承されます。

指定されたフィールドのデータに対して指定されたマーケティングアクションを実行することでポリシーに違反した場合、影響を受けるポリシー（複数可）の詳細が`violatedPolicies`配列に含まれます。ポリシーに違反しない場合は、`violatedPolicies` 配列は空（`[]`）です。

以下の応答では、`duleLabels`のリストが現在は短くなっています。リクエスト本文に指定されたフィールドのみが含まれるので、これは、各データセットの`discoveredLabels`と同様です。また、以前に違反したポリシー「広告またはコンテンツのターゲット設定」には両方の`C4 AND C6`ラベルが必要なので、違反はなくなり、`violatedPolicies`配列は空になって表示されます。

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

## ポリシー評価 [!DNL Real-time Customer Profile]

The [!DNL Policy Service] API can also be used to check for policy violations involving the use of [!DNL Real-time Customer Profile] segments. 詳しくは、[オーディエンスセグメントのデータ使用に対する準拠の適用](../../segmentation/tutorials/governance.md)に関するチュートリアルを参照してください。