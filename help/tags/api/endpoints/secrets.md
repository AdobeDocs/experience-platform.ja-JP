---
title: シークレットエンドポイント
description: Reactor API を使用して、/シークレットエンドポイントへの呼び出しを行う方法について説明しています。
source-git-commit: 103ba74646a42fd62007f47df4d1a46caf25e3e6
workflow-type: tm+mt
source-wordcount: '1187'
ht-degree: 16%

---

# シークレットエンドポイント

シークレットは、イベント転送プロパティ (属性が「属性」に設定されたプロパティ) 内にのみ存在するリソースです `platform` `edge` 。 これを使用すると、セキュリティで保護されたデータ交換用に、他のシステムに対する認証をイベント転送に許可できます

このガイドでは、Reactor API を使用してエンドポイントの呼び出しを行う方法について説明して `/secrets` います。 各秘密タイプとその使用方法の詳細については、 [ ](../guides/secrets.md) このガイドに戻る前に、シークレットの概要を参照してください。

## はじめに

このガイドで使用するエンドポイントは、[Reactor API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/reactor.yaml) の一部です。続行する前に、 [はじめる前に](../getting-started.md) で、API への認証方法に関する重要な情報を確認してください。

## プロパティのシークレットの一覧の取得 {#list-property}

GET 要求を行うことにより、プロパティに属するシークレットを一覧表示できます。

**API 形式**

```http
GET /properties/{PROPERTY_ID}/secrets
```

