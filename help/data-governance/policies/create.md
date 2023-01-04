---
keywords: Experience Platform;ホーム;人気のトピック;データガバナンス;データ使用ポリシー
solution: Experience Platform
title: API でのデータガバナンスポリシーの作成
topic-legacy: policies
type: Tutorial
description: Policy Service API を使用してデータガバナンスポリシーを作成する方法を説明します。
exl-id: 8483f8a1-efe8-4ebb-b074-e0577e5a81a4
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '1200'
ht-degree: 91%

---

# API でのデータガバナンスポリシーの作成

この [ポリシーサービス API](https://www.adobe.io/experience-platform-apis/references/policy-service/) では、データガバナンスポリシーを作成および管理して、特定のデータ使用ラベルを含むデータに対して実行できるマーケティングアクションを決定できます。

このドキュメントでは、 [!DNL Policy Service] API

>[!NOTE]
>
>アクセス制御ポリシーを作成する手順については、 `/policies` エンドポイントガイド [アクセス制御 API](../../access-control/abac/api/policies.md). 同意ポリシーの作成方法については、 [policies UI ガイド](./user-guide.md#consent-policy).

## はじめに

このチュートリアルでは、ポリシーの作成と評価に関わる、次の主要な概念の実用的な知識が必要です。

* [Adobe Experience Platform データガバナンス](../home.md)：データ使用のコンプライアンスを [!DNL Platform] が適用するフレームワーク。
   * [データ使用ラベル](../labels/overview.md)：データ使用ラベルは、XDM データフィールドに適用され、そのデータのアクセス方法に関する制限を指定します。
* [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Platform] に使用される標準化されたフレームワーク。
* [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

このチュートリアルを開始する前に、[開発者ガイド](../api/getting-started.md)を参照して、必要なヘッダーやサンプル API 呼び出しを含む、[!DNL Policy Service] API の呼び出しを正しく実行するため、必要な重要な情報を確認してください。

## マーケティングアクションの定義 {#define-action}

データガバナンスフレームワークでは、マーケティングアクションとは、[!DNL Experience Platform] のデータ消費者が行うアクションのことで、データ使用ポリシーに違反していないかをチェックする必要があります。

データ使用ポリシーを作成する最初の手順は、ポリシーが評価するマーケティングアクションを決定することです。これは、次のいずれかのオプションを使用しておこなうことができます。

* [既存のマーケティングアクションの検索](#look-up)
* [新しいマーケティングアクションの作成](#create-new)

### 既存のマーケティングアクションの検索 {#look-up}

いずれかの `/marketingActions` エンドポイントに GET リクエストを実行すると、既存のマーケティングアクションを検索して、ポリシーで評価することができます。

**API 形式**

[!DNL Experience Platform] で提供されるマーケティングアクションを検索するか、組織で作成されるカスタムマーケティングアクションを検索するかに応じて、`marketingActions/core` エンドポイントまたは `marketingActions/custom` エンドポイントを使用します。

```http
GET /marketingActions/core
GET /marketingActions/custom
```

**リクエスト**

次のリクエストは、`marketingActions/custom` エンドポイントを使用して、IMS 組織で定義されているすべてのマーケティングリストのアクションを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

応答が成功すると、検出されたマーケティングアクションの総数（`count`）と、`children` 配列内のマーケティングアクション自体の詳細がリストに返されます。

```json
{
    "_page": {
        "start": "sampleMarketingAction",
        "count": 2
    },
    "_links": {
        "page": {
            "href": "https://platform.adobe.io/marketingActions/custom?{?limit,start,property}",
            "templated": true
        }
    },
    "children": [
        {
            "name": "sampleMarketingAction",
            "description": "Marketing Action description.",
            "imsOrg": "{ORG_ID}",
            "created": 1550714012088,
            "createdClient": "{CREATED_CLIENT}",
            "createdUser": "{CREATED_USER}",
            "updated": 1550714012088,
            "updatedClient": "{UPDATED_CLIENT}",
            "updatedUser": "{UPDATED_USER}",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io:443/data/foundation/dulepolicy/marketingActions/custom/sampleMarketingAction"
                }
            }
        },
        {
            "name": "newMarketingAction",
            "description": "Another marketing action.",
            "imsOrg": "{ORG_ID}",
            "created": 1550793833224,
            "createdClient": "{CREATED_CLIENT}",
            "createdUser": "{CREATED_USER}",
            "updated": 1550793833224,
            "updatedClient": "{UPDATED_CLIENT}",
            "updatedUser": "{UPDATED_USER}",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io:443/data/foundation/dulepolicy/marketingActions/custom/newMarketingAction"
                }
            }
        }
    ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `_links.self.href` | `children` 配列内の各項目には、一覧表示されたマーケティングアクションの URI ID が含まれます。 |

使用するマーケティングアクションが見つかったら、その `href` プロパティの値を記録します。この値は、次の手順で[ポリシーを作成](#create-policy)する際に使用されます。

### 新しいマーケティングアクションの作成 {#create-new}

`/marketingActions/custom/` エンドポイントに PUT リクエストを実行して、リクエストパスの最後にマーケティングアクションの名前を指定することで、新しいマーケティングアクションを作成できます。

**API 形式**

```http
PUT /marketingActions/custom/{MARKETING_ACTION_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | 作成する新しいマーケティングアクションの名前。この名前はマーケティングアクションの主識別子として機能するので、一意である必要があります。ベストプラクティスは、マーケティングアクションに説明的で簡潔な名前を付けることです。 |

**リクエスト**

次のリクエストでは、「exportToThirdParty」という新しいカスタムマーケティングアクションが作成されます。リクエストペイロード内の `name` は、リクエストパスで指定された名前と同じであることに注意してください。

```shell
curl -X PUT \  
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/exportToThirdParty \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "exportToThirdParty",
      "description": "Export data to a third party"
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | 作成するマーケティングアクションの名前。この名前は、リクエストパスで指定された名前と一致する必要があります。一致しないと、400（不正なリクエスト）エラーが発生します。 |
| `description` | マーケティングアクションのわかりやすい説明。 |

**応答** 

応答が成功すると、HTTP ステータス 201（作成済み）と、新しく作成されたマーケティングアクションの詳細が返されます。

```json
{
    "name": "exportToThirdParty",
    "description": "Export data to a third party",
    "imsOrg": "{ORG_ID}",
    "created": 1550713341915,
    "createdClient": "{CREATED_CLIENT}",
    "createdUser": "{CREATED_USER",
    "updated": 1550713856390,
    "updatedClient": "{UPDATED_CLIENT}",
    "updatedUser": "{UPDATED_USER}",
    "_links": {
        "self": {
            "href": "https://platform.adobe.io:443/data/foundation/dulepolicy/marketingActions/custom/exportToThirdParty"
        }
    }
}
```

| プロパティ | 説明 |
| --- | --- |
| `_links.self.href` | マーケティングアクションの URI ID。 |

新しく作成したマーケティングアクションの URI ID を記録します。この ID は、ポリシー作成の次の手順で使用します。

## ポリシーを作成する {#create-policy}

新しいポリシーを作成するには、マーケティングアクションの URI ID と、そのマーケティングアクションを禁止する使用ラベルの式を指定する必要があります。

この式はポリシー式と呼ばれ、（A）ラベル、または（B）演算子とオペランドのどちらか一つのみを含むオブジェクトです。また、各演算値もポリシー式オブジェクトです。例えば、`C1 OR (C3 AND C7)` ラベルが存在する場合、サードパーティへのデータの書き出しに関するポリシーは禁止される可能性があります。この式は次のように指定します。

```json
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
}
```

>[!NOTE]
>
> OR および AND 演算子のみがサポートされます。

ポリシー式を設定したら、`/policies/custom` エンドポイントに対して POST リクエストを実行し、新しいポリシーを作成できます。

**API 形式**

```http
POST /policies/custom
```

**リクエスト**

次のリクエストは、リクエストペイロードにマーケティングアクションとポリシー式を指定することで、「Export Data to Third Party」と呼ばれるポリシーを作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/dulepolicy/policies/custom \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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

| プロパティ | 説明 |
| --- | --- |
| `marketingActionRefs` | [前の手順](#define-action)で取得したマーケティングアクションの `href` 値を含む配列。上記の例では、1 つのリストのみがマーケティングアクションとして扱われますが、複数のアクションを指定することもできます。 |
| `deny` | ポリシー式オブジェクト。`marketingActionRefs` で参照されているマーケティングアクションをポリシーが拒否する原因となる使用ラベルと条件を定義します。 |

**応答** 

応答が成功すると、HTTP ステータス 201（作成済み）と、新しく作成されたポリシーの詳細が返されます。

```json
{
    "name": "Export Data to Third Party",
    "status": "DRAFT",
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
    "imsOrg": "{ORG_ID}",
    "created": 1565651746693,
    "createdClient": "{CREATED_CLIENT}",
    "createdUser": "{CREATED_USER",
    "updated": 1565651746693,
    "updatedClient": "{UPDATED_CLIENT}",
    "updatedUser": "{UPDATED_USER}",
    "_links": {
        "self": {
            "href": "https://platform-stage.adobe.io/data/foundation/dulepolicy/policies/custom/5d51f322e553c814e67af1a3"
        }
    },
    "id": "5d51f322e553c814e67af1a3"
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | データポリシーを一意に識別する、システムで生成された読み取り専用の値。 |

次の手順でポリシーを有効にする際に使用する、新しく作成したポリシーの URI ID を記録します。

## ポリシーの有効化

>[!NOTE]
>
>この手順はオプションですが、ポリシーを `DRAFT` ステータスのままにする場合、評価に参加するには、デフォルトでポリシーのステータスが `ENABLED` に設定されている必要があることに注意してください。`DRAFT` ステータスのポリシーに対する例外を作成する方法については、[ポリシーの適用](../enforcement/api-enforcement.md)に関するガイドを参照してください。

デフォルトでは、`status` プロパティが `DRAFT` に設定されたポリシーは評価に加わりません。`/policies/custom/` エンドポイントに PATCH リクエストを実行して、リクエストパスの最後にそのポリシーの一意の ID を指定することで、ポリシーの評価を有効にできます。

**API 形式**

```http
PATCH /policies/custom/{POLICY_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{POLICY_ID}` | 有効にするポリシーの `id` 値。 |

**リクエスト**

次のリクエストは、ポリシーの `status` プロパティに対して PATCH 操作を実行し、値を `DRAFT` から `ENABLED` に変更します。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/dulepolicy/policies/custom/5d51f322e553c814e67af1a3
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '[
    {
      "op": "replace",
      "path": "/status",
      "value": "ENABLED"
    }
  ]'
```

| プロパティ | 説明 |
| --- | --- |
| `op` | 実行する PATCH 操作のタイプ。このリクエストは「置換」操作を実行します。 |
| `path` | 更新するフィールドへのパス。ポリシーを有効にする場合は、値を「/status」に設定する必要があります。 |
| `value` | `path` で指定したプロパティに割り当てる新しい値。このリクエストでは、ポリシーの `status` プロパティが「有効」に設定されます。 |

**応答** 

応答が成功すると、HTTP ステータス 200（OK）と、更新されたポリシーの詳細が返されます。`status` は `ENABLED` に設定されています。

```json
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
    "imsOrg": "{ORG_ID}",
    "created": 1565651746693,
    "createdClient": "{CREATED_CLIENT}",
    "createdUser": "{CREATED_USER}",
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
```

## 次の手順

このチュートリアルでは、マーケティングアクションのデータ使用ポリシーを作成しました。[データ使用ポリシーの実施](../enforcement/api-enforcement.md)に関するチュートリアルを続けて、ポリシー違反を確認し、エクスペリエンスアプリケーションでそれらを処理する方法を学ぶことができます。

[!DNL Policy Service] API で使用可能な様々な操作について詳しくは、『[ポリシーサービス開発者ガイド](../api/getting-started.md)』を参照してください。[!DNL Real-Time Customer Profile] データに対してポリシーを適用する方法については、オーディエンスセグメント](../../segmentation/tutorials/governance.md)に対するデータ使用コンプライアンスの適用[に関するチュートリアルを参照してください。

[!DNL Experience Platform] ユーザーインターフェイスで使用ポリシーを管理する方法については、『[ポリシーユーザガイド](user-guide.md)』を参照してください。
