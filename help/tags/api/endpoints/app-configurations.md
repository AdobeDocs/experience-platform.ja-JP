---
title: アプリ設定エンドポイント
description: Reactor APIで/app_configurationsエンドポイントを呼び出す方法を説明します。
source-git-commit: 6a1728bd995137a7cd6dc79313762ae6e665d416
workflow-type: tm+mt
source-wordcount: '590'
ht-degree: 11%

---

# アプリ設定エンドポイント

>[!WARNING]
>
>フィーチャが追加、削除、およびリワークされると、`/app_configurations`エンドポイントの実装は不安定になります。

アプリ設定を使用すると、資格情報を保存し、取得して後で使用できます。 Reactor APIの`/app_configurations`エンドポイントを使用すると、エクスペリエンスアプリケーション内のアプリ設定をプログラムで管理できます。

## はじめに

このガイドで使用するエンドポイントは、[Reactor API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/reactor.yaml)の一部です。 続行する前に、APIへの認証方法に関する重要な情報について、[はじめにのガイド](../getting-started.md)を参照してください。

## アプリ設定のリストの取得 {#list}

**API 形式**

```http
GET /companies/{COMPANY_ID}/app_configurations
```

| パラメーター | 説明 |
| --- | --- |
| `COMPANY_ID` | アプリ設定を所有する[会社](./companies.md)の`id`。 |

{style=&quot;table-layout:auto&quot;}

>[!NOTE]
>
>クエリパラメーターを使用して、リストされたアプリ設定を次の属性に基づいてフィルタリングできます。<ul><li>`app_id`</li><li>`created_at`</li><li>`key_type`</li><li>`messaging_service`</li><li>`name`</li><li>`platform`</li><li>`updated_at`</li></ul>詳しくは、[応答のフィルタリング](../guides/filtering.md)に関するガイドを参照してください。

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/companies/COdb0cd64ad4524440be94b8496416ec7d/app_configurations \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

正常な応答は、アプリ設定のリストを返します。

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

アプリ設定を検索するには、アプリリクエストのパスにIDを指定します。GETの設定

**API 形式**

```http
GET /app_configurations/{APP_CONFIGURATION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `APP_CONFIGURATION_ID` | 検索するアプリ設定の`id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/app_configurations/AC40c339ab80d24c958b90d67b698602eb \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

正常な応答は、アプリ設定の詳細を返します。

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

アプリ設定を新規作成するには、POSTリクエストを実行します。

**API 形式**

```http
POST /companies/{COMPANY_ID}/app_configurations
```

| パラメーター | 説明 |
| --- | --- |
| `COMPANY_ID` | アプリ設定を定義する[会社](./companies.md)の`id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X POST \
  https://reactor.adobe.io/companies \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Content-Type: application/json' \
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
| `platform` | アプリケーションが実行されるプラットフォーム（Webまたはモバイル）。 使用可能なメッセージングサービスを決定します。 |
| `messaging_service` | [Apple Push Notification service(APNs)](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html)や[Firebase Cloud Messaging(FCM)](https://firebase.google.com/docs/cloud-messaging)など、アプリに関連付けられたメッセージングサービスです。 使用できるキーの種類を決定します。 |
| `key_type` | プッシュサービスベンダーがサポートするプロトコルを表し、`push_credential`オブジェクトの形式を決定します。 メッセージングサービスのプロトコルが進化するにつれ、更新されたプロトコルをサポートする新しい`key_type`値が作成されます。 |
| `push_credential` | 実際の秘密鍵証明書の値。保存時に暗号化されます。 このフィールドは、通常、復号化されたり、API応答に含まれたりすることはありません。 復号化されたプッシュAdobeを含む応答を取得できるのは、特定の秘密鍵証明書サービスのみです。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、新しく作成されたアプリ設定の詳細を返します。

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

アプリの設定を更新するには、アプリリクエストのパスにIDを含めます。PUTリクエスト

**API 形式**

```http
PUT /app_configurations/{APP_CONFIGURATION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `APP_CONFIGURATION_ID` | 更新するアプリ設定の`id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、既存のアプリ設定の`app_id`を更新します。

```shell
curl -X PUT \
  https://reactor.adobe.io/app_configurations/AC40c339ab80d24c958b90d67b698602eb \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Content-Type: application/json' \
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
| `attributes` | アプリ設定用に更新する属性を表すプロパティを持つオブジェクト。 各キーは、更新する特定のアプリ設定属性と、更新先の対応する値を表します。<br><br>アプリ設定で更新できる属性は次のとおりです。<ul><li>`app_id`</li><li>`key_type`</li><li>`messaging_service`</li><li>`name`</li><li>`platform`</li><li>`push_credential`</li></ul> |
| `id` | 更新するアプリ設定の`id`。 これは、リクエストパスで指定された`{APP_CONFIGURATION_ID}`値と一致する必要があります。 |
| `type` | 更新するリソースのタイプ。 このエンドポイントでは、値を`app_configurations`にする必要があります。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、更新されたアプリ設定の詳細を返します。

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

アプリ設定を削除するには、アプリリクエストのパスにIDを含めます。DELETEリクエスト

**API 形式**

```http
DELETE /app_configurations/{APP_CONFIGURATION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `APP_CONFIGURATION_ID` | 削除するアプリ設定の`id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X DELETE \
  https://reactor.adobe.io/app_configurations/AC40c339ab80d24c958b90d67b698602eb \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
```

**応答**

正常な応答は、応答本文がないHTTPステータス204（コンテンツなし）を返し、アプリ設定が削除されたことを示します。
