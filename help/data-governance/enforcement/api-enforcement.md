---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Policy Service APIを使用してデータ使用ポリシーを適用する
topic: enforcement
translation-type: tm+mt
source-git-commit: 3e5245a718295cc5318c277a5cf9ee71da2a911b

---


# Policy Service APIを使用してデータ使用ポリシーを適用する

データのデータ使用ラベルを作成し、これらのラベルに対するマーケティングアクションの使用ポリシーを作成したら、 [DULE Policy Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml) (DOCULE Policy Service API)を使用して、マーケティングアクションがポリシー違反かどうかを評価できます。 その後、API応答に基づいてポリシー違反を処理する独自の内部プロトコルを設定できます。

>[!NOTE] デフォルトでは、ステータスがに設定されたポリシーのみが `ENABLED` 評価に参加できます。 ポリシーの評 `DRAFT` 価への参加を許可するには、リクエストパスにクエリパラメー `includeDraft=true` ターを含める必要があります。

このドキュメントでは、Policy Service APIを使用して、異なるシナリオでのポリシー違反を確認する手順を説明します。

## はじめに

このチュートリアルでは、DULEポリシーの適用に関わる次の主要概念について、十分に理解しておく必要があります。

* [データガバナンス](../home.md):プラットフォームがデータ使用のコンプライアンスを強制するフレームワーク。
   * [データ使用ラベル](../labels/overview.md):データ使用ラベルは、データセット（およびデータセット内の個々のフィールド）に適用され、そのデータの使用方法に関する制限を指定します。
   * [データ使用ポリシー](../policies/overview.md):データ使用ポリシーは、特定のDULEラベルのセットに対して許可または制限されるマーケティングアクションの種類を記述するルールです。
* [サンドボックス](../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

このチュートリアルを開始する前に、開発者ガイドを参照し [て](../api/getting-started.md) 、必要なヘッダーやサンプルAPI呼び出しを含む、DULE Policy Service APIの呼び出しを正しく行うために知っておく必要のある重要な情報を確認してください。

## DULEラベルとマーケティングアクションを使用して評価

データセット内に仮定的に存在するDULEラベルのセットに対してマーケティングアクションをテストすることで、ポリシーを評価できます。 これは、次の例に示すように、 `duleLabels` クエリパラメータを使用して行います。DULEラベルは、値のコンマ区切りリストとして提供されます。

**API形式**

```http
GET /marketingActions/core/{MARKETING_ACTION_NAME}/constraints?duleLabels={LABEL_1},{LABEL_2}
GET /marketingActions/custom/{MARKETING_ACTION_NAME}/constraints?duleLabels={LABEL_1},{LABEL_2}
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | 評価するDULEポリシーに関連付けられたマーケティングアクションの名前。 |
| `{LABEL_1}` | マーケティングアクションをテストするデータ使用ラベル。 少なくとも1つのラベルを指定する必要があります。 複数のラベルを指定する場合は、コンマで区切る必要があります。 |

**リクエスト**

次のリクエストは、マーケティングアクシ `exportToThirdParty` ョンをラベルおよびに対してテ `C1` ストしま `C3`す。 このチュートリアルで先ほど作成したデータ使用ポリシーでは、ラベルがポリシー式の条件の1つとして定義され `C1``deny` ているので、マーケティングアクションはポリシー違反をトリガーする必要があります。

>[!NOTE] データ使用ラベルでは大文字と小文字が区別されます。 ポリシー違反は、ポリシー違反で定義されたラベルが正確に一致する場合にのみ式されます。 この例では、ラベルは違 `C1` 反をトリガーしますが、ラベルは違反をト `c1` リガーしません。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/exportToThirdParty/constraints?duleLabels=C1,C3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、マーケティングアクションのURL、テスト対象のDULEラベル、およびこれらのラベルに対するアクションのテストの結果として違反されたDULEポリシーのリストを返します。 この例では、「データをサードパーティにエクスポート」ポリシーが配列に表示され、マーケティングアクションによっ `violatedPolicies` て予期されるポリシー違反がトリガーされたことが示されています。

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
| `violatedPolicies` | 提供されたに対する（で指定された）マーケティングアクションのテストに違反したDULEポリシーを `marketingActionRef`リストする配列で `duleLabels`す。 |

## データセットを使用して評価

DULEポリシーを評価するには、DULEラベルを収集できる1つ以上のデータセットに対してマーケティングアクションをテストします。 これは、次の例に示すように、リクエスト本 `/marketingActions/core/{MARKETING_ACTION_NAME}/constraints` 文内にPOSTリクエストを作成し、データセットIDを提供することで行われます。

**API形式**

```http
POST /marketingActions/core/{MARKETING_ACTION_NAME}/constraints
POST /marketingActions/custom/{MARKETING_ACTION_NAME}/constraints
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | 評価するDULEポリシーに関連付けられたマーケティングアクションの名前。 |

**リクエスト**

次のリクエストは、3つの異なるデータセ `exportToThirdParty` ットに対するマーケティングアクションをテストします。 データセットは、ペイロードで提供される配列の型とIDで参照されます。

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

成功した応答は、マーケティングアクションのURL、提供されたデータセットから収集されたDULEラベル、およびこれらのラベルに対するアクションのテストの結果として違反されたDULEポリシーのリストを返します。 この例では、「データをサードパーティにエクスポート」ポリシーが配列に表示され、マーケティングアクションによっ `violatedPolicies` て予期されるポリシー違反がトリガーされたことが示されています。

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
| `duleLabels` | 要求リストで指定されたデータセットから抽出されたDULEラベルのペイロード。 |
| `discoveredLabels` | リクエストペイロードで提供されたデータセットのリスト。各ペイロードで見つかったデータセットレベルおよびフィールドレベルのDULEラベルを表示します。 |
| `violatedPolicies` | 提供されたに対する（で指定された）マーケティングアクションのテストに違反したDULEポリシーを `marketingActionRef`リストする配列で `duleLabels`す。 |

## 次の手順

このドキュメントを読むと、データセットまたは一連のDULEラベルに対してマーケティングアクションを実行する際に、ポリシー違反の有無が正しく確認されます。 API応答で返されたデータを使用して、エクスペリエンスアプリケーション内でプロトコルを設定し、ポリシー違反が発生した場合に適切にポリシー違反を適用することができます。

リアルタイム顧客プロファイルでオーディエンスセグメントのデータ使用ポリシーを適用する手順については、次のチュートリアルを参照してく [ださい](../../segmentation/tutorials/governance.md)。