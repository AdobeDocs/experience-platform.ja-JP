---
keywords: Experience Platform、プロファイル、リアルタイム顧客プロファイル、トラブルシューティング、API
title: 計算済み属性 API エンドポイント
topic-legacy: guide
type: Documentation
description: Adobe Experience Platformでは、計算済み属性は、イベントレベルのデータをプロファイルレベルの属性に集計するために使用される関数です。 これらの関数は自動的に計算され、セグメント化、アクティブ化およびパーソナライズ機能で使用できます。このガイドでは、リアルタイム顧客プロファイル API を使用して計算済み属性を作成、表示、更新および削除する方法について説明します。
exl-id: 6b35ff63-590b-4ef5-ab39-c36c39ab1d58
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '2275'
ht-degree: 65%

---

# （アルファ）計算済み属性 API エンドポイント

>[!IMPORTANT]
>
>このドキュメントで説明されている計算済み属性機能は、現在アルファ段階であり、すべてのユーザーが使用できるわけではありません。ドキュメントと機能は変更される場合があります。

計算済み属性は、イベントレベルのデータをプロファイルレベルの属性に集計するために使用される関数です。これらの関数は自動的に計算され、セグメント化、アクティブ化およびパーソナライズ機能で使用できます。このガイドには、 `/computedAttributes` endpoint.

計算済み属性の詳細については、まず [計算済み属性の概要](overview.md).

## はじめに

このガイドで使用される API エンドポイントは、 [リアルタイム顧客プロファイル API](https://www.adobe.com/go/profile-apis-jp).

続行する前に、 [プロファイル API 入門ガイド](../api/getting-started.md) 推奨ドキュメントへのリンク、このドキュメントに表示される API 呼び出し例の読み方のガイド、および任意のExperience PlatformAPI を正しく呼び出すために必要な必須ヘッダーに関する重要な情報。

## 計算属性フィールドの設定

計算済み属性を作成するには、まず、計算済み属性値を保持するスキーマ内のフィールドを識別する必要があります。

詳しくは、 [計算済み属性の設定](configure-api.md) スキーマで計算済み属性フィールドを作成するための完全なエンドツーエンドのガイドを参照してください。

>[!WARNING]
>
>API ガイドに進むには、計算済み属性フィールドを設定する必要があります。

## 計算済み属性の作成 {#create-a-computed-attribute}

プロファイル対応スキーマで定義した計算済み属性フィールドを使用して、計算済み属性を設定できます。 まだ実行していない場合は、 [計算済み属性の設定](configure-api.md) ドキュメント。

計算済み属性を作成するには、まず `/config/computedAttributes` エンドポイント：作成する計算済み属性の詳細が含まれるリクエスト本文を持ちます。

**API 形式**

```http
POST /config/computedAttributes
```

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/config/computedAttributes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}'\
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "name": "birthdayCurrentMonth",
        "path": "_{TENANT_ID}",
        "description": "Computed attribute to capture if the customer birthday is in the current month.",
        "expression": {
            "type": "PQL", 
            "format": "pql/text", 
            "value": "person.birthDate.getMonth() = currentMonth()"
        },
        "schema": 
          {
            "name":"_xdm.context.profile"
          }
          
      }'