| パラメーター | 説明 |
| --- | --- |
| `{PROPERTY_ID}` | シークレットを一覧表示するプロパティの ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/properties/PRe005d921bb724bc88c3ff28e3e916f04/secrets \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -H 'Content-Type: application/vnd.api+json'
```

**応答**

応答が成功した場合は、プロパティに属するシークレットのリストが返されます。

```json
{
  "data": [
    {
      "id": "SE8bd20bd5d16b49ee9d925929e292526c",
      "type": "secrets",
      "attributes": {
        "created_at": "2021-07-16T22:29:29.777Z",
        "updated_at": "2021-07-16T22:29:29.777Z",
        "name": "Example Secret",
        "type_of": "token",
        "activated_at": null,
        "expires_at": null,
        "refresh_at": null,
        "status": "pending",
        "credentials": {
        }
      },
      "relationships": {
        "property": {
          "links": {
            "related": "https://reactor.adobe.io/secrets/SE8bd20bd5d16b49ee9d925929e292526c/property"
          },
          "data": {
            "id": "PRe005d921bb724bc88c3ff28e3e916f04",
            "type": "properties"
          }
        },
        "environment": {
          "links": {
            "related": "https://reactor.adobe.io/secrets/SE8bd20bd5d16b49ee9d925929e292526c/environment"
          },
          "data": {
            "id": "EN80ad9efbf4ff4f15bebd770613378a75",
            "type": "environments"
          },
          "meta": {
            "stage": "development"
          }
        },
        "notes": {
          "links": {
            "related": "https://reactor.adobe.io/secrets/SE8bd20bd5d16b49ee9d925929e292526c/notes"
          }
        }
      },
      "links": {
        "self": "https://reactor.adobe.io/secrets/SE8bd20bd5d16b49ee9d925929e292526c",
        "property": "https://reactor.adobe.io/secrets/SE8bd20bd5d16b49ee9d925929e292526c/property"
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

## 環境のシークレットの一覧の取得 {#list-environment}

GET 要求を行うことにより、環境に属するシークレットをリストすることができます。

**API 形式**

```http
GET /environments/{ENVIRONMENT_ID}/secrets
```

| パラメーター | 説明 |
| --- | --- |
| `{ENVIRONMENT_ID}` | シークレットを一覧表示する環境の ID。 |

{style = &quot;テーブル-layout: auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/environments/EN0a1b00749daf4ff48a34d2ec37286aa7/secrets \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -H 'Content-Type: application/vnd.api+json'
```

**応答**

応答が成功した場合は、環境に属するシークレットのリストが返されます。

```json
{
  "data": [
    {
      "id": "SE57b5ea69acde4595825b85befd1675b3",
      "type": "secrets",
      "attributes": {
        "created_at": "2021-07-16T22:29:35.447Z",
        "updated_at": "2021-07-16T22:29:35.447Z",
        "name": "Example Secret",
        "type_of": "token",
        "activated_at": null,
        "expires_at": null,
        "refresh_at": null,
        "status": "pending",
        "credentials": {
        }
      },
      "relationships": {
        "property": {
          "links": {
            "related": "https://reactor.adobe.io/secrets/SE57b5ea69acde4595825b85befd1675b3/property"
          },
          "data": {
            "id": "PRb302ca3557334c13b130da719222ec97",
            "type": "properties"
          }
        },
        "environment": {
          "links": {
            "related": "https://reactor.adobe.io/secrets/SE57b5ea69acde4595825b85befd1675b3/environment"
          },
          "data": {
            "id": "EN0a1b00749daf4ff48a34d2ec37286aa7",
            "type": "environments"
          },
          "meta": {
            "stage": "development"
          }
        },
        "notes": {
          "links": {
            "related": "https://reactor.adobe.io/secrets/SE57b5ea69acde4595825b85befd1675b3/notes"
          }
        }
      },
      "links": {
        "self": "https://reactor.adobe.io/secrets/SE57b5ea69acde4595825b85befd1675b3",
        "property": "https://reactor.adobe.io/secrets/SE57b5ea69acde4595825b85befd1675b3/property"
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

## シークレットの検索 {#lookup}

GET 要求のパスに ID を含めることによって、シークレットを検索することができます。

**API 形式**

```http
GET /secrets/{SECRET_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SECRET_ID}` | 検索対象となるシークレットの ID を指定します。 |

{style = &quot;テーブル-layout: auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -H 'Content-Type: application/vnd.api+json'
```

**応答**

応答が成功した場合は、その機密情報が返されます。

```json
{
  "data": {
    "id": "SEa3756b962e964fadb61e31df1f7dd5a3",
    "type": "secrets",
    "attributes": {
      "created_at": "2021-07-16T22:29:41.135Z",
      "updated_at": "2021-07-16T22:29:41.135Z",
      "name": "Example Secret",
      "type_of": "token",
      "activated_at": null,
      "expires_at": null,
      "refresh_at": null,
      "status": "pending",
      "credentials": {
        }
    },
    "relationships": {
      "property": {
        "links": {
          "related": "https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3/property"
        },
        "data": {
          "id": "PR19717d5acf114209a5aebd0f2b228438",
          "type": "properties"
        }
      },
      "environment": {
        "links": {
          "related": "https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3/environment"
        },
        "data": {
          "id": "EN04d820606cc74d5eaf51616e8b50201a",
          "type": "environments"
        },
        "meta": {
          "stage": "development"
        }
      },
      "notes": {
        "links": {
          "related": "https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3/notes"
        }
      }
    },
    "links": {
      "self": "https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3",
      "property": "https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3/property"
    }
  }
}
```

## シークレットの作成 {#create}

POST 要求を行うことによって、シークレットを作成することができます。

>[!NOTE]
>
>新しいシークレットを作成すると、API によって、そのリソースに関する情報を含む即時応答が返されます。 同時に、秘密交換タスクがトリガーされ、資格情報交換が機能することをテストできます。 このタスクは非同期で処理され、その結果に応じて、secret のステータス属性が更新され `succeeded` `failed` ます。

**API 形式**

```http
POST /properties/{PROPERTY_ID}/secrets
```

| パラメーター | 説明 |
| --- | --- |
| `{PROPERTY_ID}` | シークレットを定義するプロパティの ID を指定します。 |

{style = &quot;テーブル-layout: auto&quot;}

**リクエスト**

```shell
curl -X POST \
  https://reactor.adobe.io/properties/PR9eff664dc6014217b76939bb78b83976/secrets \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
        "data": {
          "attributes": {
            "name": "Example Secret",
            "type_of": "simple-http",
            "credentials": {
              "username": "example_username",
              "password": "pass12345"
            }
          },
          "relationships": {
            "environment": {
              "data": {
                "id": "EN04cdddbdb6574170bcac9f470f3b8087",
                "type": "environments"
              }
            }
          },
          "type": "secrets"
        }
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | 一意でわかりやすいシークレットの名前。 |
| `type_of` | 秘密が表す認証資格情報の種類です。 には、次の3つの値が受け入れられます。<ul><li>`token`: トークンストリングです。</li><li>`simple-http`: ユーザー名とパスワードを入力します。</li><li>`oauth2`: OAuth 規格に準拠した資格情報。</li></ul> |
| `credentials` | シークレットの資格情報の値が格納されているオブジェクト。 属性に応じて `type_of` 、異なるプロパティを指定する必要があります。 [ ](../guides/secrets.md#credentials) 各タイプの要件について詳しくは、シークレットガイドの資格情報に関する項を参照してください。 |
| `relationships.environment` | 各シークレットは、最初に作成したときに環境に関連付ける必要があります。 `data`このプロパティ内のオブジェクトには、 `id` 指定されている機密情報を含む環境の値が含まれている必要があり `type` `environments` ます。 |
| `type` | 作成するリソースのタイプ。この呼び出しには、値を指定する必要があり `secrets` ます。 |

{style = &quot;テーブル-layout: auto&quot;}

**応答**

応答が成功した場合は、その機密情報が返されます。 秘匿の種類によっては、の下に一部のプロパティ `credentials` が表示されないことがあります。

```json
{
  "data": { 
    "id": "SE39fdf431f8ad4600bbc24ea73fcb1154", 
    "type": "secrets", 
    "attributes": { 
    "created_at": "2021-07-14T19:33:25.628Z", 
    "updated_at": "2021-07-14T19:33:25.628Z", 
    "name": "Example Secret", 
    "type_of": "simple-http", 
    "activated_at": null, 
    "expires_at": null, 
    "refresh_at": null, 
    "status": "pending", 
    "credentials": { 
      "username": "example_username" 
      } 
    }, 
    "relationships": { 
      "property": { 
        "links": { 
          "related": "https://reactor.adobe.io/secrets/SE39fdf431f8ad4600bbc24ea73fcb1154/property" 
        }, 
        "data": { 
          "id": "PR9eff664dc6014217b76939bb78b83976", 
          "type": "properties" 
        } 
      }, 
      "environment": { 
        "links": { 
          "related": "https://reactor.adobe.io/secrets/SE39fdf431f8ad4600bbc24ea73fcb1154/environment" 
        }, 
        "data": { 
          "id": "EN04cdddbdb6574170bcac9f470f3b8087", 
          "type": "environments" 
        }, 
        "meta": { 
          "stage": "development" 
        } 
      }, 
      "notes": { 
        "links": { 
          "related": "https://reactor.adobe.io/secrets/SE39fdf431f8ad4600bbc24ea73fcb1154/notes" 
        } 
      } 
    }, 
    "links": { 
      "self": "https://reactor.adobe.io/secrets/SE39fdf431f8ad4600bbc24ea73fcb1154", 
      "property": "https://reactor.adobe.io/secrets/SE39fdf431f8ad4600bbc24ea73fcb1154/property" 
    } 
  } 
}
```

## シークレットのテスト `oauth2` {#test}

>[!NOTE]
>
>この操作は、の値を持つシークレットに対してのみ実行でき `type_of` `oauth2` ます。

`oauth2`修正プログラムの要求のパスに ID を含めることによって、シークレットをテストすることができます。テスト操作では、交換が実行され、秘密のオブジェクトの属性に認証サービス応答が含まれ `test_exchange` `meta` ます。 この操作では、シークレットそのものは更新されません。

**API 形式**

```http
PATCH /secrets/{SECRET_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SECRET_ID}` | テストする秘密キーの ID を指定し `oauth2` ます。 |

{style = &quot;テーブル-layout: auto&quot;}

**リクエスト**

```shell
curl -X PATCH \
  https://reactor.adobe.io/secrets/SE6c15a7a64f9041b5985558ed3e19a449 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
        "data": {
          "attributes": {
            "type_of": "oauth2"
          },
          "meta": {
            "action": "test"
          },
          "id": "SE6c15a7a64f9041b5985558ed3e19a449",
          "type": "secrets"
        }
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `attributes` | の値を持つプロパティが含まれている必要があり `type_of` `oauth2` ます。 |
| `meta` | の値を持つプロパティが含まれている必要があり `action` `test` ます。 |
| `id` | テストするシークレットの ID です。 これは、要求パスに指定されている ID と一致している必要があります。 |
| `type` | 操作中のリソースの型。 `secrets` に設定する必要があります。 |

{style = &quot;テーブル-layout: auto&quot;}

**応答**

応答が成功した場合は、その内容が認証サービスの応答に含まれているという情報が返され `meta.test_exchange` ます。

```json
{ 
  "data": { 
    "id": "SE6c15a7a64f9041b5985558ed3e19a449", 
    "type": "secrets", 
    "attributes": { 
      "activated_at": null, 
      "created_at": "2021-07-14T19:33:25.628Z", 
      "credentials": { 
        "authorization_url": "https://athorization_url.test/token/authorize?required_param=value",
        "client_id": "test_client_id", 
        "client_secret": "test_client_secret", 
        "refresh_offset": 14400, 
      },
      "expires_at": null, 
      "name": "Example Secret", 
      "refresh_at": null, 
      "status": "pending", 
      "type_of": "oauth2", 
      "updated_at": "2021-07-14T19:33:25.628Z", 
    }, 
    "relationships": { 
      "property": { 
        "links": { 
          "related": "https://reactor.adobe.io/secrets/SE39fdf431f8ad4600bbc24ea73fcb1154/property" 
        }, 
        "data": { 
          "id": "PR9eff664dc6014217b76939bb78b83976", 
          "type": "properties" 
        } 
      }, 
      "environment": { 
        "links": { 
          "related": "https://reactor.adobe.io/secrets/SE39fdf431f8ad4600bbc24ea73fcb1154/environment" 
        }, 
        "data": { 
          "id": "EN04cdddbdb6574170bcac9f470f3b8087", 
          "type": "environments" 
        }, 
        "meta": { 
          "stage": "development" 
        } 
      }, 
      "notes": { 
        "links": { 
          "related": "https://reactor.adobe.io/secrets/SE39fdf431f8ad4600bbc24ea73fcb1154/notes" 
        } 
      } 
    }, 
    "links": { 
      "self": "https://reactor.adobe.io/secrets/SE39fdf431f8ad4600bbc24ea73fcb1154", 
      "property": "https://reactor.adobe.io/secrets/SE39fdf431f8ad4600bbc24ea73fcb1154/property" 
    }, 
    "meta": { 
      "test_exchange": { 
        "access_token": "FpIkBn3zxW0fH6EBC4MB", 
        "expires_in": 36000, 
        "token_type": "Bearer", 
      } 
    } 
  } 
}
```

## シークレットの再実行 {#retry}

シークレットのリトライは、secret exchange を手動で起動する操作になります。 修正プログラムのリクエストのパスに ID を含めることによって、シークレットをリトライすることができます。

**API 形式**

```http
PATCH /secrets/{SECRET_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SECRET_ID}` | 再実行する秘密キーの ID を指定します。 |

{style = &quot;テーブル-layout: auto&quot;}

**リクエスト**

```shell
curl -X PATCH \
  https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
        "data": {
          "attributes": {
            "type_of": "token"
          },
          "meta": {
            "action": "retry"
          },
          "id": "SEa3756b962e964fadb61e31df1f7dd5a3",
          "type": "secrets"
        }
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `attributes` | `type_of`更新されるシークレット ( `token` 、、 `simple-http` 、または) の値と一致するプロパティが含まれている必要があり `oauth2` ます。 |
| `meta` | の値を持つプロパティが含まれている必要があり `action` `retry` ます。 |
| `id` | 再試行しているシークレットの ID です。 これは、要求パスに指定されている ID と一致している必要があります。 |
| `type` | 操作中のリソースの型。 `secrets` に設定する必要があります。 |

{style = &quot;テーブル-layout: auto&quot;}

**応答**

応答が成功した場合は、secret の詳細が返されます。この場合、状態は「にリセット」に `pending` なります。 交換が完了すると、その結果に応じて、シークレットの状態が更新され `succeeded` `failed` ます。

```json
{
  "data": {
    "id": "SEa3756b962e964fadb61e31df1f7dd5a3",
    "type": "secrets",
    "attributes": {
      "created_at": "2021-07-16T22:29:41.135Z",
      "updated_at": "2021-07-16T22:29:41.135Z",
      "name": "Example Secret",
      "type_of": "token",
      "activated_at": null,
      "expires_at": null,
      "refresh_at": null,
      "status": "pending",
      "credentials": {
        }
    },
    "relationships": {
      "property": {
        "links": {
          "related": "https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3/property"
        },
        "data": {
          "id": "PR19717d5acf114209a5aebd0f2b228438",
          "type": "properties"
        }
      },
      "environment": {
        "links": {
          "related": "https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3/environment"
        },
        "data": {
          "id": "EN04d820606cc74d5eaf51616e8b50201a",
          "type": "environments"
        },
        "meta": {
          "stage": "development"
        }
      },
      "notes": {
        "links": {
          "related": "https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3/notes"
        }
      }
    },
    "links": {
      "self": "https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3",
      "property": "https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3/property"
    }
  }
}
```

## シークレットの削除 {#delete}

削除要求のパスに ID を含めることによって、シークレットを削除することができます。 これは、ただちに適用されるハードデリートで、ライブラリの再パブリッシュは必要ありません。

この操作を行うと、それが関連付けられている環境からシークレットが削除され、下にあるリソースも削除されます。

>[!WARNING]
>
>削除された機密情報を参照するルールが展開されている場合、これらのルールはただちに停止します。 このシークレットが参照されているデータエレメントは、後で更新または削除する必要があります。

**API 形式**

```http
DELETE /secrets/{SECRET_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SECRET_ID}` | 削除する秘密キーの ID を指定します。 |

{style = &quot;テーブル-layout: auto&quot;}

**リクエスト**

```shell
curl -X DELETE \
  https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -H 'Content-Type: application/vnd.api+json'
```

**応答**

応答が成功した場合は、HTTP 状態 204 (内容なし) と空の応答本文が返されます。これは、シークレットがシステムから削除されたことを示します。

## 注意事項の一覧 {#notes}

Reactor API を使用して、機密情報などの特定のリソースにノートを追加することができます。 ノートは、リソースの動作に影響を与えないテキストのコメントであり、様々な用途に使用することができます。

>[!NOTE]
>
>[ ](./notes.md) Reactor API リソース用のノートを作成および編集する方法について詳しくは、「ノーツのエンドポイントガイド」を参照してください。

GET 要求を行うことにより、その機密情報に関連するすべてのメモを取得することができます。

**API 形式**

```http
GET /secrets/{SECRET_ID}/notes
```

| パラメーター | 説明 |
| --- | --- |
| `{SECRET_ID}` | ノートの一覧を表示する機密情報を指定します。 |

{style = &quot;テーブル-layout: auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/secrets/SE591d3b86910f4e6883f0e1c36e54bff1/notes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -H 'Content-Type: application/vnd.api+json'
```

**応答**

応答が成功した場合は、その機密情報に属するノートのリストが返されます。

```json
{
  "data": [
    {
      "id": "NTe666dbcc2f204218b6fde2fe60ab7043",
      "type": "notes",
      "attributes": {
        "author_display_name": "John Doe",
        "author_email": "jdoe@example.com",
        "created_at": "2021-07-16T22:29:58.259Z",
        "text": "This is a secret note."
      },
      "relationships": {
        "resource": {
          "links": {
            "related": "https://reactor.adobe.io/secrets/SE591d3b86910f4e6883f0e1c36e54bff1"
          },
          "data": {
            "id": "SE591d3b86910f4e6883f0e1c36e54bff1",
            "type": "secrets"
          }
        }
      },
      "links": {
        "resource": "https://reactor.adobe.io/secrets/SE591d3b86910f4e6883f0e1c36e54bff1",
        "self": "https://reactor.adobe.io/notes/NTe666dbcc2f204218b6fde2fe60ab7043"
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

## 秘匿情報に関連するリソースを取得する {#related}

以下の呼び出しは、秘匿の関連リソースを取得する方法を示しています。 [シークレットを検索する場合 ](#lookup) は、これらの関係がプロパティの下に一覧表示され `relationships` ます。

Reactor API の関係について詳しくは、 [関係に関するガイド](../guides/relationships.md) を参照してください。

### 関連する環境を参照してください。 {#environment}

GET 要求のパスに追加することによって、秘密情報を使用している環境を検索することができ `/environment` ます。

**API 形式**

```http
GET /secrets/{SECRET_ID}/environment
```

| パラメーター | 説明 |
| --- | --- |
| `{SECRET_ID}` | 検索対象の環境の機密情報を指定します。 |

{style = &quot;テーブル-layout: auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3/environment \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -H 'Content-Type: application/vnd.api+json'
```

**応答**

応答が成功すると、環境の詳細が返されます。

```json
{
  "data": {
    "id": "EN33e8d63ac647448da1f77ced5c3c8580",
    "type": "environments",
    "attributes": {
      "archive": false,
      "created_at": "2021-07-16T22:19:13.438Z",
      "library_path": "bc12f2b85d11/b9a3067414ff",
      "library_name": "launch-3b6c4eea15ae-development.min.js",
      "library_entry_points": [
        {
          "library_name": "launch-3b6c4eea15ae-development.min.js",
          "minified": true,
          "references": [
            "bc12f2b85d11/b9a3067414ff/launch-3b6c4eea15ae-development.min.js"
          ],
          "license_path": "bc12f2b85d11/b9a3067414ff/launch-3b6c4eea15ae-development.js"
        },
        {
          "library_name": "launch-3b6c4eea15ae-development.js",
          "minified": false,
          "references": [
            "bc12f2b85d11/b9a3067414ff/launch-3b6c4eea15ae-development.js"
          ]
        }
      ],
      "name": "Development Environment A",
      "path": null,
      "stage": "development",
      "updated_at": "2021-07-16T22:19:13.438Z",
      "status": "succeeded",
      "token": "3b6c4eea15ae"
    },
    "relationships": {
      "library": {
        "links": {
          "related": "https://reactor.adobe.io/environments/EN33e8d63ac647448da1f77ced5c3c8580/library"
        },
        "data": null
      },
      "builds": {
        "links": {
          "related": "https://reactor.adobe.io/environments/EN33e8d63ac647448da1f77ced5c3c8580/builds"
        }
      },
      "host": {
        "links": {
          "related": "https://reactor.adobe.io/environments/EN33e8d63ac647448da1f77ced5c3c8580/host",
          "self": "https://reactor.adobe.io/environments/EN33e8d63ac647448da1f77ced5c3c8580/relationships/host"
        },
        "data": {
          "id": "HT7d5cf665463e46a1968ada1ceb1b5ca7",
          "type": "hosts"
        }
      },
      "property": {
        "links": {
          "related": "https://reactor.adobe.io/environments/EN33e8d63ac647448da1f77ced5c3c8580/property"
        },
        "data": {
          "id": "PRba81d3027db846b084212391aa5d2f1f",
          "type": "properties"
        }
      }
    },
    "links": {
      "property": "https://reactor.adobe.io/properties/PRba81d3027db846b084212391aa5d2f1f",
      "self": "https://reactor.adobe.io/environments/EN33e8d63ac647448da1f77ced5c3c8580"
    },
    "meta": {
      "archive_encrypted": false
    }
  }
}
```

### 秘匿情報に関連するプロパティを検索します。 {#property}

GET 要求のパスに追加することによって、secret を所有しているプロパティを検索することができ `/property` ます。

**API 形式**

```http
GET /secrets/{SECRET_ID}/property
```

| パラメーター | 説明 |
| --- | --- |
| `{SECRET_ID}` | 検索対象のプロパティを持つシークレットの ID です。 |

{style = &quot;テーブル-layout: auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3/property \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -H 'Content-Type: application/vnd.api+json'
```

**応答**

応答が成功すると、プロパティの詳細が返されます。

```json
{
  "data": {
    "id": "PR2d191feafef54c5da4ddb5ef647d4867",
    "type": "properties",
    "attributes": {
      "created_at": "2021-07-16T22:26:25.320Z",
      "enabled": true,
      "name": "Kessel Edge Example Property",
      "updated_at": "2021-07-16T22:26:25.320Z",
      "platform": "edge",
      "development": false,
      "token": "8efbe7023155"
    },
    "relationships": {
      "company": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR2d191feafef54c5da4ddb5ef647d4867/company"
        },
        "data": {
          "id": "COc26fb19f87d24cbe827a70ad2a672093",
          "type": "companies"
        }
      },
      "callbacks": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR2d191feafef54c5da4ddb5ef647d4867/callbacks"
        }
      },
      "hosts": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR2d191feafef54c5da4ddb5ef647d4867/hosts"
        }
      },
      "environments": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR2d191feafef54c5da4ddb5ef647d4867/environments"
        }
      },
      "libraries": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR2d191feafef54c5da4ddb5ef647d4867/libraries"
        }
      },
      "data_elements": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR2d191feafef54c5da4ddb5ef647d4867/data_elements"
        }
      },
      "extensions": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR2d191feafef54c5da4ddb5ef647d4867/extensions"
        }
      },
      "rules": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR2d191feafef54c5da4ddb5ef647d4867/rules"
        }
      },
      "notes": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR2d191feafef54c5da4ddb5ef647d4867/notes"
        }
      }
    },
    "links": {
      "company": "https://reactor.adobe.io/companies/COc26fb19f87d24cbe827a70ad2a672093",
      "data_elements": "https://reactor.adobe.io/properties/PR2d191feafef54c5da4ddb5ef647d4867/data_elements",
      "environments": "https://reactor.adobe.io/properties/PR2d191feafef54c5da4ddb5ef647d4867/environments",
      "extensions": "https://reactor.adobe.io/properties/PR2d191feafef54c5da4ddb5ef647d4867/extensions",
      "rules": "https://reactor.adobe.io/properties/PR2d191feafef54c5da4ddb5ef647d4867/rules",
      "self": "https://reactor.adobe.io/properties/PR2d191feafef54c5da4ddb5ef647d4867"
    },
    "meta": {
      "rights": [
        "approve",
        "develop",
        "edit_property",
        "manage_environments",
        "manage_extensions",
        "publish"
      ]
    }
  }
}
```
