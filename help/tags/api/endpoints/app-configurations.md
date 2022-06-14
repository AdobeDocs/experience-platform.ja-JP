---
title: アプリ設定エンドポイント
description: Reactor API で /app_configurations エンドポイントを呼び出す方法を説明します。
exl-id: 88a1ec36-b4d2-4fb6-92cb-1da04268492a
source-git-commit: 36320addc790e844a1102314890e8692841dc5d0
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 97%

---

# アプリ設定エンドポイント

>[!WARNING]
>
>機能が追加、削除、およびリワークされると、`/app_configurations` エンドポイントの実装は不安定になります。

アプリ設定を使用すると、資格情報を保存し、取得して後で使用できます。 Reactor API の `/app_configurations` エンドポイントを使用すると、エクスペリエンスアプリケーション内のアプリ設定をプログラムで管理できます。

## はじめに

このガイドで使用するエンドポイントは、[Reactor API](https://www.adobe.io/experience-platform-apis/references/reactor/) の一部です。続行する前に、 [はじめる前に](../getting-started.md) で、API への認証方法に関する重要な情報を確認してください。

## アプリ設定のリストの取得 {#list}

**API 形式**

```http
GET /companies/{COMPANY_ID}/app_configurations
```

| パラメーター | 説明 |
| --- | --- |
| `COMPANY_ID` | アプリ設定を所有する[会社](./companies.md)の `id`。 |

{style=&quot;table-layout:auto&quot;}

>[!NOTE]
>
>クエリパラメーターを使用して、リストされたアプリ設定を次の属性に基づいてフィルタリングできます。<ul><li>`app_id`</li><li>`created_at`</li><li>`key_type`</li><li>`messaging_service`</li><li>`name`</li><li>`platform`</li><li>`updated_at`</li></ul>詳しくは、 [応答のフィルタリング](../guides/filtering.md) に関するガイドを参照してください。

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/companies/COdb0cd64ad4524440be94b8496416ec7d/app_configurations \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、アプリ設定のリストが返されます。

```json
{
  "data": [
    {
      "id": "AC40c339ab80d24c958b90d67b698602eb",
      "type": "app_configurations",
      "attributes": {
        "created_at": "2020-12-14T17:31:10.626Z",
        "updated_at": "2020-12-14T17:31:10.626Z",
        "app_id": "com.adobe.test_app",
        "name": "Kessel Apns App",
        "platform": "mobile",
        "messaging_service": "apns",
        "key_type": "p8_file"
      },
      "relationships": {
        "company": {
          "links": {
            "related": "https://reactor.adobe.io/app_configurations/AC40c339ab80d24c958b90d67b698602eb/company"
          },
          "data": {
            "id": "CO2bf094214ffd4785bb4bcf88c952a7c1",
            "type": "companies"
          }
        }
      },
      "links": {
        "company": "https://reactor.adobe.io/companies/CO2bf094214ffd4785bb4bcf88c952a7c1",
        "self": "https://reactor.adobe.io/app_configurations/AC40c339ab80d24c958b90d67b698602eb"
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

## アプリ設定の検索 {#lookup}

GET リクエストのパスで ID を指定することで、アプリ設定を検索できます。

**API 形式**

```http
GET /app_configurations/{APP_CONFIGURATION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `APP_CONFIGURATION_ID` | 検索するアプリ設定の `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/app_configurations/AC40c339ab80d24c958b90d67b698602eb \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、アプリ構成の詳細が返されます。

```json
{
  "data": {
    "id": "AC40c339ab80d24c958b90d67b698602eb",
    "type": "app_configurations",
    "attributes": {
      "created_at": "2020-12-14T17:31:10.626Z",
      "updated_at": "2020-12-14T17:31:10.626Z",
      "app_id": "com.adobe.test_app",
      "name": "Kessel Apns App",
      "platform": "mobile",
      "messaging_service": "apns",
      "key_type": "p8_file"
    },
    "relationships": {
      "company": {
        "links": {
          "related": "https://reactor.adobe.io/app_configurations/AC40c339ab80d24c958b90d67b698602eb/company"
        },
        "data": {
          "id": "CO2bf094214ffd4785bb4bcf88c952a7c1",
          "type": "companies"
        }
      }
    },
    "links": {
      "company": "https://reactor.adobe.io/companies/CO2bf094214ffd4785bb4bcf88c952a7c1",
      "self": "https://reactor.adobe.io/app_configurations/AC40c339ab80d24c958b90d67b698602eb"
    }
  }
}
```

## アプリ設定の作成 {#create}

POST リクエストをおこなうことで、新しいアプリ設定を作成できます。

**API 形式**

```http
POST /companies/{COMPANY_ID}/app_configurations
```

| パラメーター | 説明 |
| --- | --- |
| `COMPANY_ID` | アプリ設定を定義している[会社](./companies.md)の `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X POST \
  https://reactor.adobe.io/companies \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -d '{
        "data": {
          "attributes": {
            "name": "Kessel Apns App",
            "app_id": "com.adobe.test_app",
            "platform": "mobile",
            "messaging_service": "apns",
            "key_type": "p8_file",
            "push_credential": {
              "bundleId": "com.adobe.test_app",
              "keyId": "{KEY_ID}",
              "p8": "{SECRET}",
              "teamId": "{TEAM_ID}"
            }
          },
          "type": "app_configurations"
        }
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `platform` | アプリケーションが実行されるプラットフォーム（Web またはモバイル）。使用可能なメッセージングサービスを決定します。 |
| `messaging_service` | [Apple プッシュ通知サービス（APN）](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html)や[Firebase Cloud Messaging（FCM）](https://firebase.google.com/docs/cloud-messaging)など、アプリに関連付けられたメッセージングサービスです。使用できるキーのタイプを決定します。 |
| `key_type` | プッシュサービスベンダーがサポートするプロトコルを表し、`push_credential` オブジェクトの形式を決定します。メッセージングサービスのプロトコルが進化するにつれ、更新されたプロトコルをサポートする新しい `key_type` 値が作成されます。 |
| `push_credential` | 保存時に暗号化される、実際の秘密鍵証明書の値。このフィールドは通常、復号化されたり、API 応答に含まれたりすることはありません。 特定の Adobe サービスのみが、復号化されたプッシュ秘密鍵証明書を含む応答を取得できます。 |

{style=&quot;table-layout:auto&quot;}

**応答**

応答が成功すると、新しく作成されたアプリ設定の詳細が返されます。

```json
{
  "data": {
    "id": "AC40c339ab80d24c958b90d67b698602eb",
    "type": "app_configurations",
    "attributes": {
      "created_at": "2020-12-14T17:31:10.626Z",
      "updated_at": "2020-12-14T17:31:10.626Z",
      "app_id": "com.adobe.test_app",
      "name": "Kessel Apns App",
      "platform": "mobile",
      "messaging_service": "apns",
      "key_type": "p8_file"
    },
    "relationships": {
      "company": {
        "links": {
          "related": "https://reactor.adobe.io/app_configurations/AC40c339ab80d24c958b90d67b698602eb/company"
        },
        "data": {
          "id": "CO2bf094214ffd4785bb4bcf88c952a7c1",
          "type": "companies"
        }
      }
    },
    "links": {
      "company": "https://reactor.adobe.io/companies/CO2bf094214ffd4785bb4bcf88c952a7c1",
      "self": "https://reactor.adobe.io/app_configurations/AC40c339ab80d24c958b90d67b698602eb"
    }
  }
}
```

## アプリ設定の更新

アプリ設定を更新するには、アプリリクエストのパスに ID を含めます。PATCH

**API 形式**

```http
PATCH /app_configurations/{APP_CONFIGURATION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `APP_CONFIGURATION_ID` | 更新するアプリ設定の `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、既存のアプリ設定の `app_id` を更新します。

```shell
curl -X PATCH \
  https://reactor.adobe.io/app_configurations/AC40c339ab80d24c958b90d67b698602eb \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -d '{
        "data": {
          "attributes": {
            "app_id": "com.adobe.test_app_2"
          },
          "id": "AC40c339ab80d24c958b90d67b698602eb",
          "type": "app_configurations"
        }
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `attributes` | アプリ設定用に更新する属性を表すプロパティを持つオブジェクト。 各キーは、更新する特定のアプリ設定属性と、更新先の対応する値を表します。<br><br>アプリ設定では、次の属性を更新できます。<ul><li>`app_id`</li><li>`key_type`</li><li>`messaging_service`</li><li>`name`</li><li>`platform`</li><li>`push_credential`</li></ul> |
| `id` | 更新するアプリ設定の `id`。この値は、リクエストパスで指定された `{APP_CONFIGURATION_ID}` 値と一致する必要があります。 |
| `type` | 更新するリソースのタイプ。 このエンドポイントの場合は、値を `app_configurations` にする必要があります。 |

{style=&quot;table-layout:auto&quot;}

**応答**

応答が成功すると、更新されたアプリ設定の詳細が返されます。

```json
{
  "data": {
    "id": "AC40c339ab80d24c958b90d67b698602eb",
    "type": "app_configurations",
    "attributes": {
      "created_at": "2020-12-14T17:31:10.626Z",
      "updated_at": "2020-12-14T17:31:21.787Z",
      "app_id": "com.adobe.test_app_2",
      "name": "Kessel Apns App",
      "platform": "mobile",
      "messaging_service": "apns",
      "key_type": "p8_file"
    },
    "relationships": {
      "company": {
        "links": {
          "related": "https://reactor.adobe.io/app_configurations/AC40c339ab80d24c958b90d67b698602eb/company"
        },
        "data": {
          "id": "CO2bf094214ffd4785bb4bcf88c952a7c1",
          "type": "companies"
        }
      }
    },
    "links": {
      "company": "https://reactor.adobe.io/companies/CO2bf094214ffd4785bb4bcf88c952a7c1",
      "self": "https://reactor.adobe.io/app_configurations/AC40c339ab80d24c958b90d67b698602eb"
    }
  }
}
```

## アプリ設定の削除

DELETE リクエストのパスに ID を含めることで、アプリ設定を削除できます。

**API 形式**

```http
DELETE /app_configurations/{APP_CONFIGURATION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `APP_CONFIGURATION_ID` | 削除するアプリ設定の `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X DELETE \
  https://reactor.adobe.io/app_configurations/AC40c339ab80d24c958b90d67b698602eb \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、応答本文なしで HTTP ステータス 204（コンテンツなし）が返され、アプリ設定が削除されたことを示します。
