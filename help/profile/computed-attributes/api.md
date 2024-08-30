---
title: 計算属性 API エンドポイント
description: リアルタイム顧客プロファイル API を使用して計算済み属性を作成、表示、更新、削除する方法について説明します。
exl-id: f217891c-574d-4a64-9d04-afc436cf16a9
source-git-commit: 49196473f304585193e87393f8dc5dc37be7e4d9
workflow-type: tm+mt
source-wordcount: '1664'
ht-degree: 10%

---

# 計算属性 API エンドポイント

>[!IMPORTANT]
>
>API へのアクセスは制限されています。 計算済み属性 API へのアクセス方法については、Adobeサポートにお問い合わせください。

計算済み属性は、イベントレベルのデータをプロファイルレベルの属性に集計するために使用される関数です。これらの関数は自動的に計算され、セグメント化、アクティブ化およびパーソナライズ機能で使用できます。このガイドには、`/attributes` エンドポイントを使用して基本的な CRUD 操作を実行するための API 呼び出しのサンプルが含まれています。

計算属性の詳細については、まず [ 計算属性の概要 ](overview.md) を参照してください。

## はじめに

このガイドで使用する API エンドポイントは、[ リアルタイム顧客プロファイル API](https://www.adobe.com/go/profile-apis-en) の一部です。

先に進む前に、[Profile API 入門ガイド ](../api/getting-started.md) を参照し、推奨ドキュメントへのリンク、このドキュメントに表示されるサンプル API 呼び出しを読み取るためのガイドおよび任意のExperience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

さらに、次のサービスのドキュメントを確認してください。

- [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md)：[!DNL Experience Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。
   - [ スキーマレジストリ入門ガイド ](../../xdm/api/getting-started.md#know-your-tenant_id)：このガイド全体の応答に表示される、`{TENANT_ID}` ーザーに関する情報が提供されます。

## 計算属性のリストの取得 {#list}

`/attributes` エンドポイントに対してGETリクエストを行うことで、組織のすべての計算済み属性のリストを取得できます。

**API 形式**

`/attributes` エンドポイントは、結果を絞り込むのに役立つ、複数のクエリパラメーターをサポートしています。これらのパラメーターはオプションですが、リソースをリストする際の高価なオーバーヘッドを削減するために、使用することを強くお勧めします。 パラメーターを指定せずにこのエンドポイントを呼び出すと、組織で使用可能なすべての計算属性が取得されます。 複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。

```http
GET /attributes
GET /attributes?{QUERY_PARAMETERS}
```

計算済み属性のリストを取得する際には、次のクエリパラメーターを使用できます。

| クエリーパラメーター | 説明 | 例 |
| --------------- | ----------- | ------- |
| `limit` | 応答の一部として返される項目の最大数を指定するパラメーター。 このパラメーターの最小値は「1」、最大値は「40」です。 このパラメーターを含めない場合、デフォルトでは 20 個の項目が返されます。 | `limit=20` |
| `offset` | 項目を返す前にスキップする項目数を指定するパラメーター。 | `offset=5` |
| `sortBy` | 返された項目が並べ替えられる順序を指定するパラメーター。 使用可能なオプションには、`name`、`status`、`updateEpoch`、`createEpoch` などがあります。 並べ替えオプションの前に `-` を含めるか含めないかで、昇順または降順で並べ替えるかどうかを選択することもできます。 デフォルトでは、項目は `updateEpoch` 順に降順で並べ替えられます。 | `sortBy=name` |
| `property` | 様々な計算属性フィールドでフィルタリングできるパラメーター。 サポートされるプロパティには、`name`、`createEpoch`、`mergeFunction.value`、`updateEpoch`、`status` などがあります。 サポートされる操作は、表示されるプロパティによって異なります。 <ul><li>`name`: `EQUAL` （=）、`NOT_EQUAL` （!=）、`CONTAINS` （=contains （））、`NOT_CONTAINS` （=!contains （））</li><li>`createEpoch`: `GREATER_THAN_OR_EQUALS` （&lt;=）、`LESS_THAN_OR_EQUALS` （>=） </li><li>`mergeFunction.value`: `EQUAL` （=）、`NOT_EQUAL` （!=）、`CONTAINS` （=contains （））、`NOT_CONTAINS` （=!contains （））</li><li>`updateEpoch`: `GREATER_THAN_OR_EQUALS` （&lt;=）、`LESS_THAN_OR_EQUALS` （>=）</li><li>`status`: `EQUAL` （=）、`NOT_EQUAL` （!=）、`CONTAINS` （=contains （））、`NOT_CONTAINS` （=!contains （））</li></ul> | `property=updateEpoch>=1683669114845`<br/>`property=name!=testingrelease`<br/>`property=status=contains(new,processing,disabled)` |

**リクエスト**

次のリクエストでは、組織で更新された最後の 3 つの計算済み属性を取得します。

+++ 計算済み属性のリストを取得するリクエストの例です。

```shell
curl -X GET https://platform.adobe.io/data/core/ca/attributes?limit=3 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200 が、組織およびサンドボックスに属する最新の 3 つの更新済み計算属性のリストと共に返されます。

+++ 計算済み属性のリストを取得するためのサンプル応答。

```json
{
    "_links": {
        "last": {
            "href": "/attributes?offset=3&limit=1"
        },
        "next": {
            "href": "/attributes?offset=20&limit=20"
        },
        "prev": {
            "href": "/attributes?offset=0&limit=20"
        },
        "self": {
            "href": "/attributes?offset=0&limit=20"
        }
    },
    "computedAttributes": [
        {
            "id": "2e3bf98c-5840-4eb5-98c9-fcd7bde82188",
            "type": "ComputedAttribute",
            "name": "multipleFilterClauses19",
            "displayName": "Multiple Filter Clauses 19",
            "description": "Multiple Filter Clauses 19",
            "imsOrgId": "{ORG_ID}",
            "sandbox": {
                "sandboxId": "e4f64b40-d8d9-11e9-a7ce-f3356ed0508b",
                "sandboxName": "prod",
                "type": "production",
                "default": true
            },
            "path": "{TENANT_ID}/ComputedAttributes",
            "keepCurrent": false,
            "expression": {
                "type": "PQL",
                "format": "pql/text",
                "value": "xEvent[(commerce.checkouts.value > 0.0 or commerce.purchases.value > 1.0 or commerce.order.priceTotal >= 10.0)",
            },
            "mergeFunction": {
                "value": "SUM"
            },
            "status": "DRAFT",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "duration": {
                "count": 7,
                "unit": "DAYS"
            },
            "lastEvaluationTs": "",
            "createEpoch": 1671223530322,
            "updateEpoch": 1673043640946,
            "createdBy": "{USER_ID}"
        },
        {
            "id": "d9fbbd3d-049a-4561-b826-adc162511950",
            "type": "ComputedAttribute",
            "name": "multipleFilterClauses20",
            "displayName": "Multiple Filter Clauses 20",
            "description": "Multiple Filter Clauses 20",
            "imsOrgId": "{ORG_ID}",
            "sandbox": {
                "sandboxId": "e4f64b40-d8d9-11e9-a7ce-f3356ed0508b",
                "sandboxName": "prod",
                "type": "production",
                "default": true
            },
            "path": "{TENANT_ID}/ComputedAttributes",
            "keepCurrent": true,
            "expression": {
                "type": "PQL",
                "format": "pql/text",
                "value": "xEvent[eventType.equals(\"commerce.backofficeOrderPlaced\", false)].topN(timestamp, 1).map({\"timestamp\": timestamp, \"value\": producedBy}).head()"
            },
            "mergeFunction": {
                "value": "MOST_RECENT"
            },
            "status": "DRAFT",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "duration": {
                "count": 7,
                "unit": "DAYS"
            },
            "lastEvaluationTs": "",
            "createEpoch": 1671223586455,
            "updateEpoch": 1671223586455,
            "createdBy": "{USER_ID}"
        },
        {
            "id": "afedff07-9d15-4385-b181-49708229d73b",
            "type": "ComputedAttribute",
            "name": "multipleFilterClauses18",
            "displayName": "Multiple Filter Clauses 18",
            "description": "Multiple Filter Clauses 18",
            "imsOrgId": "{ORG_ID}",
            "sandbox": {
                "sandboxId": "e4f64b40-d8d9-11e9-a7ce-f3356ed0508b",
                "sandboxName": "prod",
                "type": "production",
                "default": true
            },
            "path": "{TENANT_ID}/ComputedAttributes",
            "expression": {
                "type": "PQL",
                "format": "pql/text",
                "value": "xEvent[(commerce.checkouts.value > 0.0 or commerce.purchases.value > 1.0 or commerce.order.priceTotal >= 10.0)",
            },
            "mergeFunction": {
                "value": "SUM"
            },
            "status": "PROCESSED",
            "schema": {
                "name": "_xdm.context.profile"
            },
            "duration": {
                "count": 7,
                "unit": "DAYS"
            },
            "lastEvaluationTs": "2023-08-27T00:14:55.028",
            "createEpoch": 1671220358902,
            "updateEpoch": 1671220358902,
            "createdBy": "{USER_ID}"
        }
    ],
    "_page": {
        "offset": 0,
        "limit": 20,
        "count": 3,
        "totalCount": 3
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `_links` | 結果の最後のページ、結果の次のページ、結果の前のページ、結果の現在のページにアクセスするために必要なページネーション情報を含むオブジェクト。 |
| `computedAttributes` | クエリパラメーターに基づいて計算された属性を含む配列。 計算属性の配列について詳しくは、[ 特定の計算属性の取得 ](#get) を参照してください。 |
| `_page` | 返される結果に関するメタデータを含むオブジェクト。 これには、現在のオフセット、返される計算済み属性の数、計算済み属性の合計の数、および返される計算済み属性の制限に関する情報が含まれます。 |

+++

## 計算属性の作成 {#create}

計算属性を作成するには、まず、作成する計算属性の詳細を含むリクエスト本文を使用して、`/attributes` エンドポイントに対してPOSTリクエストを行います。

**API 形式**

```http
POST /attributes
```

**リクエスト**

+++ 新しい計算属性を作成するリクエストのサンプル

```shell
curl -X POST https://platform.adobe.io/data/core/ca/attributes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}'\
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "name": "testing",
        "displayName": "Sample Display Name",
        "description": "Sample Description",
        "expression": {
            "type": "PQL", 
            "format": "pql/text", 
            "value": "xEvent[(commerce.checkouts.value > 0.0 or commerce.purchases.value > 1.0 or commerce.order.priceTotal >= 10.0)].sum(commerce.order.priceTotal)"
        },
        "keepCurrent": false,
        "duration": {
            "count": 4,
            "unit": "DAYS"
        },
        "status": "DRAFT"
      }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | 文字列としての計算属性フィールドの名前。 計算属性の名前は、英数字（スペースやアンダースコアを含まない）のみで構成できます。 この値は **必ず** すべての計算済み属性の中で一意です。 ベストプラクティスとして、この名前は `displayName` の camelCase バージョンにしてください。 |
| `description` | 計算済み属性の説明。これは、組織内の他のユーザーが使用する正しい計算属性を決定するのに役立つので、複数の計算属性が定義された後で特に便利です。 |
| `displayName` | 計算属性の表示名。 これは、Adobe Experience Platform UI 内で計算済み属性をリストする際に表示される名前です。 |
| `expression` | 作成しようとしている計算属性のクエリ式を表すオブジェクト。 |
| `expression.type` | 式のタイプ。 現在は、PQLのみがサポートされています。 |
| `expression.format` | 式の形式。 現在は、`pql/text` のみがサポートされています。 |
| `expression.value` | 式の値。 |
| `keepCurrent` | 高速更新を使用して計算属性の値を最新の状態に保つかどうかを決定するブール値。 現在、この値は `false` に設定する必要があります。 |
| `duration` | 計算属性のルックバック期間を表すオブジェクト。 ルックバック期間は、計算属性を計算するために遡ることができる期間を表します。 |
| `duration.count` | ルックバック期間の期間を表す数値。 使用可能な値は、`duration.unit` フィールドの値によって異なります。 <ul><li>`HOURS`: 1-24</li><li>`DAYS`: 1-7</li><li>`WEEKS`: 1-4</li><li>`MONTHS`: 1-6</li></ul> |
| `duration.unit` | ルックバック期間に使用される時間単位を表す文字列。 使用可能な値：`HOURS`、`DAYS`、`WEEKS`、`MONTHS` |
| `status` | 計算属性のステータス。 使用可能な値は `DRAFT` および `NEW` です。 |

+++

**応答**

応答に成功すると、HTTP ステータス 200 と、新しく作成された計算属性に関する情報が返されます。

+++ 新しい計算属性を作成する際のサンプル応答。

```json
{
    "id": "1e8d0d77-b2bb-4b17-bbe6-2dbc08c1a631",
    "type": "ComputedAttribute",
    "name": "testing",
    "displayName": "Sample Display Name",
    "description": "Sample Description",
    "imsOrgId": "{ORG_ID}",
    "sandbox": {
        "sandboxId": "02dd69f0-da73-11e9-9ea1-af59ce7c24e8",
        "sandboxName": "prod",
        "type": "production",
        "isDefault": true
    },
    "path": "{TENANT_ID}/ComputedAttributes",
    "keepCurrent": false,
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "xEvent[(commerce.checkouts.value > 0.0 or commerce.purchases.value > 1.0 or commerce.order.priceTotal >= 10.0)].sum(commerce.order.priceTotal)"
    },
    "mergeFunction": {
        "value": "SUM"
    },
    "status": "DRAFT",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "lastEvaluationTs": "",
    "createEpoch": 1680070188696,
    "updateEpoch": 1680070188696,
    "createdBy": "{USER_ID}"
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | 新しく作成した計算属性のシステム生成 ID。 |
| `status` | 計算属性のステータス。 `DRAFT` または `NEW` のいずれかを指定できます。 |
| `createEpoch` | 計算属性が作成された時間（秒）。 |
| `updateEpoch` | 計算属性が最後に更新された時間（秒）。 |
| `createdBy` | 計算属性を作成したユーザーの ID。 |

+++

## 特定の計算属性の取得 {#get}

特定の計算属性に関する詳細な情報を取得するには、`/attributes` エンドポイントにGETリクエストを実行し、取得する計算属性の ID をリクエストパスで指定します。

**API 形式**

```http
GET /attributes/{ATTRIBUTE_ID}
```

**リクエスト**

+++ 特定の計算属性を取得するリクエストの例です。

```shell
curl -X GET 'https://platform.adobe.io/data/core/ca/attributes/1e8d0d77-b2bb-4b17-bbe6-2dbc08c1a631' \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答が成功すると、指定された計算属性の詳細情報とともに HTTP ステータス 200 が返されます。

+++ 特定の計算属性を取得する際のサンプル応答。

```json
{
    "id": "1e8d0d77-b2bb-4b17-bbe6-2dbc08c1a631",
    "type": "ComputedAttribute",
    "name": "testing",
    "displayName": "Sample Display Name",
    "description": "Sample Description",
    "imsOrgId": "{ORG_ID}",
    "sandbox": {
        "sandboxId": "02dd69f0-da73-11e9-9ea1-af59ce7c24e8",
        "sandboxName": "prod",
        "type": "production",
        "isDefault": true
    },
    "path": "{TENANT_ID}/ComputedAttributes",
    "keepCurrent": false,
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "xEvent[(commerce.checkouts.value > 0.0 or commerce.purchases.value > 1.0 or commerce.order.priceTotal >= 10.0) and (timestamp occurs <= 7 days before now)].sum(commerce.order.priceTotal)"
    },
    "mergeFunction": {
        "value": "SUM"
    },
    "status": "DRAFT",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "lastEvaluationTs": "",
    "createEpoch": 1680070188696,
    "updateEpoch": 1680070188696,
    "createdBy": "{USER_ID}"
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | 他の API 操作中に計算済み属性を参照するために使用できる、システムで生成された一意の読み取り専用 ID が含まれます。 |
| `type` | 返されたオブジェクトが計算属性であることを示す文字列。 |
| `name` | 計算属性の名前。 |
| `displayName` | 計算属性の表示名。 これは、Adobe Experience Platform UI 内で計算済み属性をリストする際に表示される名前です。 |
| `description` | 計算済み属性の説明。これは、組織内の他のユーザーが使用する正しい計算属性を決定するのに役立つので、複数の計算属性が定義された後で特に便利です。 |
| `imsOrgId` | 計算属性が属する組織の ID。 |
| `sandbox` | サンドボックスオブジェクトには、計算済み属性が設定されたサンドボックスの詳細が含まれます。この情報は、リクエストで送信されるサンドボックスヘッダーから取得されます。詳しくは、[サンドボックスの概要](../../sandboxes/home.md)を参照してください。 |
| `path` | 計算属性の `path`。 |
| `keepCurrent` | 高速更新を使用して計算属性の値を最新の状態に保つかどうかを決定するブール値。 |
| `expression` | 計算属性の式を含むオブジェクト。 |
| `mergeFunction` | 計算属性の結合関数を含むオブジェクト。 この値は、計算属性の式内の対応する集計パラメーターに基づいています。 使用可能な値は、`SUM`、`MIN`、`MAX`、`MOST_RECENT` などです。 |
| `status` | 計算属性のステータス。 `DRAFT`、`NEW`、`INITIALIZING`、`PROCESSING`、`PROCESSED`、`FAILED`、`DISABLED` のいずれかの値を指定できます。 |
| `schema` | 式が評価されるスキーマに関する情報を含むオブジェクト。 現在は、`_xdm.context.profile` のみがサポートされています。 |
| `lastEvaluationTs` | 計算属性が最後に評価された日時を表すタイムスタンプ。 |
| `createEpoch` | 計算属性が作成された時間（秒）。 |
| `updateEpoch` | 計算属性が最後に更新された時間（秒）。 |
| `createdBy` | 計算属性を作成したユーザーの ID。 |

+++

## 特定の計算属性の削除 {#delete}

DELETE `/attributes` エンドポイントに対して削除リクエストを実行し、リクエストパスで削除する計算属性の ID を指定することで、特定の計算属性を削除できます。

>[!IMPORTANT]
>
>削除リクエストは、ステータスが **ドラフト** （`DRAFT`）の計算済み属性の削除にのみ使用できます。 このエンドポイントは **使用できません** 他の状態の計算属性を削除するために使用します。

**API 形式**

```http
DELETE /attributes/{ATTRIBUTE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{ATTRIBUTE_ID}` | 削除する計算属性の `id` 値。 |

**リクエスト**

+++ 計算属性を削除するリクエストのサンプル。

```shell
curl -X DELETE https://platform.adobe.io/data/core/ca/attributes/1e8d0d77-b2bb-4b17-bbe6-2dbc08c1a631 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 202 が、削除された計算属性の詳細と共に返されます。

+++ 計算属性を削除する際のサンプル応答。

```json
{
    "id": "03ae581b-5f7b-48da-a9eb-4ef0daf4bc3c",
    "type": "ComputedAttribute",
    "name": "testdemopd2",
    "displayName": "testdemopd2",
    "description": "testdemopd2",
    "imsOrgId": "{ORG_ID}",
    "sandbox": {
        "sandboxId": "02dd69f0-da73-11e9-9ea1-af59ce7c24e8",
        "sandboxName": "prod",
        "type": "production",
        "isDefault": true
    },
    "path": "{TENANT_ID}/ComputedAttributes",
    "keepCurrent": false,
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "xEvent[(commerce.shipping.shipDate occurs <= 1 days before now) and (timestamp occurs <= 1 days before now)].min(commerce.shipping.shipDate)"
    },
    "mergeFunction": {
        "value": "MIN"
    },
    "status": "DRAFT",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "lastEvaluationTs": "",
    "createEpoch": 1681365690928,
    "updateEpoch": 1681365690928,
    "createdBy": "{USER_ID}"
}
```

+++

## 特定の計算属性の更新

`/attributes` エンドポイントに計算リクエストを実行し、リクエストパスで更新するPATCH属性の ID を指定することで、特定の計算属性を更新できます。

>[!IMPORTANT]
>
>計算属性を更新する場合、次のフィールドのみを更新できます。
>
>- 現在のステータスが `NEW` の場合、ステータスは `DISABLED` にのみ変更できます。
>- 現在のステータスが `DRAFT` の場合、フィールド `name`、`description`、`keepCurrent`、`expression`、`duration` の値を変更できます。 また、ステータスを `DRAFT` から `NEW` に変更することもできます。 `mergeFunction` や `path` など、システム生成フィールドに変更を加えるとエラーが返されます。
>- 現在のステータスが `PROCESSING` または `PROCESSED` の場合、ステータスは `DISABLED` にのみ変更できます。

**API 形式**

```http
PATCH /attributes/{ATTRIBUTE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{ATTRIBUTE_ID}` | 更新する計算属性の `id` 値。 |

**リクエスト**

次のリクエストは、計算属性のステータスを `DRAFT` から `NEW` に更新します。

+++ 計算属性を更新するリクエストのサンプル。

```shell
curl -X PATCH https://platform.adobe.io/data/core/ca/attributes/1e8d0d77-b2bb-4b17-bbe6-2dbc08c1a631 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
 {
    "description": "Sample Description",
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "xEvent[(commerce.checkouts.value > 0.0 or commerce.purchases.value > 1.0 or commerce.order.priceTotal >= 10.0) and (timestamp occurs <= 7 days before now)].sum(commerce.order.priceTotal)"
    },
    "status": "NEW"
 }'
```

+++

**応答**

応答に成功すると、HTTP ステータス 200 と、新しく更新された計算属性に関する情報が返されます。

+++ 計算属性を更新する際のサンプル応答。

```json
{
    "id": "1e8d0d77-b2bb-4b17-bbe6-2dbc08c1a631",
    "type": "ComputedAttribute",
    "name": "testing123",
    "displayName": "Sample Display Name",
    "description": "Sample Description",
    "imsOrgId": "{ORG_ID}",
    "sandbox": {
        "sandboxId": "02dd69f0-da73-11e9-9ea1-af59ce7c24e8",
        "sandboxName": "prod",
        "type": "production",
        "isDefault": true
    },
    "path": "{TENANT_ID}/ComputedAttributes",
    "keepCurrent": false,
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "xEvent[(commerce.checkouts.value > 0.0 or commerce.purchases.value > 1.0 or commerce.order.priceTotal >= 10.0) and (timestamp occurs <= 7 days before now)].sum(commerce.order.priceTotal)"
    },
    "mergeFunction": {
        "value": "SUM"
    },
    "status": "NEW",
    "schema": {
        "name": "_xdm.context.profile"
    },
    "lastEvaluationTs": "",
    "createEpoch": 1680071726825,
    "updateEpoch": 1680074429192,
    "createdBy": "{USER_ID}"
}
```

+++

## 次の手順

計算属性の基本を理解したので、次は組織の定義を開始します。 Experience PlatformUI で計算済み属性を使用する方法については、[ 計算属性 UI ガイド ](./ui.md) を参照してください。
