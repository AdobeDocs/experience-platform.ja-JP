---
keywords: Experience Platform；ホーム；人気の高いトピック；ポリシーの適用；自動適用；APIベースの適用；データガバナンス
solution: Experience Platform
title: ポリシー評価APIエンドポイント
topic-legacy: developer guide
description: マーケティングアクションが作成され、ポリシーが定義されたら、Policy Service API を使用して、特定のアクションによってポリシーが違反したかどうかを評価できます。返される制約は、データ使用ラベルを含む指定されたデータに対してマーケティングアクションを試みることで違反するポリシーのセットの形をとります。
exl-id: f9903939-268b-492c-aca7-63200bfe4179
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1544'
ht-degree: 17%

---

# ポリシー評価エンドポイント

マーケティングアクションが作成され、ポリシーが定義されたら、[!DNL Policy Service] APIを使用して、特定のアクションによってポリシーが違反されたかどうかを評価できます。 返される制約は、データ使用ラベルを含む指定されたデータに対してマーケティングアクションを試みることで違反するポリシーのセットの形をとります。

デフォルトでは、`ENABLED`に設定されたステータスのポリシーのみが評価に参加します。 ただし、クエリパラメーター`?includeDraft=true`を使用して、評価に`DRAFT`ポリシーを含めることはできます。

評価のリクエストは、次の 3 つの方法のいずれかでおこなうことができます。

1. マーケティングアクションとデータ使用ラベルのセットが指定されている場合、そのアクションはポリシーに違反していますか。
1. マーケティングアクションと1つ以上のデータセットが指定された場合、そのアクションはポリシーに違反していますか。
1. マーケティングアクション、1つ以上のデータセット、およびそれらの各データセット内の1つ以上のフィールドのサブセットを指定した場合、アクションはポリシーに違反しますか。

## はじめに

