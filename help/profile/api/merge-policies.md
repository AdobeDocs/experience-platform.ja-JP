---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: リアルタイムの顧客プロファイルAPI開発ガイド
topic: guide
translation-type: tm+mt
source-git-commit: 33091568c850375b399435f375e854667493152c
workflow-type: tm+mt
source-wordcount: '2057'
ht-degree: 3%

---


# 結合ポリシー

Adobe Experience Platformを使用すると、複数のソースからデータを統合し、それを組み合わせて、個々の顧客の完全な表示を確認できます。 このデータを統合する場合、統合ポリシーは、データの優先順位付け方法と、統合されたビューを作成するためにどのデータを組み合わせるかを決定するためにプラットフォームで使用されるルールです。RESTful APIまたはユーザーインターフェイスを使用して、新しい結合ポリシーを作成し、既存のポリシーを管理し、組織のデフォルトの結合ポリシーを設定できます。 このガイドでは、APIを使用した結合ポリシーの操作手順を示します。 UIを使用してマージポリシーを操作するには、 [マージポリシーユーザーガイドを参照してください](../ui/merge-policies.md)。

## はじめに

このガイドで使用されるAPIエンドポイントは、リアルタイム顧客プロファイルAPIの一部です。 先に進む前に、 [Real-time Customer Customer API開発者ガイドを参照してください](getting-started.md)。 特に、プロファイル開発ガイドの「 [はじめに](getting-started.md#getting-started) 」の節には、関連トピックへのリンク、このドキュメントのサンプルAPI呼び出しを読むためのガイド、Experience Platform APIの呼び出しを成功させるために必要なヘッダーに関する重要な情報が含まれています。

## 結合ポリシーのコンポーネント {#components-of-merge-policies}

マージポリシーはIMS組織に対して非公開であり、異なるポリシーを作成して、スキーマを必要に応じて結合できます。 プロファイルデータにアクセスするAPIにはマージポリシーが必要ですが、明示的に指定されない場合はデフォルトが使用されます。 プラットフォームは、デフォルトの結合ポリシーを提供します。または、特定のスキーマに結合ポリシーを作成し、それを組織のデフォルトとしてマークすることができます。 各組織は、スキーマごとに複数の結合ポリシーを持つことができますが、各スキーマは、1つのデフォルトの結合ポリシーしか持つことができません。 デフォルトとして設定されたマージポリシーは、スキーマ名が指定され、マージポリシーが必須で指定されていない場合に使用されます。 マージポリシーをデフォルトとして設定すると、以前にデフォルトとして設定された既存のマージポリシーは自動的に更新され、デフォルトとして使用されなくなります。

### マージポリシーオブジェクトの完了

complete merge policyオブジェクトは、プロファイルフラグメントのマージ側面を制御する一連の環境設定を表します。

**Merge policyオブジェクト**

```json
    {
        "id": "{MERGE_POLICY_ID}",
        "name": "{NAME}",
        "imsOrgId": "{IMS_ORG}",
        "schema": {
            "name": "{SCHEMA_NAME}"
        },
        "version": 1,
        "identityGraph": {
            "type": "{IDENTITY_GRAPH_TYPE}"
        },
        "attributeMerge": {
            "type": "{ATTRIBUTE_MERGE_TYPE}"
        },
        "default": {BOOLEAN},
        "updateEpoch": {UPDATE_TIME}
    }
```

| プロパティ | 説明 |
|---|---|
| `id` | 作成時に割り当てられた、システムで生成された一意の識別子 |
| `name` | 結合ポリシーをリスト表示で識別できるわかりやすい名前。 |
| `imsOrgId` | このマージポリシーが属する組織ID |
| `identityGraph` | [関連するIDの取得元となるIDグラフを示すIDグラフ](#identity-graph) ・オブジェクト。 すべての関連IDで見つかったプロファイルフラグメントは結合されます。 |
| `attributeMerge` | [データの競合が発生した場合にマージポリシーがプロファイル属性値に優先順位を付ける方法を示す属性マージ](#attribute-merge) ・オブジェクト。 |
| `schema` | マージポリシーを使用できる [スキーマ](#schema) ・オブジェクト。 |
| `default` | このマージポリシーが指定したスキーマのデフォルトであるかどうかを示すブール値です。 |
| `version` | プラットフォームは結合ポリシーのバージョンを保持しました。 この読み取り専用の値は、マージポリシーが更新されるたびに増加します。 |
| `updateEpoch` | マージポリシーの最後の更新日。 |

**マージポリシーの例**

```json
    {
        "id": "10648288-cda5-11e8-a8d5-f2801f1b9fd1",
        "name": "profile-default",
        "imsOrgId": "{IMS_ORG}",
        "schema": {
            "name": "_xdm.context.profile"
        },
        "version": 1,
        "identityGraph": {
            "type": "none"
        },
        "attributeMerge": {
            "type": "timestampOrdered"
        },
        "default": true,
        "updateEpoch": 1551660639
    }
```

### 識別グラフ {#identity-graph}

[Adobe Experience Platform Identity Service](../../identity-service/home.md) は、グローバルに使用し、エクスペリエンスプラットフォーム上の各組織で使用するIDグラフを管理します。 マージポリシーの `identityGraph` 属性は、ユーザーの関連IDの決定方法を定義します。

**identityGraphオブジェクト**

```json
    "identityGraph": {
        "type": "{IDENTITY_GRAPH_TYPE}"
    }
```

は、次のい `{IDENTITY_GRAPH_TYPE}` ずれかの値です。

* **&quot;none&quot;:** IDの切り替えを実行しません。
* **&quot;pdg&quot;:** プライベートIDグラフに基づいてIDの切り替えを実行します。

**例`identityGraph`**

```json
    "identityGraph": {
        "type": "pdg"
    }
```

### 属性の結合 {#attribute-merge}

プロファイルフラグメントとは、特定のユーザーに対して存在するIDのリストのうち、1つのIDに対するプロファイル情報のことです。 使用するIDグラフのタイプが複数のIDになる場合、プロファイルのプロパティと優先度の値が競合する可能性があります。 を使用 `attributeMerge`すると、結合の競合のイベント時に優先順位を付けるデータセットプロファイルの値を指定できます。

**attributeMergeオブジェクト**

```json
    "attributeMerge": {
        "type": "{ATTRIBUTE_MERGE_TYPE}"
    }
```

は、次のい `{ATTRIBUTE_MERGE_TYPE}` ずれかの値です。

* **&quot;timestampOrdered&quot;**: （デフォルト）競合が発生した場合に最後に更新されたプロファイルを優先します。 このマージ・タイプを使用する場合、 `data` 属性は不要です。
* **&quot;dataSetPrecedence&quot;** : プロファイルフラグメントの送信元のデータセットに基づいて、そのフラグメントを優先します。 これは、あるデータセットに存在する情報が、別のデータセットのデータよりも優先されたり、信頼されたりする場合に使用できます。 このマージ・タイプを使用する場合、属性は必須です。属性は優先順にデータセットをリストするために必要です。 `order`
   * **&#39;&#39;order&#39;**: &quot;dataSetPrecedence&quot;を使用する場合は、配列にデータセットのリストを指定する必要があり `order` ます。 リストに含まれていないデータセットはマージされません。 つまり、プロファイルに統合するデータセットは、明示的にリストする必要があります。 アレイは、優先順にデータセットのIDをリストします。 `order`

**dataSetPrecedence型を使用するattributeMergeオブジェクトの例**

```json
    "attributeMerge": {
        "type": "dataSetPrecedence",
        "order" : [
            "dataSetId_2", 
            "dataSetId_3", 
            "dataSetId_1", 
            "dataSetId_4"
        ]
    }
```

**timestampOrdered型を使用するattributeMergeオブジェクトの例**

```json
    "attributeMerge": {
        "type": "timestampOrdered"
    }
```

### スキーマ {#schema}

スキーマオブジェクトは、このマージポリシーを作成するXDMスキーマを指定します。

**`schema`object **

```json
    "schema": {
        "name": "{SCHEMA_NAME}"
    }
```

の値 `name` は、マージポリシーに関連付けられたスキーマが基になるXDMクラスの名前です。

**例`schema`**

```json
    "schema": {
        "name": "_xdm.context.profile"
    }
```

## アクセス結合ポリシー {#access-merge-policies}

エンドポイントでは、リアルタイム顧客プロファイルAPIを使用して、特定のマージポリシーをIDで表示するか、特定の条件でフィルタリングしたIMS組織のすべてのマージポリシーにアクセスするためのルックアップリクエストを実行できます。 `/config/mergePolicies` また、エンドポイントを使用して、ID別に複数のマージポリシーを取得することもで `/config/mergePolicies/bulk-get` きます。 これらの各呼び出しを実行する手順を、以下の各節で説明します。

### IDによる単一の結合ポリシーへのアクセス

エンドポイントにGETリクエストを行い、リクエストパスにを含めることで、IDを使用して1つのマージポリシーにアクセ `/config/mergePolicies` ス `mergePolicyId` できます。

**API形式**

```http
GET /config/mergePolicies/{mergePolicyId}
```

| パラメーター | 説明 |
|---|---|
| `{mergePolicyId}` | 削除するマージポリシーの識別子。 |

**リクエスト**

```shell
curl -X GET \
  'https://platform.adobe.io/data/core/ups/config/mergePolicies/10648288-cda5-11e8-a8d5-f2801f1b9fd1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}
```

**応答**

正常に応答すると、マージポリシーの詳細が返されます。

```json
{
    "id": "10648288-cda5-11e8-a8d5-f2801f1b9fd1",
    "imsOrgId": "{IMS_ORG}",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "version": 1,
    "identityGraph": {
        "type": "pdg"
    },
    "attributeMerge": {
        "type": "timestampOrdered"
    },
    "default": false,
    "updateEpoch": 1551127597
}
```

結合ポリシーを構成する個々の要素の詳細については、このドキュメントの最初の [](#components-of-merge-policies) 結合ポリシーのコンポーネントに関する節を参照してください。

### ID別に複数のマージポリシーを取得する

エンドポイントにPOSTリクエストを行い、取得するマージポリシーのIDをリクエスト本文に含めることで、複数のマージポリシーを取得でき `/config/mergePolicies/bulk-get` ます。

**API形式**

```http
POST /config/mergePolicies/bulk-get
```

**リクエスト**

要求本文には、詳細を取得する各マージポリシーの「id」を含む個々のオブジェクトを持つ「ids」配列が含まれます。

```shell
curl -X POST \
  'https://platform.adobe.io/data/core/ups/config/mergePolicies/bulk-get' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "ids": [
          {
            "id": "0bf16e61-90e9-4204-b8fa-ad250360957b"
          }
          {
            "id": "42d4a596-b1c6-46c0-994e-ca5ef1f85130"
          }
        ]
      }'
```

**応答**

正常な応答は、HTTP Status 207(Multi-Status)と、POSTリクエストでIDが提供されたマージポリシーの詳細を返します。

```json
{
    "id": "0bf16e61-90e9-4204-b8fa-ad250360957b",
    "name": "Profile Default Merge Policy",
    "imsOrgId": "{IMS_ORG}",
    "sandbox": {
        "sandboxId": "ff0f6870-c46d-11e9-8ca3-036939a64204",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "schema": {
        "name": "_xdm.context.profile"
    },
    "version": 1,
    "identityGraph": {
        "type": "none"
    },
    "attributeMerge": {
        "type": "timestampOrdered"
    },
    "default": true,
    "updateEpoch": 1552086578
},
{
    "id": "42d4a596-b1c6-46c0-994e-ca5ef1f85130",
    "name": "Dataset Precedence Merge Policy",
    "imsOrgId": "{IMS_ORG}",
    "sandbox": {
        "sandboxId": "ff0f6870-c46d-11e9-8ca3-036939a64204",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "schema": {
        "name": "_xdm.context.profile"
    },
    "version": 1,
    "identityGraph": {
        "type": "pdg"
    },
    "attributeMerge": {
        "type": "dataSetPrecedence",
        "order": [
            "5b76f86b85d0e00000be5c8b",
            "5b76f8d787a6af01e2ceda18"
        ]
    },
    "default": false,
    "updateEpoch": 1576099719
}
```

結合ポリシーを構成する個々の要素の詳細については、このドキュメントの最初の [結合ポリシーのコンポーネントに関する節を参照してください](#components-of-merge-policies) 。

### 条件別の複数の結合ポリシーのリスト

エンドポイントにGETリクエストを発行し、オプションのクエリパラメーターを使用して応答のフィルタリング、順序付け、ページネーションを行うことで、IMS組織内で複数のマージポリシーをリストできます。 `/config/mergePolicies` 複数のパラメーターを含める場合は、アンパサンド(&amp;)で区切ります。 パラメーターを指定しないでこのエンドポイントを呼び出すと、組織で使用可能なすべての結合ポリシーが取得されます。

**API形式**

```http
GET /config/mergePolicies?{QUERY_PARAMS}
```

| パラメーター | 説明 |
|---|---|
| `default` | マージポリシーがスキーマクラスのデフォルトであるかどうかによってフィルターが結果を出すboolean値です。 |
| `limit` | ページに含める結果の数を制御するためのページサイズ制限を指定します。 デフォルト値： 20 |
| `orderBy` | 結果の並べ替えの基準となるフィールド `orderBy=name` または名前 `orderBy=+name` で昇順に並べ替えるか、降順に並べ替えるかを指定し `orderBy=-name`ます。 この値を省略すると、のデフォルトの並べ替え順が昇順 `name` になります。 |
| `schema.name` | 使用可能なマージポリシーを取得するスキーマの名前。 |
| `identityGraph.type` | フィルターは、IDグラフのタイプで結果を返します。 有効な値は、「none」と「pdg」（プライベートグラフ）です。 |
| `attributeMerge.type` | 使用される属性結合タイプによってフィルターが結果が出ます。 指定可能な値には、「timestampOrdered」および「dataSetPrecedence」があります。 |
| `start` | ページオフセット — 取得するデータの開始IDを指定します。 デフォルト値： 0 |
| `version` | 特定のバージョンのマージポリシーを使用する場合は、これを指定します。 デフォルトでは、最新バージョンが使用されます。 |

、、 `schema.name`およびに関する詳細は、このガイドで前述した結合ポリシーの `identityGraph.type`コンポー `attributeMerge.type`ネント [](#components-of-merge-policies) ・セクションを参照してください。


**リクエスト**

次のリクエストリストは、特定のスキーマのすべてのマージポリシーを指定します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/core/ups/config/mergePolicies?schema.name=_xdm.context.profile' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}
```

**応答**

成功した応答は、要求で送信されるクエリパラメーターで指定された条件を満たすマージポリシーのページ分割リストを返します。

```json
{
    "_page": {
        "totalCount": 2,
        "pageSize": 2
    },
    "children": [
        {
            "id": "0bf16e61-90e9-4204-b8fa-ad250360957b",
            "name": "Profile Default Merge Policy",
            "imsOrgId": "{IMS_ORG}",
            "sandbox": {
                "sandboxId": "ff0f6870-c46d-11e9-8ca3-036939a64204",
                "sandboxName": "prod",
                "type": "production",
                "default": true
            },
            "schema": {
                "name": "_xdm.context.profile"
            },
            "version": 1,
            "identityGraph": {
                "type": "none"
            },
            "attributeMerge": {
                "type": "timestampOrdered"
            },
            "default": true,
            "updateEpoch": 1552086578
        },
        {
            "id": "42d4a596-b1c6-46c0-994e-ca5ef1f85130",
            "name": "Dataset Precedence Merge Policy",
            "imsOrgId": "{IMS_ORG}",
            "sandbox": {
                "sandboxId": "ff0f6870-c46d-11e9-8ca3-036939a64204",
                "sandboxName": "prod",
                "type": "production",
                "default": true
            },
            "schema": {
                "name": "_xdm.context.profile"
            },
            "version": 1,
            "identityGraph": {
                "type": "pdg"
            },
            "attributeMerge": {
                "type": "dataSetPrecedence",
                "order": [
                    "5b76f86b85d0e00000be5c8b",
                    "5b76f8d787a6af01e2ceda18"
                ]
            },
            "default": false,
            "updateEpoch": 1576099719
        }
    ],
    "_links": {
        "next": {
            "href": "@/mergePolicies?start=K1JJRDpFaWc5QUpZWHY1c2JBQUFBQUFBQUFBPT0jUlQ6MSNUUkM6MiNGUEM6QWdFQUFBQldBQkVBQVBnaFFQLzM4VUIvL2tKQi8rLysvMUpBLzMrMi8wRkFmLzR4UUwvL0VrRC85em4zRTBEcmNmYi92Kzh4UUwvL05rQVgzRi8rMStqNS80WHQwN2NhUUVzQUFBUUFleGpLQ1JnVXRVcEFCQUFFQVBBRA==&orderBy=&limit=2"
        }
    }
}
```

| プロパティ | 説明 |
|---|---|
| `_links.next.href` | 結果の次のページのURIアドレス。 このURIを、ページを表示する同じエンドポイントへの別のAPI呼び出しのリクエストパラメーターとして使用します。 次のページが存在しない場合、この値は空の文字列になります。 |

## マージポリシーの作成

エンドポイントにPOSTリクエストを作成して、組織の新しい結合ポリシーを作成でき `/config/mergePolicies` ます。

**API形式**

```http
POST /config/mergePolicies
```

**リクエスト**&#x200B;次のリクエストは、ペイロードで提供される属性値によって設定される新しい結合ポリシーを作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/config/mergePolicies \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "Loyalty members ordered by ID",
    "identityGraph" : {
        "type": "none"
    },
    "attributeMerge" : {
        "type":"dataSetPrecedence",
        "order" : [
            "5b76f86b85d0e00000be5c8b",
            "5b76f8d787a6af01e2ceda18"
        ]
    },
    "schema": {
        "name":"_xdm.context.profile"
    },
    "default": true
}'
```

| プロパティ | 説明 |
|---|---|
| `name` | マージポリシーをリスト表示で識別できるわかりやすい名前。 |
| `identityGraph.type` | マージする関連IDを取得するIDグラフのタイプ。 可能な値： &quot;none&quot;または&quot;pdg&quot;（プライベートグラフ）。 |
| `attributeMerge` | データの競合が発生した場合にプロファイル属性値に優先順位を付ける方法。 |
| `schema` | マージポリシーに関連付けられているXDMスキーマクラスです。 |
| `default` | このマージポリシーをスキーマの既定にするかどうかを指定します。 |

詳細は、「 [components of merge policies](#components-of-merge-policies) 」の節を参照してください。

**応答**

正常に応答すると、新たに作成されたマージポリシーの詳細が返されます。

```json
{
    "id": "e5bc94de-cd14-4cdf-a2bc-88b6e8cbfac2",
    "name": "Loyalty members ordered by ID",
    "imsOrgId": "{IMS_ORG}",
    "sandbox": {
        "sandboxId": "ff0f6870-c46d-11e9-8ca3-036939a64204",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "schema": {
        "name": "_xdm.context.profile"
    },
    "version": 1,
    "identityGraph": {
        "type": "none"
    },
    "attributeMerge": {
        "type": "dataSetPrecedence",
        "order": [
            "5b76f86b85d0e00000be5c8b",
            "5b76f8d787a6af01e2ceda18"
        ]
    },
    "default": true,
    "updateEpoch": 1551898378
}
```

結合ポリシーを構成する個々の要素の詳細については、このドキュメントの最初の [](#components-of-merge-policies) 結合ポリシーのコンポーネントに関する節を参照してください。

## マージポリシーの更新 {#update}

既存のマージ・ポリシーを変更するには、個々の属性(PATCH)を編集するか、マージ・ポリシー全体を新しい属性(PUT)で上書きします。 それぞれの例を以下に示します。

### 個々の結合ポリシーフィールドの編集

エンドポイントにPATCH要求を行うと、マージポリシー用の個々のフィールドを編集でき `/config/mergePolicies/{mergePolicyId}` ます。

**API形式**

```http
PATCH /config/mergePolicies/{mergePolicyId}
```

| パラメーター | 説明 |
|---|---|
| `{mergePolicyId}` | 削除するマージポリシーの識別子。 |

**リクエスト**

次の要求は、指定されたマージポリシーを更新します。その `default` プロパティの値を次に変更しま `true`す。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/core/ups/config/mergePolicies/e5bc94de-cd14-4cdf-a2bc-88b6e8cbfac2 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "op": "add",
    "path": "/default",
    "value": "true"
  }'
```

| プロパティ | 説明 |
|---|---|
| `op` | 実行する操作を指定します。 その他のPATCH操作の例は、 [JSONパッチドキュメントを参照してください](http://jsonpatch.com) |
| `path` | 更新するフィールドのパス。 指定できる値は次のとおりです。 &quot;/name&quot;、&quot;/identityGraph.type&quot;、&quot;/attributeMerge.type&quot;、&quot;/schema.name&quot;、&quot;/version&quot;、&quot;/default&quot; |
| `value` | 指定したフィールドに設定する値。 |

詳細は、「 [components of merge policies](#components-of-merge-policies) 」の節を参照してください。


**応答**

正常に応答すると、新たに更新されたマージポリシーの詳細が返されます。

```json
{
    "id": "e5bc94de-cd14-4cdf-a2bc-88b6e8cbfac2",
    "name": "Loyalty members ordered by ID",
    "imsOrgId": "{IMS_ORG}",
    "sandbox": {
        "sandboxId": "ff0f6870-c46d-11e9-8ca3-036939a64204",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "schema": {
        "name": "_xdm.context.profile"
    },
    "version": 1,
    "identityGraph": {
        "type": "none"
    },
    "attributeMerge": {
        "type": "dataSetPrecedence",
        "order": [
            "5b76f86b85d0e00000be5c8b",
            "5b76f8d787a6af01e2ceda18"
        ]
    },
    "default": true,
    "updateEpoch": 1551898378
}
```

### マージポリシーを上書きする

マージポリシーを変更する別の方法は、PUT要求を使用することです。PUT要求はマージポリシー全体を上書きします。

**API形式**

```http
PUT /config/mergePolicies/{mergePolicyId}
```

| パラメーター | 説明 |
|---|---|
| `{mergePolicyId}` | 上書きする結合ポリシーの識別子。 |

**リクエスト**

次の要求は、指定されたマージポリシーを上書きし、属性値をペイロードで指定された属性値に置き換えます。 この要求は既存のマージポリシーを完全に置き換えるので、最初にマージポリシーを定義する際に必要だったのと同じフィールドをすべて指定する必要があります。 ただし、今回は、変更するフィールドに更新された値を指定します。

```shell
curl -X PUT \
  https://platform.adobe.io/data/core/ups/config/mergePolicies/e5bc94de-cd14-4cdf-a2bc-88b6e8cbfac2 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "name": "Loyalty members ordered by ID",
        "imsOrgId": "{IMS_ORG}",
        "schema": {
            "name": "_xdm.context.profile"
        },
        "version": 1,
        "identityGraph": {
            "type": "none"
        },
        "attributeMerge": {
            "type": "dataSetPrecedence",
            "order": [
                "5b76f86b85d0e00000be5c8b",
                "5b76f8d787a6af01e2ceda18"
            ]
        },
        "default": true,
        "updateEpoch": 1551898378
    }'
```

| プロパティ | 説明 |
|---|---|
| `name` | マージポリシーをリスト表示で識別できるわかりやすい名前。 |
| `identityGraph` | 結合する関連IDの取得元となるIDグラフ。 |
| `attributeMerge` | データの競合が発生した場合にプロファイル属性値に優先順位を付ける方法。 |
| `schema` | マージポリシーに関連付けられているXDMスキーマクラスです。 |
| `default` | このマージポリシーをスキーマの既定にするかどうかを指定します。 |

詳細は、「 [components of merge policies](#components-of-merge-policies) 」の節を参照してください。


**応答**

正常に応答すると、更新された結合ポリシーの詳細が返されます。

```json
{
    "id": "e5bc94de-cd14-4cdf-a2bc-88b6e8cbfac2",
    "name": "Loyalty members ordered by ID",
    "imsOrgId": "{IMS_ORG}",
    "sandbox": {
        "sandboxId": "ff0f6870-c46d-11e9-8ca3-036939a64204",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "schema": {
        "name": "_xdm.context.profile"
    },
    "version": 1,
    "identityGraph": {
        "type": "none"
    },
    "attributeMerge": {
        "type": "dataSetPrecedence",
        "order": [
            "5b76f86b85d0e00000be5c8b",
            "5b76f8d787a6af01e2ceda18"
        ]
    },
    "default": true,
    "updateEpoch": 1551898378
}
```

## マージポリシーの削除

マージポリシーを削除するには、エンドポイントにDELETE要求を行い、削除するマージポリシーのIDを要求パスに含め `/config/mergePolicies` ます。

**API形式**

```http
DELETE /config/mergePolicies/{mergePolicyId}
```

| パラメーター | 説明 |
|---|---|
| `{mergePolicyId}` | 削除するマージポリシーの識別子。 |

**リクエスト**

次の要求は、マージポリシーを削除します。

```shell
curl -X DELETE \
  https://platform.adobe.io/data/core/ups/config/mergePolicies/e5bc94de-cd14-4cdf-a2bc-88b6e8cbfac2 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

削除が成功すると、HTTPステータス200(OK)と空の応答本文が返されます。 削除が成功したことを確認するために、GET要求を実行し、マージポリシーをIDで表示できます。 マージポリシーが削除された場合は、HTTPステータス404（見つかりません）エラーが表示されます。

## 次の手順

これで、IMS組織の結合ポリシーを作成および設定する方法がわかり、それらを使用してリアルタイム顧客プロファイルデータからオーディエンスセグメントを作成できます。 セグメントの定義と操作を開始するには、 [Adobe Experience Platform Segmentation Serviceのドキュメント](../../segmentation/home.md) を参照してください。



