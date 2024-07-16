---
keywords: Experience Platform；ホーム；人気のトピック；api；属性ベースのアクセス制御；属性ベースのアクセス制御
solution: Experience Platform
title: 製品 API エンドポイント
description: 属性ベースのアクセス制御 API の/products エンドポイントを使用すると、Adobe Experience Platformでプロダクトをプログラムで管理できます。
role: Developer
exl-id: 44ee9a9d-7a13-4d59-a1a9-97764dbd3763
source-git-commit: c16ce1020670065ecc5415bc3e9ca428adbbd50c
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 25%

---

# 製品エンドポイント

>[!NOTE]
>
>ユーザートークンが渡される場合、トークンのユーザーには、リクエストされた組織の「組織管理者」の役割が必要です。

属性ベースのアクセス制御 API の `/products` エンドポイントを使用すると、プログラムによって組織内の製品に関連付けられた権限カテゴリおよび権限セットと同様に、製品を管理できます。

## はじめに

このガイドで使用する API エンドポイントは、属性ベースのアクセス制御 API の一部です。 先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## 資格のある製品のリストの取得 {#list}

`/products` エンドポイントにGETリクエストを行うことで、資格のある製品のリストを取得できます。

**API 形式**

```http
GET /products/
```

**リクエスト**

次のリクエストは、組織に属する資格のある製品のリストを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/access-control/administration/products \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答**

応答が成功すると、組織に属する資格のある製品のリストが返されます。

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
| `id` | クエリされた製品の対応する ID。 |
| `name` | クエリされた製品の名前。 |
| `serviceCode` | クエリされた製品の対応するサービスコード。 |

## 製品 ID 別に権限カテゴリを検索

商品 ID を指定しながら `/products/{PRODUCT_ID}/categories` エンドポイントに対してGETリクエストを実行することで、特定の商品の権限カテゴリを検索できます。

**API 形式**

```http
GET /products/{PRODUCT_ID}/categories
```

| パラメーター | 説明 |
| --- | --- |
| {PRODUCT_ID} | 検索する権限カテゴリに関連付けられている製品の ID。 |

**リクエスト**

次のリクエストは、`{PRODUCT_ID}` に関連付けられた権限カテゴリを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/access-control/administration/products/{PRODUCT_ID}/categories \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答**

応答が成功すると、クエリした製品 ID に関連付けられた権限カテゴリが返されます。

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
| `category` | クエリされた製品 ID 内で使用可能な権限カテゴリ。 |
| `name` | 権限カテゴリの名前。 |

## 製品 ID 別の権限セットの検索

商品 ID を指定しながら `/products/{PRODUCT_ID}/permission-sets` エンドポイントに対してGETリクエストを実行することで、特定の商品の権限セットを検索できます。

**API 形式**

```http
GET /products/{PRODUCT_ID}/permission-sets
```

| パラメーター | 説明 |
| --- | --- |
| {PRODUCT_ID} | 検索する権限セットに関連付けられている製品の ID。 |

**リクエスト**

次のリクエストは、`{PRODUCT_ID}` に関連付けられた権限セットを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/access-control/administration/products/{PRODUCT_ID}/permission-sets \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答**

応答が成功すると、クエリした製品 ID に関連付けられた権限セットが返されます。

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
| `permission-sets` | 権限セットは、管理者が役割に適用できる権限のグループを表します。 管理者は、個々の権限を割り当てる代わりに、役割に権限セットを割り当てることができます。これにより、権限のグループを含む事前定義済みの役割からカスタムの役割を作成できます。 |
| `id` | クエリされた権限セットの対応する ID。 |
| `name` | クエリされたアクセス権セットの対応する名前。 |
| `category` | 使用可能な権限カテゴリ。 |
| `permissions` | 権限には、サンドボックスの作成、スキーマの定義、データセットの管理など、 Platform の機能の表示や使用の機能が含まれます。 |
| `permissions.resource` | サブジェクトがアクセスできるまたはアクセスできないアセットまたはオブジェクト。 リソースには、ファイル、アプリケーション、サーバー、API があります。 |
| `permissions.actions` | クエリされたリソースに対してサブジェクトが実行できるアクション。 使用可能な値：`view`、`read`、`create`、`edit`、`delete` |