このガイドで使用されるAPIエンドポイントは、[[!DNL Policy Service] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml)の一部です。 先に進む前に、[はじめにガイド](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプルAPI呼び出しの読み方、および任意の[!DNL Experience Platform] APIの呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## データ使用ラベル{#labels}を使用してポリシー違反を評価

GET要求で`duleLabels`クエリパラメーターを使用すると、特定のデータ使用ラベルのセットの存在に基づいてポリシー違反を評価できます。

**API 形式**

```http
GET /marketingActions/core/{MARKETING_ACTION_NAME}/constraints?duleLabels={LABELS_LIST}
GET /marketingActions/custom/{MARKETING_ACTION_NAME}/constraints?duleLabels={LABELS_LIST}
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | 一連のデータ使用ラベルに対してテストするマーケティングアクションの名前。 マーケティングアクションのエンドポイント](./marketing-actions.md#list)に[GETリクエストを行うと、使用可能なマーケティングアクションのリストを取得できます。 |
| `{LABELS_LIST}` | マーケティングアクションをテストするためのデータ使用ラベル名のコンマ区切りリスト。 次に例を示します。`duleLabels=C1,C2,C3`<br><br>ラベル名では大文字と小文字が区別されます。 `duleLabels`パラメーターにリストする場合は、大文字と小文字を正しく指定していることを確認してください。 |

**リクエスト**

次の例のリクエストは、ラベル C1 および C3 に対するマーケティングアクションを評価します。

>[!IMPORTANT]
>
>ポリシーでの表現における`AND`および`OR`演算子に注意してください。以下の例では、リクエスト内でラベル（`C1`または`C3`）が単独で表示されていた場合、マーケティングアクションはこのポリシーに違反していません。 違反ポリシーを返すには、ラベル（`C1`と`C3`）の両方を使用します。 ポリシーを慎重に評価し、ポリシー式を十分に定義します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/sampleMarketingAction/constraints?duleLabels=C1,C3' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答には`violatedPolicies`配列が含まれます。この配列には、指定されたラベルに対するマーケティングアクションの実行の結果として違反されたポリシーの詳細が含まれます。 ポリシーに違反しない場合は、`violatedPolicies`配列は空になります。

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

## データセット{#datasets}を使用してポリシー違反を評価

データ使用ラベルを収集できる1つ以上のデータセットのセットに基づいて、ポリシー違反を評価できます。 これは、特定のマーケティングアクションに対して`/constraints`エンドポイントにPOSTリクエストを実行し、リクエスト本文内のデータセットIDのリストを提供することで行います。

**API 形式**

```http
POST /marketingActions/core/{MARKETING_ACTION_NAME}/constraints
POST /marketingActions/custom/{MARKETING_ACTION_NAME}/constraints
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | 1つ以上のデータセットに対してテストするマーケティングアクションの名前。 マーケティングアクションのエンドポイント](./marketing-actions.md#list)に[GETリクエストを行うと、使用可能なマーケティングアクションのリストを取得できます。 |

**リクエスト**

次のリクエストは、3つのデータセットのセットに対して`crossSiteTargeting`マーケティングアクションを実行し、ポリシー違反の評価を行います。

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
| `entityType` | 兄弟`entityId`プロパティでIDが示されるエンティティのタイプ。 現在、`dataSet`の値のみが許可されています。 |
| `entityId` | マーケティングアクションをテストするデータセットのID。 データセットとそれに対応するIDのリストは、[!DNL Catalog Service] APIの`/dataSets`エンドポイントにGETリクエストを行うことで取得できます。 詳しくは、[ [!DNL Catalog] オブジェクト](../../catalog/api/list-objects.md)のリストを参照してください。 |

**応答**

成功した応答には`violatedPolicies`配列が含まれます。この配列には、指定されたデータセットに対するマーケティングアクションの実行の結果として違反されたポリシーの詳細が含まれます。 ポリシーに違反しない場合は、`violatedPolicies`配列は空になります。

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

## 特定のデータセットフィールド{#fields}を使用してポリシー違反を評価します。

1つ以上のデータセット内のフィールドのサブセットに基づいてポリシー違反を評価できるので、これらのフィールドに適用されたデータ使用ラベルのみが評価されます。

データセットフィールドを使用してポリシーを評価する場合は、次の点に注意してください。

* **フィールド名では大文字と小文字が区別されます**。フィールドを指定する場合は、データセットに表示されるとおりに記述する必要があります( `firstName` vsなど `firstname`)。
* **データセットラベルの継承**:データセット内の個々のフィールドは、データセットレベルで適用されたラベルを継承します。ポリシーの評価が期待どおりに返されない場合は、フィールドレベルで適用されるラベルに加え、データセットレベルからフィールドに継承されたラベルがないかどうかを確認してください。

**API 形式**

```http
POST /marketingActions/core/{MARKETING_ACTION_NAME}/constraints
POST /marketingActions/custom/{MARKETING_ACTION_NAME}/constraints
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | データセットフィールドのサブセットに対してテストするマーケティングアクションの名前。 マーケティングアクションのエンドポイント](./marketing-actions.md#list)に[GETリクエストを行うと、使用可能なマーケティングアクションのリストを取得できます。 |

**リクエスト**

次のリクエストは、3つのデータセットに属する特定のフィールドセットに対してマーケティングアクション`crossSiteTargeting`をテストします。 ペイロードは、データセット](#datasets)のみを含む[評価リクエストに似ており、ラベルを収集する各データセットに対して特定のフィールドを追加します。

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
| `entityType` | 兄弟`entityId`プロパティでIDが示されるエンティティのタイプ。 現在、`dataSet`の値のみが許可されています。 |
| `entityId` | マーケティングアクションに対してフィールドが評価されるデータセットのID。 データセットとそれに対応するIDのリストは、[!DNL Catalog Service] APIの`/dataSets`エンドポイントにGETリクエストを行うことで取得できます。 詳しくは、[ [!DNL Catalog] オブジェクト](../../catalog/api/list-objects.md)のリストを参照してください。 |
| `entityMeta.fields` | データセットのスキーマ内の特定のフィールドへのパスの配列。JSONポインター文字列の形式で提供されます。 これらの文字列に許可された構文の詳細については、APIの基本原則ガイドの[JSONポインタ](../../landing/api-fundamentals.md#json-pointer)の節を参照してください。 |

**応答**

成功した応答には`violatedPolicies`配列が含まれます。この配列には、指定されたデータセットフィールドに対するマーケティングアクションの実行結果として違反されたポリシーの詳細が含まれます。 ポリシーに違反しない場合は、`violatedPolicies`配列は空になります。

以下の例の応答と、データセット](#datasets)のみを含む[応答とを比較すると、収集されたラベルのリストが短いことに注意してください。 また、各データセットの`discoveredLabels`も、リクエスト本文で指定されたフィールドのみが含まれるので、減らされています。 また、以前に違反したポリシー`Targeting Ads or Content`には、両方の`C4 AND C6`ラベルが必要なので、空の`violatedPolicies`配列で示されるように違反することはありません。

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

## ポリシーを一括で評価{#bulk}

`/bulk-eval`エンドポイントを使用すると、1回のAPI呼び出しで複数の評価ジョブを実行できます。

**API 形式**

```http
POST /bulk-eval
```

**リクエスト**

一括評価要求のペイロードは、オブジェクトの配列でなければなりません。実行する評価ジョブごとに1つずつ。 データセットとフィールドに基づいて評価されるジョブの場合は、`entityList`配列を指定する必要があります。 データ使用ラベルに基づいて評価されるジョブの場合は、`labels`配列を指定する必要があります。

>[!WARNING]
>
>リストにある評価ジョブに`entityList`と`labels`の両方の配列が含まれている場合、エラーが発生します。 データセットとラベルの両方に基づいて同じマーケティングアクションを評価する場合は、そのマーケティングアクションに対して別々の評価ジョブを含める必要があります。

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
| `evalRef` | ポリシー違反のラベルまたはデータセットに対してテストするマーケティングアクションのURIです。 |
| `includeDraft` | デフォルトでは、有効なポリシーのみが評価に参加します。 `includeDraft`を`true`に設定すると、`DRAFT`ステータスのポリシーも参加します。 |
| `labels` | マーケティングアクションをテストするための一連のデータ使用ラベル。<br><br>**重要**:このプロパティを使用する場合、 `entityList` プロパティを同じオブジェクトに含めないでください。データセットやフィールドを使用して同じマーケティングアクションを評価するには、`entityList`配列を含むリクエストペイロードに別のオブジェクトを含める必要があります。 |
| `entityList` | 一連のデータセットと、それらのデータセット内の（オプションで）特定のフィールドに対して、マーケティングアクションをテストします。<br><br>**重要**:このプロパティを使用する場合、 `labels` プロパティを同じオブジェクトに含めないでください。特定のデータ使用ラベルを使用して同じマーケティングアクションを評価するには、`labels`配列を含むリクエストペイロードに別のオブジェクトを含める必要があります。 |
| `entityType` | マーケティングアクションをテストするエンティティのタイプ。 現在は、`dataSet` のみがサポートされています。 |
| `entityId` | マーケティングアクションをテストするデータセットのID。 |
| `entityMeta.fields` | （オプション）マーケティングアクションをテストするデータセット内の特定のフィールドのリスト。 |

**応答**

成功した応答は、評価結果の配列を返します。リクエストで送信されたポリシー評価ジョブごとに1つ。

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

## [!DNL Real-time Customer Profile]のポリシー評価

[!DNL Policy Service] APIは、[!DNL Real-time Customer Profile]セグメントの使用に関するポリシー違反の有無を確認する場合にも使用できます。 詳しくは、[オーディエンスセグメントのデータ使用に対する準拠の適用](../../segmentation/tutorials/governance.md)に関するチュートリアルを参照してください。
