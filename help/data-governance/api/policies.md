---
keywords: Experience Platform；ホーム；人気の高いトピック；ポリシーの適用；APIベースの適用；データガバナンス
solution: Experience Platform
title: ポリシーAPIエンドポイント
topic-legacy: developer guide
description: データ使用ポリシーは組織が導入するルールで、Experience Platform 内のデータに対して実行を許可（／制限）するマーケティングアクションの種類を記述するものです。/policiesエンドポイントは、データ使用ポリシーの表示、作成、更新または削除に関連するすべてのAPI呼び出しに使用されます。
exl-id: 62a6f15b-4c12-4269-bf90-aaa04c147053
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1817'
ht-degree: 9%

---

# ポリシーエンドポイント

データ使用ポリシーとは、[!DNL Experience Platform]内のデータに対して実行を許可（制限）されるマーケティングアクションの種類を記述するルールです。 [!DNL Policy Service API]の`/policies`エンドポイントを使用すると、組織のデータ使用ポリシーをプログラムで管理できます。

## はじめに

このガイドで使用されるAPIエンドポイントは、[[!DNL Policy Service] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml)の一部です。 先に進む前に、[はじめにガイド](getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプルAPI呼び出しの読み方、および任意の[!DNL Experience Platform] APIの呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## ポリシーのリストを取得{#list}

`/policies/core`または`/policies/custom`に対してGETリクエストを行うことで、すべての`core`または`custom`ポリシーをリストできます。

**API 形式**

```http
GET /policies/core
GET /policies/custom
```

**リクエスト**

次のリクエストは、組織で定義されたカスタムポリシーのリストを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/dulepolicy/policies/custom \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答には`children`配列が含まれ、`id`値を含む取得された各ポリシーの詳細をリストします。 特定のポリシーの`id`フィールドを使用して、そのポリシーの[lookup](#lookup)、[update](#update)、[delete](#delete)の各リクエストを実行できます。

```JSON
{
    "_page": {
        "start": "5c6dacdf685a4913dc48937c",
        "count": 2
    },
    "_links": {
        "page": {
            "href": "https://platform.adobe.io/policies/custom?{?limit,start,property}",
            "templated": true
        }
    },
    "children": [
        {
            "name": "Export Data to Third Party",
            "status": "DRAFT",
            "marketingActionRefs": [
                "https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/exportToThirdParty"
            ],
            "description": "Conditions under which data cannot be exported to a third party",
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
            "created": 1550691551888,
            "createdClient": "{CLIENT_ID}",
            "createdUser": "{USER_ID}",
            "updated": 1550701472910,
            "updatedClient": "{CLIENT_ID}",
            "updatedUser": "{USER_ID}",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/dulepolicy/policies/custom/5c6dacdf685a4913dc48937c"
                }
            },
            "id": "5c6dacdf685a4913dc48937c"
        },
        {
            "name": "Combine Data",
            "status": "ENABLED",
            "marketingActionRefs": [
                "https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/combineData"
            ],
            "description": "Data that meets these conditions cannot be combined.",
            "deny": {
                "operator": "AND",
                "operands": [
                    {
                        "label": "C3"
                    },
                    {
                        "label": "I1"
                    }
                ]
            },
            "imsOrg": "{IMS_ORG}",
            "created": 1550703519823,
            "createdClient": "{CLIENT_ID}",
            "createdUser": "{USER_ID}",
            "updated": 1550714340335,
            "updatedClient": "{CLIENT_ID}",
            "updatedUser": "{USER_ID}",
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

| プロパティ | 説明 |
| --- | --- |
| `_page.count` | 取得したポリシーの合計数です。 |
| `name` | ポリシーの表示名です。 |
| `status` | ポリシーの現在のステータス。 次の3つのステータスが考えられます。`DRAFT`、`ENABLED`、または`DISABLED`。 デフォルトでは、`ENABLED`ポリシーのみが評価に参加します。 詳しくは、[ポリシー評価](../enforcement/overview.md)の概要を参照してください。 |
| `marketingActionRefs` | ポリシーに適用されるすべてのマーケティングアクションのURIをリストする配列。 |
| `description` | ポリシーの使用例に関する詳細なコンテキストを提供する、オプションの説明です。 |
| `deny` | ポリシーに関連付けられたマーケティングアクションが実行されるのを制限する、特定のデータ使用ラベルを表すオブジェクトです。 このプロパティの詳細については、[ポリシー](#create-policy)の作成の節を参照してください。 |

## ポリシー{#look-up}を検索

GET要求のパスにそのポリシーの`id`プロパティを含めると、特定のポリシーを検索できます。

**API 形式**

```http
GET /policies/core/{POLICY_ID}
GET /policies/custom/{POLICY_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{POLICY_ID}` | 検索するポリシーの`id`。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/dulepolicy/policies/custom/5c6dacdf685a4913dc48937c \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

成功した場合は、ポリシーの詳細が返されます。

```JSON
{
    "name": "Export Data to Third Party",
    "status": "DRAFT",
    "marketingActionRefs": [
        "https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/exportToThirdParty"
    ],
    "description": "Conditions under which data cannot be exported to a third party",
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
    "createdClient": "{CLIENT_ID}",
    "createdUser": "{USER_ID}",
    "updated": 1550714340335,
    "updatedClient": "{CLIENT_ID}",
    "updatedUser": "{USER_ID}",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/dulepolicy/policies/custom/5c6dacdf685a4913dc48937c"
        }
    },
    "id": "5c6dacdf685a4913dc48937c"
}
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ポリシーの表示名です。 |
| `status` | ポリシーの現在のステータス。 次の3つのステータスが考えられます。`DRAFT`、`ENABLED`、または`DISABLED`。 デフォルトでは、`ENABLED`ポリシーのみが評価に参加します。 詳しくは、[ポリシー評価](../enforcement/overview.md)の概要を参照してください。 |
| `marketingActionRefs` | ポリシーに適用されるすべてのマーケティングアクションのURIをリストする配列です。 |
| `description` | ポリシーの使用例に関する詳細なコンテキストを提供する、オプションの説明です。 |
| `deny` | ポリシーに関連付けられたマーケティングアクションが実行されるのを制限する、特定のデータ使用ラベルを表すオブジェクトです。 このプロパティの詳細については、[ポリシー](#create-policy)の作成の節を参照してください。 |

## カスタムポリシー{#create-policy}の作成

[!DNL Policy Service] APIでは、ポリシーは次のように定義されます。

* 特定のマーケティングアクションへの参照
* マーケティングアクションの実行が制限されているデータ使用ラベルを説明する式

後者の要件を満たすには、ポリシー定義に、データ使用ラベルの存在に関するブール式を含める必要があります。 この式はポリシー式と呼ばれます。

ポリシー式は、各ポリシー定義内で`deny`プロパティの形式で提供されます。 単純な`deny`オブジェクトの例で、単一のラベルの存在のみをチェックする場合は、次のようになります。

```json
"deny": {
    "label": "C1"
}
```

ただし、多くのポリシーでは、データ使用ラベルの存在に関して、より複雑な条件を指定しています。 これらの使用例をサポートするために、ポリシー式を説明するブール演算を含めることもできます。 ポリシー式オブジェクトには、ラベルまたは演算子と演算値のいずれかを含める必要がありますが、両方を含めることはできません。 また、各演算値もポリシー式オブジェクトです。

例えば、`C1 OR (C3 AND C7)`ラベルが存在するデータに対してマーケティングアクションを実行できないようにするポリシーを定義する場合、ポリシーの`deny`プロパティは次のように指定します。

```JSON
"deny": {
  "operator": "OR",
  "operands": [
    {"label": "C1"},
    {
      "operator": "AND",
      "operands": [
        {"label": "C3"},
        {"label": "C7"}
      ]
    }
  ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `operator` | 兄弟`operands`配列に指定されたラベル間の条件付き関係を示します。 使用できる値は次のとおりです。 <ul><li>`OR`:式は、 `operands` 配列内のラベルのいずれかが存在する場合はtrueに解決されます。</li><li>`AND`:式は、 `operands` 配列内のすべてのラベルが存在する場合にのみtrueに解決されます。</li></ul> |
| `operands` | オブジェクトの配列。各オブジェクトは、1つのラベルか、または追加の`operator`と`operands`のプロパティのペアを表します。 `operands`配列内のラベルや操作の存在は、兄弟`operator`プロパティの値に基づいてtrueまたはfalseに解決されます。 |
| `label` | ポリシーに適用される単一のデータ使用ラベルの名前です。 |

`/policies/custom`エンドポイントにPOSTリクエストを行うことで、新しいカスタムポリシーを作成できます。

**API 形式**

```http
POST /policies/custom
```

**リクエスト**

次のリクエストは、`C1 OR (C3 AND C7)`というラベルを含むデータに対するマーケティング操作`exportToThirdParty`の実行を制限する新しいポリシーを作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/dulepolicy/policies/custom \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "name": "Export Data to Third Party",
        "status": "DRAFT",
        "marketingActionRefs": [
          "https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/exportToThirdParty"
        ],
        "description": "Conditions under which data cannot be exported to a third party",
        "deny": {
          "operator": "OR",
          "operands": [
            {"label": "C1"},
            {
              "operator": "AND",
              "operands": [
                {"label": "C3"},
                {"label": "C7"}
              ]
            }
          ]
        }
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ポリシーの表示名です。 |
| `status` | ポリシーの現在のステータス。 次の3つのステータスが考えられます。`DRAFT`、`ENABLED`、または`DISABLED`。 デフォルトでは、`ENABLED`ポリシーのみが評価に参加します。 詳しくは、[ポリシー評価](../enforcement/overview.md)の概要を参照してください。 |
| `marketingActionRefs` | ポリシーに適用されるすべてのマーケティングアクションのURIをリストする配列です。 マーケティングアクションのURIは、[マーケティングアクション](./marketing-actions.md#look-up)の検索に対する応答として`_links.self.href`の下に提供されます。 |
| `description` | ポリシーの使用例に関する詳細なコンテキストを提供する、オプションの説明です。 |
| `deny` | ポリシーに関連付けられたマーケティングアクションの実行が制限されている特定のデータ使用ラベルを説明するポリシー式です。 |

**応答**

正常に応答すると、新しく作成されたポリシーの詳細（`id`を含む）が返されます。 この値は読み取り専用で、ポリシーの作成時に自動的に割り当てられます。

```JSON
{
    "name": "Export Data to Third Party",
    "status": "DRAFT",
    "marketingActionRefs": [
        "https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/exportToThirdParty"
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
    "created": 1550691551888,
    "createdClient": "{CLIENT_ID}",
    "createdUser": "{USER_ID}",
    "updated": 1550691551888,
    "updatedClient": "{CLIENT_ID}",
    "updatedUser": "{USER_ID}",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/dulepolicy/policies/custom/5c6dacdf685a4913dc48937c"
        }
    },
    "id": "5c6dacdf685a4913dc48937c"
}
```

## カスタムポリシー{#update}の更新

>[!IMPORTANT]
>
>更新できるのは、カスタムポリシーのみです。 コアポリシーを有効または無効にする場合は、[有効なコアポリシーのリストの更新](#update-enabled-core)の節を参照してください。

既存のカスタムポリシーを更新するには、ポリシーの更新された形式全体を含むペイロードを、PUTリクエストのパスに指定します。 つまり、PUTリクエストは基本的にポリシーを書き換えます。

>[!NOTE]
>
>ポリシーを上書きせずに、1つ以上のフィールドのみを更新する場合は、[カスタムポリシーの一部の更新](#patch)の節を参照してください。

**API 形式**

```http
PUT /policies/custom/{POLICY_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{POLICY_ID}` | 更新するポリシーの`id`。 |

**リクエスト**

この例では、データをサードパーティにエクスポートするための条件が変更されたため、`C1 AND C5` データラベルが存在する場合にこのマーケティングアクションを拒否する、作成済みのポリシーが必要です。

次の要求は、既存のポリシーを更新して新しいポリシー式を含めます。 このリクエストは基本的にポリシーを書き換えるので、値の一部が更新されていない場合でも、すべてのフィールドをペイロードに含める必要があります。

```shell
curl -X PUT \
  https://platform.adobe.io/data/foundation/dulepolicy/policies/custom/5c6dacdf685a4913dc48937c \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "name": "Export Data to Third Party",
        "status": "DRAFT",
        "marketingActionRefs": [
          "../marketingActions/custom/exportToThirdParty"
        ],
        "description": "Conditions under which data cannot be exported to a third party",
        "deny": {
          "operator": "AND",
          "operands": [
            {"label": "C1"},
            {"label": "C5"}
          ]
        }
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ポリシーの表示名です。 |
| `status` | ポリシーの現在のステータス。 次の3つのステータスが考えられます。`DRAFT`、`ENABLED`、または`DISABLED`。 デフォルトでは、`ENABLED`ポリシーのみが評価に参加します。 詳しくは、[ポリシー評価](../enforcement/overview.md)の概要を参照してください。 |
| `marketingActionRefs` | ポリシーに適用されるすべてのマーケティングアクションのURIをリストする配列です。 マーケティングアクションのURIは、[マーケティングアクション](./marketing-actions.md#look-up)の検索に対する応答として`_links.self.href`の下に提供されます。 |
| `description` | ポリシーの使用例に関する詳細なコンテキストを提供する、オプションの説明です。 |
| `deny` | ポリシーに関連付けられたマーケティングアクションの実行が制限されている特定のデータ使用ラベルを説明するポリシー式です。 このプロパティの詳細については、[ポリシー](#create-policy)の作成の節を参照してください。 |

**応答**

正常に応答すると、更新されたポリシーの詳細が返されます。

```JSON
{
    "name": "Export Data to Third Party",
    "status": "DRAFT",
    "marketingActionRefs": [
        "https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/core/exportToThirdParty"
    ],
    "description": "Conditions under which data cannot be exported to a third party",
    "deny": {
        "operator": "AND",
        "operands": [
            {
                "label": "C1"
            },
            {
                "label": "C5"
            }
        ]
    },
    "imsOrg": "{IMS_ORG}",
    "created": 1550691551888,
    "createdClient": "{CLIENT_ID}",
    "createdUser": "{USER_ID}",
    "updated": 1550701472910,
    "updatedClient": "{CLIENT_ID}",
    "updatedUser": "{USER_ID}",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/dulepolicy/policies/custom/5c6dacdf685a4913dc48937c"
        }
    },
    "id": "5c6dacdf685a4913dc48937c"
}
```

## カスタムポリシーの一部の更新{#patch}

>[!IMPORTANT]
>
>更新できるのは、カスタムポリシーのみです。 コアポリシーを有効または無効にする場合は、[有効なコアポリシーのリストの更新](#update-enabled-core)の節を参照してください。

ポリシーの特定の部分を更新するには、PATCH リクエストを使用します。ポリシーを書き換えるPUT要求とは異なり、PATCH要求では、要求本文で指定されたプロパティのみが更新されます。 これは、適切なプロパティ(`/status`)とその値（`ENABLED`または`DISABLED`）へのパスを指定するだけで済むので、ポリシーを有効または無効にする場合に特に便利です。

>[!NOTE]
>
>PATCH要求のペイロードは、JSONパッチの形式に従います。 受け入れられる構文の詳細については、[APIの基本原則のガイド](../../landing/api-fundamentals.md)を参照してください。

[!DNL Policy Service] APIは、JSONパッチ操作`add`、`remove`、`replace`をサポートし、以下の例に示すように、複数の更新を1回の呼び出しに組み合わせることができます。

**API 形式**

```http
PATCH /policies/custom/{POLICY_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{POLICY_ID}` | プロパティを更新するポリシーの`id`。 |

**リクエスト**

次の要求では、2つの`replace`操作を使用して、ポリシーの状態を`DRAFT`から`ENABLED`に変更し、`description`フィールドを新しい説明で更新します。

>[!IMPORTANT]
>
>1回のリクエストで複数のPATCH操作を送信する場合、配列に表示される順に処理されます。 必要に応じて、リクエストを正しい順序で送信していることを確認します。

```SHELL
curl -X PATCH \
  https://platform.adobe.io/data/foundation/dulepolicy/policies/custom/5c6dacdf685a4913dc48937c \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d ' [
          {
            "op": "replace",
            "path": "/status",
            "value": "ENABLED"
          },
          {
            "op": "replace",
            "path": "/description",
            "value": "New policy description."
          }
        ]'
```

**応答**

正常に応答すると、更新されたポリシーの詳細が返されます。


```JSON
{
    "name": "Export Data to Third Party",
    "status": "ENABLED",
    "marketingActionRefs": [
        "https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/exportToThirdParty"
    ],
    "description": "New policy description.",
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
    "createdClient": "{CLIENT_ID}",
    "createdUser": "{USER_ID}",
    "updated": 1550712163182,
    "updatedClient": "{CLIENT_ID}",
    "updatedUser": "{USER_ID}",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/dulepolicy/policies/custom/5c6dacdf685a4913dc48937c"
        }
    },
    "id": "5c6dacdf685a4913dc48937c"
}
```

## カスタムポリシー{#delete}の削除

DELETE要求のパスに`id`を含めると、カスタムポリシーを削除できます。

>[!WARNING]
>
>削除したポリシーは復元できません。まず、[ルックアップ(GET)リクエスト](#lookup)を実行して、ポリシーを表示し、削除する正しいポリシーであることを確認することをお勧めします。

**API 形式**

```http
DELETE /policies/custom/{POLICY_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{POLICY_ID}` | 削除するポリシーのID。 |

**リクエスト**

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/dulepolicy/policies/custom/5c6ddb56eb60ca13dbf8b9a8 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

応答が成功すると、HTTPステータス200(OK)が空白の本文で返されます。

ポリシーを再度参照(GET)すると、削除を確認できます。 ポリシーが正常に削除された場合は、HTTP 404 （見つかりません）エラーが表示されます。

## 有効なコアポリシーのリストを取得{#list-enabled-core}

デフォルトでは、有効なデータ使用ポリシーのみが評価に関与します。 `/enabledCorePolicies`エンドポイントにGETリクエストを行うことで、組織で現在有効になっているコアポリシーのリストを取得できます。

**API 形式**

```http
GET /enabledCorePolicies
```

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/dulepolicy/enabledCorePolicies \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常に応答すると、有効なコアポリシーのリストが`policyIds`配列の下に返されます。

```json
{
  "policyIds": [
    "corepolicy_0001",
    "corepolicy_0002",
    "corepolicy_0003",
    "corepolicy_0004",
    "corepolicy_0005",
    "corepolicy_0006",
    "corepolicy_0007",
    "corepolicy_0008"
  ],
  "imsOrg": "{IMS_ORG}",
  "created": 1529696681413,
  "createdClient": "{CLIENT_ID}",
  "createdUser": "{USER_ID}",
  "updated": 1529697651972,
  "updatedClient": "{CLIENT_ID}",
  "updatedUser": "{USER_ID}",
  "_links": {
    "self": {
      "href": "https://platform.adobe.io:443/data/foundation/dulepolicy/enabledCorePolicies"
    }
  }
}
```

## 有効なコアポリシーのリストの更新{#update-enabled-core}

デフォルトでは、有効なデータ使用ポリシーのみが評価に関与します。 `/enabledCorePolicies`エンドポイントにPUTリクエストを送信すると、1回の呼び出しで、組織で有効になっているコアポリシーのリストを更新できます。

>[!NOTE]
>
>このエンドポイントで有効または無効にできるのは、コアポリシーのみです。 カスタムポリシーを有効または無効にする方法については、[ポリシー](#patch)の一部の更新の節を参照してください。

**API 形式**

```http
PUT /enabledCorePolicies
```

**リクエスト**

次のリクエストは、ペイロードで提供されたIDに基づいて、有効なコアポリシーのリストを更新します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/dulepolicy/enabledCorePolicies \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "policyIds": [
          "corepolicy_0001",
          "corepolicy_0002",
          "corepolicy_0007",
          "corepolicy_0008"
        ]
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `policyIds` | 有効にするコアポリシーIDのリストです。 含まれないコアポリシーは`DISABLED`ステータスに設定され、評価には参加しません。 |

**応答**

正常に応答すると、有効なコアポリシーの更新リストが`policyIds`配列に返されます。

```json
{
  "policyIds": [
    "corepolicy_0001",
    "corepolicy_0002",
    "corepolicy_0007",
    "corepolicy_0008"
  ],
  "imsOrg": "{IMS_ORG}",
  "created": 1529696681413,
  "createdClient": "{CLIENT_ID}",
  "createdUser": "{USER_ID}",
  "updated": 1595876052649,
  "updatedClient": "{CLIENT_ID}",
  "updatedUser": "{USER_ID}",
  "_links": {
    "self": {
      "href": "https://platform.adobe.io:443/data/foundation/dulepolicy/enabledCorePolicies"
    }
  }
}
```

## 次の手順

新しいポリシーを定義したり、既存のポリシーを更新したりしたら、[!DNL Policy Service] APIを使用して、特定のラベルやデータセットに対するマーケティングアクションをテストし、ポリシーで期待どおりの違反が発生しているかどうかを確認できます。 詳しくは、[ポリシー評価エンドポイント](./evaluation.md)のガイドを参照してください。
