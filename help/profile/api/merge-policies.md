---
keywords: Experience Platform;プロファイル;リアルタイム顧客プロファイル;トラブルシューティング;API
title: 結合ポリシー API エンドポイント
type: Documentation
description: Adobe Experience Platform では、個々の顧客の全体像を把握するために、複数のソースから得られたデータフラグメントを統合し組み合わせることができます。結合ポリシーとは、データを統合する場合のデータの優先順位付け方法と、統合されたビューを作成するためにどのデータを組み合わせるかを決定するためにExperience Platformで使用されるルールです。
role: Developer
exl-id: fb49977d-d5ca-4de9-b185-a5ac1d504970
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '2468'
ht-degree: 63%

---

# 結合ポリシーエンドポイント

Adobe Experience Platform では、個々の顧客の全体像を把握するために、複数のソースから得られたデータフラグメントを統合し組み合わせることができます。結合ポリシーとは、データを統合する場合のデータの優先順位付け方法と、統合されたビューを作成するためにどのデータを組み合わせるかを決定するために [!DNL Experience Platform] が使用するルールです。

例えば、顧客が複数のチャネルをまたがって自社のブランドとやり取りを行う場合、1 人の顧客に関連する複数のプロファイルフラグメントが複数のデータセットに表示されます。これらのフラグメントがExperience Platformに取り込まれると、それらのフラグメントが結合され、その顧客用に単一のプロファイルが作成されます。 複数のソースからのデータが競合する場合（例えば、1 つのフラグメントリストが顧客について「独身」、他のリストが「既婚」としている場合）、結合ポリシーが、個人のプロファイルに含める情報を決定します。

RESTful API またはユーザーインターフェイスを介すると、新しい結合ポリシーの作成、既存のポリシーの管理、組織のデフォルトの結合ポリシーの設定をおこなえます。このガイドでは、API を使用して結合ポリシーを操作する手順を説明します。