```

| プロパティ | 説明 |
|---|---|
| `name` | 計算済み属性フィールドの名前（文字列）。 |
| `path` | 計算済み属性を含むフィールドへのパス。このパスは、スキーマの `properties` 属性内にあり、パスにフィールド名を含めないでください。パスを書き込む場合は、複数レベルの `properties` 属性を省略します。 |
| `{TENANT_ID}` | テナント ID に慣れていない場合は、[スキーマレジストリ開発者ガイド](../../xdm/api/getting-started.md#know-your-tenant_id)で、テナント ID を見つける手順を参照してください。 |
| `description` | 計算済み属性の説明。複数の計算済み属性を定義した場合は特に便利です。IMS 組織内の他のユーザーが、使用する正しい計算済み属性を判断するのに役立ちます。 |
| `expression.value` | 有効な [!DNL Profile Query Language] (PQL) 式を使用します。 計算済み属性は現在、次の関数をサポートしています。sum、count、min、max および boolean。 サンプル式のリストについては、 [サンプル PQL 式](expressions.md) ドキュメント。 |
| `schema.name` | 計算済み属性フィールドを含むスキーマの基となるクラス。例：XDM ExperienceEvent クラスに基づくスキーマの場合 `_xdm.context.experienceevent`。 |

**応答** 

正常に作成された計算済み属性は、HTTP ステータス 200（OK）と、新しく作成された計算済み属性の詳細を含む応答本文を返します。これらの詳細には、他の API 操作中に計算済み属性を参照するために使用できる、システムで生成された一意の読み取り専用 `id` が含まれます。

```json
{
    "id": "2afcf410-450e-4a39-984d-2de99ab58877",
    "imsOrgId": "{ORG_ID}",
    "sandbox": {
        "sandboxId": "{SANDBOX_ID}",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "name": "birthdayCurrentMonth",
    "path": "_{TENANT_ID}",
    "positionPath": [
        "_{TENANT_ID}"
    ],
    "description": "Computed attribute to capture if the customer birthday is in the current month.",
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "person.birthDate.getMonth() = currentMonth()"
    },
    "schema": {
        "name": "_xdm.context.profile"
    },
    "returnSchema": {
        "meta:xdmType": "string"
    },
    "definedOn": [
        {
            "meta:resourceType": "unions",
            "meta:containerId": "tenant",
            "$ref": "https://ns.adobe.com/xdm/context/profile__union"
        }
    ],
    "encodedDefinedOn":"?\b?VR)JMS?R?())(????+?KL?OJ?K???H??O??+I?(?/(?O??I??/????S?8{?E:",
    "dependencies": [],
    "dependents": [],
    "active": true,
    "type": "ComputedAttribute",
    "createEpoch": 1572555223,
    "updateEpoch": 1572555225
}
```

| プロパティ | 説明 |
|---|---|
| `id` | 他の API 操作中に計算済み属性を参照するために使用できる、システムで生成された一意の読み取り専用 ID が含まれます。 |
| `imsOrgId` | 計算済み属性に関連する IMS 組織は、要求で送信される値と一致する必要があります。 |
| `sandbox` | サンドボックスオブジェクトには、計算済み属性が設定されたサンドボックスの詳細が含まれます。この情報は、リクエストで送信されるサンドボックスヘッダーから取得されます。詳しくは、[サンドボックスの概要](../../sandboxes/home.md)を参照してください。 |
| `positionPath` | リクエストで送信されたフィールドに分解された `path` を含む配列。 |
| `returnSchema.meta:xdmType` | 計算済み属性が保存されるフィールドのタイプ。 |
| `definedOn` | 計算済み属性が定義された和集合スキーマを示す配列。和集合スキーマごとに 1 つのオブジェクトを含みます。つまり、異なるクラスに基づいて計算された属性が複数のスキーマに追加された場合、配列内に複数のオブジェクトが存在する可能性があります。 |
| `active` | 計算済み属性が現在アクティブかどうかを示すブール値。デフォルト値は `true` です。 |
| `type` | 作成されるリソースのタイプ。この場合は「ComputedAttribute」がデフォルト値です。 |
| `createEpoch` および `updateEpoch` | 計算済み属性が作成された時刻と最後に更新された時刻。 |

## 既存の計算済み属性を参照する計算済み属性を作成します

既存の計算済み属性を参照する計算済み属性を作成することもできます。 これをおこなうには、まず `/config/computedAttributes` endpoint. リクエスト本文には、計算済み属性への参照が `expression.value` フィールドに値を入力します。

**API 形式**

```http
POST /config/computedAttributes
```

**リクエスト**

この例では、2 つの計算済み属性が既に作成されており、3 番目の計算済み属性を定義するために使用されます。 既存の計算済み属性は次のとおりです。

* **`totalSpend`:** 顧客が支出した合計ドル額をキャプチャします。
* **`countPurchases`:** 顧客が行った購入の数をカウントします。

以下のリクエストは、2 つの既存の計算済み属性を参照し、新しいを計算するために有効な PQL を使用して除算します `averageSpend` 計算済み属性。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/config/computedAttributes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}'\
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "name": "averageSpend",
        "path": "_{TENANT_ID}.purchaseSummary",
        "description": "Computed attribute to capture the average dollar amount that a customer spends on each purchase.",
        "expression": {
            "type": "PQL", 
            "format": "pql/text", 
            "value": "_{TENANT_ID}.purchaseSummary.totalSpend/_{TENANT_ID}.purchaseSummary.countPurchases"
        },
        "schema": 
          {
            "name":"_xdm.context.profile"
          }
          
      }'
```

