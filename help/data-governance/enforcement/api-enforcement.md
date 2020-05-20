---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Policy Service APIを使用してデータ使用ポリシーを適用する
topic: enforcement
translation-type: tm+mt
source-git-commit: 3e5245a718295cc5318c277a5cf9ee71da2a911b
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 3%

---


# Policy Service APIを使用してデータ使用ポリシーを適用する

データのデータ使用ラベルを作成し、それらのラベルに対するマーケティングアクションの使用ポリシーを作成したら、 [DULE Policy Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml) (DOCULE Policy Service API)を使用して、マーケティングアクションがポリシー違反かどうかを評価できます。 その後、独自の内部プロトコルを設定し、API応答に基づくポリシー違反を処理できます。

>[!NOTE] デフォルトでは、ステータスがに設定されているポリシーのみが評価に参加 `ENABLED` できます。 ポリシーが評価に参加できるようにするには、リクエストパス `DRAFT``includeDraft=true` にクエリパラメーターを含める必要があります。

このドキュメントでは、Policy Service APIを使用して様々なシナリオでのポリシー違反を確認する手順を説明します。

## はじめに

このチュートリアルでは、DULEポリシーの適用に関連する次の主要概念を十分に理解している必要があります。

* [Data Governance](../home.md): プラットフォームがデータ使用のコンプライアンスを強制するフレームワーク。
   * [データ使用ラベル](../labels/overview.md): データ使用ラベルは、データセット（および/またはそのデータセット内の個々のフィールド）に適用され、そのデータの使用方法に関する制限を指定します。
   * [データ使用ポリシー](../policies/overview.md): データ使用ポリシーは、特定のDULEラベルのセットに対して許可または制限されるマーケティングアクションの種類を記述するルールです。
* [サンドボックス](../../sandboxes/home.md): Experience Platformは、1つのプラットフォームインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

このチュートリアルを開始する前に、 [](../api/getting-started.md) 開発者ガイドを参照して、必要なヘッダーやAPI呼び出し例の読み取り方法など、DULE Policy Service APIの呼び出しを正しく行うために必要な重要な情報を確認してください。

## DULEラベルとマーケティングアクションを使用して評価

データセット内に仮定的に存在するDULEラベルのセットに対してマーケティングアクションをテストすることで、ポリシーを評価できます。 これは、次の例に示すように、DULEラベルが値のカンマ区切りリストとして提供される `duleLabels` クエリパラメータを使用して行います。

**API形式**

