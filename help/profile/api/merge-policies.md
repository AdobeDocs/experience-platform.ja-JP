---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
title: マージポリシー — リアルタイム顧客プロファイルAPI
topic: guide
translation-type: tm+mt
source-git-commit: 6bfc256b50542e88e28f8a0c40cec7a109a05aa6
workflow-type: tm+mt
source-wordcount: '2494'
ht-degree: 56%

---


# ポリシーエンドポイントの結合

Adobe Experience Platformでは、複数のソースからデータフラグメントをまとめ、それらを組み合わせて、個々の顧客の完全な表示を確認できます。 When bringing this data together, merge policies are the rules that [!DNL Platform] uses to determine how data will be prioritized and what data will be combined to create that unified view.

例えば、顧客が複数のチャネルをまたがって自社のブランドとやり取りを行う場合、1人の顧客に関連する複数のプロファイルフラグメントが複数のデータセットに表示されます。 これらのフラグメントがPlatformに取り込まれると、それらのフラグメントが結合され、そのお客様用の単一のプロファイルが作成されます。 複数のソースからのデータが競合する場合(例えば、1つのフラグメントリストが顧客を「独身」、他のリストが「既婚」)、結合ポリシーによって個人のプロファイルに含める情報が決定されます。

RESTful API またはユーザーインターフェイスを介すると、新しい結合ポリシーの作成、既存のポリシーの管理、組織のデフォルトの結合ポリシーの設定をおこなえます。このガイドでは、APIを使用して結合ポリシーを操作する手順を説明します。

UI を使用して結合ポリシーを使用するには、『[結合ポリシーのユーザーガイド](../ui/merge-policies.md)』を参照してください。

## はじめに

