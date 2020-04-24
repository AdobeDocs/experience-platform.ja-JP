---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ポリシー
topic: developer guide
translation-type: tm+mt
source-git-commit: 08d02e7323f75c450e7a250835f26a569685cdd1

---


# ポリシー

データ使用ポリシーは、組織が採用するルールで、エクスペリエンスプラットフォーム内のデータで実行を許可（制限）されるマーケティングアクションの種類を示します。

エンドポ `/policies` イントは、データ使用ポリシーの表示、作成、更新または削除に関連するすべてのAPI呼び出しに使用されます。

## リストすべてのポリシー

ポリシーのリストを表示するには、指定したコンテナに対してGETリクエストを行う `/policies/core` か、ま `/policies/custom` たは指定したポリシーのすべてのポリシーを返す。

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

応答には、指定したコンテナ内のポリシーの合計数と、そのポリシーを含む各ポリシーの詳細を示す「カウント」が含まれま `id`す。 このフ `id` ィールドは、表示固有のポリシーに対する参照要求の実行、および更新と削除の操作の実行に使用されます。

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

各ポリシーには、特定 `id` のポリシーの詳細を要求するために使用できるフィールドが含まれています。 ポリシーの `id` 内容が不明な場合は、前の手順で示したように、特定のコンテナ（または）内のすべてのポリシーをリストするために、リスト(GET)リクエストを使用して見つけることがで`core``custom`きます。

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

応答には、 `id` （このフィールドはリクエストで送信されたものと一致する必要がある）、 `id` 、 `name`、およびなどのキーフィールドや、ポリシーの基になるマーケティングアクションへの参照リンク( `status``description``marketingActionRefs`)が含まれます。

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

ポリシーを作成するには、マーケティングアクションを、そのマーケティングアクションを禁止するDULEラベルの式と共に含める必要があります。 ポリシー定義には、DULEラベルの存 `deny` 在に関するブール式であるプロパティを含める必要があります。

この式は「a」と呼ば `PolicyExpression` れ、ラベルまたは演算子と演算 _値_ (両方では _な_ い)を含むオブジェクトです。 これに対し、各演算値もオブジェクト `PolicyExpression` です。 例えば、ラベルが存在する場合、サードパーティへのデータのエクスポートに関するポリシーは禁止さ `C1 OR (C3 AND C7)` れる可能性があります。 この式は次のように指定します。

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

正常に作成された場合は、HTTPステータス201 （作成済み）が返され、応答本文には、新しく作成されたポリシーの詳細が含まれま `id`す。 この値は読み取り専用で、ポリシーの作成時に自動的に割り当てられます。

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

データ使用ポリシーの作成後、そのポリシーを更新する必要がある場合があります。 これは、ポリシーの更新されたフォーム全体を含むペ `id` イロードを使用して、ポリシーに対するPUT要求を通じて行われます。 つまり、PUT要求は基本的にポリシーを書き換え _るので_ 、本体には以下の例に示すように、必要なすべての情報を含める必要があります。

**API形式**

```http
PUT /policies/custom/{id}
```

**リクエスト**

この例では、データをサードパーティにエクスポートするための条件が変更され、データラベルが存在する場合は、このマーケティングアクションを拒否するために作成したポリシーが `C1 AND (C3 OR C7)` 必要になりました。 次の呼び出しを使用して、既存のポリシーを更新します。

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

更新要求が成功すると、HTTPステータス200(OK)が返され、応答本文に更新されたポリシーが表示されます。 は、リク `id` エストで送信され `id` たものと一致する必要があります。

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

ポリシーの特定の部分は、PATCHリクエストを使用して更新できます。 ポリシーを書き換えるPUT _要求とは異な_ り、PATCH要求は、要求本文で指定されたパスのみを更新します。 これは、更新する特定のパス（または）とその値(または`/status`)のみを送信する必要があるので、ポリシーを有効または無効にする場合に特に便`ENABLE` 利 `DISABLE`です。

Policy Service APIは、現在、「add」、「replace」、「remove」の各PATCH操作をサポートしており、次の例に示すように、複数の更新を1回の呼び出しに組み合わせて、各更新を配列内のオブジェクトとして追加できます。

**API形式**

```http
PATCH /policies/custom/{id}
```

**リクエスト**

この例では、「replace」操作を使用して、ポリシーのステータスを「DRAFT」から「ENABLED」に変更し、説明フィールドを新しい説明で更新します。 また、「delete」操作を使用してポリシーの説明を削除し、「add」操作を使用して新しい1回だけ追加することで、次のように説明フィールドを更新することもできます。

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

1回のリクエストで複数のPATCH操作を送信する場合は、配列内での表示順に処理されるので、必要に応じてリクエストを正しい順序で送信するようにしてください。

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

更新要求が成功すると、HTTPステータス200 (OK)が返され、応答本文に更新されたポリシーが表示されます（「status」は「ENABLED」になり、「description」は変更されました）。 ポリシーは、 `id` 要求で送信され `id` たものと一致する必要があります。


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

作成したポリシーを削除する必要がある場合は、削除するポリシーのに対してDELETEリクエストを発行す `id` ることで削除できます。 まず参照(GET)要求を実行して、ポリシーを表示し、削除する正しいポリシーであることを確認することをお勧めします。 **削除したポリシーは復元できません。**

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

ポリシーが正常に削除された場合、応答本文はHTTPステータス200(OK)で空白になります。

ポリシーを再度参照(GET)すると、削除を確認できます。 ポリシーが削除されたので、HTTPステータス404（見つかりません）と「見つかりません」というエラーメッセージが表示されます。