```http
GET /marketingActions/core/{MARKETING_ACTION_NAME}/constraints?duleLabels={LABEL_1},{LABEL_2}
GET /marketingActions/custom/{MARKETING_ACTION_NAME}/constraints?duleLabels={LABEL_1},{LABEL_2}
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | 評価するDULEポリシーに関連付けられているマーケティングアクションの名前です。 |
| `{LABEL_1}` | マーケティングアクションをテストするためのデータ使用ラベル。 少なくとも1つのラベルを入力する必要があります。 複数のラベルを指定する場合は、コンマで区切る必要があります。 |

**リクエスト**

次のリクエストは、ラベル `exportToThirdParty` と `C1``C3`マーケティングアクションに対してテストします。 このチュートリアルで前に作成したデータ使用ポリシーでは、 `C1` ラベルがそのポリシー式の `deny` 条件の1つとして定義されているので、マーケティングアクションはポリシー違反をトリガーする必要があります。

>[!NOTE] データ使用量のラベルでは、大文字と小文字が区別されます。 ポリシー違反は、そのポリシー式に定義されたラベルが正確に一致する場合にのみ発生します。 この例では、 `C1` ラベルは違反をトリガーしますが、 `c1` ラベルは違反をトリガーしません。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/exportToThirdParty/constraints?duleLabels=C1,C3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、マーケティングアクションのURL、テスト対象のDULEラベル、およびこれらのラベルに対するアクションのテストの結果として違反されたDULEポリシーのリストを返します。 この例では、「データをサードパーティにエクスポート」ポリシーが `violatedPolicies` 配列に表示され、マーケティングアクションが予期されたポリシー違反をトリガーしたことを示しています。

```json
{
    "timestamp": 1565727821209,
    "clientId": "string",
    "userId": "string",
    "imsOrg": "{IMS_ORG}",
    "marketingActionRef": "https://platform-stage.adobe.io:443/data/foundation/dulepolicy/marketingActions/custom/exportToThirdParty",
    "duleLabels": [
        "C1",
        "C3"
    ],
    "violatedPolicies": [
        {
            "name": "Export Data to Third Party",
            "status": "ENABLED",
            "marketingActionRefs": [
                "https://platform-stage.adobe.io:443/data/foundation/dulepolicy/marketingActions/custom/exportToThirdParty"
            ],
            "description": "Conditions under which data cannot be exported to a third party",
            "deny": {
                "operator": "OR",
                "operands": [
                    {
                        "label": "C1"
                    },
                    {
                        "operator": "AND",
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
            "created": 1565651746693,
            "createdClient": "{CREATED_CLIENT}",
            "createdUser": "{CREATED_USER",
            "updated": 1565723012139,
            "updatedClient": "{UPDATED_CLIENT}",
            "updatedUser": "{UPDATED_USER}",
            "_links": {
                "self": {
                    "href": "https://platform-stage.adobe.io/data/foundation/dulepolicy/policies/custom/5d51f322e553c814e67af1a3"
                }
            },
            "id": "5d51f322e553c814e67af1a3"
        }
    ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `violatedPolicies` | 提供された製品に対して（で指定された）マーケティングアクションをテストすることで違反したDULEポリシーをリストした配列 `marketingActionRef``duleLabels`。 |

## データセットを使用した評価

DULEポリシーを評価するには、DULEラベルを収集できる1つ以上のデータセットに対してマーケティングアクションをテストします。 これは、以下の例に示すように、リクエスト本文内にPOSTリクエストを作成 `/marketingActions/core/{MARKETING_ACTION_NAME}/constraints` し、データセットIDを提供することで行われます。

**API形式**

```http
POST /marketingActions/core/{MARKETING_ACTION_NAME}/constraints
POST /marketingActions/custom/{MARKETING_ACTION_NAME}/constraints
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | 評価するDULEポリシーに関連付けられているマーケティングアクションの名前です。 |

**リクエスト**

次のリクエストは、3つの異なるデータセットに対して `exportToThirdParty` マーケティングアクションをテストします。 データセットは、ペイロードで提供された配列の型とIDで参照されます。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/exportToThirdParty/constraints
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
| `entityType` | ペイロード配列の各項目は、定義するエンティティのタイプを示す必要があります。 この使用例では、値は常に「dataSet」になります。 |
| `entityId` | ペイロード配列の各項目は、データセットの一意のIDを提供する必要があります。 |

**応答**

「正常な応答」は、マーケティングアクションのURL、指定されたデータセットから収集されたDULEラベル、およびこれらのラベルに対するアクションのテストの結果として違反されたDULEポリシーのリストを返します。 この例では、「データをサードパーティにエクスポート」ポリシーが `violatedPolicies` 配列に表示され、マーケティングアクションが予期されたポリシー違反をトリガーしたことを示しています。

```json
{
    "timestamp": 1556324277895,
    "clientId": "{CLIENT_ID}",
    "userId": "{USER_ID}",
    "imsOrg": "{IMS_ORG}",
    "marketingActionRef": "https://platform.adobe.io:443/data/foundation/dulepolicy/marketingActions/custom/exportToThirdParty",
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
            "name": "Export Data to Third Party",
            "status": "ENABLED",
            "marketingActionRefs": [
                "https://platform-stage.adobe.io:443/data/foundation/dulepolicy/marketingActions/custom/exportToThirdParty"
            ],
            "description": "Conditions under which data cannot be exported to a third party",
            "deny": {
                "operator": "OR",
                "operands": [
                    {
                        "label": "C1"
                    },
                    {
                        "operator": "AND",
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
            "created": 1565651746693,
            "createdClient": "{CREATED_CLIENT}",
            "createdUser": "{CREATED_USER",
            "updated": 1565723012139,
            "updatedClient": "{UPDATED_CLIENT}",
            "updatedUser": "{UPDATED_USER}",
            "_links": {
                "self": {
                    "href": "https://platform-stage.adobe.io/data/foundation/dulepolicy/policies/custom/5d51f322e553c814e67af1a3"
                }
            },
            "id": "5d51f322e553c814e67af1a3"
        }
    ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `duleLabels` | 要求ペイロードで提供されたデータセットから抽出されたDULEラベルのリスト。 |
| `discoveredLabels` | リクエストペイロードで提供されたデータセットのリスト。各ペイロードで見つかったデータセットレベルおよびフィールドレベルのDULEラベルが表示されます。 |
| `violatedPolicies` | 提供された製品に対して（で指定された）マーケティングアクションをテストすることで違反したDULEポリシーをリストした配列 `marketingActionRef``duleLabels`。 |

## 次の手順

このドキュメントを読むと、データセットまたは一連のDULEラベルに対するマーケティングアクションを実行する際のポリシー違反の確認が成功します。 API応答で返されるデータを使用して、エクスペリエンスアプリケーション内でプロトコルを設定し、ポリシー違反が発生した場合に適切にポリシー違反を適用することができます。

リアルタイム顧客プロファイルでオーディエンスセグメントに対してデータ使用ポリシーを適用する手順については、次の [チュートリアルを参照してください](../../segmentation/tutorials/governance.md)。