The API endpoint used in this guide is part of the [[!DNL Real-time Customer Profile API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/real-time-customer-profile.yaml). 先に進む前に、 [はじめに](getting-started.md)[!DNL Experience Platform] 、関連ドキュメントへのリンク、このドキュメントのサンプルAPI呼び出しを読むためのガイド、APIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報を確認してください。

## 結合ポリシーのコンポーネント {#components-of-merge-policies}

マージポリシーはIMS組織に対して非公開なので、異なるポリシーを作成して、必要な方法でスキーマを結合できます。 Any API accessing [!DNL Profile] data requires a merge policy, though a default will be used if one is not explicitly provided. [!DNL Platform] 組織にデフォルトの結合ポリシーを提供します。または、特定のExperience Data Model(XDM)スキーマクラス用に結合ポリシーを作成し、それを組織のデフォルトとしてマークすることができます。

各組織はスキーマクラスごとに複数のマージポリシーを持つことができますが、各クラスはデフォルトのマージポリシーを1つだけ持つことができます。 デフォルトとして設定されたマージポリシーは、スキーマクラスの名前が指定され、マージポリシーが必須で指定されていない場合に使用されます。

>[!NOTE]
>
>新しい結合ポリシーをデフォルトとして設定すると、以前にデフォルトとして設定された既存の結合ポリシーが自動的に更新され、デフォルトとして使用されなくなります。

### 完全な結合ポリシーオブジェクト

完全な結合ポリシーオブジェクトは、プロファイルフラグメントの結合の側面を制御する一連の設定を表します。

**結合ポリシーオブジェクト**

```json
    {
        "id": "{MERGE_POLICY_ID}",
        "name": "{NAME}",
        "imsOrgId": "{IMS_ORG}",
        "schema": {
            "name": "{SCHEMA_CLASS_NAME}"
        },
        "version": 1,
        "identityGraph": {
            "type": "{IDENTITY_GRAPH_TYPE}"
        },
        "attributeMerge": {
            "type": "{ATTRIBUTE_MERGE_TYPE}"
        },
        "default": "{BOOLEAN}",
        "updateEpoch": "{UPDATE_TIME}"
    }
```

| プロパティ | 説明 |
|---|---|
| `id` | 作成時に割り当てられる、システムで生成された一意の識別子 |
| `name` | リスト表示で結合ポリシーを識別できるわかりやすい名前。 |
| `imsOrgId` | この結合ポリシーが属する組織 ID |
| `identityGraph` | 関連 ID の取得元（ID）の ID グラフを示す [ID グラフ](#identity-graph)オブジェクト。関連するすべての ID で見つかったプロファイルフラグメントが結合されます。 |
| `attributeMerge` | [データの競合が発生した場合に、マージポリシーがプロファイル属性に優先順位を付ける方法を示す属性マージ](#attribute-merge) ・オブジェクト。 |
| `schema.name` | オブジェクトの一部で [`schema`](#schema) あり、 `name` フィールドにはマージポリシーが関連付けられているXDMスキーマクラスが含まれます。 スキーマとクラスの詳細については、 [XDMのドキュメントを読んでください](../../xdm/home.md)。 |
| `default` | この結合ポリシーが指定されたスキーマのデフォルトかどうかを示すブール値。 |
| `version` | [!DNL Platform] マージポリシーのバージョンを維持しました。 この読み取り専用の値は、結合ポリシーが更新されるたびに増加します。 |
| `updateEpoch` | 結合ポリシーの最後の更新日。 |

**結合ポリシーの例**

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

### ID グラフ {#identity-graph}

[Adobe Experience Platformアイデンティティサービス](../../identity-service/home.md) は、グローバルに、およびの各組織で使用されるIDグラフを管理 [!DNL Experience Platform]します。 結合ポリシーの `identityGraph` 属性は、ユーザーの関連 ID の決定方法を定義します。

**ID グラフオブジェクト**

```json
    "identityGraph": {
        "type": "{IDENTITY_GRAPH_TYPE}"
    }
```

ここで、`{IDENTITY_GRAPH_TYPE}` は次のいずれかです。

* **none**：ID を結合しません。
* **pdg：**&#x200B;個人の ID グラフに基づいて ID を結合します。

**例`identityGraph`**

```json
    "identityGraph": {
        "type": "pdg"
    }
```

### 属性の結合 {#attribute-merge}

プロファイルフラグメントとは、特定のユーザーに存在する ID のリストからの 1 つの ID のプロファイル情報のことです。使用するIDグラフのタイプが複数のIDになる場合、プロファイル属性と優先度が競合する可能性があります。 を使用 `attributeMerge`すると、キー値（レコードデータ）型のデータセット間の結合の競合をイベントする際に、優先順位を付けるプロファイル属性を指定できます。

**attributeMerge オブジェクト**

```json
    "attributeMerge": {
        "type": "{ATTRIBUTE_MERGE_TYPE}"
    }
```

ここで、`{ATTRIBUTE_MERGE_TYPE}` は次のいずれかです。

* **`timestampOrdered`**:（デフォルト）最後に更新されたプロファイルを優先します。 この結合タイプを使用する場合、`data` 属性は不要です。`timestampOrdered` また、データセット内またはデータセット間でプロファイルフラグメントを結合する場合に優先されるカスタムタイムスタンプもサポートします。 詳しくは、カスタムタイムスタンプの [使用に関する付録の節を参照してください](#custom-timestamps)。
* **`dataSetPrecedence`** :プロファイルフラグメントの送信元のデータセットに基づいて、そのフラグメントを優先します。 これは、あるデータセットに存在する情報が別のデータセットのデータよりも優先または信頼されている場合に使用できます。この結合タイプを使用する場合、`order` 属性は優先順にデータセットをリストするので、必須です。
   * **`order`**:&quot;dataSetPrecedence&quot;を使用する場合は、配列にデータセットのリストを指定する必要があり `order` ます。 データセットに含まれていないリストは結合されません。つまり、データセットをプロファイルに結合するには、データセットを明示的にリストする必要があります。`order` 配列は、データセットの ID を優先順にリストします。

#### 型を使用した `attributeMerge``dataSetPrecedence` オブジェクトの例

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

#### 型を使用した `attributeMerge``timestampOrdered` オブジェクトの例

```json
    "attributeMerge": {
        "type": "timestampOrdered"
    }
```

### スキーマ {#schema}

このスキーマオブジェクトは、この結合ポリシーが作成されるExperience Data Model(XDM)スキーマクラスを指定します。

**`schema`オブジェクト**

```json
    "schema": {
        "name": "{SCHEMA_NAME}"
    }
```

`name` の値は、結合ポリシーに関連付けられたスキーマの基となる XDM クラスの名前です。

**例`schema`**

```json
    "schema": {
        "name": "_xdm.context.profile"
    }
```

XDMの詳細とExperience Platformでのスキーマの使い方については、 [XDM Systemの概要を読んでください](../../xdm/home.md)。

## 結合ポリシーへのアクセス {#access-merge-policies}

Using the [!DNL Real-time Customer Profile] API, the `/config/mergePolicies` endpoint allows you perform a lookup request to view a specific merge policy by its ID, or access all of the merge policies in your IMS Organization, filtered by specific criteria. また、エンドポイントを使用して、ID別に複数のマージポリシーを取得することもで `/config/mergePolicies/bulk-get` きます。 これらの各呼び出しを実行する手順を、以下の各節で説明します。

### ID による単一の結合ポリシーへのアクセス

`/config/mergePolicies` エンドポイントに GET リクエストを送信し、リクエストパスに `mergePolicyId` を含めることで、ID を使用して 1 つの結合ポリシーにアクセスすることができます。

**API 形式**

```http
GET /config/mergePolicies/{mergePolicyId}
```

| パラメーター | 説明 |
|---|---|
| `{mergePolicyId}` | 削除する結合ポリシーの識別子。 |

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

正常な応答は、結合ポリシーの詳細を返します。

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

結合ポリシーを構成する個々の要素の詳細については、このドキュメントの最初にある「[結合ポリシーのコンポーネント](#components-of-merge-policies)」の節を参照してください。

### ID別に複数のマージポリシーを取得する

エンドポイントにPOSTリクエストを送信し、取得するマージポリシーのIDをリクエスト本文に含めることで、複数のマージポリシーを取得でき `/config/mergePolicies/bulk-get` ます。

**API 形式**

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
          },
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
    "results": { 
        "0bf16e61-90e9-4204-b8fa-ad250360957b": {
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
        "42d4a596-b1c6-46c0-994e-ca5ef1f85130": {
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
    }
}
```

結合ポリシーを構成する個々の要素の詳細については、このドキュメントの最初にある「[結合ポリシーのコンポーネント](#components-of-merge-policies)」の節を参照してください。

### 条件別の複数の結合ポリシーのリスト

`/config/mergePolicies` エンドポイントに GET リクエストを発行し、オプションのクエリーパラメーターを使用して応答をフィルター、並び替え、ページネートすることで、IMS 組織内で複数の結合ポリシーをリストできます。複数のパラメーターを含める場合は、アンパサンド（&amp;）で区切ります。パラメーターを指定しないでこのエンドポイントを呼び出すと、組織で使用可能なすべての結合ポリシーが取得されます。

**API 形式**

```http
GET /config/mergePolicies?{QUERY_PARAMS}
```

| パラメーター | 説明 |
|---|---|
| `default` | 結合ポリシーがフィルタークラスのデフォルトであるかどうかによって結果をスキーマするブール値。 |
| `limit` | ページに含める結果の数を制御するためのページサイズの制限を指定します。デフォルト値：20 |
| `orderBy` | 名前を昇順で並べ替えるには `orderBy=name` または `orderBy=+name` のように結果を並べ替えるフィールドを指定し、降順で並べ替えるには `orderBy=-name` を指定します。この値を省略すると、`name` のデフォルトの並べ替え順が昇順になります。 |
| `schema.name` | 使用可能な結合スキーマを取得するポリシーの名前。 |
| `identityGraph.type` | フィルターは、ID グラフのタイプ別に表示されます。有効な値は、「none」と「pdg」（プライベートグラフ）です。 |
| `attributeMerge.type` | フィルターは、使用される属性の結合タイプ別に結果を返します。指定できる値は、「timestampOrdered」と「dataSetPrecedence」です。 |
| `start` | ページオフセット — 取得するデータの開始 ID を指定します。デフォルト値：0 |
| `version` | 特定のバージョンの結合ポリシーを使用する場合に指定します。デフォルトでは、最新バージョンが使用されます。 |

`schema.name`、`identityGraph.type`、`attributeMerge.type` に関する詳細は、このガイドで前述した「[結合ポリシーのコンポーネント](#components-of-merge-policies)」の節を参照してください。


**リクエスト**

次のリクエストは、特定のスキーマのすべての統合ポリシーをリストします。

```shell
curl -X GET \
  'https://platform.adobe.io/data/core/ups/config/mergePolicies?schema.name=_xdm.context.profile' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}
```

**応答** 

正常な応答は、リクエストで送信されたクエリーパラメーターで指定された基準を満たす結合ポリシーのページ付けされたリストを返します。

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
| `_links.next.href` | 結果の次のページの URI アドレス。この URI を、同じエンドポイントに対する別の API 呼び出しのリクエストパラメーターとして使用し、ページを表示します。次のページが存在しない場合、この値は空の文字列になります。 |

## 結合ポリシーの作成

`/config/mergePolicies`エンドポイントに POST リクエストをおこなうことで、組織の新しい結合ポリシーを作成できます。

**API 形式**

```http
POST /config/mergePolicies
```

**リクエスト**
次のリクエストは、ペイロードで指定された属性値によって設定される新しい結合ポリシーを作成します。

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
| `name` | 結合ポリシーをリストビューで識別できる、わかりやすい名前。 |
| `identityGraph.type` | 結合する関連 ID を取得する ID グラフのタイプ。可能な値：「none」または「pdg」（プライベートグラフ）。 |
| `attributeMerge` | データの競合時にプロファイル属性値に優先順位を付ける方法。 |
| `schema` | 結合ポリシーに関連付けられた XDM スキーマクラス。 |
| `default` | この結合ポリシーがスキーマのデフォルトかどうかを指定します。 |

詳細は、「[結合ポリシーのコンポーネント](#components-of-merge-policies)」の節を参照してください。

**応答** 

正常な応答は、新しく作成された結合ポリシーの詳細を返します。

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

結合ポリシーを構成する個々の要素の詳細については、このドキュメントの最初にある「[結合ポリシーのコンポーネント](#components-of-merge-policies)」の節を参照してください。

## 結合ポリシーの更新 {#update}

既存の結合ポリシーを変更するには、個々の属性を編集するか（PATCH）、結合ポリシー全体を新しい属性で上書きします（PUT）。それぞれの例を以下に示します。

### 個々の結合ポリシーフィールドの編集

`/config/mergePolicies/{mergePolicyId}` エンドポイントに PATCH リクエストをおこなうことで、結合ポリシーの個々のフィールドを編集できます。

**API 形式**

```http
PATCH /config/mergePolicies/{mergePolicyId}
```

| パラメーター | 説明 |
|---|---|
| `{mergePolicyId}` | 削除する結合ポリシーの識別子。 |

**リクエスト**

次のリクエストは、`default` プロパティの値を `true` に変更することで、指定した結合ポリシーを更新します。

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
| `op` | 操作を指定します。その他のパッチ操作の例については、 [JSON パッチのドキュメント](http://jsonpatch.com)を参照してください |
| `path` | 更新するフィールドのパス。指定できる値は「/name」、「/identityGraph.type」、「/attributeMerge.type」、「/schema.name」、「/version」、「/default」です。 |
| `value` | 指定したフィールドに設定する値。 |

詳細は、「[結合ポリシーのコンポーネント](#components-of-merge-policies)」の節を参照してください。


**応答** 

正常な応答は、新しく更新された結合ポリシーの詳細を返します。

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

### 結合ポリシーの上書き

結合ポリシーを変更する別の方法は、結合ポリシー全体を上書きする PUT リクエストを使用することです。

**API 形式**

```http
PUT /config/mergePolicies/{mergePolicyId}
```

| パラメーター | 説明 |
|---|---|
| `{mergePolicyId}` | 上書きする結合ポリシーの識別子。 |

**リクエスト**

次のリクエストは、指定された結合ポリシーを上書きし、その属性値をペイロードで提供されたものに置き換えます。このリクエストは既存の結合ポリシーを完全に置き換えるため、最初に結合ポリシーを定義するときに必要だったのと同じフィールドをすべて指定する必要があります。ただし、今回は、変更するフィールドの更新された値を指定します。

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
| `name` | 結合ポリシーをリストビューで識別できる、わかりやすい名前。 |
| `identityGraph` | 結合する関連 ID を取得する ID グラフ。 |
| `attributeMerge` | データの競合時にプロファイル属性値に優先順位を付ける方法。 |
| `schema` | 結合ポリシーに関連付けられた XDM スキーマクラス。 |
| `default` | この結合ポリシーがスキーマのデフォルトかどうかを指定します。 |

詳細は、「[結合ポリシーのコンポーネント](#components-of-merge-policies)」の節を参照してください。


**応答** 

正常な応答は、更新された結合ポリシーの詳細を返します。

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

## 結合ポリシーの削除

結合ポリシーを削除するには、`/config/mergePolicies` エンドポイントに DELETE リクエストをおこない、削除する結合ポリシーの ID をリクエストパスに含めます。

**API 形式**

```http
DELETE /config/mergePolicies/{mergePolicyId}
```

| パラメーター | 説明 |
|---|---|
| `{mergePolicyId}` | 削除する結合ポリシーの識別子。 |

**リクエスト**

次のリクエストは、結合ポリシーを削除します。

```shell
curl -X DELETE \
  https://platform.adobe.io/data/core/ups/config/mergePolicies/e5bc94de-cd14-4cdf-a2bc-88b6e8cbfac2 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答** 

削除リクエストが成功すると、HTTP ステータス 200（OK）と空の応答本文が返されます。削除が成功したことを確認するには、GET リクエストを実行して、ID ごとに結合ポリシーを表示します。結合ポリシーが削除されている場合は、HTTP ステータス 404（不検知）エラーが表示されます。

## 次の手順

組織の結合ポリシーを作成および設定する方法がわかったので、結合ポリシーを使用してPlatform内での顧客プロファイルの表示を調整したり、 [!DNL Real-time Customer Profile] データからオーディエンスセグメントを作成したりできます。 セグメントの定義と使用を開始するには、[Adobe Experience Platform セグメント化サービス](../../segmentation/home.md)のドキュメントを参照してください。

## 付録

この節では、マージポリシーの操作に関する補足情報を説明します。

### カスタムタイムスタンプの使用 {#custom-timestamps}

記録をExperience Platformに取り込むと、取り込み時にシステムタイムスタンプを取得し、記録に追加する。 マージポリシーの `timestampOrdered``attributeMerge` 種類としてを選択すると、プロファイルはシステムのタイムスタンプに基づいてマージされます。 つまり、記録がプラットフォームに取り込まれた時のタイムスタンプに基づいて結合が行われます。

場合によっては、データのバックフィルや、レコードが順番に取り込まれない場合のイベントの正しい順序の確認など、カスタムタイムスタンプを指定し、マージポリシーでシステムタイムスタンプではなくカスタムタイムスタンプが適用される場合があります。

カスタムタイムスタンプを使用するには、プロファイルスキーマにタイムスタンプを追加する [[!DNL External Source System Audit Details Mixin]](#mixin-details) 必要があります。 追加したカスタムタイムスタンプは、この `xdm:lastUpdatedDate` フィールドを使用して入力できます。 レコードを取り込むときに `xdm:lastUpdatedDate` フィールドに値が入力され、Experience Platformはそのフィールドを使用して、データセット内およびデータセット間でレコードまたはプロファイルフラグメントを結合します。 が存在しな `xdm:lastUpdatedDate` い場合、または入力されない場合、プラットフォームはシステムタイムスタンプを引き続き使用します。

>[!NOTE]
>
>同じレコード上でPATCHを送信する場合は、タイム `xdm:lastUpdatedDate` スタンプが設定されていることを確認する必要があります。

スキーマレジストリAPIを使用してスキーマを操作する手順(スキーマにミックスインを追加する方法など)については、APIを使用したスキーマの作成に関する [チュートリアルを参照してください](../../xdm/tutorials/create-schema-api.md)。

UIを使用してカスタムタイムスタンプを操作するには、 [マージポリシーユーザーガイドのカスタムタイムスタンプの](../ui/merge-policies.md#custom-timestamps) 使用に関する節を参照してください [](../ui/merge-policies.md)。

#### [!DNL External Source System Audit Details Mixin] details {#mixin-details}

次の例は、に正しく入力されたフィールドを示してい [!DNL External Source System Audit Details Mixin]ます。 完全なミックスインJSONは、GitHubの [パブリックエクスペリエンスデータモデル(XDM)レポートでも表示できます](https://github.com/adobe/xdm/blob/master/components/mixins/shared/external-source-system-audit-details.schema.json) 。

```json
{
  "xdm:createdBy": "{CREATED_BY}",
  "xdm:createdDate": "2018-01-02T15:52:25+00:00",
  "xdm:lastUpdatedBy": "{LAST_UPDATED_BY}",
  "xdm:lastUpdatedDate": "2018-01-02T15:52:25+00:00",
  "xdm:lastActivityDate": "2018-01-02T15:52:25+00:00",
  "xdm:lastReferencedDate": "2018-01-02T15:52:25+00:00",
  "xdm:lastViewedDate": "2018-01-02T15:52:25+00:00"
 }
```