| プロパティ | 説明 |
|---|---|
| `name` | 計算済み属性フィールドの名前（文字列）。 |
| `path` | 計算済み属性を含むフィールドへのパス。このパスは、スキーマの `properties` 属性内にあり、パスにフィールド名を含めないでください。パスを書き込む場合は、複数レベルの `properties` 属性を省略します。 |
| `{TENANT_ID}` | テナント ID に慣れていない場合は、[スキーマレジストリ開発者ガイド](../../xdm/api/getting-started.md#know-your-tenant_id)で、テナント ID を見つける手順を参照してください。 |
| `description` | 計算済み属性の説明。複数の計算済み属性を定義した場合は特に便利です。IMS 組織内の他のユーザーが、使用する正しい計算済み属性を判断するのに役立ちます。 |
| `expression.value` | 有効な PQL 式。 計算済み属性は現在、次の関数をサポートしています。sum、count、min、max および boolean。 サンプル式のリストについては、 [サンプル PQL 式](expressions.md) ドキュメント。<br/><br/>この例では、式は 2 つの既存の計算済み属性を参照します。 属性は、 `path` そして `name` 計算済み属性が定義されたスキーマ内に表示される計算済み属性。 例えば、 `path` 最初に参照される計算済み属性の `_{TENANT_ID}.purchaseSummary` そして `name` が `totalSpend`. |
| `schema.name` | 計算済み属性フィールドを含むスキーマの基となるクラス。例：XDM ExperienceEvent クラスに基づくスキーマの場合 `_xdm.context.experienceevent`。 |

**応答** 

正常に作成された計算済み属性は、HTTP ステータス 200（OK）と、新しく作成された計算済み属性の詳細を含む応答本文を返します。これらの詳細には、他の API 操作中に計算済み属性を参照するために使用できる、システムで生成された一意の読み取り専用 `id` が含まれます。

```json
{
    "id": "2afcf410-450e-4a39-984d-2de99ab58877",
    "imsOrgId": "{ORG_ID}",
    "sandbox": {
        "sandboxId": "{SANDBOX_ID}",
        "sandboxName": "{SANDBOX_NAME}",
        "type": "production",
        "default": true
    },
    "name": "averageSpend",
    "path": "_{TENANT_ID}.purchaseSummary",
    "positionPath": [
        "_{TENANT_ID}",
        "purchaseSummary"
    ],
    "description": "Computed attribute to capture the average dollar amount that a customer spends on each purchase.",
    "expression": {
            "type": "PQL", 
            "format": "pql/text", 
            "value":  "_{TENANT_ID}.purchaseSummary.totalSpend/_{TENANT_ID}.purchaseSummary.countPurchases"
    },
    "schema": {
        "name": "_xdm.context.profile"
    },
    "returnSchema": {
        "meta:xdmType": "number"
    },
    "definedOn": [
        {
          "meta:resourceType": "unions",
          "meta:containerId": "tenant",
          "$ref": "https://ns.adobe.com/xdm/context/profile__union"
        }
    ],
    "encodedDefinedOn":"\bVR)JMSR())(+KLOJKկHO+I(/(OI/S8{E:",
    "dependencies": [
        "c08a92f3-2418-4a3d-89d0-96f15fda3e5d",
        "4ed9e3aa-57ae-4705-9e8a-7fba9a6a7010"
    ],
    "dependents": [],
    "active": true,
    "evaluationInfo": {
        "batch": {
          "enabled": false
        },
        "continuous": {
          "enabled": true
        },
        "synchronous": {
          "enabled": false
        }
      },
    "type": "ComputedAttribute",
    "createEpoch": 1613696592,
    "updateEpoch": 1613696593
}
```

| プロパティ | 説明 |
|---|---|
| `id` | 他の API 操作中に計算済み属性を参照するために使用できる、システムで生成された一意の読み取り専用 ID が含まれます。 |
| `imsOrgId` | 計算済み属性に関連する IMS 組織は、要求で送信される値と一致する必要があります。 |
| `sandbox` | サンドボックスオブジェクトには、計算済み属性が設定されたサンドボックスの詳細が含まれます。この情報は、リクエストで送信されるサンドボックスヘッダーから取得されます。詳しくは、[サンドボックスの概要](../../sandboxes/home.md)を参照してください。 |
| `positionPath` | リクエストで送信されたフィールドに分解された `path` を含む配列。 |
| `returnSchema.meta:xdmType` | 計算済み属性が保存されるフィールドのタイプ。 |
| `definedOn` | 計算済み属性が定義された和集合スキーマを示す配列。和集合スキーマごとに 1 つのオブジェクトを含みます。つまり、異なるクラスに基づいて計算された属性が複数のスキーマに追加された場合、配列内に複数のオブジェクトが存在する可能性があります。 |
| `active` | 計算済み属性が現在アクティブかどうかを示すブール値。デフォルト値は `true` です。 |
| `type` | 作成されるリソースのタイプ。この場合は「ComputedAttribute」がデフォルト値です。 |
| `createEpoch` および `updateEpoch` | 計算済み属性が作成された時刻と最後に更新された時刻。 |

## 計算済み属性へのアクセス

API を使用して計算済み属性を操作する場合、組織で定義された計算済み属性にアクセスするためのオプションは 2 つあります。1 つ目は、すべての計算済み属性をリストする方法、もう 1 つは、一意の `id` によって固有の計算済み属性を表示する方法です。

両方のアクセスパターンの手順については、このドキュメントで概要を説明します。 開始するには、次のいずれかを選択します。

* **[既存の計算済み属性をすべてリスト](#list-all-computed-attributes):** 組織が作成したすべての既存の計算済み属性のリストを返します。
* **[特定の計算済み属性の表示](#view-a-computed-attribute):** リクエスト時に ID を指定して、1 つの計算済み属性の詳細を返します。

### すべての計算済み属性のリスト {#list-all-computed-attributes}

IMS 組織は複数の計算済み属性を作成できます。`/config/computedAttributes` エンドポイントに対して GET リクエストを実行すると、組織の既存計算済み属性をすべてリストできます。

**API 形式**

```http
GET /config/computedAttributes
```

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/config/computedAttributes/ \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答** 

成功応答には、`_page` 属性が含まれ、計算済み属性（`totalCount`）の数とページ上の計算済み属性の数（`pageSize`）を提供します。

応答には、1 つ以上のオブジェクトで構成される `children` 配列も含まれ、それぞれに 1 つの計算済み属性の詳細が含まれます。計算済み属性が組織にない場合、 `totalCount` および `pageSize` は 0（ゼロ）で、 `children` 配列が空になります。

```json
{
    "_page": {
        "totalCount": 2,
        "pageSize": 2
    },
    "children": [
        {
            "id": "2afcf410-450e-4a39-984d-2de99ab58877",
            "imsOrgId": "{ORG_ID}",
            "sandbox": {
                "sandboxId": "ff0f6870-c46d-11e9-8ca3-036939a64204",
                "sandboxName": "prod",
                "type": "production",
                "default": true
            },
            "name": "birthdayCurrentMonth",
            "path": "person._{TENANT_ID}",
            "positionPath": [
                "person",
                "_{TENANT_ID}"
            ],
            "description": "Computed attribute to capture if the customer birthday is in the current month.",
            "expression": {
                "type": "PQL",
                "format": "pql/text",
                "value": "person.birthDate.getMonth() = currentMonth()"
            },
            "schema": {
                "name": "_xdm.context.profile"
            },
            "returnSchema": {
                "meta:xdmType": "string"
            },
            "definedOn": [
                {
                    "meta:resourceType": "unions",
                    "meta:containerId": "tenant",
                    "$ref": "https://ns.adobe.com/xdm/context/profile__union"
                }
            ],
            "encodedDefinedOn":"?\b?VR)JMS?R?())(????+?KL?OJ?K???H??O??+I?(?/(?O??I??/????S?8{?E:",
            "dependencies": [],
            "dependents": [],
            "active": true,
            "type": "ComputedAttribute",
            "createEpoch": 1572555223,
            "updateEpoch": 1572555225
        },
        {
            "id": "ae0c6552-cf49-4725-8979-116366e8e8d3",
            "imsOrgId": "{ORG_ID}",
            "sandbox": {
                "sandboxId": "",
                "sandboxName": "",
                "type": "production",
                "default": true
            },
            "name": "productDownloads",
            "path": "_{TENANT_ID}",
            "positionPath": [
                "_{TENANT_ID}"
            ],
            "description": "Calculate total product downloads.",
            "expression": {
                "type": "PQL", 
                "format": "pql/text", 
                "value":  "let Y = xEvent[_coresvc.event.subType = \"DOWNLOAD\"].groupBy(_coresvc.attributes[name = \"product\"].value).map({
                  \"downloaded\": this.head()._coresvc.attributes[name = \"product\"].head().value,
                  \"downloadsSum\": this.count(),
                  \"downloadsToday\": this[timestamp occurs today].count(),
                  \"downloadsPast30Days\": this[timestamp occurs < 30 days before now].count(),
                  \"downloadsPast60Days\": this[timestamp occurs < 60 days before now].count(),
                  \"downloadsPast90Days\": this[timestamp occurs < 90 days before now].count() }) in { \"uniqueProductDownloadSum\": Y.count(), \"products\": Y }"
            },
            "returnSchema": {
                "meta:xdmType": "string"
            },
            "definedOn": [
                {
                    "meta:resourceType": "unions",
                    "meta:containerId": "tenant",
                    "$ref": "https://ns.adobe.com/xdm/context/profile__union"
                }
            ],
            "schema": {
                "name": "_xdm.context.profile"
            },
            "encodedDefinedOn": "\u001f?\b\u0000\u0000\u0000\u0000\u0000\u0000\u0000?VR)JMS?R?())(????+?KL?OJ?K???H??O??+I?(?/(?O??I??/????S?\u0005\u00008{?E:\u0000\u0000\u0000",
            "dependencies": [],
            "dependents": [],
            "active": true,
            "type": "ComputedAttribute",
            "createEpoch": 1571945277,
            "updateEpoch": 1571945280
        }
    ],
    "_links": {
        "next": {}
    }
}
```

| プロパティ | 説明 |
|---|---|
| `_page.totalCount` | IMS 組織で定義された計算済み属性の合計数です。 |
| `_page.pageSize` | 結果のこのページで返された計算済み属性の数。`pageSize` が `totalCount` と等しい場合、結果のページが 1 ページのみで、計算済みの属性がすべて返されたことを意味します。等しくない場合は、アクセス可能な結果のページが他にあります。詳細は、`_links.next` を参照してください。 |
| `children` | 1 つ以上のオブジェクトで構成される配列。各オブジェクトには、1 つの計算済み属性の詳細が含まれます。計算済みの属性が定義されていない場合、`children` 配列は空になります。 |
| `id` | 計算済み属性の作成時に自動的に割り当てられる、システムで生成された読み取り専用の一意の値。計算済み属性オブジェクトのコンポーネントの詳細については、このチュートリアルで既に説明した[計算済み属性の作成](#create-a-computed-attribute)の節を参照してください。 |
| `_links.next` | 1 ページの計算済み属性が返される場合、前述の応答例のように、`_links.next` は空のオブジェクトになります。組織に多数の計算済み属性がある場合、`_links.next` 値に対して GET リクエストを送信することでアクセスできる、複数のページで返されます。 |

### 計算済み属性の表示 {#view-a-computed-attribute}

特定の計算済み属性を表示するには、 `/config/computedAttributes` エンドポイントを作成し、計算済み属性 ID をリクエストパスに含めます。

**API 形式**

```http
GET /config/computedAttributes/{ATTRIBUTE_ID}
```

| パラメーター | 説明 |
|---|---|
| `{ATTRIBUTE_ID}` | 表示する計算済み属性の ID。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/config/computedAttributes/2afcf410-450e-4a39-984d-2de99ab58877 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答** 

```json
{
    "id": "2afcf410-450e-4a39-984d-2de99ab58877",
    "imsOrgId": "{ORG_ID}",
    "sandbox": {
        "sandboxId": "ff0f6870-c46d-11e9-8ca3-036939a64204",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "name": "birthdayCurrentMonth",
    "path": "_{TENANT_ID}",
    "positionPath": [
        "_{TENANT_ID}"
    ],
    "description": "Computed attribute to capture if the customer birthday is in the current month.",
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "person.birthDate.getMonth() = currentMonth()"
    },
    "schema": {
        "name": "_xdm.context.profile"
    },
    "returnSchema": {
        "meta:xdmType": "string"
    },
    "definedOn": [
        {
            "meta:resourceType": "unions",
            "meta:containerId": "tenant",
            "$ref": "https://ns.adobe.com/xdm/context/profile__union"
        }
    ],
    "encodedDefinedOn":"?\b?VR)JMS?R?())(????+?KL?OJ?K???H??O??+I?(?/(?O??I??/????S?8{?E:",
    "dependencies": [],
    "dependents": [],
    "active": true,
    "type": "ComputedAttribute",
    "createEpoch": 1572555223,
    "updateEpoch": 1572555225
}
```

## 計算済み属性の更新

既存の計算済み属性を更新する必要がある場合、これをおこなうには、`/config/computedAttributes` エンドポイントに PATCH リクエストを作成し、更新する計算済みの属性の ID をリクエストパスに含めます。

**API 形式**

```http
PATCH /config/computedAttributes/{ATTRIBUTE_ID}
```

| パラメーター | 説明 |
|---|---|
| `{ATTRIBUTE_ID}` | 更新する計算済み属性の ID。 |

**リクエスト**

このリクエストでは、[JSON パッチのフォーマット設定](https://datatracker.ietf.org/doc/html/rfc6902)を使用して、「式」フィールドの「値」を更新します。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/core/ups/config/computedAttributes/ae0c6552-cf49-4725-8979-116366e8e8d3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}'\
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \  
  -d '[
        {
          "op": "add",
          "path": "/expression",
          "value": 
          {
            "type": "PQL", 
            "format": "pql/text", 
            "value":  "{NEW_EXPRESSION_VALUE}"
          }
        }
      ]'
```

| プロパティ | 説明 |
|---|---|
| `{NEW_EXPRESSION_VALUE}` | 有効な [!DNL Profile Query Language] (PQL) 式を使用します。 計算済み属性は現在、次の関数をサポートしています。sum、count、min、max および boolean。 サンプル式のリストについては、 [サンプル PQL 式](expressions.md) ドキュメント。 |

**応答** 

成功応答は、HTTP ステータス 204（コンテンツなし）と空の応答本文が返されます。削除が成功したことを確認するには、GET リクエストを実行して、計算済み属性を ID で参照できます。

## 計算済み属性の削除

API を使用して、計算済み属性を削除することもできます。これには、`/config/computedAttributes` エンドポイントに DELETE リクエストを送信し、削除する計算済み属性の ID をリクエストパスに含めます。

>[!NOTE]
>
>計算済み属性を削除する場合は、複数のスキーマで使用されている可能性があるので注意が必要です。また、DELETE 操作を元に戻すことはできません。

**API 形式**

```http
DELETE /config/computedAttributes/{ATTRIBUTE_ID}
```

| パラメーター | 説明 |
|---|---|
| `{ATTRIBUTE_ID}` | 削除する計算済み属性の ID。 |

**リクエスト**

```shell
curl -X DELETE \
  https://platform.adobe.io/data/core/ups/config/computedAttributes/ae0c6552-cf49-4725-8979-116366e8e8d3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}'
  -H 'x-sandbox-name: {SANDBOX_NAME}' \ 
```

**応答** 

削除リクエストが成功すると、HTTP ステータス 200（OK）と空の応答本文が返されます。削除が成功したことを確認するために、GET リクエストを実行して、計算済み属性を ID で参照できます。属性が削除された場合は、HTTP ステータス 404（見つかりません）エラーが表示されます。

## 計算済み属性を参照するセグメント定義の作成

Adobe Experience Platform を使用すると、プロファイルのグループから特定の属性やビヘイビアーのグループを定義するセグメントを作成できます。セグメント定義には、PQL で記述されたクエリをカプセル化する式が含まれます。 これらの式は、計算済み属性を参照することもできます。

次の例では、既存の計算済み属性を参照するセグメント定義を作成します。 セグメント定義と、セグメント化サービス API での使用方法について詳しくは、 [セグメント定義 API エンドポイントガイド](../../segmentation/api/segment-definitions.md).

まず、 `/segment/definitions` エンドポイント：リクエスト本文に計算済み属性を指定します。

**API 形式**

```http
POST /segment/definitions
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/core/ups/segment/definitions
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '{
        "name": "downloadedInLast7Days",
        "description": "Has product been downloaded in last 7 days?",
        "schema": {
            "name": "_xdm.context.profile"
        },
        "ttlInDays": 30,
        "expression": {
            "type": "PQL",
            "format": "pql/text",
            "value": "_{TENANT_ID}.downloadsLast7Days > 0",
            "meta": "m"
        },
        "dataGovernancePolicy": {
            "excludeOptOut": true
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | セグメントの一意の名前（文字列）。 |
| `description` | 人間が判読できる、定義の説明。 |
| `schema.name` | 。セグメント内のエンティティに関連付けられているスキーマです。`id` か `name` のどちらかのフィールドで構成されます。 |
| `expression` | セグメント定義に関する情報を含むフィールドを含むオブジェクト。 |
| `expression.type` | 式タイプを指定します。現時点では、「PQL」のみサポートされています。 |
| `expression.format` | 値内の式の構造を示します。現在は、`pql/text` のみがサポートされています。 |
| `expression.value` | 有効な PQL 式。この例では、既存の計算済み属性への参照が含まれます。 |

スキーマ定義属性の詳細については、 [セグメント定義 API エンドポイントガイド](../../segmentation/api/segment-definitions.md).

**応答**

リクエストが成功した場合は、新しく作成したセグメント定義の詳細と HTTP ステータス 200 が返されます。セグメント定義の応答オブジェクトについて詳しくは、 [セグメント定義 API エンドポイントガイド](../../segmentation/api/segment-definitions.md).

```json
{
    "id": "add3933f-ac5c-4240-8259-3a4528ee4885",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "ttlInDays": 60,
    "id": "119835c3-5fab-4c18-ae01-4ccab328fc5c",
    "imsOrgId": "{ORG_ID}",
    "sandbox": {
        "sandboxId": "{SANDBOX_ID}",
        "sandboxName": "{SANDBOX_NAME}",
        "type": "production",
        "default": true
    },
    "name": "segment-downloadedInLast7Days",
    "description": "Has product been downloaded in last 7 days?",
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "_{TENANT_ID}.downloadsLast7Days > 0",
        "meta": "m"
    },
    "evaluationInfo": {
        "batch": {
            "enabled": false
        },
        "continuous": {
            "enabled": true
        },
        "synchronous": {
            "enabled": false
        }
    },
    "dataGovernancePolicy": {
        "excludeOptOut": true
    },
    "creationTime": 1602016581000,
    "updateEpoch": 1610609459,
    "updateTime": 1610609459000,
    "createEpoch": 1602016554,
    "_etag": "\"8b01611a-0000-0200-0000-5ffff3330000\"",
    "dependents": [
        "023d46c9-a27c-4ea9-a887-2c91ba83f2d1"
    ],
    "definedOn": [
        {
            "meta:resourceType": "unions",
            "meta:containerId": "tenant",
            "$ref": "https://ns.adobe.com/xdm/context/profile__union"
        }
    ],
    "dependencies": [
    "103d46c9-a27c-4ea9-a887-2c91ba83f2d1"
    ],
    "type": "SegmentDefinition"
}
```

## 次の手順

これで、計算済み属性の基本について学び、組織に合わせて定義を開始する準備が整いました。
