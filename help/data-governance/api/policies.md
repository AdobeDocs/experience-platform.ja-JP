---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ポリシー
topic: developer guide
translation-type: tm+mt
source-git-commit: d4964231ee957349f666eaf6b0f5729d19c408de
workflow-type: tm+mt
source-wordcount: '862'
ht-degree: 92%

---


# ポリシー

Data usage policies are rules your organization adopts that describe the kinds of marketing actions that you are allowed to, or restricted from, performing on data within [!DNL Experience Platform].

データ使用ポリシーの表示、作成、更新、削除に関する API 呼び出しには、すべて `/policies` を使用します。

## すべてのポリシーのリストを表示する

ポリシーのリストを表示するには、GET リクエストを `/policies/core` または `/policies/custom` に対して実行します。こうすると、指定したコンテナのすべてのポリシーが返されます。

**API 形式**

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

レスポンスには、指定したコンテナ内のポリシーの合計数を表す「count」と、その `id` を含む各ポリシーの詳細が示されます。`id` フィールドは、特定のポリシーを表示する検索リクエストの実行、および更新操作と削除操作の実行に使用されます。

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

## ポリシーの検索

各ポリシーに含まれる `id` フィールドは、特定のポリシーの詳細を要求する際に使用できます。ポリシーの `id` が不明な場合は、前の手順で示したように、特定のコンテナ（`core` または `custom`）内のすべてのポリシーをリスト表示するリスト（GET）リクエストを使用して見つけることができます。

**API 形式**

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

レスポンスにはポリシーの詳細が表示されます。この中には、`id`（このフィールドはリクエストで送信した `id` に一致します）、`name`、`status`、`description` などの重要フィールド、さらにポリシーの基になるマーケティングアクションへの参照リンク（`marketingActionRefs`）が含まれます。

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

## ポリシーを作成する {#create-policy}

ポリシーを作成する際には、マーケティングアクションと、そのマーケティングアクションを禁止する DULE ラベルの式を含める必要があります。ポリシー定義には `deny` プロパティが必要です。これは、DULE ラベルの存在に関するブール式です。

この式は `PolicyExpression` と呼ばれ、ラベルまたは演算子とオペランドの&#x200B;_いずれか__のみ_&#x200B;を含むオブジェクトです。これに対し、各オペランドも `PolicyExpression` オブジェクトです。例えば、`C1 OR (C3 AND C7)` ラベルが存在する場合、サードパーティへのデータのエクスポートに関するポリシーは禁止される可能性があります。この式は次のように指定します。

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

**API 形式**

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

正常に作成された場合は、HTTPステータス 201（Created）が返され、本文には新しく作成したポリシーの詳細とその `id` が含まれます。この値は読み取り専用で、ポリシーの作成時に自動的に割り当てられます。

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

## ポリシーを更新する

場合によっては、作成後にデータ使用ポリシーを更新する必要があります。それには、ポリシーの更新済みフォーム全体を含むペイロードを指定して、ポリシー `id` に PUT リクエストを実行します。つまり、PUT リクエストは基本的にポリシーを&#x200B;_書き換える_&#x200B;ので、本文には必要な情報をすべて含める必要があります（以下の例を参照）。

**API 形式**

```http
PUT /policies/custom/{id}
```

**リクエスト**

この例では、データをサードパーティにエクスポートするための条件が変更されたため、`C1 AND (C3 OR C7)` データラベルが存在する場合にこのマーケティングアクションを拒否する、作成済みのポリシーが必要です。次の呼び出しを使用して、既存のポリシーを更新します。

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

更新リクエストが正常に実行された場合は、HTTP ステータス 200（OK）が返され、本文には更新されたポリシーが表示されます。`id` は、リクエストで送信した `id` と一致します。

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

## ポリシーの一部を更新する

ポリシーの特定の部分を更新するには、PATCH リクエストを使用します。ポリシーを&#x200B;_書き換える_ PUT リクエストとは異なり、PATCH リクエストでは、リクエスト本文で指定されたパスのみが更新されます。更新する特定のパス（`/status`）およびその値（`ENABLE` または `DISABLE`）のみを送信できるので、このリクエストはポリシーを有効または無効にする場合に特に便利です。

The [!DNL Policy Service] API currently supports &quot;add&quot;, &quot;replace&quot;, and &quot;remove&quot; PATCH operations, and allows you to combine several updates together into a single call by adding each as an object within the array, as shown in the following examples.

**API 形式**

```http
PATCH /policies/custom/{id}
```

**リクエスト**

この例では、「replace」操作を使用して、ポリシーのステータスを「DRAFT」から「ENABLED」に変更し、説明フィールドを新しい説明に更新しています。また、次に示すように、「delete」操作を使用してポリシーの説明を削除し、次に「add」操作を使用して新しい説明を 1 回追加することで、説明フィールドを更新することもできます。

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

1 つのリクエストで複数の PATCH 操作を送信する場合、各操作は配列での表示順に処理されるので、リクエストを送信する際には操作を正しい順番で指定するようにしてください（必要な場合）。

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

更新リクエストが正常に実行されると、HTTP ステータス 200（OK）が返され、本文には更新されたポリシーが表示されます（「status」は「ENABLED」になり、「description」は変更されます）。ポリシーの `id` は、リクエストで送信した `id` と一致します。


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

## ポリシーを削除する

作成したポリシーを削除する必要がある場合は、削除するポリシーの `id` に DELETE リクエストを発行します。最初に検索（GET）リクエストを実行してポリシーを表示し、それが削除するポリシーであるかどうかを確認することをお勧めします。**削除したポリシーは復元できません。**

**API 形式**

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

ポリシーが正常に削除された場合、本文には HTTP ステータス 200（OK）のみが表示されます。

削除されたことを確認するには、ポリシーの検索（GET）をもう一度試みます。ポリシーが削除されているので、HTTP ステータス 404（Not Found）と「見つかりません」というエラーメッセージが表示されます。
