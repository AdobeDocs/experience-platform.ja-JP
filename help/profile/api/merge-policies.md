---
keywords: Experience Platform;プロファイル；リアルタイム顧客プロファイル；トラブルシューティング；API
title: ポリシーの結合APIエンドポイント
topic-legacy: guide
type: Documentation
description: Adobe Experience Platformでは、複数のソースからデータフラグメントをまとめ、それらを組み合わせて、個々の顧客の完全な表示を確認できます。 このデータを統合する際、統合ポリシーは、データの優先順位付け方法と統合表示を作成するデータをPlatformが決定する際に使用するルールです。
exl-id: fb49977d-d5ca-4de9-b185-a5ac1d504970
translation-type: tm+mt
source-git-commit: ab0798851e5f2b174d9f4241ad64ac8afa20a938
workflow-type: tm+mt
source-wordcount: '2569'
ht-degree: 54%

---

# ポリシーエンドポイントの結合

Adobe Experience Platformでは、複数のソースからデータフラグメントをまとめ、それらを組み合わせて、個々の顧客の完全な表示を確認できます。 このデータを統合する際、結合ポリシーは、[!DNL Platform]がデータの優先順位付け方法と統合データを決定する際に使用するルールです。統合表示を作成する際には、どのデータを結合するかを決定します。

例えば、顧客が複数のチャネルをまたがって自社のブランドとやり取りを行う場合、1人の顧客に関連する複数のプロファイルフラグメントが複数のデータセットに表示されます。 これらのフラグメントがPlatformに取り込まれると、それらのフラグメントが結合され、そのお客様用の単一のプロファイルが作成されます。 複数のソースからのデータが競合する場合(例えば、1つのフラグメントリストが顧客を「独身」、他のリストが「既婚」)、結合ポリシーによって個人のプロファイルに含める情報が決定されます。

RESTful API またはユーザーインターフェイスを介すると、新しい結合ポリシーの作成、既存のポリシーの管理、組織のデフォルトの結合ポリシーの設定をおこなえます。このガイドでは、APIを使用して結合ポリシーを操作する手順を説明します。

UIを使用して結合ポリシーを操作するには、[merge policies UIガイド](../ui/merge-policies.md)を参照してください。

## はじめに

