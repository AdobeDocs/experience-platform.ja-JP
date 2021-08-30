---
keywords: Experience Platform;ホーム;人気のトピック;ポリシーの適用;自動適用;API ベースの適用;データガバナンス;テスト
solution: Experience Platform
title: Policy Service API を使用してデータ使用ポリシーを適用する
topic-legacy: guide
type: Tutorial
description: データのデータ使用ラベルを作成し、これらのラベルに対するマーケティングアクションの使用ポリシーを作成したら、Policy Service API を使用して、データセットまたはラベルの任意のグループに対して実行されたマーケティングアクションがポリシー違反となるかどうかを評価します。その後、API 応答に基づいてポリシー違反を処理する独自の内部プロトコルを設定できます。
exl-id: 093db807-c49d-4086-a676-1426426b43fd
source-git-commit: 8133804076b1c0adf2eae5b748e86a35f3186d14
workflow-type: tm+mt
source-wordcount: '1002'
ht-degree: 99%

---

# [!DNL Policy Service] API を使用してデータ使用ポリシーを適用する

データのデータ使用ラベルを作成し、これらのラベルに対するマーケティングアクションの使用ポリシーを作成したら、[[!DNL Policy Service API]](https://www.adobe.io/experience-platform-apis/references/policy-service/) を使用して、データセットまたはラベルの任意のグループに対して実行されたマーケティングアクションがポリシー違反となるかどうかを評価します。その後、API 応答に基づいてポリシー違反を処理する独自の内部プロトコルを設定できます。

>[!NOTE]
>
> デフォルトでは、ステータスが `ENABLED` に設定されたポリシーのみが評価に参加できます。`DRAFT` ポリシーの評価への参加を許可するには、リクエストパスに `includeDraft=true` クエリパラメーターを含める必要があります。

このドキュメントでは、[!DNL Policy Service] API を使用して、異なるシナリオでのポリシー違反を確認する手順を説明します。

## はじめに

このチュートリアルでは、データ使用ポリシーの適用に関わる次の主な概念に関する十分な知識が必要です。 

* [データガバナンス](../home.md)：[!DNL Platform] がデータ使用のコンプライアンスを強制するフレームワーク。
   * [データ使用ラベル](../labels/overview.md)：データ使用ラベルは、データセット（や、データセット内の個々のフィールド）に適用され、そのデータの使用方法に関する制限を指定します。
   * [データ使用ポリシー](../policies/overview.md)：データ使用ポリシーは、特定のデータラベルのセットに対して許可または制限されるマーケティングアクションの種類を記述するルールです。
* [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

このチュートリアルを開始する前に、[開発者ガイド](../api/getting-started.md)を参照して、必要なヘッダーやサンプル API 呼び出しを含む、[!DNL Policy Service] API の呼び出しを正しく実行するため、必要な重要な情報を確認してください。

## データラベルとマーケティングアクションを使用した評価

データセット内に存在すると仮定したデータ使用ラベルのセットに対してマーケティングアクションをテストすることで、ポリシーを評価できます。これは、次の例に示すように、`duleLabels` クエリパラメータを使用して行います。ラベルは、値のコンマ区切りリストとして提供されます。

**API 形式**

```http
GET /marketingActions/core/{MARKETING_ACTION_NAME}/constraints?duleLabels={LABEL_1},{LABEL_2}
GET /marketingActions/custom/{MARKETING_ACTION_NAME}/constraints?duleLabels={LABEL_1},{LABEL_2}
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | 評価するデータ使用ポリシーに関連付けられたマーケティングアクションの名前。 |
| `{LABEL_1}` | マーケティングアクションをテストするデータ使用ラベル。少なくとも 1 つのラベルを指定する必要があります。複数のラベルを指定する場合は、コンマで区切る必要があります。 |

**リクエスト**

次のリクエストは、`exportToThirdParty` マーケティングアクションを `C1` ラベルおよび `C3` ラベルに対してテストします。このチュートリアルで先ほど作成したデータ使用ポリシーでは、`C1` ラベルが `deny` ポリシー式の条件の 1 つとして定義されているので、マーケティングアクションはポリシー違反をトリガーする必要があります。

>[!NOTE]
>
> データ使用ラベルでは大文字と小文字が区別されます。ポリシー違反は、ポリシー違反で定義されたラベルが正確に一致する場合にのみ式されます。この例では、`C1` ラベルは違反をトリガーしますが、`c1` ラベルは違反をトリガーしません。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/exportToThirdParty/constraints?duleLabels=C1,C3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、マーケティングアクションの URL、テスト対象の使用ラベル、およびこれらのラベルに対するアクションのテストの結果として違反したポリシーのリストが返されます。この例では、「サードパーティへのデータ書き出し」ポリシーが `violatedPolicies` 配列に表示され、マーケティングアクションによって予期されるポリシー違反がトリガーされたことが示されています。

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
| `violatedPolicies` | 提供された `duleLabels` に対して（`marketingActionRef` で指定された）マーケティングアクションのテストに違反したポリシーをリストする配列です。 |

## データセットを使用した評価

データ使用ポリシーを評価するには、ラベルを収集できる 1 つ以上のデータセットに対してマーケティングアクションをテストします。これは、次の例に示すように、リクエスト本文内にデータセット ID を指定して `/marketingActions/core/{MARKETING_ACTION_NAME}/constraints` に POST リクエストを実行することでおこなわれます。

**API 形式**

```http
POST /marketingActions/core/{MARKETING_ACTION_NAME}/constraints
POST /marketingActions/custom/{MARKETING_ACTION_NAME}/constraints
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | 評価するポリシーに関連付けられたマーケティングアクションの名前。 |

**リクエスト**

次のリクエストは、3 つの異なるデータセットに対する `exportToThirdParty` マーケティングアクションをテストします。データセットは、ペイロードで提供される配列のタイプと ID で参照されます。

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
      "entityId": "5cc1fb685410ef14b748c55f",
      "entityMeta": {
          "fields": [
              "/properties/personalEmail/properties/address",
              "/properties/person/properties/name/properties/fullName"
          ]
      }
    }
  ]'