UI を使用して結合ポリシーを使用するには、『 [ 結合ポリシーのユーザガイド ](../merge-policies/ui-guide.md) 』を参照してください。 一般的な結合ポリシーとそのExperience Platform内での役割について詳しくは、まず [ 結合ポリシーの概要 ](../merge-policies/overview.md) をお読みください。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Real-Time Customer Profile API]](https://www.adobe.com/go/profile-apis-en) の一部です。先に進む前に、[はじめる前に](getting-started.md)のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の [!DNL Experience Platform] API の呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## 結合ポリシーのコンポーネント {#components-of-merge-policies}

結合ポリシーは組織に対して非公開であり、必要に応じてスキーマを結合するために様々なポリシーを作成できます。 [!DNL Profile] データにアクセスする API には結合ポリシーが必要ですが、明示的に指定されていない場合はデフォルトが使用されます。 組織に [!DNL Experience Platform] デフォルトの結合ポリシーを提供するか、特定のエクスペリエンスデータモデル（XDM）スキーマクラスの結合ポリシーを作成して、組織のデフォルトとしてマークすることができます。

各組織はスキーマクラスごとに複数の結合ポリシーを持つ可能性がありますが、各クラスにはデフォルトの結合ポリシーを 1 つのみ持つことができます。 デフォルトとして設定された結合ポリシーは、スキーマクラスの名前が指定され、結合ポリシーは必要だが指定されていない場合に使用されます。

>[!NOTE]
>
>新しい結合ポリシーをデフォルトとして設定すると、以前デフォルトとして設定されていた既存の結合ポリシーは自動的に更新され、デフォルトとして使用されなくなります。

すべてのプロファイルコンシューマーがエッジで同じビューを使用していることを確認するために、結合ポリシーをエッジでアクティブとしてマークできます。オーディエンスをエッジでアクティブ化（エッジオーディエンスとしてマーク）するには、エッジでアクティブとマークされた結合ポリシーに結び付ける必要があります。 オーディエンスがエッジでアクティブとしてマークされている結合ポリシーに結び付けられて **ない** 場合、そのオーディエンスはエッジでアクティブとマークされず、ストリーミングオーディエンスとしてマークされます。

さらに、各組織は、エッジでアクティブな **1** 結合ポリシーのみを持つことができます。 結合ポリシーがエッジでアクティブになっている場合は、Edge プロファイル、Edge セグメント化、Edgeの宛先など、エッジ上の他のシステムに使用できます。

### 完全な結合ポリシーオブジェクト

完全な結合ポリシーオブジェクトは、プロファイルフラグメントの結合の側面を制御する一連の設定を表します。

**結合ポリシーオブジェクト**

```json
    {
        "id": "{MERGE_POLICY_ID}",
        "name": "{NAME}",
        "imsOrgId": "{ORG_ID}",
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
        "isActiveOnEdge": "{BOOLEAN}",
        "default": "{BOOLEAN}",
        "updateEpoch": "{UPDATE_TIME}"
    }
```

| プロパティ | 説明 |
|---|---|
| `id` | 作成時に割り当てられる、システムで生成された一意の ID |
| `name` | リスト表示で結合ポリシーを識別できるわかりやすい名前。 |
| `imsOrgId` | この結合ポリシーが属する組織 ID |
| `schema.name` | [`schema`](#schema) オブジェクトの一部で、`name` フィールドには、結合ポリシーが関連付けられる XDM スキーマクラスが含まれます。 スキーマとクラスについて詳しくは、[XDM のドキュメント ](../../xdm/home.md) を参照してください。 |
| `version` | 結合ポリシ [!DNL Experience Platform] のバージョンが維持されました。 この読み取り専用の値は、結合ポリシーが更新されるたびに増加します。 |
| `identityGraph` | 関連 ID の取得元（ID）の ID グラフを示す [ID グラフ](#identity-graph)オブジェクト。関連するすべての ID で見つかったプロファイルフラグメントが結合されます。 |
| `attributeMerge` | [ 属性の結合 ](#attribute-merge) データが競合する場合に結合ポリシーがプロファイル属性に優先順位を付ける方法を示すオブジェクト。 |
| `isActiveOnEdge` | この結合ポリシーをエッジで使用できるかどうかを示すブール値。 デフォルトでは、この値は `false` です。 |
| `default` | この結合ポリシーが指定されたスキーマのデフォルトかどうかを示すブール値。 |
| `updateEpoch` | 結合ポリシーの最後の更新日。 |

**結合ポリシーの例**

```json
    {
        "id": "10648288-cda5-11e8-a8d5-f2801f1b9fd1",
        "name": "profile-default",
        "imsOrgId": "{ORG_ID}",
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
        "isActiveOnEdge": false,
        "default": true,
        "updateEpoch": 1551660639
    }
```

### ID グラフ {#identity-graph}

[Adobe Experience Platform ID サービス ](../../identity-service/home.md) は、[!DNL Experience Platform] でグローバルおよび各組織に使用される ID グラフを管理します。 結合ポリシーの `identityGraph` 属性は、ユーザーの関連 ID の決定方法を定義します。

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

### 属性結合 {#attribute-merge}

プロファイルフラグメントとは、特定のユーザーに存在する ID のリストからの 1 つの ID のプロファイル情報のことです。使用される ID グラフタイプの結果が複数の ID になる場合は、プロファイル属性が競合する可能性があり、優先度を指定する必要があります。 `attributeMerge` を使用すると、キー値（レコードデータ）タイプのデータセット間で結合の競合が発生した場合に優先順位を付けるプロファイル属性を指定できます。

**attributeMerge オブジェクト**

```json
    "attributeMerge": {
        "type": "{ATTRIBUTE_MERGE_TYPE}"
    }
```

ここで、`{ATTRIBUTE_MERGE_TYPE}` は次のいずれかです。

* **`timestampOrdered`**: （デフォルト）最後に更新されたプロファイルを優先します。 この結合タイプを使用する場合、`data` 属性は不要です。
* **`dataSetPrecedence`**：元のデータセットに基づいて、プロファイルフラグメントを優先します。 これは、あるデータセットに存在する情報が別のデータセットのデータよりも優先または信頼されている場合に使用できます。この結合タイプを使用する場合、`order` 属性は優先順にデータセットをリストするので、必須です。
   * **`order`**: 「dataSetPrecedence」を使用する場合は、データセットのリストを `order` 配列に指定する必要があります。 データセットに含まれていないリストは結合されません。つまり、データセットをプロファイルに結合するには、データセットを明示的にリストする必要があります。`order` 配列は、データセットの ID を優先順にリストします。

#### `dataSetPrecedence` タイプ `attributeMerge` 使用したオブジェクトの例

```json
    "attributeMerge": {
        "type": "dataSetPrecedence",
        "order": [
            "dataSetId_2", 
            "dataSetId_3", 
            "dataSetId_1", 
            "dataSetId_4"
        ]
    }
```

#### `timestampOrdered` タイプ `attributeMerge` 使用したオブジェクトの例

```json
    "attributeMerge": {
        "type": "timestampOrdered"
    }
```

### スキーマ {#schema}

スキーマオブジェクトは、この結合ポリシーが作成されるエクスペリエンスデータモデル（XDM）スキーマクラスを指定します。

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

XDM とExperience Platformでのスキーマの使用について詳しくは、まず [XDM システムの概要 ](../../xdm/home.md) を参照してください。

## 結合ポリシーへのアクセス {#access-merge-policies}

[!DNL Real-Time Customer Profile] API を使用して、`/config/mergePolicies` エンドポイントで検索リクエストを実行して、ID で特定の結合ポリシーを表示したり、特定の条件でフィルタリングされた、組織内のすべての結合ポリシーにアクセスしたりできます。 `/config/mergePolicies/bulk-get` エンドポイントを使用して、ID ごとに複数の結合ポリシーを取得することもできます。 これらの各呼び出しを実行する手順の概要を、次の節で説明します。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}
```

**応答** 

正常な応答は、結合ポリシーの詳細を返します。

```json
{
    "id": "10648288-cda5-11e8-a8d5-f2801f1b9fd1",
    "imsOrgId": "{ORG_ID}",
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
    "isActiveOnEdge": "false",
    "default": false,
    "updateEpoch": 1551127597
}
```

結合ポリシーを構成する個々の要素の詳細については、このドキュメントの最初にある「[結合ポリシーのコンポーネント](#components-of-merge-policies)」の節を参照してください。

### ID による複数の結合ポリシーの取得

`/config/mergePolicies/bulk-get` エンドポイントに POST リクエストを実行し、取得する結合ポリシーの ID をリクエスト本文に含めることで、複数の結合ポリシーを取得できます。

**API 形式**

```http
POST /config/mergePolicies/bulk-get
```

**リクエスト**

リクエスト本文には、詳細を取得したい各結合ポリシーの「id」を含む個々のオブジェクトを持つ「ids」配列が含まれます。

```shell
curl -X POST \
  'https://platform.adobe.io/data/core/ups/config/mergePolicies/bulk-get' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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

応答が成功すると、HTTP ステータス 207 （複数ステータス）と、POST リクエストで ID が指定された結合ポリシーの詳細が返されます。

```json
{ 
    "results": { 
        "0bf16e61-90e9-4204-b8fa-ad250360957b": {
            "id": "0bf16e61-90e9-4204-b8fa-ad250360957b",
            "name": "Profile Default Merge Policy",
            "imsOrgId": "{ORG_ID}",
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
            "isActiveOnEdge": true,
            "default": true,
            "updateEpoch": 1552086578
        },
        "42d4a596-b1c6-46c0-994e-ca5ef1f85130": {
            "id": "42d4a596-b1c6-46c0-994e-ca5ef1f85130",
            "name": "Dataset Precedence Merge Policy",
            "imsOrgId": "{ORG_ID}",
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
            "isActiveOnEdge": false,
            "default": false,
            "updateEpoch": 1576099719
        }
    }
}
```

結合ポリシーを構成する個々の要素の詳細については、このドキュメントの最初にある「[結合ポリシーのコンポーネント](#components-of-merge-policies)」の節を参照してください。

### 条件別の複数の結合ポリシーのリスト

`/config/mergePolicies` エンドポイントに対してGET リクエストを発行し、オプションのクエリパラメーターを使用して応答をフィルタリング、並べ替え、ページ付けすることで、組織内の複数の結合ポリシーをリストできます。 複数のパラメーターを含める場合は、アンパサンド（&amp;）で区切ります。パラメーターを指定しないでこのエンドポイントを呼び出すと、組織で使用可能なすべての結合ポリシーが取得されます。

**API 形式**

```http
GET /config/mergePolicies?{QUERY_PARAMS}
```

| パラメーター | 説明 |
|---|---|
| `default` | 結合ポリシーがフィルタークラスのデフォルトであるかどうかによって結果をスキーマするブール値。 |
| `limit` | ページに含める結果の数を制御するためのページサイズの制限を指定します。デフォルト値：20 |
| `orderBy` | 名前を昇順で並べ替えるには `orderBy=name` または `orderBy=+name` のように結果を並べ替えるフィールドを指定し、降順で並べ替えるには `orderBy=-name` を指定します。この値を省略すると、`name` のデフォルトの並べ替え順が昇順になります。 |
| `isActiveOnEdge` | エッジで結合ポリシーがアクティブかどうかで結果をフィルタリングするブール値。 |
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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
            "imsOrgId": "{ORG_ID}",
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
            "isActiveOnEdge": true,
            "default": true,
            "updateEpoch": 1552086578
        },
        {
            "id": "42d4a596-b1c6-46c0-994e-ca5ef1f85130",
            "name": "Dataset Precedence Merge Policy",
            "imsOrgId": "{ORG_ID}",
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
            "isActiveOnEdge": false,
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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "Loyalty members ordered by ID",
    "identityGraph": {
        "type": "none"
    },
    "attributeMerge": {
        "type":"dataSetPrecedence",
        "order": [
            "5b76f86b85d0e00000be5c8b",
            "5b76f8d787a6af01e2ceda18"
        ]
    },
    "schema": {
        "name":"_xdm.context.profile"
    },
    "isActiveOnEdge": true,
    "default": true
}'
```

| プロパティ | 説明 |
|---|---|
| `name` | 結合ポリシーをリスト表示で識別できる、わかりやすい名前。 |
| `identityGraph.type` | 結合する関連 ID を取得する ID グラフのタイプ。可能な値：「none」または「pdg」（プライベートグラフ）。 |
| `attributeMerge` | データの競合時にプロファイル属性値に優先順位を付ける方法。 |
| `schema` | 結合ポリシーに関連付けられた XDM スキーマクラス。 |
| `isActiveOnEdge` | この結合ポリシーがエッジ上でアクティブかどうかを指定します。 |
| `default` | この結合ポリシーがスキーマのデフォルトかどうかを指定します。 |

詳細は、「[結合ポリシーのコンポーネント](#components-of-merge-policies)」の節を参照してください。

**応答** 

正常な応答は、新しく作成された結合ポリシーの詳細を返します。

```json
{
    "id": "e5bc94de-cd14-4cdf-a2bc-88b6e8cbfac2",
    "name": "Loyalty members ordered by ID",
    "imsOrgId": "{ORG_ID}",
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
    "isActiveOnEdge": true,
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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `op` | 操作を指定します。その他のパッチ操作の例については、 [JSON パッチのドキュメント](https://datatracker.ietf.org/doc/html/rfc6902)を参照してください |
| `path` | 更新するフィールドのパス。指定できる値は、「/name」、「/identityGraph.type」、「/attributeMerge.type」、「/schema.name」、「/version」、「/default」、「/isActiveOnEdge」です。 |
| `value` | 指定したフィールドに設定する値。 |

詳細は、「[結合ポリシーのコンポーネント](#components-of-merge-policies)」の節を参照してください。


**応答** 

正常な応答は、新しく更新された結合ポリシーの詳細を返します。

```json
{
    "id": "e5bc94de-cd14-4cdf-a2bc-88b6e8cbfac2",
    "name": "Loyalty members ordered by ID",
    "imsOrgId": "{ORG_ID}",
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
    "isActiveOnEdge": true,
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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "name": "Loyalty members ordered by ID",
        "imsOrgId": "{ORG_ID}",
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
        "isActiveOnEdge": true,
        "default": true,
        "updateEpoch": 1551898378
    }'
```

| プロパティ | 説明 |
|---|---|
| `name` | 結合ポリシーをリスト表示で識別できる、わかりやすい名前。 |
| `identityGraph` | 結合する関連 ID を取得する ID グラフ。 |
| `attributeMerge` | データの競合時にプロファイル属性値に優先順位を付ける方法。 |
| `schema` | 結合ポリシーに関連付けられた XDM スキーマクラス。 |
| `isActiveOnEdge` | この結合ポリシーがエッジ上でアクティブかどうかを指定します。 |
| `default` | この結合ポリシーがスキーマのデフォルトかどうかを指定します。 |

詳細は、「[結合ポリシーのコンポーネント](#components-of-merge-policies)」の節を参照してください。

**応答** 

正常な応答は、更新された結合ポリシーの詳細を返します。

```json
{
    "id": "e5bc94de-cd14-4cdf-a2bc-88b6e8cbfac2",
    "name": "Loyalty members ordered by ID",
    "imsOrgId": "{ORG_ID}",
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
    "isActiveOnEdge": true,
    "default": true,
    "updateEpoch": 1551898378
}
```

## 結合ポリシーの削除

結合ポリシーを削除するには、`/config/mergePolicies` エンドポイントに DELETE リクエストをおこない、削除する結合ポリシーの ID をリクエストパスに含めます。

>[!NOTE]
>
>結合ポリシーが true に設定されてい `isActiveOnEdge` い場合、結合ポリシー **削除できません**。 結合ポリシーを削除する前に、[PATCH](#edit-individual-merge-policy-fields) エンドポイントまたは [PUT](#overwrite-a-merge-policy) エンドポイントを使用して結合ポリシーを更新します。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答** 

削除リクエストが成功すると、HTTP ステータス 200（OK）と空の応答本文が返されます。削除が成功したことを確認するには、GET リクエストを実行して、ID ごとに結合ポリシーを表示します。結合ポリシーが削除されている場合は、HTTP ステータス 404（不検知）エラーが表示されます。

## 次の手順

これで、組織の結合ポリシーを作成および設定する方法がわかったので、それらを使用して、Experience Platform内の顧客プロファイルのビューを調整し、[!DNL Real-Time Customer Profile] データからオーディエンスを作成できます。

オーディエンスの定義と操作を開始するには ](../../segmentation/home.md)[Adobe Experience Platform セグメント化サービスのドキュメントを参照してください。
