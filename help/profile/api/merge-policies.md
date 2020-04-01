---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: リアルタイム顧客プロファイルAPI開発ガイド
topic: guide
translation-type: tm+mt
source-git-commit: 5aad9fa71051a58fe1c4678553f47077d81d23fc

---


# 結合ポリシー

Adobe Experience Platformを使用すると、複数のソースからデータを統合し、それを組み合わせて、個々の顧客の完全な表示を確認できます。 このデータを統合する場合、統合ポリシーは、データの優先順位付け方法と、統合されたビューを作成するためにどのデータを組み合わせるかを決定するためにプラットフォームで使用されるルールです。RESTful APIまたはユーザーインターフェイスを使用して、新しい結合ポリシーを作成し、既存のポリシーを管理し、組織のデフォルトの結合ポリシーを設定できます。 このガイドでは、APIを使用して結合ポリシーを操作する手順を示します。 UIを使用して結合ポリシーを使用するには、結合ポリシーのユーザーガ [イドを参照してください](../ui/merge-policies.md)。

## はじめに

このガイドで使用されるAPIエンドポイントは、リアルタイム顧客プロファイルAPIの一部です。 先に進む前に、リアルタイム顧客 [プロファイルAPI開発ガイドを参照してください](getting-started.md)。 特に、プロファイル開発ガ [イドの](getting-started.md#getting-started) 「はじめに」の節には、関連トピックへのリンク、このドキュメントのサンプルAPI呼び出しの読み方のガイド、Experience Platform APIを正常に呼び出すために必要なヘッダーに関する重要な情報が含まれています。

## 結合ポリシーのコンポーネント {#components-of-merge-policies}

マージポリシーはIMS組織に対して非公開であり、必要に応じてスキーマをマージするために異なるポリシーを作成できます。 プロファイルデータにアクセスするAPIには結合ポリシーが必要ですが、明示的に指定されていない場合はデフォルトが使用されます。 スキーマは、デフォルトの結合ポリシーを提供します。また、特定のプラットフォームの結合ポリシーを作成し、組織のデフォルトとしてマークすることもできます。 各組織は、1つの組織に複数の結合ポリシーを持つことができますが、スキーマごとに1つのデフォルトの結合ポリシーを持つことができるスキーマは1つだけです。 デフォルトとして設定されたマージポリシーは、スキーマ名が指定され、マージポリシーが必要であるが指定されていない場合に使用されます。 マージポリシーをデフォルトとして設定すると、以前にデフォルトとして設定された既存のマージポリシーが自動的に更新され、デフォルトとして使用されなくなります。

### マージポリシーオブジェクトの完了

complete merge policyオブジェクトは、ユーザーフラグメントの結合の側面を制御する一連の環境設定プロファイルを表します。

**ポリシーオブジェクトの結合**

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
| `id` | 作成時に割り当てられる、システムで生成された一意の識別子 |
| `name` | 結合ポリシーを識別できるわかりやすい名前をリスト表示。 |
| `imsOrgId` | この結合ポリシーが属する組織ID |
| `identityGraph` | [関連IDの取得元](#identity-graph) (ID)のIDグラフを示すIDグラフオブジェクト。 プロファイルフラグメントは、すべての関連IDに対して結合されます。 |
| `attributeMerge` | [データの競合時に](#attribute-merge) 、マージポリシーがプロファイル属性値に優先順位を付ける方法を示す属性マージオブジェクト。 |
| `schema` | マージ [スキーマ](#schema) ・ポリシーを使用できるポリシー・オブジェクト。 |
| `default` | このマージポリシーが指定したポリシーのデフォルトであるかどうかを示すブール値スキーマです。 |
| `version` | プラットフォームがマージポリシーのバージョンを管理していました。 この読み取り専用の値は、マージポリシーが更新されるたびに増加します。 |
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

### アイデンティティグラフ {#identity-graph}

[Adobe Experience Platform Identity Serviceは](../../identity-service/home.md) 、グローバルに、Experience Platformの各組織で使用されるIDグラフを管理します。 結合ポ `identityGraph` リシーの属性は、ユーザーの関連IDの決定方法を定義します。

**identityGraphオブジェクト**

```json
    "identityGraph": {
        "type": "{IDENTITY_GRAPH_TYPE}"
    }
```

は、次 `{IDENTITY_GRAPH_TYPE}` のいずれかです。

* **&quot;none&quot;:** IDステッチを実行しない。
* **&quot;pdg&quot;:** 個人のIDグラフに基づいてIDステッチを実行します。

**例`identityGraph`**

```json
    "identityGraph": {
        "type": "pdg"
    }
```

### 属性の結合 {#attribute-merge}

プロファイルフラグメントとは、特定のプロファイルに存在するIDのリストのうち、1つのIDに関する情報のことです。 使用されるIDグラフのタイプが複数のIDを生み出す場合、プロファイルのプロパティと優先度の値が競合する可能性があります。 を使用す `attributeMerge`ると、結合の競合をイベントする際に優先順位を付けるデータセットプロファイルの値を指定できます。

**attributeMergeオブジェクト**

```json
    "attributeMerge": {
        "type": "{ATTRIBUTE_MERGE_TYPE}"
    }
```

は、次 `{ATTRIBUTE_MERGE_TYPE}` のいずれかです。

* **&quot;timestampOrdered&quot;**:（デフォルト）競合が発生した場合に最後に更新されたプロファイルを優先します。 この結合タイプを使用する場合、属 `data` 性は不要です。
* **&quot;dataSetPrecedence&quot;** :訪問者のプロファイルセットに基づいて、データフラグメントを優先します。 これは、あるデータセットに存在する情報が、別のデータセットのデータよりも優先されたり、信頼されたりする場合に使用できます。 このマージタイプを使用する場合、属性は `order` 優先順にデータセットをリストするので、必須です。
   * **&quot;order&quot;**:&quot;dataSetPrecedence&quot;を使用する場合、配列にはデ `order` ータセットのリストが必要です。 データセットに含まれていないリストは結合されません。 つまり、データセットをプロファイルに結合するには、データセットを明示的にリストする必要があります。 アレイ `order` は、リストセットのIDを優先順にします。

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

**timestampOrdered型を使用したattributeMergeオブジェクトの例**

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

の値は、マージポ `name` リシーに関連付けられたスキーマの基となるXDMクラスの名前です。

**例`schema`**

```json
    "schema": {
        "name": "_xdm.context.profile"
    }
```

## 結合ポリシーへのアクセス {#access-merge-policies}

エンドポイントでは、リアルタイム顧客プロファイルAPIを使用して、特定の結合ポリシーをIDで表示するか、特定の条件でフィルタリングされたIMS組織のすべての結合ポリシーにアクセスするための参照リクエストを実行できます。 `/config/mergePolicies`

### IDによる単一の結合ポリシーへのアクセス

エンドポイントにGETリクエストを送信し、リクエストパスにを含めることで、IDを使用して1 `/config/mergePolicies` つのマージポリシーにア `mergePolicyId` クセスすることができます。

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

成功した応答は、マージポリシーの詳細を返します。

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

結合ポリシ [ーを構成する個々の要素の詳細については](#components-of-merge-policies) 、このドキュメントの最初にある「結合ポリシーのコンポーネント」の節を参照してください。

### リスト条件別の複数の結合ポリシー

エンドポイントにGETリクエストを発行し、オプションのクエリパラメーターを使用して応答をフィルター、順序、ページネートすることで、IMS組織内で複数のマージポリシーをリストできます。 `/config/mergePolicies` 複数のパラメーターを含める場合は、アンパサンド(&amp;)で区切ります。 パラメーターを指定しないでこのエンドポイントを呼び出すと、組織で使用可能なすべての結合ポリシーが取得されます。

**API形式**

```http
GET /config/mergePolicies?{QUERY_PARAMS}
```

| パラメーター | 説明 |
|---|---|
| `default` | マージポリシーがフィルタークラスのデフォルトであるかどうかによって結果をスキーマするboolean値。 |
| `limit` | ページに含める結果の数を制御するためのページサイズの制限を指定します。 デフォルト値：20 |
| `orderBy` | 結果の並べ替えの基準となるフィールド、または名 `orderBy=name` 前順 `orderBy=+name` で昇順に並べ替えるフィールド、または降順 `orderBy=-name`で並べ替えるフィールドを指定します。 この値を省略すると、のデフォルトの並べ替え順が昇順 `name` になります。 |
| `schema.name` | 使用可能なマージスキーマを取得するポリシーの名前。 |
| `identityGraph.type` | フィルターは、IDグラフのタイプ別に表示されます。 有効な値は、「none」と「pdg」（プライベートグラフ）です。 |
| `attributeMerge.type` | フィルターは、使用される属性の結合タイプ別に結果を返します。 指定できる値は、「timestampOrdered」と「dataSetPrecedence」です。 |
| `start` | ページオフセット — 取得するデータの開始IDを指定します。 デフォルト値：0 |
| `version` | 特定のバージョンのマージポリシーを使用する場合に指定します。 デフォルトでは、最新バージョンが使用されます。 |

、、およびに関する詳 `schema.name`細は、このガ `identityGraph.type`イドで前 `attributeMerge.type`述した結合ポリシーのコン [ポーネントの節](#components-of-merge-policies) を参照してください。


**リクエスト**

次の要求リストは、指定したポリシーのすべての結合スキーマです。

```shell
curl -X GET \
  'https://platform.adobe.io/data/core/ups/config/mergePolicies?schema.name=_xdm.context.profile' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}
```

**応答**

成功した応答は、リクエストで送信されるリストパラメーターで指定された条件を満たすマージポリシーのページ分割クエリを返します。

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
| `_links.next.href` | 結果の次のページのURIアドレス。 このURIを、同じエンドポイントに対する別のAPI呼び出しのリクエストパラメーターとして使用し、ページを表示します。 次のページが存在しない場合、この値は空の文字列になります。 |

## マージポリシーの作成

エンドポイントにPOSTリクエストを行うことで、組織の新しい結合ポリシーを作成で `/config/mergePolicies` きます。

**API形式**

```http
POST /config/mergePolicies
```

**リクエスト**&#x200B;次のリクエストは、ペイロードで指定された属性値によって設定される新しいマージポリシーを作成します。

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
| `name` | 結合ポリシーを識別できるわかりやすい名前をリスト表示。 |
| `identityGraph.type` | 結合する関連IDを取得するIDグラフのタイプ。 可能な値：「none」または「pdg」（プライベートグラフ）。 |
| `attributeMerge` | データの競合時にプロファイル属性値に優先順位を付ける方法。 |
| `schema` | マージポリシーに関連付けられたXDMスキーマクラスです。 |
| `default` | このマージポリシーをデフォルトのポリシーにするかどうかをスキーマします。 |

詳細は、「結合ポ [リシーのコンポーネント](#components-of-merge-policies) 」の節を参照してください。

**応答**

正常に完了すると、新しく作成されたマージポリシーの詳細が返されます。

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

結合ポリシ [ーを構成する個々の要素の詳細については](#components-of-merge-policies) 、このドキュメントの最初にある「結合ポリシーのコンポーネント」の節を参照してください。

## マージポリシーの更新

既存のマージポリシーを変更するには、個々の属性(PATCH)を編集するか、マージポリシー全体を新しい属性(PUT)で上書きします。 それぞれの例を以下に示します。

### 個々の結合ポリシーフィールドの編集

エンドポイントにPATCHリクエストを行うことで、マージポリシーの個々のフィールドを編集で `/config/mergePolicies/{mergePolicyId}` きます。

**API形式**

```http
PATCH /config/mergePolicies/{mergePolicyId}
```

| パラメーター | 説明 |
|---|---|
| `{mergePolicyId}` | 削除するマージポリシーの識別子。 |

**リクエスト**

次の要求は、プロパティの値を次に変更することで、指定したマージポリシーを更 `default` 新しま `true`す。

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
| `op` | 取る操作を指定します。 その他のPATCH操作の例は、 [JSONパッチのドキュメントを参照してください](http://jsonpatch.com) |
| `path` | 更新するフィールドのパス。 指定できる値は次のとおりです。&quot;/name&quot;、&quot;/identityGraph.type&quot;、&quot;/attributeMerge.type&quot;、&quot;/schema.name&quot;、&quot;/version&quot;、&quot;/default&quot; |
| `value` | 指定したフィールドに設定する値。 |

詳細は、「結合ポ [リシーのコンポーネント](#components-of-merge-policies) 」の節を参照してください。


**応答**

正常に完了すると、新しく更新されたマージポリシーの詳細が返されます。

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

### マージポリシーの上書き

マージポリシーを変更する別の方法は、PUT要求を使用することです。この要求は、マージポリシー全体を上書きします。

**API形式**

```http
PUT /config/mergePolicies/{mergePolicyId}
```

| パラメーター | 説明 |
|---|---|
| `{mergePolicyId}` | 上書きするマージポリシーの識別子。 |

**リクエスト**

次の要求は、指定されたマージポリシーを上書きし、属性値をペイロードで指定された値に置き換えます。 このリクエストは既存のマージポリシーを完全に置き換えるので、最初にマージポリシーを定義したときに必要だったのと同じフィールドをすべて指定する必要があります。 ただし、今回は、変更するフィールドの更新された値を指定します。

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
| `name` | 結合ポリシーを識別できるわかりやすい名前をリスト表示。 |
| `identityGraph` | 結合する関連IDを取得するIDグラフです。 |
| `attributeMerge` | データの競合時にプロファイル属性値に優先順位を付ける方法。 |
| `schema` | マージポリシーに関連付けられたXDMスキーマクラスです。 |
| `default` | このマージポリシーをデフォルトのポリシーにするかどうかをスキーマします。 |

詳細は、「結合ポ [リシーのコンポーネント](#components-of-merge-policies) 」の節を参照してください。


**応答**

正常に応答すると、更新されたマージポリシーの詳細が返されます。

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

マージポリシーを削除するには、エンドポイントにDELETE要求を行い、削除するマージ `/config/mergePolicies` ポリシーのIDを要求パスに含めます。

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

削除要求が成功すると、HTTPステータス200(OK)と空の応答本文が返されます。 削除が成功したことを確認するために、マージポリシーを表示するGET要求をIDで実行できます。 マージポリシーが削除された場合は、HTTPステータス404（見つかりません）エラーが表示されます。

## 次の手順

これで、IMS組織の結合ポリシーの作成および設定方法がわかり、それらを使用してリアルタイム顧客プロファイルデータからオーディエンスセグメントを作成できます。 セグメントの定義と操作を開始するには、 [Adobe Experience Platform Segmentation Serviceのドキュメントを参照して](../../segmentation/home.md) 、セグメントの定義と操作を行ってください。



