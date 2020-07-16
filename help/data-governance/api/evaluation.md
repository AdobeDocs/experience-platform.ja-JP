---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ポリシー
topic: developer guide
translation-type: tm+mt
source-git-commit: 0534fe8dcc11741ddc74749d231e732163adf5b0
workflow-type: tm+mt
source-wordcount: '938'
ht-degree: 2%

---


# ポリシー評価

マーケティングアクションが作成され、ポリシーが定義されたら、 [!DNL Policy Service] APIを使用して、特定のアクションによってポリシーが違反されたかどうかを評価できます。 返される制約は、データ使用ラベルを含む指定されたデータに対してマーケティングアクションを試みることで違反する一連のポリシーの形式をとります。

デフォルトでは、ステータスが「有効」に設定されているポリシーの **みが評価に参加します**。ただし、クエリパラメーターを使用して、評価に「ドラフト」ポリシー `?includeDraft=true` を含めることができます。

評価のリクエストは、次の3つの方法のいずれかで行うことができます。

1. 一連のデータ使用ラベルとマーケティングアクションが指定されている場合、そのアクションはポリシーに違反していますか。
1. 1つ以上のデータセットとマーケティングアクションを指定した場合、そのアクションはポリシーに違反していますか。
1. 1つ以上のデータセットと、それらの各データセット内の1つ以上のフィールドのサブセットを指定した場合、アクションはポリシーに違反しますか。

## データ使用ラベルとマーケティングアクションを使用してポリシーを評価

データ使用ラベルの存在に基づいてポリシー違反を評価するには、要求中にデータに存在するラベルのセットを指定する必要があります。 これは、次の例に示すように、クエリパラメーターを使用して行います。ここでは、データ使用ラベルは、値のカンマ区切りリストとして提供されます。

**API形式**

```http
GET /marketingActions/core/{marketingActionName}/constraints?duleLabels={value1},{value2}
GET /marketingActions/custom/{marketingActionName}/constraints?duleLabels={value1},{value2}
```

**リクエスト**

次の例のリクエストは、ラベルC1およびC3に対するマーケティングアクションを評価します。 データ使用ラベルを使用してポリシーを評価する場合は、次の点に注意してください。
* **データ使用ラベルでは大文字と小文字が区別されます。** 上に示したリクエストは違反ポリシーを返しますが、同じリクエストを小文字のラベル( `"c1,c3"`、 `"C1,c3"`、 `"c1,C3"`)は含まれません。
* **ポリシー式ーの`AND`および`OR`演算子に注意してください。** この例では、リクエスト内でラベル(`C1` または `C3`)のいずれかが単独で表示された場合、マーケティングアクションはこのポリシーに違反していません。 両方のラベル(`C1 AND C3`)を使用して、違反ポリシーを返します。 ポリシーを慎重に評価し、ポリシー式を同じ注意を払って定義する必要があります。

```SHELL
curl -X GET \
  'https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/sampleMarketingAction/constraints?duleLabels=C1,C3' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答オブジェクトには、要求で送信されるラベルと一致する `duleLabels` 配列が含まれます。 データ使用ラベルに対して指定したマーケティング操作を実行するとポリシーに違反する場合、 `violatedPolicies` アレイには影響を受けるポリシー（またはポリシー）の詳細が含まれます。 ポリシーに違反しない場合、 `violatedPolicies` アレイは空(`[]`)で表示されます。

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

また、データ使用ラベルを収集できる1つ以上のデータセットのIDを指定することで、ポリシー違反を評価することもできます。 これを行うには、以下に示すように、マーケティングアクションのコアエンドポイントまたはカスタム `/constraints` エンドポイントに対してPOSTリクエストを実行し、リクエスト本文内でデータセットIDを指定します。

**API形式**

```http
POST /marketingActions/core/{marketingActionName}/constraints
POST /marketingActions/custom/{marketingActionName}/constraints
```

**リクエスト**

リクエスト本文には、各データセットIDに対して1つのオブジェクトを持つ配列が含まれています。 リクエストの本文を送信しているので、「Content-Type: application/json&quot;リクエストヘッダーは必須です。以下に例を示します。

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

応答オブジェクトには、指定したデータセット内のすべてのラベルの統合リストを含む `duleLabels` 配列が含まれます。 このリストには、データセット内のすべてのフィールドにデータセットレベルおよびフィールドレベルのラベルが含まれます。

また、この応答には各データセットのオブジェクトが含まれた `discoveredLabels` 配列も含まれ、データセットとフィールドレベルのラベルに `datasetLabels` 分類されて表示されます。 各フィールドレベルのラベルには、そのラベルを持つ特定のフィールドへのパスが表示されます。

指定したマーケティング操作がデータセット `duleLabels` 内のポリシーに違反する場合、 `violatedPolicies` アレイには影響を受けるポリシー（またはポリシー）の詳細が含まれます。 ポリシーに違反しない場合、 `violatedPolicies` アレイは空(`[]`)で表示されます。

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

データセットIDを1つ以上指定するだけでなく、各データセット内のフィールドのサブセットも指定できます。これは、これらのフィールドのデータ使用ラベルのみが評価されることを示しています。 データセットのみを含むPOSTリクエストと同様に、このリクエストは各データセットの特定のフィールドをリクエスト本文に追加します。

データセットフィールドを使用してポリシーを評価する場合は、次の点に注意してください。

* **フィールド名では大文字と小文字が区別されます。** フィールドを指定する場合は、データセットに表示されるとおりに記述する必要があります(例： `firstName` vs `firstname`)。
* **データセットラベルの継承。** データ使用ラベルは複数のレベルで適用でき、下方向に継承されます。 ポリシーの評価が予想通りに返されない場合は、フィールドレベルで適用されるラベルに加えて、データセットからフィールドに継承されたラベルも必ず確認してください。

**API形式**

```http
POST /marketingActions/core/{marketingActionName}/constraints
POST /marketingActions/custom/{marketingActionName}/constraints
```

**リクエスト**

リクエスト本文には、各データセットIDのオブジェクトと、評価に使用する必要のあるそのデータセット内のフィールドのサブセットを含む配列が含まれています。 リクエストの本文を送信しているので、「Content-Type: application/json&quot;リクエストヘッダーは必須です。以下に例を示します。

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

応答オブジェクトには、指定したフィールドで見つかったラベルの統合リストを含む `duleLabels` 配列が含まれます。 データセットラベルも含まれ、フィールドに継承されます。

指定されたフィールドのデータに対して指定されたマーケティングアクションを実行することでポリシーに違反した場合、 `violatedPolicies` 配列には影響を受けたポリシー（またはポリシー）の詳細が含まれます。 ポリシーに違反しない場合、 `violatedPolicies` アレイは空(`[]`)で表示されます。

以下の応答では、のリストが短くなりました。これ `duleLabels` は、各データセットにリクエスト本文で指定されたフィールドのみが含まれ `discoveredLabels` るので、各データセットのと同様です。 また、以前に違反したポリシー「広告またはコンテンツのターゲット設定」には、両方の `C4 AND C6` ラベルが必要なので、違反がなくなり、配 `violatedPolicies` 列が空になります。

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

また、 [!DNL Policy Service] APIを使用して、セグメントの使用に関連するポリシー違反の有無を確認することもでき [!DNL Real-time Customer Profile] ます。 詳しくは、[オーディエンスセグメントのデータ使用に対する準拠の適用](../../segmentation/tutorials/governance.md)に関するチュートリアルを参照してください。