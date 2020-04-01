---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ使用ポリシーの作成
topic: policies
translation-type: tm+mt
source-git-commit: 04b2e07ba39df9f530c9c93c4bf1af9e2cf30169

---


# データ使用ポリシーの作成

Data Usage Labeling and Enforcement(DULE)は、Adobe Experience Platform Data Governanceの中核的なメカニズムです。 [DULE Policy Service APIを使用すると](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml) 、DULEポリシーを作成および管理して、特定のDULEラベルを含むデータに対して実行できるマーケティングアクションを決定できます。

このドキュメントでは、Policy Service APIを使用してDULEポリシーを作成する手順を説明するチュートリアルを提供します。 APIで利用可能な様々な操作の詳細なガイドについては、Policy Service開発者ガ [イドを参照してください](../api/getting-started.md)。

## はじめに

このチュートリアルでは、SCHEDULEポリシーの作成と評価に関わる次の主要概念について、十分に理解しておく必要があります。

* [データガバナンス](../home.md):プラットフォームがデータ使用のコンプライアンスを強制するフレームワーク。
* [データ使用ラベル](../labels/overview.md):データ使用ラベルはXDMデータフィールドに適用され、そのデータへのアクセス方法に関する制限を指定します。
* [エクスペリエンスデータモデル(XDM)](../../xdm/home.md):プラットフォームが顧客エクスペリエンスデータを整理する際に使用する標準化されたフレームワーク。
* [サンドボックス](../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

このチュートリアルを開始する前に、開発者ガイドを参照し [て](../api/getting-started.md) 、必要なヘッダーやサンプルAPI呼び出しを含む、DULE Policy Service APIの呼び出しを正しく行うために知っておく必要のある重要な情報を確認してください。

## マーケティングアクションの定義 {#define-action}

Data Governanceフレームワークでは、マーケティングアクションとは、Experience Platformのデータ消費者が行うアクションのことで、データ使用ポリシーの違反を確認する必要があります。

DULEポリシーを作成する最初の手順は、ポリシーが評価するマーケティングアクションを決定することです。 これは、次のいずれかのオプションを使用して行うことができます。

* [既存のマーケティングアクションの検索](#look-up)
* [新しいマーケティングアクションの作成](#create-new)

### 既存のマーケティングアクションの検索 {#look-up}

いずれかのエンドポイントにGETリクエストを行うことで、既存のマーケティングアクションを検索して、DULEポリシーで評価することがで `/marketingActions` きます。

**API形式**

エクスペリエンスプラットフォームで提供されるマーケティングアクションを検索するか、組織で作成されるカスタムマーケティングアクションを検索するかに応じて、エンドポイントを使 `marketingActions/core` 用するか、エ `marketingActions/custom` ンドポイントを使用します。

```http
GET /marketingActions/core
GET /marketingActions/custom
```

**リクエスト**

次のリクエストは、エンドポイ `marketingActions/custom` ントを使用して、IMS組織で定義されているすべてのマーケティングリストのアクションを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、検出されたマーケティングアクションの総数(`count`)と、配列内のマーケティングアクション自体の詳細をリストに返し `children` ます。

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
            "imsOrg": "{IMS_ORG}",
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
            "imsOrg": "{IMS_ORG}",
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
| `_links.self.href` | 配列内の各項目には、 `children` 一覧表示されたマーケティングアクションのURI IDが含まれます。 |

使用するマーケティングアクションが見つかったら、そのプロパティの値を記録し `href` ます。 この値は、DULEポリシーを作成する次の手 [順で使用されます](#create-policy)。

### 新しいマーケティングアクションの作成 {#create-new}

エンドポイントにPUTリクエストを作成し、リクエストパスの最後にマーケテ `/marketingActions/custom/` ィングアクションの名前を指定することで、新しいマーケティングアクションを作成できます。

**API形式**

```http
PUT /marketingActions/custom/{MARKETING_ACTION_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | 作成する新しいマーケティングアクションの名前。 この名前はマーケティングアクションの主な識別子として機能するので、一意である必要があります。 マーケティングアクションには、説明的で簡潔な名前を付けることをお勧めします。 |

**リクエスト**

次のリクエストでは、「exportToThirdParty」という新しいカスタムマーケティングアクションが作成されます。 リクエストペイ `name` ロード内の名前は、リクエストパスで指定された名前と同じであることに注意してください。

```shell
curl -X PUT \  
  https://platform.adobe.io/data/foundation/dulepolicy/marketingActions/custom/exportToThirdParty \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "exportToThirdParty",
      "description": "Export data to a third party"
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | 作成するマーケティングアクションの名前。 この名前は、要求パスで指定された名前と一致する必要があります。一致しないと、400（不正な要求）エラーが発生します。 |
| `description` | マーケティングアクションのわかりやすい説明。 |

**応答**

成功した応答は、HTTPステータス201（作成済み）と、新しく作成されたマーケティングアクションの詳細を返します。

```json
{
    "name": "exportToThirdParty",
    "description": "Export data to a third party",
    "imsOrg": "{IMS_ORG}",
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
| `_links.self.href` | マーケティングアクションのURI ID。 |

新しく作成したマーケティングアクションのURI IDを記録します。URI IDは、DULEポリシーの作成の次の手順で使用されます。

## MODULEポリシーの作成 {#create-policy}

新しいポリシーを作成するには、マーケティングアクションのURI IDと、そのマーケティングアクションを禁止するDULEラベルの式を指定する必要があります。

この式はポリシー **式と呼ばれ** 、(A) DULEラベル、または(B)演算子と演算値のどちらかを含むオブジェクトですが、両方を含まないオブジェクトです。 また、各オペランドもポリシー式オブジェクトです。 例えば、ラベルが存在する場合、サードパーティへのデータのエクスポートに関するポリシーは禁止さ `C1 OR (C3 AND C7)` れる可能性があります。 この式は次のように指定します。

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

>[!NOTE] ORおよびAND演算子のみがサポートされます。

ポリシー式を設定したら、エンドポイントにPOSTリクエストを作成して、新しいDULEポリシーを作成で `/policies/custom` きます。

**API形式**

```http
POST /policies/custom
```

**リクエスト**

次のリクエストは、リクエストペイロードにマーケティングアクションとポリシー式を指定することで、「Export Data to Third Party」と呼ばれるDULEポリシーを作成します。

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
| `marketingActionRefs` | 前の手順で取得 `href` したマーケティングアクションの値を含 [む配列](#define-action)。 上記の例では、1つのリストのみがマーケティングアクションとして扱われますが、複数のアクションを指定することもできます。 |
| `deny` | ポリシー式オブジェクト。 で参照されているマーケティングアクションをポリシーで拒否する原因となるDULEのラベルと条件を定義しま `marketingActionRefs`す。 |

**応答**

成功した応答は、HTTPステータス201（作成済み）と、新しく作成されたポリシーの詳細を返します。

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
    "imsOrg": "{IMS_ORG}",
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
| `id` | DULEポリシーを一意に識別する、読み取り専用のシステム生成値。 |

次の手順でポリシーを有効にする際に使用する、新しく作成したDULEポリシーのURI IDを記録します。

## DULEポリシーの有効化

>[!NOTE] この手順はオプションですが、SCHEDULEポリシーをステータスのままにする場合は、評価に参加するために、デフォルトでポリシーのステータスが「 `DRAFT` 」に設定されている必要が `ENABLED` あることに注意してください。 ステータスのポリシーに対する例 [外を作成する方法については](../enforcement/api-enforcement.md) 、「DULEポリシーの適用に関するチュートリアル」を参照して `DRAFT` ください。

デフォルトでは、プロパティが設定され `status` たDULEポリシ `DRAFT` ーは評価に参加しません。 エンドポイントにPATCHリクエストを作成し、そのポリシーの一意の識別子を要求パスの最後に `/policies/custom/` 指定することで、ポリシーの評価を有効にすることができます。

**API形式**

```http
PATCH /policies/custom/{POLICY_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{POLICY_ID}` | 有効 `id` にするポリシーの値。 |

**リクエスト**

次の要求は、DULEポリシーのプロパティに対し `status` てPATCH操作を実行し、値をからに変更 `DRAFT` します `ENABLED`。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/dulepolicy/policies/custom/5d51f322e553c814e67af1a3
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `op` | 実行するPATCH操作のタイプ。 このリクエストは「置換」操作を実行します。 |
| `path` | 更新するフィールドへのパス。 ポリシーを有効にする場合は、値を「/status」に設定する必要があります。 |
| `value` | で指定したプロパティに割り当てる新しい値 `path`。 この要求では、ポリシーのプロパティ `status` が「有効」に設定されます。 |

**応答**

正常な応答は、HTTPステータス200(OK)と、更新されたポリシーの詳細を返します。現在は、に設 `status` 定されていま `ENABLED`す。

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
    "imsOrg": "{IMS_ORG}",
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

このチュートリアルに従うと、マーケティングアクションのDULEポリシーが正常に作成されます。 これで、DULEポリシーの適用に関するチュートリアルを [続けて、ポリシー違反を確認し](../enforcement/api-enforcement.md) 、エクスペリエンスアプリケーションでそれらを処理する方法を学ぶことができます。

Policy Service APIで使用可能な様々な操作について詳しくは、Policy Service開発者ガイド [を参照してください](../api/getting-started.md)。 リアルタイム顧客プロファイルデータに対してDULEポリシーを適用する方法については、オーディエンスセグメントに対するデータ使用コンプライアンスの [適用に関するチュートリアルを参照してくださ](../../segmentation/tutorials/governance.md)い。