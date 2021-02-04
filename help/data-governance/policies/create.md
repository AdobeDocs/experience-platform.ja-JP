---
keywords: Experience Platform；ホーム；人気の高いトピック；データガバナンス；データ使用ポリシー
solution: Experience Platform
title: データ使用ポリシーの作成
topic: policies
type: Tutorial
description: Policy Service APIを使用すると、データ使用ポリシーを作成および管理して、特定のデータ使用ラベルを含むデータに対して実行できるマーケティングアクションを決定できます。 このドキュメントでは、Policy Service APIを使用してポリシーを作成するための手順を説明するチュートリアルを提供します。
translation-type: tm+mt
source-git-commit: 8c94d3631296c1c3cc97501ccf1a3ed995ec3cab
workflow-type: tm+mt
source-wordcount: '1211'
ht-degree: 49%

---


# APIでのデータ使用ポリシーの作成

[Policy Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml)を使用すると、データ使用ポリシーを作成および管理して、特定のデータ使用ラベルを含むデータに対して実行できるマーケティングアクションを決定できます。

このドキュメントでは、[!DNL Policy Service] APIを使用してポリシーを作成するための手順を説明するチュートリアルを提供します。 API で利用可能な様々な操作についての詳しいガイドは、『[ポリシーサービス開発者ガイド](../api/getting-started.md)』を参照してください。

## はじめに

このチュートリアルでは、ポリシーの作成と評価に関わる次の主要概念について、十分に理解している必要があります。

