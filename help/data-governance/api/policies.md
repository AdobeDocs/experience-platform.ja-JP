---
keywords: Experience Platform;ホーム;人気のトピック;ポリシーの適用;API ベースの適用;データガバナンス
solution: Experience Platform
title: ポリシー API エンドポイント
topic-legacy: developer guide
description: データ使用ポリシーは組織が導入するルールで、Experience Platform 内のデータに対して実行を許可（／制限）するマーケティングアクションの種類を記述するものです。データ使用ポリシーの表示、作成、更新、削除に関する API 呼び出しには、すべて /policies エンドポイントを使用します。
exl-id: 62a6f15b-4c12-4269-bf90-aaa04c147053
source-git-commit: 8133804076b1c0adf2eae5b748e86a35f3186d14
workflow-type: ht
source-wordcount: '1813'
ht-degree: 100%

---

# ポリシーエンドポイント

データ使用ポリシーは、[!DNL Experience Platform] 内のデータに対して実行を許可または制限するマーケティングアクションの種類を記述するルールです。[!DNL Policy Service API] の `/policies` エンドポイントを使用すると、組織のデータ使用ポリシーをプログラムによって管理できます。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Policy Service] API](https://www.adobe.io/experience-platform-apis/references/policy-service/) の一部です。先に進む前に、[はじめる前に](getting-started.md)のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の [!DNL Experience Platform] API の呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## ポリシーのリストの取得 {#list}

`/policies/core` または `/policies/custom` に対して GET リクエストをおこなうと、すべての `core` または `custom` ポリシーをリストできます。

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

正常な応答には `children` 配列が含まれ、`id` 値など、取得された各ポリシーの詳細がリストされています。特定のポリシーの `id` フィールドを使用して、そのポリシーの[検索](#lookup)、[更新](#update)、および[削除](#delete)リクエストを実行できます。

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
| `status` | ポリシーの現在のステータスです。`DRAFT`、`ENABLED`、`DISABLED` の 3 つのステータスが考えられます。デフォルトでは、`ENABLED` ポリシーのみが評価に使用されます。詳細情報については、[ポリシーの評価](../enforcement/overview.md)に関する概要を参照してください。 |
| `marketingActionRefs` | ポリシーに適用されるすべてのマーケティングアクションの URI をリストする配列です。 |
| `description` | ポリシーのユースケースに関する詳細なコンテキストを提供する、オプションの説明です。 |
| `deny` | ポリシーの関連付けられたマーケティングアクションの実行を制限する、特定のデータ使用ラベルを表すオブジェクトです。このプロパティの詳細については、[ポリシーの作成](#create-policy)の節を参照してください。 |

## ポリシーの検索 {#look-up}

GET リクエストのパスに特定のポリシーの `id` プロパティを含めると、そのポリシーを検索できます。

**API 形式**

```http
GET /policies/core/{POLICY_ID}
GET /policies/custom/{POLICY_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{POLICY_ID}` | 検索するポリシーの `id` です。 |

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

応答が成功すると、ポリシーの詳細が返されます。

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
| `status` | ポリシーの現在のステータスです。`DRAFT`、`ENABLED`、`DISABLED` の 3 つのステータスが考えられます。デフォルトでは、`ENABLED` ポリシーのみが評価に使用されます。詳細情報については、[ポリシーの評価](../enforcement/overview.md)に関する概要を参照してください。 |
| `marketingActionRefs` | ポリシーに適用されるすべてのマーケティングアクションの URI をリストする配列です。 |
| `description` | ポリシーのユースケースに関する詳細なコンテキストを提供する、オプションの説明です。 |
| `deny` | ポリシーの関連付けられたマーケティングアクションの実行を制限する、特定のデータ使用ラベルを表すオブジェクトです。このプロパティの詳細については、[ポリシーの作成](#create-policy)の節を参照してください。 |

## カスタムポリシーの作成 {#create-policy}

[!DNL Policy Service] API では、ポリシーは以下によって定義されます。

* 特定のマーケティングアクションへの参照
* マーケティングアクションの実行が制限されているデータ使用ラベルを表す式

後者の要件を満たすには、ポリシー定義に、データ使用ラベルの存在に関するブール式を含める必要があります。この式はポリシー式と呼ばれます。

ポリシー式は、各ポリシー定義内で `deny` プロパティの形式で指定されます。単一のラベルの存在のみをチェックする単純な `deny` オブジェクトの例は、次のようになります。

```json
"deny": {
    "label": "C1"
}
```

ただし、多くのポリシーでは、データ使用ラベルの存在に関して、より複雑な条件を指定しています。これらのユースケースをサポートするために、ポリシー式を表すためのブール演算を含めることもできます。ポリシー式オブジェクトには、ラベルまたは演算子およびオペランドのいずれかを含める必要がありますが、両方を含めることはできません。また、各演算値もポリシー式オブジェクトです。

例えば、`C1 OR (C3 AND C7)` ラベルが存在するデータに対してマーケティングアクションを実行できないようにするポリシーを定義する場合、ポリシーの `deny` プロパティは次のように指定します。

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
| `operator` | 兄弟 `operands` 配列に指定されたラベル間の条件付き関係を示します。使用できる値は次のとおりです。 <ul><li>`OR`：式は、`operands` 配列のラベルが存在する場合は true に解決されます。</li><li>`AND`：式は、`operands` 配列のすべてのラベルが存在する場合にのみ true に解決されます。</li></ul> |
| `operands` | オブジェクトの配列。各オブジェクトは、1 つのラベル、または `operator` プロパティと `operands` プロパティの追加ペアを表します。`operands` 配列内のラベルや操作の存在は、兄弟 `operator` プロパティの値に基づいて true または false に解決されます。 |
| `label` | ポリシーに適用される単一のデータ使用ラベルの名前です。 |

`/policies/custom` エンドポイントに POST リクエストを実行することで、新しいカスタムポリシーを作成できます。

**API 形式**

```http
POST /policies/custom
```

**リクエスト**

次のリクエストは、`C1 OR (C3 AND C7)` ラベルを含むデータに対するマーケティングアクション `exportToThirdParty` の実行を制限する新しいポリシーを作成します。

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
| `status` | ポリシーの現在のステータスです。`DRAFT`、`ENABLED`、`DISABLED` の 3 つのステータスが考えられます。デフォルトでは、`ENABLED` ポリシーのみが評価に使用されます。詳細情報については、[ポリシーの評価](../enforcement/overview.md)に関する概要を参照してください。 |
| `marketingActionRefs` | ポリシーに適用されるすべてのマーケティングアクションの URI をリストする配列です。マーケティングアクションの URIは、[マーケティングアクションの検索](./marketing-actions.md#look-up)に対する応答として `_links.self.href` の下に提供されます。 |
| `description` | ポリシーのユースケースに関する詳細なコンテキストを提供する、オプションの説明です。 |
| `deny` | ポリシーの関連付けられたマーケティングアクションの実行が制限されている、特定のデータ使用ラベルを表すポリシー式です。 |

**応答** 

応答が成功すると、`id` など、新しく作成されたポリシーの詳細が返されます。この値は読み取り専用で、ポリシーの作成時に自動的に割り当てられます。

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

## カスタムポリシーの更新 {#update}

>[!IMPORTANT]
>
>更新できるのは、カスタムポリシーのみです。コアポリシーを有効または無効にする場合は、[有効なコアポリシーのリストの更新](#update-enabled-core)の節を参照してください。

既存のカスタムポリシーを更新するには、更新後のポリシー全体が含まれるペイロードを使用して、ポリシーの ID を PUT リクエストのパスに指定します。つまり、PUT リクエストは基本的にポリシーを書き換えます。

>[!NOTE]
>
>ポリシーを上書きするのではなく、1 つ以上のフィールドのみを更新する場合は、[カスタムポリシーの一部を更新](#patch)の節を参照してください。

**API 形式**

```http
PUT /policies/custom/{POLICY_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{POLICY_ID}` | 更新するポリシーの `id`。 |

**リクエスト**

この例では、データをサードパーティにエクスポートするための条件が変更されたため、`C1 AND C5` データラベルが存在する場合にこのマーケティングアクションを拒否する、作成済みのポリシーが必要です。

次のリクエストは、既存のポリシーを更新して新しいポリシー式を含めます。このリクエストは基本的にポリシーを書き換えるので、値の一部が更新されない場合でも、すべてのフィールドをペイロードに含める必要があります。

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
| `status` | ポリシーの現在のステータスです。`DRAFT`、`ENABLED`、`DISABLED` の 3 つのステータスが考えられます。デフォルトでは、`ENABLED` ポリシーのみが評価に使用されます。詳細情報については、[ポリシーの評価](../enforcement/overview.md)に関する概要を参照してください。 |
| `marketingActionRefs` | ポリシーに適用されるすべてのマーケティングアクションの URI をリストする配列です。マーケティングアクションの URIは、[マーケティングアクションの検索](./marketing-actions.md#look-up)に対する応答として `_links.self.href` の下に提供されます。 |
| `description` | ポリシーのユースケースに関する詳細なコンテキストを提供する、オプションの説明です。 |
| `deny` | ポリシーの関連付けられたマーケティングアクションの実行が制限されている、特定のデータ使用ラベルを表すポリシー式です。このプロパティの詳細については、[ポリシーの作成](#create-policy)の節を参照してください。 |

**応答** 

応答が成功すると、更新されたポリシーの詳細が返されます。

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

## カスタムポリシーの一部を更新 {#patch}

>[!IMPORTANT]
>
>更新できるのは、カスタムポリシーのみです。コアポリシーを有効または無効にする場合は、[有効なコアポリシーのリストの更新](#update-enabled-core)の節を参照してください。

ポリシーの特定の部分を更新するには、PATCH リクエストを使用します。ポリシーを書き換える PUT リクエストとは異なり、PATCH リクエストでは、リクエスト本文で指定したプロパティのみが更新されます。適切なプロパティ（`/status`）とその値（`ENABLED` または `DISABLED`）へのパスを指定するだけで済むので、ポリシーを有効化または無効化する場合に特に便利です。

>[!NOTE]
>
>PATCH リクエストのペイロードは、JSON パッチの形式に従います。使用可能な構文の詳細については、[API の基本原則のガイド](../../landing/api-fundamentals.md)を参照してください。

[!DNL Policy Service] API は、JSON パッチ操作 `add`、`remove`、`replace` をサポートし、以下の例に示すように、複数の更新を 1 回の呼び出しに統合することができます。

**API 形式**

```http
PATCH /policies/custom/{POLICY_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{POLICY_ID}` | プロパティを更新するポリシーの `id`。 |

**リクエスト**

次のリクエストは、2 つの `replace` 操作を使用して、ポリシーのステータスを `DRAFT` から `ENABLED` に変更し、`description` フィールドを新しい説明で更新します。

>[!IMPORTANT]
>
>1 回のリクエストで複数の PATCH 操作を送信すると、配列に表示される順に処理されます。必要に応じて、リクエストを正しい順序で送信していることを確認します。

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

応答が成功すると、更新されたポリシーの詳細が返されます。


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

## カスタムポリシーの削除 {#delete}

DELETE リクエストのパスに `id` を含めることで、カスタムポリシーを削除できます。

>[!WARNING]
>
>削除したポリシーは復元できません。最初に[参照（GET）リクエストを実行](#lookup)してポリシーを表示し、それが削除するポリシーであるかどうかを確認することをお勧めします。

**API 形式**

```http
DELETE /policies/custom/{POLICY_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{POLICY_ID}` | 削除するポリシーの ID。 |

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

応答が成功すると、HTTP ステータス 200（OK）が空白の本文とともに返されます。

削除されたことを確認するには、ポリシーの参照（GET）をもう一度試みます。ポリシーが正常に削除された場合は、HTTP 404 （見つかりません）エラーが表示されます。

## 有効なコアポリシーのリストを取得 {#list-enabled-core}

デフォルトでは、有効なデータ使用ポリシーのみが評価に使用されます。`/enabledCorePolicies` エンドポイントに GET リクエストをおこなうことで、組織で現在有効になっているコアポリシーのリストを取得できます。

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

応答が成功すると、`policyIds` 配列の下に有効なコアポリシーのリストが返されます。

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

## 有効なコアポリシーのリストの更新 {#update-enabled-core}

デフォルトでは、有効なデータ使用ポリシーのみが評価に使用されます。`/enabledCorePolicies` エンドポイントに PUT リクエストを送信すると、1 回の呼び出しで、組織で有効になっているコアポリシーのリストを更新できます。

>[!NOTE]
>
>このエンドポイントで有効または無効にできるのは、コアポリシーのみです。カスタムポリシーを有効または無効にする方法については、[ポリシーの一部の更新](#patch)の節を参照してください。

**API 形式**

```http
PUT /enabledCorePolicies
```

**リクエスト**

次のリクエストは、ペイロードで指定された ID に基づいて、有効なコアポリシーのリストを更新します。

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
| `policyIds` | 有効にするコアポリシー ID のリストです。リストに含まれないコアポリシーは `DISABLED` ステータスに設定され、評価には使用されません。 |

**応答** 

応答が成功すると、有効なコアポリシーの更新リストが `policyIds` 配列の下に返されます。

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

新しいポリシーを定義するか、既存のポリシーを更新したら、[!DNL Policy Service] API を使用して、特定のラベルやデータセットに対するマーケティングアクションをテストし、想定どおりにポリシーで違反が発生しているかどうかを確認できます。詳しくは、[ポリシー評価エンドポイント](./evaluation.md)に関するガイドを参照してください。
