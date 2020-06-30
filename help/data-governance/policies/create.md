---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ使用ポリシーの作成
topic: policies
translation-type: tm+mt
source-git-commit: d4964231ee957349f666eaf6b0f5729d19c408de
workflow-type: tm+mt
source-wordcount: '1194'
ht-degree: 3%

---


# APIでのデータ使用ポリシーの作成

Data Usage Labeling and Enforcement(DULE)は、Adobe Experience Platformデータガバナンスの中核的なメカニズムです。 [DULE Policy Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml) (DULE Policy Service API)を使用すると、DULEポリシーを作成および管理して、特定のDULEラベルを含むデータに対して実行できるマーケティングアクションを決定できます。

このドキュメントでは、 [!DNL Policy Service] APIを使用してDULEポリシーを作成するための、手順を追ったチュートリアルを提供します。 APIで利用可能な様々な操作の詳細なガイドについては、 [Policy Service開発者ガイドを参照してください](../api/getting-started.md)。

## はじめに

このチュートリアルでは、DULEポリシーの作成と評価に関する次の主要概念について、十分に理解している必要があります。

* [Data Governance](../home.md): データ使用のコンプライアンスを [!DNL Platform] 適用するフレームワーク。
* [データ使用ラベル](../labels/overview.md): データ使用ラベルは、XDMデータフィールドに適用され、そのデータへのアクセス方法に関する制限を指定します。
* [Experience Data Model(XDM)](../../xdm/home.md): 顧客体験データを [!DNL Platform] 整理するための標準化されたフレームワーク。
* [サンドボックス](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

このチュートリアルを開始する前に、 [開発者ガイドを参照して](../api/getting-started.md) 、必要なヘッダーやAPI呼び出し例の読み方など、DULE [!DNL Policy Service] APIを正しく呼び出すために必要な重要な情報を確認してください。

## マーケティングアクションの定義 {#define-action}

この [!DNL Data Governance] フレームワークでは、マーケティングアクションとは、 [!DNL Experience Platform] データ消費者が行うアクションのことで、データ使用ポリシーの違反を確認する必要があります。

DULEポリシーを作成する最初の手順は、ポリシーが評価するマーケティングアクションを決定することです。 これは、次のいずれかのオプションを使用して行うことができます。

* [既存のマーケティングアクションの検索](#look-up)
* [新しいマーケティングアクションの作成](#create-new)

### 既存のマーケティングアクションの検索 {#look-up}

GETリクエストをエンドポイントの1つに作成することで、DULEポリシーで評価される既存のマーケティングアクションを検索でき `/marketingActions` ます。

**API形式**

組織が提供するマーケティングアクションを検索するか、組織が作成したカスタムマーケティングアクション [!DNL Experience Platform] を検索するかに応じて、それぞれ `marketingActions/core` のエンドポイントを使用し `marketingActions/custom` ます。

```http
GET /marketingActions/core
GET /marketingActions/custom
```

**リクエスト**

次のリクエストは、エンドポイントを使用します。 `marketingActions/custom` エンドポイントは、IMS組織によって定義されているすべてのマーケティングアクションのリストを取得します。

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
| `_links.self.href` | 配列内の各項目には、一覧表示されたマーケティングアクションのURI IDが含まれ `children` ます。 |

使用するマーケティングアクションが見つかったら、その `href` プロパティの値を記録します。 この値は、DULEポリシーを [作成する次の手順で使用されます](#create-policy)。

### 新しいマーケティングアクションの作成 {#create-new}

エンドポイントにPUTリクエストを作成し、リクエストパスの最後にマーケティングアクションの名前を指定することで、新しいマーケティングアクションを作成でき `/marketingActions/custom/` ます。

**API形式**

```http
PUT /marketingActions/custom/{MARKETING_ACTION_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{MARKETING_ACTION_NAME}` | 作成する新しいマーケティングアクションの名前。 この名前はマーケティングアクションの主な識別子として機能するので、一意である必要があります。 マーケティングアクションにわかりやすいが簡潔な名前を付けることをお勧めします。 |

**リクエスト**

次のリクエストは、「exportToThirdParty」という新しいカスタムマーケティングアクションを作成します。 リクエストペイロード `name` 内のは、リクエストパスで指定された名前と同じであることに注意してください。

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
| `name` | 作成するマーケティングアクションの名前。 この名前は、要求パスに指定された名前と一致する必要があります。一致しないと、400(Bad Request)エラーが発生します。 |
| `description` | マーケティングアクションに関する人間が読める説明です。 |

**応答**

正常な応答が返されると、HTTPステータス201（作成済み）と、新しく作成されたマーケティングアクションの詳細が返されます。

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

新しく作成したマーケティングアクションのURI IDを記録します。URI IDは、DULEポリシーを作成する次の手順で使用します。

## SCHEDULEポリシーの作成 {#create-policy}

新しいポリシーを作成するには、マーケティングアクションのURI IDと、そのマーケティングアクションを禁止するDULEラベルの式を指定する必要があります。

この式は **ポリシー式と呼ばれ** 、(A) DULEラベル、または(B)演算子と演算値のどちらかを含むオブジェクトですが、両方を含むわけではありません。 また、各オペランドもポリシー式オブジェクトです。 例えば、データのサードパーティへのエクスポートに関するポリシーは、ラベルが存在する場合は禁止され `C1 OR (C3 AND C7)` る可能性があります。 この式は次のように指定します。

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

ポリシー式を設定したら、エンドポイントにPOSTリクエストを行って、新しいDULEポリシーを作成でき `/policies/custom` ます。

**API形式**

```http
POST /policies/custom
```

**リクエスト**

次のリクエストは、リクエストペイロードにマーケティングアクションとポリシーの式を指定して、「Export Data to Third Party」というDULEポリシーを作成します。

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
| `marketingActionRefs` | `href` 前の手順で取得したマーケティングアクションの [値を含む配列](#define-action)。 上記の例では、1つのマーケティングリストのみが行われますが、複数のアクションを指定することもできます。 |
| `deny` | ポリシー式オブジェクトです。 で参照されるマーケティングアクションをポリシーが拒否する原因となるDULEのラベルと条件を定義し `marketingActionRefs`ます。 |

**応答**

正常に応答すると、HTTPステータス201（作成済み）と、新しく作成されたポリシーの詳細が返されます。

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
| `id` | DULEポリシーを一意に識別する、読み取り専用の、システム生成値。 |

次の手順でポリシーを有効にする際に使用する、新しく作成したDULEポリシーのURI IDを記録します。

## DULEポリシーの有効化

>[!NOTE] この手順はオプションですが、SCHEDULEポリシーのステータスを変更しない場合は、デフォルトで、評価に参加するにはポリシーのステータスがに設定され `DRAFT``ENABLED` ている必要があります。 ステータスのポリシーに対して例外を行う方法については、 [DULEポリシーの](../enforcement/api-enforcement.md) 適用に関するチュートリアルを参照してく `DRAFT` ださい。

デフォルトでは、プロパティが「 `status` 」に設定されているSCHEDULEポリシーは評価に `DRAFT` 関与しません。 エンドポイントにPATCH要求を行い、要求パスの最後にポリシーの一意の識別子を指定することで、評価用のポリシーを有効にすることがで `/policies/custom/` きます。

**API形式**

```http
PATCH /policies/custom/{POLICY_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{POLICY_ID}` | 有効にするポリシーの `id` 値。 |

**リクエスト**

次の要求は、DULEポリシーの `status` プロパティに対してPATCH操作を実行し、値をからに変更 `DRAFT` し `ENABLED`ます。

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
| `path` | 更新するフィールドへのパス。 ポリシーを有効にする場合、値を「/status」に設定する必要があります。 |
| `value` | で指定したプロパティに割り当てる新しい値 `path`。 この要求では、ポリシーのプロパティが「ENABLED」に設定され `status` ます。 |

**応答**

正常に応答すると、HTTPステータス200 (OK)と、更新されたポリシーの詳細が返されます。 `status` 現在は、に設定されて `ENABLED`います。

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

このチュートリアルに従うと、マーケティングアクションのデータ使用ポリシーが正しく作成されました。 データ使用ポリシーの [](../enforcement/api-enforcement.md) 適用に関するチュートリアルを続行して、ポリシー違反を確認し、エクスペリエンスアプリケーションでそれらを処理する方法を学ぶことができます。

APIで使用可能な様々な操作について詳しくは、 [!DNL Policy Service] Policy Service開発者ガイドを参照してください [](../api/getting-started.md)。 データに対してポリシーを適用する方法については、オーディエンスセグメントに対するデータの使用に対するコンプライアンスの [!DNL Real-time Customer Profile] 適用に関するチュートリアル [](../../segmentation/tutorials/governance.md)を参照してください。

ユーザーインターフェイスで使用ポリシーを管理する方法については、「 [!DNL Experience Platform][ポリシーユーザーガイド](user-guide.md)」を参照してください。