* [[!DNL Data Governance]](../home.md):データ使用のコンプライアンスを [!DNL Platform] 強制するフレームワーク。
* [データ使用ラベル](../labels/overview.md)：データ使用ラベルは、XDM データフィールドに適用され、そのデータのアクセス方法に関する制限を指定します。
* [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):顧客体験データを [!DNL Platform] 編成する際に使用される標準化されたフレームワーク。
* [サンドボックス](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

このチュートリアルを始める前に、[開発者ガイド](../api/getting-started.md)を参照し、必要なヘッダーやAPI呼び出し例の読み方を含む[!DNL Policy Service] APIの呼び出しを正しく行うために必要な重要な情報を確認してください。

## マーケティングアクションの定義 {#define-action}

[!DNL Data Governance]フレームワークでは、マーケティングアクションとは、[!DNL Experience Platform]データコンシューマーが行うアクションです。この場合、データ使用ポリシーの違反を確認する必要があります。

データ使用ポリシーを作成する最初の手順は、ポリシーが評価するマーケティングアクションを決定することです。 これは、次のいずれかのオプションを使用しておこなうことができます。

* [既存のマーケティングアクションの検索](#look-up)
* [新しいマーケティングアクションの作成](#create-new)

### 既存のマーケティングアクションの検索 {#look-up}

`/marketingActions`エンドポイントの1つにGETリクエストを行うことで、ポリシーで評価される既存のマーケティングアクションを検索できます。

**API 形式**

[!DNL Experience Platform]が提供するマーケティングアクションを検索するか、組織が作成したカスタムマーケティングアクションを検索するかに応じて、それぞれ`marketingActions/core`エンドポイントまたは`marketingActions/custom`エンドポイントを使用します。

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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、検出されたマーケティングアクションの総数（`count`）と、`children` 配列内のマーケティングアクション自体の詳細をリストに返します。

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
| `_links.self.href` | `children` 配列内の各項目には、一覧表示されたマーケティングアクションの URI ID が含まれます。 |

使用するマーケティングアクションが見つかったら、その `href` プロパティの値を記録します。この値は、[ポリシー](#create-policy)の作成の次の手順で使用されます。

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
| `name` | 作成するマーケティングアクションの名前。この名前は、リクエストパスで指定された名前と一致する必要があります。一致しないと、400（不正なリクエスト）エラーが発生します。 |
| `description` | マーケティングアクションのわかりやすい説明。 |

**応答** 

正常な応答は、HTTP ステータス 201（作成済み）と、新しく作成されたマーケティングアクションの詳細を返します。

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
| `_links.self.href` | マーケティングアクションの URI ID。 |

新しく作成したマーケティングアクションのURI IDを記録します。URI IDは、ポリシーを作成する次の手順で使用します。

## ポリシーを作成する {#create-policy}

新しいポリシーを作成するには、マーケティングアクションのURI IDと、そのマーケティングアクションを禁止する使用ラベルの式を指定する必要があります。

この式はポリシー式と呼ばれ、(A)ラベル、または(B)演算子と演算値のどちらかを含むオブジェクトですが、両方を含むわけではありません。 また、各演算値もポリシー式オブジェクトです。例えば、`C1 OR (C3 AND C7)` ラベルが存在する場合、サードパーティへのデータの書き出しに関するポリシーは禁止される可能性があります。この式は次のように指定します。

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

ポリシー式を設定したら、`/policies/custom`エンドポイントにPOST要求を行って、新しいポリシーを作成できます。

**API 形式**

```http
POST /policies/custom
```

**リクエスト**

次のリクエストは、リクエストペイロードにマーケティングアクションとポリシー式を指定して、「データをサードパーティにエクスポート」という名前のポリシーを作成します。

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
| `marketingActionRefs` | [前の手順](#define-action)で取得したマーケティングアクションの `href` 値を含む配列。上記の例では、1 つのリストのみがマーケティングアクションとして扱われますが、複数のアクションを指定することもできます。 |
| `deny` | ポリシー式オブジェクト。`marketingActionRefs`で参照されているマーケティングアクションをポリシーが拒否する原因となる使用方法のラベルと条件を定義します。 |

**応答** 

正常な応答は、HTTP ステータス 201（作成済み）と、新しく作成されたポリシーの詳細を返します。

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
| `id` | ポリシーを一意に識別する、読み取り専用の、システム生成の値です。 |

次の手順でポリシーを有効にする際に使用する、新しく作成したポリシーのURI IDを記録します。

## ポリシーの有効化

>[!NOTE]
>
>この手順は`DRAFT`ステータスのままにする場合はオプションですが、評価に参加するには、デフォルトでポリシーのステータスを`ENABLED`に設定する必要があります。 `DRAFT`ステータスのポリシーに例外を設定する方法については、[ポリシーの適用](../enforcement/api-enforcement.md)のガイドを参照してください。

デフォルトでは、`status`プロパティが`DRAFT`に設定されているポリシーは評価に関与しません。 `/policies/custom/` エンドポイントに PATCH リクエストを実行して、リクエストパスの最後にそのポリシーの一意の識別子を指定することで、ポリシーの評価を有効にできます。

**API 形式**

```http
PATCH /policies/custom/{POLICY_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{POLICY_ID}` | 有効にするポリシーの `id` 値。 |

**リクエスト**

次の要求は、ポリシーの`status`プロパティに対してPATCH操作を実行し、値を`DRAFT`から`ENABLED`に変更します。

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
| `op` | 実行する PATCH 操作のタイプ。このリクエストは「置換」操作を実行します。 |
| `path` | 更新するフィールドへのパス。ポリシーを有効にする場合は、値を「/status」に設定する必要があります。 |
| `value` | `path` で指定したプロパティに割り当てる新しい値。このリクエストでは、ポリシーの `status` プロパティが「有効」に設定されます。 |

**応答** 

正常な応答は、HTTP ステータス 200（OK）と、更新されたポリシーの詳細を返します。`status` は `ENABLED` に設定されています。

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

このチュートリアルでは、マーケティングアクションのデータ使用ポリシーを作成しました。[データ使用ポリシーの実施](../enforcement/api-enforcement.md)に関するチュートリアルを続けて、ポリシー違反を確認し、エクスペリエンスアプリケーションでそれらを処理する方法を学ぶことができます。

[!DNL Policy Service] APIで利用可能な様々な操作について詳しくは、[Policy Service開発者ガイド](../api/getting-started.md)を参照してください。 [!DNL Real-time Customer Profile]データに対してポリシーを適用する方法については、[オーディエンスセグメント](../../segmentation/tutorials/governance.md)に対するデータ使用のコンプライアンスの適用に関するチュートリアルを参照してください。

[!DNL Experience Platform]ユーザーインターフェイスで使用ポリシーを管理する方法については、[policy user guide](user-guide.md)を参照してください。