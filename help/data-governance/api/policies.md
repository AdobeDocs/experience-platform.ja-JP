---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ポリシー
topic: developer guide
translation-type: tm+mt
source-git-commit: 08d02e7323f75c450e7a250835f26a569685cdd1
workflow-type: tm+mt
source-wordcount: '866'
ht-degree: 1%

---


# ポリシー

データ使用ポリシーとは、組織が採用するルールで、エクスペリエンスプラットフォーム内のデータに対して実行を許可（制限）されるマーケティングアクションの種類を示します。

エンドポイント `/policies` は、データ使用ポリシーの表示、作成、更新または削除に関連するすべてのAPI呼び出しに使用されます。

## すべてのポリシーのリスト

ポリシーのリストを表示するために、指定したコンテナに対してGETリクエストを行うこ `/policies/core` とができるか、または指定したのすべてのポリシー `/policies/custom` を返すことができます。

**API形式**

```http
GET /policies/core
GET /policies/custom
```

**リクエスト**

```SHELL
curl -X GET \
  https://platform.adobe.io/data/foundation/dulepolicy/policies/custom \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

この応答には、指定したコンテナ内のポリシーの合計数を示す「カウント」と、そのポリシーを含む各ポリシーの詳細が含まれ `id`ます。 この `id` フィールドは、表示固有のポリシーに対するルックアップリクエストや、更新および削除の操作を実行するために使用されます。

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
            "createdClient": "string",
            "createdUser": "string",
            "updated": 1550701472910,
            "updatedClient": "string",
            "updatedUser": "string",
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
            "createdClient": "string",
            "createdUser": "string",
            "updated": 1550714340335,
            "updatedClient": "string",
            "updatedUser": "string",
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

## 特定のポリシーの検索

各ポリシーには、特定のポリシーの詳細を要求するために使用できる `id` フィールドが含まれています。 ポリシー `id` の内容が不明な場合は、リスト(GET)リクエストを使用して、前の手順に示したように、特定のコンテナ(`core` または `custom`)内のすべてのポリシーをリストすることができます。

**API形式**

```http
GET /policies/core/{id}
GET /policies/custom/{id}
```

**リクエスト**

```SHELL
curl -X GET \
  https://platform.adobe.io/data/foundation/dulepolicy/policies/custom/5c6dacdf685a4913dc48937c \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

この応答には、 `id` (このフィールドはリクエストで `id` 送信されたものと一致する必要があります)、 `name`、 `status`、などのキーフィールドや、ポリシーの基となるマーケティングアクションへの参照リンクなど、ポリシーの詳細が含まれ `description``marketingActionRefs`ます。

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
    "created": 1550691551888,
    "createdClient": "string",
    "createdUser": "string",
    "updated": 1550701472910,
    "updatedClient": "string",
    "updatedUser": "string",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/dulepolicy/policies/custom/5c6dacdf685a4913dc48937c"
        }
    },
    "id": "5c6dacdf685a4913dc48937c"
}
```

## ポリシーの作成 {#create-policy}

ポリシーを作成するには、マーケティングアクションを、そのマーケティングアクションを禁止するDULEラベルの式と共に含める必要があります。 ポリシー定義には、DULEラベルの存在に関するブール値の式である `deny` プロパティを含める必要があります。

この式は「a」と呼ば `PolicyExpression` れ、ラベル _または演算子と演算値のいずれかを含むオブジェクト_ ですが __ 、両方を含むわけではありません。 一方、各演算値も `PolicyExpression` オブジェクトです。 例えば、データのサードパーティへのエクスポートに関するポリシーは、ラベルが存在する場合は禁止され `C1 OR (C3 AND C7)` る可能性があります。 この式は次のように指定します。

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

**API形式**

```http
POST /policies/custom
```

**リクエスト**

```SHELL
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
          "../marketingActions/custom/exportToThirdParty"
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

**応答**