このガイドで使用されるAPIエンドポイントは、[[!DNL Real-time Customer Profile API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/real-time-customer-profile.yaml)の一部です。 先に進む前に、[はじめにガイド](getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプルAPI呼び出しの読み方、および任意の[!DNL Experience Platform] APIの呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## 結合ポリシーのコンポーネント {#components-of-merge-policies}

マージポリシーはIMS組織に対して非公開なので、異なるポリシーを作成して、必要な方法でスキーマを結合できます。 [!DNL Profile]データにアクセスするAPIにはマージポリシーが必要ですが、明示的に指定されない場合はデフォルトが使用されます。 [!DNL Platform] 組織にデフォルトの結合ポリシーを提供します。または、特定のExperience Data Model(XDM)スキーマクラス用に結合ポリシーを作成し、それを組織のデフォルトとしてマークすることができます。

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
| `id` | 作成時に割り当てられる、システムで生成された一意の ID |
| `name` | リスト表示で結合ポリシーを識別できるわかりやすい名前。 |
| `imsOrgId` | この結合ポリシーが属する組織 ID |
| `identityGraph` | 関連 ID の取得元（ID）の ID グラフを示す [ID グラフ](#identity-graph)オブジェクト。関連するすべての ID で見つかったプロファイルフラグメントが結合されます。 |
| `attributeMerge` | [データの競合が発生した場合に、マージポリシーでプロファイル属性の優先順位を決定する方法を示す属性](#attribute-merge) mergeobjectです。 |
| `schema.name` | [`schema`](#schema)オブジェクトの一部である`name`フィールドには、マージポリシーが関連付けられているXDMスキーマクラスが含まれます。 スキーマとクラスの詳細は、[XDMドキュメント](../../xdm/home.md)を読んでください。 |
| `default` | この結合ポリシーが指定されたスキーマのデフォルトかどうかを示すブール値。 |
| `version` | [!DNL Platform] マージポリシーのバージョンを維持しました。この読み取り専用の値は、結合ポリシーが更新されるたびに増加します。 |
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

[Adobe Experience PlatformID](../../identity-service/home.md) サービスは、グローバルに、およびの各組織で使用するIDグラフを管理 [!DNL Experience Platform]します。結合ポリシーの `identityGraph` 属性は、ユーザーの関連 ID の決定方法を定義します。

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

プロファイルフラグメントとは、特定のユーザーに存在する ID のリストからの 1 つの ID のプロファイル情報のことです。使用するIDグラフのタイプが複数のIDになる場合、プロファイル属性と優先度が競合する可能性があります。 `attributeMerge`を使用すると、キー値（レコードデータ）型のデータセット間の結合の競合をイベントする際に、優先順位を付けるプロファイル属性を指定できます。

**attributeMerge オブジェクト**

```json
    "attributeMerge": {
        "type": "{ATTRIBUTE_MERGE_TYPE}"
    }
```

ここで、`{ATTRIBUTE_MERGE_TYPE}` は次のいずれかです。

* **`timestampOrdered`**:（デフォルト）最後に更新されたプロファイルを優先します。この結合タイプを使用する場合、`data` 属性は不要です。`timestampOrdered` また、データセット内またはデータセット間でプロファイルフラグメントを結合する場合に優先されるカスタムタイムスタンプもサポートします。詳しくは、[カスタムタイムスタンプの使用](#custom-timestamps)の付録の節を参照してください。
* **`dataSetPrecedence`** :プロファイルフラグメントの送信元のデータセットに基づいて、そのフラグメントを優先します。これは、あるデータセットに存在する情報が別のデータセットのデータよりも優先または信頼されている場合に使用できます。この結合タイプを使用する場合、`order` 属性は優先順にデータセットをリストするので、必須です。
   * **`order`**:&quot;dataSetPrecedence&quot;を使用する場合は、 `order` 配列にデータセットのリストを指定する必要があります。データセットに含まれていないリストは結合されません。つまり、データセットをプロファイルに結合するには、データセットを明示的にリストする必要があります。`order` 配列は、データセットの ID を優先順にリストします。

#### `attributeMerge`オブジェクトの例（`dataSetPrecedence`型を使用）

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

#### `attributeMerge`オブジェクトの例（`timestampOrdered`型を使用）

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

XDMの詳細やExperience Platformでのスキーマの使い方については、まず[XDM System overview](../../xdm/home.md)を読んでください。

## 結合ポリシーへのアクセス {#access-merge-policies}

[!DNL Real-time Customer Profile] APIを使用して`/config/mergePolicies`エンドポイントでルックアップリクエストを実行し、特定の結合ポリシーをIDで表示するか、特定の条件でフィルタリングしたIMS組織のすべての結合ポリシーにアクセスできます。 また、`/config/mergePolicies/bulk-get`エンドポイントを使用して、IDに基づいて複数のマージポリシーを取得することもできます。 これらの各呼び出しを実行する手順を、以下の各節で説明します。

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

`/config/mergePolicies/bulk-get`エンドポイントにPOSTリクエストを送信し、取得するマージポリシーのIDをリクエスト本文に含めることで、複数のマージポリシーを取得できます。

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

## 結合ポリシーの更新  {#update}

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

組織の結合ポリシーの作成および設定方法がわかったので、結合ポリシーを使用してPlatform内の顧客プロファイルの表示を調整したり、[!DNL Real-time Customer Profile]データからオーディエンスセグメントを作成したりできます。 セグメントの定義と使用を開始するには、[Adobe Experience Platform セグメント化サービス](../../segmentation/home.md)のドキュメントを参照してください。

## 付録

この節では、マージポリシーの操作に関する補足情報を説明します。

### カスタムタイムスタンプの使用{#custom-timestamps}

記録をExperience Platformに取り込むと、取り込み時にシステムタイムスタンプを取得し、記録に追加する。 マージポリシーの`attributeMerge`タイプとして`timestampOrdered`が選択されている場合、プロファイルはシステムのタイムスタンプに基づいてマージされます。 つまり、記録がプラットフォームに取り込まれた時のタイムスタンプに基づいて結合が行われます。

場合によっては、データのバックフィルや、レコードが順番に取り込まれない場合のイベントの正しい順序の確認など、カスタムタイムスタンプを指定し、マージポリシーでシステムタイムスタンプではなくカスタムタイムスタンプが適用される場合があります。

カスタムタイムスタンプを使用するには、[[!DNL External Source System Audit Details] スキーマフィールドグループ](#field-group-details)をプロファイルスキーマに追加する必要があります。 追加したカスタムタイムスタンプは、`xdm:lastUpdatedDate`フィールドを使用して入力できます。 `xdm:lastUpdatedDate`フィールドにデータを入力してレコードを取り込むと、Experience Platformはそのフィールドを使用して、データセット内およびデータセット間でレコードまたはプロファイルフラグメントを結合します。 `xdm:lastUpdatedDate`が存在しない場合、または入力されていない場合、プラットフォームは引き続きシステムタイムスタンプを使用します。

>[!NOTE]
>
>同じレコードでPATCHを送信する場合は、`xdm:lastUpdatedDate`タイムスタンプが設定されていることを確認する必要があります。

スキーマレジストリAPIを使用したスキーマの操作手順(スキーマへのフィールドグループの追加方法など)については、[API](../../xdm/tutorials/create-schema-api.md)を使用したスキーマの作成のチュートリアルを参照してください。

UIを使用してカスタムタイムスタンプを操作するには、『[merge policies user guide](../ui/merge-policies.md)』の「using custom timestamps](../ui/merge-policies.md#custom-timestamps)」の節を参照してください。[

#### [!DNL External Source System Audit Details] フィールドグループの詳細  {#field-group-details}

次の例は、[!DNL External Source System Audit Details]フィールドグループに正しく入力されたフィールドを示しています。 完全なフィールドグループJSONは、GitHubの[パブリックエクスペリエンスデータモデル(XDM)レポ](https://github.com/adobe/xdm/blob/master/components/mixins/shared/external-source-system-audit-details.schema.json)でも表示できます。

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
