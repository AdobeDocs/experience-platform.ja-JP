---
title: 計算済み属性 API エンドポイント
description: リアルタイム顧客プロファイル API を使用して、計算済み属性を作成、表示、更新および削除する方法について説明します。
badge: 「ベータ版」
source-git-commit: 3b4e1e793a610c9391b3718584a19bd11959e3be
workflow-type: tm+mt
source-wordcount: '1565'
ht-degree: 13%

---

# 計算済み属性 API エンドポイント

>[!IMPORTANT]
>
>計算済み属性機能は現在ベータ版です。 ドキュメントと機能は変更される場合があります。
>
>また、API へのアクセスは制限されます。 計算済み属性 API へのアクセス方法については、Adobeサポートにお問い合わせください。

計算済み属性は、イベントレベルのデータをプロファイルレベルの属性に集計するために使用される関数です。これらの関数は自動的に計算され、セグメント化、アクティブ化およびパーソナライズ機能で使用できます。このガイドには、 `/attributes` endpoint.

計算済み属性の詳細については、まず [計算済み属性の概要](overview.md).

## Destination SDK の

このガイドで使用される API エンドポイントは、 [リアルタイム顧客プロファイル API](https://www.adobe.com/go/profile-apis-en).

続行する前に、 [プロファイル API 入門ガイド](../api/getting-started.md) 推奨ドキュメントへのリンク、このドキュメントに表示される API 呼び出し例の読み方のガイド、および任意のExperience PlatformAPI を正しく呼び出すために必要な必須ヘッダーに関する重要な情報。

さらに、次のサービスのドキュメントを確認してください。

- [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md)：[!DNL Experience Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。
   - [スキーマレジストリ入門ガイド](../../xdm/api/getting-started.md#know-your-tenant_id):次に関する情報： `{TENANT_ID}`は、このガイド全体での応答として表示されます。

## 計算済み属性のリストの取得 {#list}

組織のすべての計算済み属性のリストを取得するには、 `/attributes` endpoint.

**API 形式**

`/attributes` エンドポイントは、結果を絞り込むのに役立つ、複数のクエリパラメーターをサポートしています。これらのパラメーターはオプションですが、リソースをリストする際の高価なオーバーヘッドを削減するために、パラメーターの使用を強くお勧めします。 パラメーターを指定せずにこのエンドポイントを呼び出すと、組織で使用可能なすべての計算済み属性が取得されます。 複数のパラメーターを使用する場合は、アンパサンド（`&`）で区切ります。

```http
GET /attributes
GET /attributes?{QUERY_PARAMETERS}
```

計算済み属性のリストを取得する際には、次のクエリパラメーターを使用できます。

| クエリーパラメーター | 説明 | 例 |
| --------------- | ----------- | ------- |
| `limit` | 応答の一部として返される項目の最大数を指定するパラメーター。 このパラメーターの最小値は 1 で、最大値は 40 です。 このパラメーターを含めない場合、デフォルトでは 20 個の項目が返されます。 | `limit=20` |
| `offset` | 項目を返す前にスキップする項目の数を指定するパラメーター。 | `offset=5` |
| `sortBy` | 返された項目の並べ替え順序を指定するパラメーター。 次のオプションを使用できます。 `name`, `status`, `updateEpoch`、および `createEpoch`. 昇順と降順のどちらで並べ替えるかを、 `-` をクリックします。 デフォルトでは、項目は次の順に並べ替えられます。 `updateEpoch` 降順で並べ替えます。 | `sortBy=name` |
| `status` | 計算済み属性のステータスでフィルタリングできるパラメーター。 次のオプションを使用できます。 `draft`, `new`, `processing`, `processed`, `failed`, `disabled`、および `initializing`. このオプションでは、大文字と小文字が区別されません。 | `status=draft` |

**リクエスト**

次のリクエストは、組織内で更新された最後の 3 つの計算済み属性を取得します。

+++ 計算済み属性のリストを取得するリクエストのサンプルです。

```shell
curl -X GET https://platform.adobe.io/data/core/ca/attributes?limit=3 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

正常な応答は、HTTP ステータス 200 と、組織およびサンドボックスに属する、更新された最新 3 つの計算済み属性のリストを返します。

+++ 計算済み属性のリストを取得するためのサンプルレスポンスです。

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
            "expression": {
                "type": "PQL",
                "format": "pql/text",
                "value": "xEvent[(commerce.checkouts.value > 0.0 or commerce.purchases.value > 1.0 or commerce.order.priceTotal >= 10.0)",
                "meta": " "
            },
            "mergeFunction": {
                "value": "-"
            },
            "status": "DRAFT",
            "schema": {
                "name": "_xdm.context.profile"
            },
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
            "expression": {
                "type": "PQL",
                "format": "pql/text",
                "value": "xEvent[(commerce.checkouts.value > 0.0 or commerce.purchases.value > 1.0 or commerce.order.priceTotal >= 10.0)",
                "meta": " "
            },
            "mergeFunction": {
                "value": "-"
            },
            "status": "DRAFT",
            "schema": {
                "name": "_xdm.context.profile"
            },
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
                "meta": " "
            },
            "mergeFunction": {
                "value": "-"
            },
            "status": "DRAFT",
            "schema": {
                "name": "_xdm.context.profile"
            },
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
| `_links` | 結果の最後のページ、結果の次のページ、結果の前のページ、または結果の現在のページへのアクセスに必要なページネーション情報を含むオブジェクト。 |
| `computedAttributes` | クエリパラメーターに基づく計算済み属性を含む配列。 計算済み属性配列の詳細については、 [特定の計算済み属性セクションの取得](#get). |
| `_page` | 返される結果に関するメタデータを含むオブジェクト。 これには、現在のオフセット、返される計算済み属性の数、計算済み属性の総数、返される計算済み属性の制限に関する情報が含まれます。 |

+++

## 計算済み属性の作成 {#create}

計算済み属性を作成するには、まず `/attributes` エンドポイント：作成する計算済み属性の詳細が含まれるリクエスト本文を持ちます。

**API 形式**

```http
POST /attributes
```

**リクエスト**

+++ 新しい計算済み属性を作成するリクエストのサンプルです。

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
            "value": "xEvent[(commerce.checkouts.value > 0.0 or commerce.purchases.value > 1.0 or commerce.order.priceTotal >= 10.0)"
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
| `name` | 計算済み属性フィールドの名前（文字列）。計算済み属性の名前は、スペースやアンダースコアを含まない英数字でのみ構成できます。 この値 **必須** は、すべての計算済み属性の中で一意です。 ベストプラクティスとして、この名前はキャメルケースバージョンの `displayName`. |
| `description` | 計算済み属性の説明。複数の計算済み属性を定義した場合は特に便利です。組織内の他のユーザーが、使用する正しい計算済み属性を判断するのに役立ちます。 |
| `displayName` | 計算済み属性の表示名。 これは、Adobe Experience Platform UI 内に計算済み属性をリストする際に表示される名前です。 |
| `expression` | 作成しようとしている計算済み属性のクエリ式を表すオブジェクト。 |
| `expression.type` | 式のタイプ。 現在、PQL のみがサポートされています。 |
| `expression.format` | 式の形式。 現在は、`pql/text` のみがサポートされています。 |
| `expression.value` | 式の値。 |
| `keepCurrent` | 計算済み属性の値を最新の状態に保つかどうかを決定するブール値です。 現在、この値は `false`. |
| `duration` | 計算済み属性のルックバック期間を表すオブジェクト。 ルックバック期間は、計算済み属性を計算するために遡って参照できる範囲を表します。 |
| `duration.count` | ルックバック期間の期間を表す数値。 可能な値は、 `duration.unit` フィールドに入力します。 <ul><li>`HOURS`: 1-24</li><li>`DAYS`: 1-7</li><li>`WEEKS`: 1-4</li><li>`MONTHS`: 1-6</li></ul> |
| `duration.unit` | ルックバック期間に使用される時間の単位を表す string。 次の値を指定できます。 `HOURS`, `DAYS`, `WEEKS`、および `MONTHS`. |
| `status` | 計算済み属性のステータス。 以下の値を指定できます。 `DRAFT` および `NEW`. |

+++

**応答**

正常な応答は、HTTP ステータス 200 と、新しく作成された計算済み属性に関する情報を返します。

+++ 新しい計算済み属性を作成する際のレスポンスのサンプルです。

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
    "createEpoch": 1680070188696,
    "updateEpoch": 1680070188696,
    "createdBy": "{USER_ID}"
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | 新しく作成した計算済み属性のシステム生成 ID。 |
| `status` | 計算済み属性のステータス。 これは、 `DRAFT` または `NEW`. |
| `createEpoch` | 計算済み属性が作成された時刻（秒）。 |
| `updateEpoch` | 計算済み属性が最後に更新された時刻（秒）。 |
| `createdBy` | 計算済み属性を作成したユーザーの ID。 |

+++

## 特定の計算済み属性の取得 {#get}

特定の計算済み属性に関する詳細な情報を取得するには、に対してGETリクエストを実行します。 `/attributes` エンドポイントを作成し、リクエストパスで取得する計算済み属性の ID を指定します。

**API 形式**

```http
GET /attributes/{ATTRIBUTE_ID}
```

**リクエスト**

+++ 特定の計算済み属性を取得するサンプルリクエストです。

```shell
curl -X GET 'https://platform.adobe.io/data/core/ca/attributes/1e8d0d77-b2bb-4b17-bbe6-2dbc08c1a631' \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

正常な応答は、HTTP ステータス 200 と、指定された計算済み属性に関する詳細情報を返します。

+++ 特定の計算済み属性を取得する際のレスポンスのサンプルです。

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
    "createEpoch": 1680070188696,
    "updateEpoch": 1680070188696,
    "createdBy": "{USER_ID}"
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `id` | 他の API 操作中に計算済み属性を参照するために使用できる、システムで生成された一意の読み取り専用 ID が含まれます。 |
| `type` | 返されるオブジェクトが計算済み属性であることを示す文字列。 |
| `imsOrgId` | 計算済み属性が属する組織の ID。 |
| `sandbox` | サンドボックスオブジェクトには、計算済み属性が設定されたサンドボックスの詳細が含まれます。この情報は、リクエストで送信されるサンドボックスヘッダーから取得されます。詳しくは、[サンドボックスの概要](../../sandboxes/home.md)を参照してください。 |
| `path` | この `path` を計算済み属性に追加します。 |
| `expression` | 計算済み属性の式を含むオブジェクト。 |
| `mergeFunction` | 計算済み属性の結合関数を含むオブジェクト。 この値は、計算済み属性の式内の対応する集計パラメーターに基づきます。 |
| `status` | 計算済み属性のステータス。 次のいずれかの値を指定できます。 `DRAFT`, `NEW`, `INITIALIZING`, `PROCESSING`, `PROCESSED`, `FAILED`または `DISABLED`. |
| `schema` | 式が評価されるスキーマに関する情報を格納するオブジェクト。 現在は、`_xdm.context.profile` のみがサポートされています。 |
| `createEpoch` | 計算済み属性が作成された時刻（秒）。 |
| `updateEpoch` | 計算済み属性が最後に更新された時刻（秒）。 |
| `createdBy` | 計算済み属性を作成したユーザーの ID。 |

+++

## 特定の計算済み属性の削除 {#delete}

特定の計算済み属性を削除するには、 `/attributes` エンドポイントを作成し、リクエストパスで削除する計算済み属性の ID を指定します。

>[!IMPORTANT]
>
>削除リクエストは、ステータスが「 **ドラフト** (`DRAFT`) をクリックします。 このエンドポイント **できません** を使用して、他の状態の計算済み属性を削除できます。

**API 形式**

```http
DELETE /attributes/{ATTRIBUTE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{ATTRIBUTE_ID}` | この `id` 削除する計算済み属性の値。 |

**リクエスト**

+++ 計算済み属性を削除するリクエストの例です。

```shell
curl -X DELETE https://platform.adobe.io/data/core/ca/attributes/1e8d0d77-b2bb-4b17-bbe6-2dbc08c1a631 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

正常な応答は、削除された計算済み属性の詳細と共に HTTP ステータス 202 を返します。

+++ 計算済み属性を削除する際のレスポンスのサンプルです。

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
    "createEpoch": 1681365690928,
    "updateEpoch": 1681365690928,
    "createdBy": "{USER_ID}"
}
```

+++

## 特定の計算済み属性の更新

特定の計算済み属性を更新するには、 `/attributes` エンドポイントを作成し、リクエストパスで更新する計算済み属性の ID を指定します。

>[!IMPORTANT]
>
>計算済み属性を更新する場合、更新できるのは次のフィールドのみです。
>
>- 現在のステータスが `NEW`の場合、ステータスは `DISABLED`.
>- 現在のステータスが `DRAFT`に値を入力すると、次のフィールドの値を変更できます。 `name`, `description`, `keepCurrent`, `expression`、および `duration`. ステータスは、 `DRAFT` から `NEW`. システム生成フィールドに対する変更（例： ） `mergeFunction` または `path` はエラーを返します。
>- 現在のステータスが `PROCESSING` または `PROCESSED`の場合、ステータスは `DISABLED`.

**API 形式**

```http
PATCH /attributes/{ATTRIBUTE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{ATTRIBUTE_ID}` | この `id` 更新する計算済み属性の値。 |

**リクエスト**

次のリクエストは、計算済み属性のステータスを更新します： `DRAFT` から `NEW`.

+++ 計算済み属性を更新するリクエストのサンプルです。

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

正常な応答は、HTTP ステータス 200 と、新しく更新された計算済み属性に関する情報を返します。

+++ 計算済み属性を更新する際のレスポンスのサンプルです。

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
    "createEpoch": 1680071726825,
    "updateEpoch": 1680074429192,
    "createdBy": "{USER_ID}"
}
```

+++

## 次の手順

これで、計算済み属性の基本について学び、組織に合わせて定義を開始する準備が整いました。Experience PlatformUI で計算済み属性を使用する方法については、 [計算済み属性 UI ガイド](./ui.md).