正常に作成されると、HTTPステータス201 （作成済み）が返され、応答本文には新しく作成されたポリシーの詳細（ポリシーを含む）が含まれ `id`ます。 この値は読み取り専用で、ポリシーの作成時に自動的に割り当てられます。

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
    "createdClient": "string",
    "createdUser": "string",
    "updated": 1550691551888,
    "updatedClient": "string",
    "updatedUser": "string",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/dulepolicy/policies/custom/5c6dacdf685a4913dc48937c"
        }
    },
    "id": "5c6dacdf685a4913dc48937c"
}
```

## ポリシーの更新

データ使用ポリシーの作成後は、そのポリシーを更新する必要がある場合があります。 これは、ポリシーの更新された形式全体を含むペイロード `id` を使用して、ポリシーに対するPUT要求を通じて行われます。 つまり、PUT要求は基本的にポリシーを _書き直すので_ 、本体には以下の例に示すように、必要な情報をすべて含める必要があります。

**API形式**

```http
PUT /policies/custom/{id}
```

**リクエスト**

この例では、データをサードパーティにエクスポートするための条件が変更され、データラベルが存在する場合にこのマーケティングアクションを拒否するために作成したポリシーが必要になり `C1 AND (C3 OR C7)` ます。 次の呼び出しを使用して、既存のポリシーを更新します。

```SHELL
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
            {
              "operator": "OR",
              "operands": [
                {"label": "C3"},
                {"label": "C7"}
              ]
            }
          ]
        }
      }'
```

**応答**

更新要求が成功すると、HTTPステータス200(OK)が返され、応答本文に更新されたポリシーが表示されます。 は、要求で `id``id` 送信されたものと一致する必要があります。

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
    "createdClient": "string",
    "createdUser": "string",
    "updated": 1550701472910,
    "updatedClient": "string",
    "updatedUser": "string",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/dulepolicy/policies/custom/5c6dacdf685a4913dc48937c"
        }
    },
    "id": "5c6dacdf685a4913dc48937c"
}
```

## ポリシーの一部の更新

ポリシーの特定の部分は、PATCH要求を使用して更新できます。 ポリシーを _書き換えるPUT要求とは異なり_ 、PATCH要求は要求本文で指定されたパスのみを更新します。 これは、ポリシーを有効または無効にする場合に特に便利です。更新する特定のパス(`/status`)とその値(`ENABLE` または `DISABLE`)のみを送信する必要があるからです。

Policy Service APIは現在、「add」、「replace」、「remove」の各PATCH操作をサポートしており、次の例に示すように、各更新をアレイ内のオブジェクトとして1回の呼び出しに結合できます。

**API形式**

```http
PATCH /policies/custom/{id}
```

**リクエスト**

この例では、「replace」操作を使用して、ポリシーのステータスを「DRAFT」から「ENABLED」に変更し、説明フィールドを新しい説明に更新します。 また、「delete」操作を使用してポリシーの説明を削除し、「add」操作を使用して新しい1回追加することで、次のように説明フィールドを更新することもできます。

```SHELL
[
    {
        "op": "remove",
        "path": "/description"
    },
    {
        "op": "add",
        "path": "/description",
        "value": "New policy description."
    }
]
```

1回のリクエストで複数のPATCH操作を送信する場合、配列内で出現する順に処理されることに注意し、必要に応じてリクエストを正しい順序で送信するようにしてください。

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

更新要求が成功すると、HTTPステータス200 (OK)が返され、応答本体に更新されたポリシーが表示されます（「status」は「ENABLED」、「description」は変更されました）。 ポリシー `id` は、要求で `id` 送信されたものと一致する必要があります。


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
    "createdClient": "string",
    "createdUser": "string",
    "updated": 1550712163182,
    "updatedClient": "string",
    "updatedUser": "string",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/dulepolicy/policies/custom/5c6dacdf685a4913dc48937c"
        }
    },
    "id": "5c6dacdf685a4913dc48937c"
}
```

## ポリシーの削除

作成したポリシーを削除する必要がある場合は、削除するポリシーのに対してDELETEリクエストを発行して削除 `id` できます。 最初に参照(GET)要求を実行してポリシーを表示し、削除する正しいポリシーであることを確認することをお勧めします。 **削除したポリシーは復元できません。**

**API形式**

```http
DELETE /policies/custom/{id}
```

**リクエスト**

```SHELL
curl -X DELETE \
  https://platform.adobe.io/data/foundation/dulepolicy/policies/custom/5c6ddb56eb60ca13dbf8b9a8 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

ポリシーが正常に削除されると、応答本文は空白になり、HTTPステータスは200(OK)になります。

ポリシーを再度参照(GET)して、削除を確認できます。 ポリシーが削除されたので、HTTPステータス404（見つかりません）と共に「見つかりません」というエラーメッセージが表示されます。
