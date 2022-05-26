---
keywords: Experience Platform；ホーム；人気の高いトピック；api；属性ベースのアクセス制御；属性ベースのアクセス制御
solution: Experience Platform
title: 製品 API エンドポイント
description: 属性ベースのアクセス制御 API の/products エンドポイントを使用すると、Adobe Experience Platformで製品をプログラムで管理できます。
hide: true
hidefromtoc: true
exl-id: 44ee9a9d-7a13-4d59-a1a9-97764dbd3763
source-git-commit: 19f1e8df8cd8b55ed6b03f80e42810aefd211474
workflow-type: tm+mt
source-wordcount: '523'
ht-degree: 16%

---

# 製品エンドポイント

>[!IMPORTANT]
>
>現在、米国ベースの医療関連のお客様向けに、属性ベースのアクセス制御が限定的なリリースで提供されています。 この機能は、完全にリリースされると、すべてのReal-time Customer Data Platformのお客様が利用できるようになります。

この `/products` 属性ベースのアクセス制御 API のエンドポイントを使用すると、製品、および組織内の製品に関連する権限カテゴリや権限セットをプログラムで管理できます。

## はじめに

このガイドで使用される API エンドポイントは、属性ベースのアクセス制御 API の一部です。 先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## 使用権限を持つ製品のリストの取得 {#list}

権利が付与されている製品のリストを取得するには、次に対してGETリクエストを実行します： `/products` endpoint.

**API 形式**

```http
GET /products/
```

**リクエスト**

次のリクエストでは、組織に属する権利が付与されている製品のリストを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/access-control/administration/products \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答**

正常な応答は、組織に属する権利が付与されている製品のリストを返します。

```json
{
  "products": [
    {
      "id": "{ID}",
      "name": "Adobe Experience Platform",
      "serviceCode": "{SERVICE_CODE}"
    }
  ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | 問い合わせられた製品の対応する ID。 |
| `name` | 問い合わせられた製品の名前。 |
| `serviceCode` | 問い合わせ対象の製品の対応するサービスコード。 |

## 製品 ID 別に権限カテゴリを検索します

特定の製品の権限カテゴリを検索するには、 `/products/{PRODUCT_ID}/categories` エンドポイントを使用して製品 ID を指定できます。

**API 形式**

```http
GET /products/{PRODUCT_ID}/categories
```

| パラメーター | 説明 |
| --- | --- |
| {PRODUCT_ID} | 検索する権限カテゴリに関連付けられている製品の ID。 |

**リクエスト**

次のリクエストは、 `{PRODUCT_ID}`.

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/access-control/administration/products/{PRODUCT_ID}/categories \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答**

正常な応答は、問い合わせた製品 ID に関連付けられた権限カテゴリを返します。

```json
{
  "categories": [
    {
        "name": "Profile Management"
    },
    {
        "name": "Data Ingestion"
    },
    {
        "name": "Sandbox Administration"
    },
    {
        "name": "Query Service"
    },
    {
        "name": "Data Management"
    },
    {
        "name": "Identity Management"
    },
    {
        "name": "Data Modeling"
    },
    {
        "name": "Data Science Workspace"
    },
    {
        "name": "Dashboards"
    },
    {
        "name": "Alerts"
    },
    {
        "name": "Data Governance"
    }
  ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `category` | 照会された製品 ID 内で使用できる権限カテゴリです。 |
| `name` | 権限カテゴリの名前。 |

## 製品 ID による権限セットの検索

特定の製品の権限セットを検索するには、 `/products/{PRODUCT_ID}/permission-sets` エンドポイントを使用して製品 ID を指定できます。

**API 形式**

```http
GET /products/{PRODUCT_ID}/permission-sets
```

| パラメーター | 説明 |
| --- | --- |
| {PRODUCT_ID} | 検索する権限セットに関連付けられている製品の ID。 |

**リクエスト**

次のリクエストは、 `{PRODUCT_ID}`.

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/access-control/administration/products/{PRODUCT_ID}/permission-sets \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答**

正常な応答は、問い合わせた製品 ID に関連付けられた権限セットを返します。

```json
{
  "permission-sets": [
      {
          "id": "manage-schemas",
          "name": "Manage Schemas",
          "category": "Data Modeling",
          "permissions": [
              {
                  "resource": "schemas",
                  "actions": [
                      "read",
                      "write",
                      "delete"
                  ]
              },
              {
                  "resource": "schema-fields",
                  "actions": [
                      "read",
                      "write",
                      "delete"
                  ]
              },
              {
                  "resource": "sandboxes",
                  "actions": [
                      "view"
                  ]
              }
          ]
      },
      {
          "id": "view-schemas",
          "name": "View Schemas",
          "category": "Data Modeling",
          "permissions": [
              {
                  "resource": "schemas",
                  "actions": [
                      "read"
                  ]
              },
              {
                  "resource": "schema-fields",
                  "actions": [
                      "read"
                  ]
              },
              {
                  "resource": "sandboxes",
                  "actions": [
                      "view"
                  ]
              }
          ]
      },
  ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `permission-sets` | 権限セットは、管理者が役割に適用できる権限のグループを表します。 管理者は、個々の権限を割り当てる代わりに、権限セットを役割に割り当てることができます。 これにより、権限のグループを含む事前定義済みの役割からカスタム役割を作成できます。 |
| `id` | クエリされた権限セットの対応する ID。 |
| `name` | 照会された権限セットの対応する名前。 |
| `category` | 使用可能な権限カテゴリ。 |
| `permissions` | 権限には、サンドボックスの作成、スキーマの定義、データセットの管理など、 Platform の機能の表示や使用の機能が含まれます。 |
| `permissions.resource` | 主体がアクセスできる、またはアクセスできないアセットまたはオブジェクト。 リソースには、ファイル、アプリケーション、サーバー、API などがあります。 |
| `permissions.actions` | 問い合わせられたリソースに対して主体が実行できるアクション。 次の値を指定できます。 `view`, `read`, `create`, `edit`、および `delete` |