```

| プロパティ | 説明 |
| --- | --- |
| `entityType` | ペイロード配列の各項目は、定義するエンティティのタイプを示す必要があります。この使用例では、値は常に「dataSet」になります。 |
| `entityId` | ペイロード配列の各項目は、データセットの一意の ID を提供する必要があります。 |
| `entityMeta.fields` | （オプション）データセットのスキーマー内の特定のフィールドを参照する [JSON ポインター](../../landing/api-fundamentals.md#json-pointer)文字列の配列。この配列を含めると、配列に含まれるフィールドのみが評価に使用されます。配列に含まれていないスキーマフィールドは評価に使用されません。<br><br>このフィールドを含めない場合は、データセットスキーマ内のすべてのフィールドが評価対象に含まれます。 |

**応答** 

応答が成功すると、マーケティングアクションの URL、提供されたデータセットから収集された使用ラベル、およびこれらのラベルに対するアクションのテストの結果として違反したポリシーのリストが返されます。この例では、「サードパーティへのデータ書き出し」ポリシーが `violatedPolicies` 配列に表示され、マーケティングアクションによって予期されるポリシー違反がトリガーされたことが示されています。

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
                        "path": "/properties/personalEmail/properties/address",
                    },
                    {
                        "labels": [
                            "C5"
                        ],
                        "path": "/properties/person/properties/name/properties/fullName"
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
| `duleLabels` | リクエストペイロードで提供されるデータセットから抽出されたデータ使用ラベルのリスト。 |
| `discoveredLabels` | リクエストペイロードで提供されたデータセットのリスト。それぞれに見つかったデータセットレベルとフィールドレベルのラベルを表示します。 |
| `violatedPolicies` | 提供された `duleLabels` に対して（`marketingActionRef` で指定された）マーケティングアクションのテストに違反したポリシーをリストする配列です。 |

## 次の手順

このドキュメントでは、データセットまたは一連のデータ使用ラベルに対してマーケティングアクションを実行する際に、ポリシー違反を正しく確認する方法を説明しました。API 応答で返されたデータを使用して、エクスペリエンスアプリケーション内でプロトコルを設定し、ポリシー違反が発生した場合に適切にポリシー違反を実施することができます。

Platform がポリシーをアクティブ化されたセグメントに自動的に適用する方法について詳しくは、[自動適用](./auto-enforcement.md)のガイドを参照してください。
