---
title: 拡張機能パッケージ使用承認エンドポイント
description: Reactor API で/extension_package_usage 許可エンドポイントを呼び出す方法を説明します。
source-git-commit: fdf01451527e2fab8eb6e6f9d7b4901a85381450
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 16%

---


# 拡張機能パッケージ使用権限エンドポイント

拡張機能パッケージは、拡張機能開発者が作成した[拡張機能](./extensions.md)を表します。 ユーザーにタグ付けできる追加機能は、拡張機能パッケージによって定義されます。 これらの機能には、メインモジュールや共有モジュールを含めることができますが、最も頻繁には [ ルールコンポーネント ](./rule-components.md) （イベント、条件、アクション）および [ データ要素 ](./data-elements.md) として提供されます。

拡張機能パッケージは、開発者の [ 会社 ](./companies.md) が所有しています。 拡張機能パッケージの所有者は、他の企業に対してパッケージの非公開バージョンの使用を許可できます。 各認証済みビジネスには、1 つの拡張機能パッケージの使用権限が付与されます。この権限は、パッケージの今後および現在のプライベートバージョンすべてに有効です。

## はじめに

このガイドで使用するエンドポイントは、[Reactor API](https://www.adobe.io/experience-platform-apis/references/reactor/) の一部です。続行する前に、 [はじめる前に](../getting-started.md) で、API への認証方法に関する重要な情報を確認してください。

## 拡張機能パッケージの拡張機能パッケージ使用権限の取得 {#list}

拡張機能パッケージの使用権限のリストを取得するには、次のエンドポイントに対してGETリクエストを行います。

**API 形式**

```http
GET /extension_packages/{EXTENSION_PACKAGE_ID}/extension_package_usage_authorizations
```

| パラメーター | 説明 |
| --- | --- |
| `{PROPERTY_ID}` | 拡張機能パッケージの使用権限のリストを取得するプロパティの `ID`。 |

{style="table-layout:auto"}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/extension_packages/{EXTENSION_PACKAGE_ID}/extension_package_usage_authorizations \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、拡張機能パッケージのリストが返されます。

```json
{
  "data": [
    {
      "id": "EA722482c30fe44b54aa6a7317890b3bdb",
      "type": "extension_package_usage_authorizations",
      "attributes": {
        "created_at": "2024-06-05T23:17:35.776Z",
        "updated_at": "2024-06-05T23:17:35.776Z",
        "name": "Acme",
        "platform": "web",
        "owner_org_id": "{ORG_ID}",
        "owner_org_name": "Reactor QE",
        "authorized_org_id": "{ORG_ID}",
        "authorized_org_name": "Acme Inc'",
        "state": "pending_approval",
        "created_by_email": "example@adobe.com",
        "created_by_display_name": "john snow",
        "updated_by_email": "Restricted",
        "updated_by_display_name": "Restricted"
      },
      "relationships": {
        "extension_package": {
          "links": {
            "related": "https://reactor.adobe.io/extension_package_usage_authorizations/EA722482c30fe44b54aa6a7317890b3bdb/extension_package"
          },
          "data": {
            "id": "EPecefc8291ae346c3b3887d5b2da533b8",
            "type": "extension_packages"
          }
        }
      },
      "links": {
        "self": "https://reactor.adobe.io/extension_package_usage_authorizations/EA722482c30fe44b54aa6a7317890b3bdb"
      }
    }
  ],
  "meta": {
    "pagination": {
      "current_page": 1,
      "next_page": null,
      "prev_page": null,
      "total_pages": 1,
      "total_count": 1
    }
  }
}
```

## 拡張機能パッケージの使用承認を作成する {#create}

認証する組織の [ 拡張機能パッケージ ](./extension-packages.md) および `{ORG_ID}` ごとに、拡張機能パッケージの使用状況認証を作成します。 新しい拡張機能パッケージ使用権限を作成するには、以下のエンドポイントに対してPOSTリクエストを実行します。

**API 形式**

```http
POST /extension_packages/{EXTENSION_PACKAGE_ID}/extension_package_usage_authorizations
```

| パラメーター | 説明 |
| --- | --- |
| `EXTENSION_PACKAGE_ID` | 認証を作成する拡張機能パッケージの `ID`。」 |

{style="table-layout:auto"}

**リクエスト**

```shell
curl -X POST \
  https://reactor.adobe.io/extension_packages/{EXTENSION_PACKAGE_ID}/extension_package_usage_authorizations \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -d '{
        "data": {
          "attributes": {
            "authorized_org_id": "{ORG_ID}"
          },
          "type": "extension_package_usage_authorizations"
        }
      } 
```

| プロパティ | 説明 |
| --- | --- |
| `attributes.authorized_org_id` | 認証する組織の `ID`。 |

**応答**

応答が成功すると、新しく作成した拡張機能パッケージの使用承認の詳細が返されます。

```json
{
  "data": {
    "id": "EA35d0e731f73645e6972df9fcac101434",
    "type": "extension_package_usage_authorizations",
    "attributes": {
      "created_at": "2024-06-05T23:17:30.308Z",
      "updated_at": "2024-06-05T23:17:30.308Z",
      "name": "Acme",
      "platform": "web",
      "owner_org_id": "{ORG_ID}",
      "owner_org_name": "Reactor QE",
      "authorized_org_id": "{ORG_ID}",
      "authorized_org_name": "Acme Inc'",
      "state": "pending_approval",
      "created_by_email": "example@adobe.com",
      "created_by_display_name": "john snow",
      "updated_by_email": "Restricted",
      "updated_by_display_name": "Restricted"
    },
    "relationships": {
      "extension_package": {
        "links": {
          "related": "https://reactor.adobe.io/extension_package_usage_authorizations/EA35d0e731f73645e6972df9fcac101434/extension_package"
        },
        "data": {
          "id": "EP43649cc8856d4f09a7c2a21a4b1e449d",
          "type": "extension_packages"
        }
      }
    },
    "links": {
      "self": "https://reactor.adobe.io/extension_package_usage_authorizations/EA35d0e731f73645e6972df9fcac101434"
    }
  }
}
```

>[!NOTE]
>
>上記の応答例では、認証は現在 `pending_approval` の段階にあります。 拡張機能パッケージを使用する前に、組織は認証を承認する必要があります。 組織のユーザーは、認証が承認保留中の間はプライベート拡張機能パッケージを参照できますが、インストールできず、拡張機能カタログに見つかりません。

## 拡張機能パッケージの使用権限のリストの取得 {#list_authorizations}

GETリクエストをおこなうと、拡張機能パッケージの使用権限のリストを取得できます。

**API 形式**

```http
GET /extension_package_usage_authorizations
```

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/extension_package_usage_authorizations \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、拡張機能パッケージのリストが返されます。

```json
{
  "data": [
    {
      "id": "EA35d0e731f73645e6972df9fcac101434",
      "type": "extension_package_usage_authorizations",
      "attributes": {
        "created_at": "2024-06-05T23:17:30.308Z",
        "updated_at": "2024-06-05T23:17:30.308Z",
        "name": "Acme",
        "platform": "web",
        "owner_org_id": "{ORG_ID}",
        "owner_org_name": "Reactor QE",
        "authorized_org_id": "{ORG_ID}",
        "authorized_org_name": "Acme Inc'",
        "state": "pending_approval",
        "created_by_email": "Restricted",
        "created_by_display_name": "Restricted",
        "updated_by_email": "example@adobe.com",
        "updated_by_display_name": "john snow"
      },
      "relationships": {
        "extension_package": {
          "links": {
            "related": "https://reactor.adobe.io/extension_package_usage_authorizations/EA35d0e731f73645e6972df9fcac101434/extension_package"
          },
          "data": null
        }
      },
      "links": {
        "self": "https://reactor.adobe.io/extension_package_usage_authorizations/EA35d0e731f73645e6972df9fcac101434"
      }
    }
  ],
  "links": {
    "self": "https://reactor.adobe.io/extension_package_usage_authorizations?page%5Bnumber%5D=1&page%5Bsize%5D=25",
    "next": "https://reactor.adobe.io/extension_package_usage_authorizations?page%5Bnumber%5D=2&page%5Bsize%5D=25",
    "last": "https://reactor.adobe.io/extension_package_usage_authorizations?page%5Bnumber%5D=3&page%5Bsize%5D=25"
  },
  "meta": {
    "pagination": {
      "current_page": 1,
      "next_page": 2,
      "prev_page": null,
      "total_pages": 3,
      "total_count": 57
    }
  }
}
```

## 拡張機能パッケージの使用権限を削除 {#delete}

拡張機能パッケージの使用権限を削除するには、DELETEリクエストのパスに `ID` を含めます。 これにより、承認済み組織は、カタログ内の拡張機能パッケージのプライベートバージョンを表示したり、拡張機能パッケージをプロパティにインストールしたりできなくなります。

>[!NOTE]
>
>以前にインストールしたプライベートバージョンは、引き続き期待どおりに動作します。

**API 形式**

```http
DELETE /extension_package_usage_authorizations/{ID}
```

| パラメーター | 説明 |
| --- | --- |
| `ID` | 削除する拡張機能パッケージの使用状況認証の `ID`。 |

{style="table-layout:auto"}

**リクエスト**

```shell
curl -X DELETE \
  https://reactor.adobe.io/extension_package_usage_authorizations/{ID} \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}'
```

**応答**

応答が成功すると、応答本文なしで HTTP ステータス 204 （コンテンツなし）が返されます。 これは、拡張機能が削除されたことを示します。

## 拡張機能パッケージの使用権限の更新 {#update}

拡張機能パッケージの使用権限を承認または却下するには、PATCHリクエストのパスに `ID` を含めます。

>[!NOTE]
>
>会社の拡張機能パッケージの使用承認を承認または却下するには、`manage_properties` 権限が必要です。

**API 形式**

```http
PATCH /extension_package_usage_authorizations/{ID}
```

| パラメーター | 説明 |
| --- | --- |
| `ID` | 削除する拡張機能パッケージの使用状況認証の `ID`。 |

{style="table-layout:auto"}

**リクエスト**

```shell
curl -X PATCH \
  https://reactor.adobe.io/extension_package_usage_authorizations/{ID} \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -d '{
        "data": {
          "attributes": {
            "state": "approved"
          },
          "type": "extension_package_usage_authorizations",
          "id": "EA86f54b48dd7042a68686508e03be8ba9"
        }
      } 
```

| プロパティ | 説明 |
| --- | --- |
| `attributes` | 改訂する属性。 拡張機能パッケージの使用権限の場合は、`state` を変更できます。 |

**応答**

応答が成功すると、変更された拡張機能パッケージの使用承認の詳細が返されます。

```json
{
  "data": {
    "id": "EA86f54b48dd7042a68686508e03be8ba9",
    "type": "extension_package_usage_authorizations",
    "attributes": {
      "created_at": "2024-06-05T23:17:59.480Z",
      "updated_at": "2024-06-05T23:18:00.115Z",
      "name": "Acme",
      "platform": "web",
      "owner_org_id": "{ORG_ID}",
      "owner_org_name": "Reactor QE",
      "authorized_org_id": "{ORG_ID}",
      "authorized_org_name": "Acme Inc'",
      "state": "approved",
      "created_by_email": "Restricted",
      "created_by_display_name": "Restricted",
      "updated_by_email": "example@adobe.com",
      "updated_by_display_name": "john snow"
    },
    "relationships": {
      "extension_package": {
        "links": {
          "related": "https://reactor.adobe.io/extension_package_usage_authorizations/EA86f54b48dd7042a68686508e03be8ba9/extension_package"
        },
        "data": {
          "id": "EPb91d54cad9f749dba4e5566459f84c9c",
          "type": "extension_packages"
        }
      }
    },
    "links": {
      "self": "https://reactor.adobe.io/extension_package_usage_authorizations/EA86f54b48dd7042a68686508e03be8ba9"
    }
  }
}
```

>[!NOTE]
>
>認証が承認されると、組織はプロパティに拡張機能パッケージをインストールできます。

## 拡張機能パッケージの使用承認用に、拡張機能パッケージのデータを取得します {#retrieve_data}

拡張機能パッケージの使用承認用の拡張機能パッケージのデータを取得するには、GETリクエストを行います。

**API 形式**

```http
GET /extension_package_usage_authorizations/{ID}/extension_package
```

| パラメーター | 説明 |
| --- | --- |
| `ID` | 取得する拡張機能パッケージの使用権限認証の `ID`。 |

{style="table-layout:auto"}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/extension_package_usage_authorizations/{ID}/extension_package \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、拡張機能パッケージ認証の拡張機能パッケージのデータが返されます。

```json
{
  "data": {
    "id": "EP45ae063fd75c4c22936d3d456c858cfa",
    "type": "extension_packages",
    "attributes": {
      "actions": [],
      "author": {
        "url": "http://adobe.com",
        "name": "Acme",
        "email": "acme@adobe.com"
      },
      "availability": "private",
      "cdn_path": "https://assets.adobedtm.com/staging/extensions/EP45ae063fd75c4c22936d3d456c858cfa",
      "conditions": [],
      "configuration": null,
      "created_at": "2024-06-05T23:17:48.607Z",
      "data_elements": [],
      "description": "Provides nothing.",
      "discontinued": false,
      "display_name": "Acme Template Test",
      "ecma_version": null,
      "events": [],
      "exchange_url": null,
      "hosted_lib_files": null,
      "icon_path": "resources/icons/core.svg",
      "main": "null",
      "name": "Acme",
      "owner_org_id": "{ORG_ID}",
      "resources": null,
      "shared_modules": null,
      "status": "succeeded",
      "platform": "web",
      "updated_at": "2024-06-05T23:17:53.806Z",
      "version": "1.0.0",
      "view_base_path": "dist/",
      "created_by_email": "example@adobe.com",
      "created_by_display_name": "john snow",
      "updated_by_email": "example@adobe.com",
      "updated_by_display_name": "john snow"
    },
    "relationships": {
      "extension_package": {
        "links": {
          "related": "https://reactor.adobe.io/extension_packages/EP45ae063fd75c4c22936d3d456c858cfa/extension_package_usage_authorizations"
        }
      }
    },
    "links": {
      "self": "https://reactor.adobe.io/extension_packages/EP45ae063fd75c4c22936d3d456c858cfa"
    }
  }
}
```
