---
title: 秘密鍵エンドポイント
description: Reactor API で /secrets エンドポイントを呼び出す方法を説明します。
exl-id: 76875a28-5d13-402d-8543-24db7e2bee8e
source-git-commit: 4f3c97e2cad6160481adb8b3dab3d0c8b23717cc
workflow-type: tm+mt
source-wordcount: '1286'
ht-degree: 92%

---

# 秘密鍵エンドポイント

秘密鍵は、イベント転送プロパティ（`platform` 属性を持つプロパティが `edge` に設定されます）のみに存在するリソースです。 安全にデータ交換できるよう、別のシステムに対してイベント転送を認証できます。

このガイドでは、Reactor API で `/secrets` エンドポイントに呼び出しを行う方法を説明します。 様々な秘密鍵の種類とその使用方法について詳しくは、このガイドに戻る前に[秘密鍵](../guides/secrets.md)の概要を参照してください。

## はじめに

このガイドで使用するエンドポイントは、[Reactor API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/reactor.yaml) の一部です。続行する前に、 [はじめる前に](../getting-started.md) で、API への認証方法に関する重要な情報を確認してください。

## プロパティの秘密鍵のリストの取得 {#list-property}

プロパティに属する秘密鍵のリストを作成するには、GET リクエストを実行します。

**API 形式**

```http
GET /properties/{PROPERTY_ID}/secrets
```

| パラメーター | 説明 |
| --- | --- |
| `{PROPERTY_ID}` | 秘密鍵をリストするプロパティの ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/properties/PRe005d921bb724bc88c3ff28e3e916f04/secrets \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -H 'Content-Type: application/vnd.api+json'
```

**応答**

応答が成功すると、プロパティに属する秘密鍵のリストが返されます。

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

## 環境の秘密鍵のリストの取得 {#list-environment}

環境に属する秘密鍵のリストを作成するには、GET リクエストを実行します。

**API 形式**

```http
GET /environments/{ENVIRONMENT_ID}/secrets
```

| パラメーター | 説明 |
| --- | --- |
| `{ENVIRONMENT_ID}` | 秘密鍵をリストする環境の ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/environments/EN0a1b00749daf4ff48a34d2ec37286aa7/secrets \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -H 'Content-Type: application/vnd.api+json'
```

**応答**

応答が成功すると、環境に属する秘密鍵のリストが返されます。

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

## 秘密鍵の検索 {#lookup}

秘密鍵を検索するには、GET リクエストのパスで ID を指定します。

**API 形式**

```http
GET /secrets/{SECRET_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SECRET_ID}` | 検索する秘密鍵の ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -H 'Content-Type: application/vnd.api+json'
```

**応答**

応答が成功すると、秘密鍵の詳細が返されます。

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

## 秘密鍵の作成 {#create}

POST リクエストを行うことで、秘密鍵を作成できます。

>[!NOTE]
>
>新しい秘密鍵を作成すると、API はそのリソースの情報を含む応答を即座に返します。同時に、秘密鍵の交換タスクがトリガーされ、認証情報の交換が機能しているかどうかをテストします。このタスクは非同期で処理され、秘密鍵のステータス属性が結果に応じて `succeeded` または `failed` に更新されます。

**API 形式**

```http
POST /properties/{PROPERTY_ID}/secrets
```

| パラメーター | 説明 |
| --- | --- |
| `{PROPERTY_ID}` | 秘密鍵を定義するプロパティの ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X POST \
  https://reactor.adobe.io/properties/PR9eff664dc6014217b76939bb78b83976/secrets \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `name` | 秘密鍵の一意でわかりやすい名前。 |
| `type_of` | 秘密鍵が表す認証情報の種類。次の 3 つの値を指定できます。<ul><li>`token`：トークン文字列。</li><li>`simple-http`：ユーザー名とパスワード。</li><li>`oauth2`：OAuth 標準に準拠する認証情報。</li></ul> |
| `credentials` | 秘密鍵の認証情報の値を格納するオブジェクト。 `type_of` 属性に応じて、異なるプロパティを指定する必要があります。各タイプの要件の詳細については、秘密鍵ガイドの[認証情報](../guides/secrets.md#credentials)の節を参照してください。 |
| `relationships.environment` | 各秘密鍵は、最初に作成されたときに環境に関連付ける必要があります。 このプロパティ内の `data` オブジェクトは、`environments` の `type` 値と一緒に、秘密鍵が割り当てられた環境の `id` を持つ必要があります。 |
| `type` | 作成するリソースのタイプ。この呼び出しの場合は、値を `secrets` にする必要があります。 |

{style=&quot;table-layout:auto&quot;}

**応答**

応答が成功すると、秘密鍵の詳細が返されます。秘密鍵の種類に応じて、`credentials` 配下のプロパティは非表示になる場合があります。

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

## `oauth2` 秘密鍵のテスト {#test}

>[!NOTE]
>
>この操作は、`type_of` 値が `oauth2` の秘密鍵でのみ実行できます。

`oauth2` 秘密鍵は、 PATCH リクエストのパスに ID を含めることでテストできます。テスト操作は交換を実行し、承認サービス応答を `test_exchange` 秘密鍵の属性 `meta` のオブジェクトに含みます。この操作では、秘密鍵自体は更新されません。

**API 形式**

```http
PATCH /secrets/{SECRET_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SECRET_ID}` | テストする `oauth2` 秘密鍵の ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X PATCH \
  https://reactor.adobe.io/secrets/SE6c15a7a64f9041b5985558ed3e19a449 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `attributes` | 値が `oauth2` の `type_of` プロパティを含める必要があります。 |
| `meta` | `test` の値を持つ `action` プロパティを含む必要があります。 |
| `id` | テストする秘密鍵の ID。これは、リクエストパスで提供された ID と一致する必要があります。 |
| `type` | 操作しているリソースのタイプ。`secrets` に設定する必要があります。 |

{style=&quot;table-layout:auto&quot;}

**応答**

応答が成功すると、秘密鍵の詳細と、`meta.test_exchange` 配下に格納される認証サービスの応答が返されます。

```json
{ 
  "data": { 
    "id": "SE6c15a7a64f9041b5985558ed3e19a449", 
    "type": "secrets", 
    "attributes": { 
      "activated_at": null, 
      "created_at": "2021-07-14T19:33:25.628Z", 
      "credentials": { 
        "token_url": "https://token_url.test/token/authorize?required_param=value",
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

## 秘密鍵の再試行 {#retry}

秘密鍵の再試行は、秘密鍵の交換を手動でトリガーする操作です。PATCH リクエストのパスに ID を含めることで、秘密鍵を再試行できます。

**API 形式**

```http
PATCH /secrets/{SECRET_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SECRET_ID}` | 再試行する秘密鍵の ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X PATCH \
  https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `attributes` | 更新される秘密鍵のプロパティ（`token`、`simple-http` または `oauth2`）と一致する `type_of` プロパティが含まれている必要があります。 |
| `meta` | 値が `retry` の `action` プロパティが含まれている必要があります。 |
| `id` | 再試行する秘密鍵の ID。これは、リクエストパスで提供された ID と一致する必要があります。 |
| `type` | 操作しているリソースのタイプ。`secrets` に設定する必要があります。 |

{style=&quot;table-layout:auto&quot;}

**応答**

応答が成功すると、秘密鍵の詳細が返され、ステータスは `pending` にリセットされます。交換が完了すると、結果に応じて秘密鍵のステータスが `succeeded` または `failed` に更新されます。

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

## 再認証： `oauth2-google` 秘密鍵 {#reauthorize}

各 `oauth2-google` シークレットには次が含まれます： `meta.token_url_expires_at` 認証 URL の有効期限を示すプロパティ。 この時間が過ぎると、秘密鍵を再認証して認証プロセスを更新する必要があります。

を再認証するには、以下を実行します。 `oauth2-google` シークレット：問題の秘密のPATCHリクエストを作成します。

**API 形式**

```http
PATCH /secrets/{SECRET_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SECRET_ID}` | この `id` 再認証する秘密鍵を設定します。 |

**リクエスト**

この `data` リクエストペイロードのオブジェクトには、 `meta.action` プロパティを `reauthorize`.

```shell
curl -X PATCH \
  https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
        "data": {
          "attributes": {
            "type_of": "oauth2-google"
          },
          "meta": {
            "action": "reauthorize"
          },
          "id": "SEa3756b962e964fadb61e31df1f7dd5a3",
          "type": "secrets"
        }
      }'
```

**応答**

正常な応答は、更新された秘密鍵の詳細を返します。 ここから、 `meta.token_url` をブラウザーに追加して、認証プロセスを完了します。

```json
{
  "data": {
    "id": "SE5fdfa4c0a2d8404e8b1bc38827cc41c9",
    "type": "secrets",
    "attributes": {
      "created_at": "2021-07-15T19:00:25.628Z", 
      "updated_at": "2021-07-15T19:00:25.628Z", 
      "name": "Example Secret", 
      "type_of": "oauth2-google", 
      "activated_at": null, 
      "expires_at": null, 
      "refresh_at": null, 
      "status": "pending", 
      "credentials": { 
        "scopes": [
          "https://www.googleapis.com/auth/adwords",
          "https://www.googleapis.com/auth/pubsub"
        ], 
      } 
    }, 
    "relationships": { 
      "property": { 
        "links": { 
          "related": "https://reactor.adobe.io/secrets/SE5fdfa4c0a2d8404e8b1bc38827cc41c9/property" 
        }, 
        "data": { 
          "id": "PR9eff664dc6014217b76939bb78b83976", 
          "type": "properties" 
        } 
      }, 
      "environment": { 
        "links": { 
          "related": "https://reactor.adobe.io/secrets/SE5fdfa4c0a2d8404e8b1bc38827cc41c9/environment" 
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
          "related": "https://reactor.adobe.io/secrets/SE5fdfa4c0a2d8404e8b1bc38827cc41c9/notes" 
        } 
      } 
    }, 
    "links": { 
      "self": "https://reactor.adobe.io/secrets/SE5fdfa4c0a2d8404e8b1bc38827cc41c9", 
      "property": "https://reactor.adobe.io/secrets/SE5fdfa4c0a2d8404e8b1bc38827cc41c9/property" 
    }, 
    "meta": { 
      "token_url": "https://accounts.google.com/o/oauth2/auth?access_type=offline&approval_prompt=force&client_id=434635668552-0qvlu519fdjtnkvk8hu8c8dj8rg3723r.apps.googleusercontent.com&redirect_uri=https%3A%2F%2Freactor.adobe.io%2Foauth2%2Fcallback&response_type=code&scope=https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fadwords&state=state", 
      "token_url_expires_at": "2021-07-15T20:00:25.628Z" 
    } 
  } 
}
```

## 秘密鍵の削除 {#delete}

DELETE リクエストのパスに ID を含めることで、秘密鍵を削除できます。これは、すぐに反映される物理的な削除であり、ライブラリの再公開は不要です。

この操作により、関連する環境から秘密鍵が削除され、基になるリソースが削除されます。

>[!WARNING]
>
>削除された秘密鍵を参照するルールがデプロイされている場合、それらのルールは直ちに機能しなくなります。この秘密鍵を参照するデータ要素は、後で更新または削除する必要があります。

**API 形式**

```http
DELETE /secrets/{SECRET_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SECRET_ID}` | 削除する秘密鍵の ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X DELETE \
  https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -H 'Content-Type: application/vnd.api+json'
```

**応答**

正常な応答は、HTTP ステータス 204（コンテンツなし）と空の応答本文を返し、システムから秘密鍵が削除されたことを示します。

## 秘密鍵のメモのリスト {#notes}

Reactor API を使用すると、秘密鍵を含む特定のリソースにメモを追加できます。メモは、リソースの動作に影響を与えないテキスト注釈で、様々なユースケースで使用できます。

>[!NOTE]
>
>Reactor API リソースのメモの作成および編集方法について詳しくは、[メモエンドポイントガイド](./notes.md)を参照してください。

GET リクエストを行うと、秘密鍵に関連するすべてのメモを取得できます。

**API 形式**

```http
GET /secrets/{SECRET_ID}/notes
```

| パラメーター | 説明 |
| --- | --- |
| `{SECRET_ID}` | リストするメモの秘密鍵の ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/secrets/SE591d3b86910f4e6883f0e1c36e54bff1/notes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -H 'Content-Type: application/vnd.api+json'
```

**応答**

応答が成功すると、秘密鍵に属するメモのリストが返されます。

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

## 秘密鍵の関連リソースの取得 {#related}

次の呼び出しは、秘密鍵の関連リソースを取得する方法を示しています。[秘密鍵を検索すると](#lookup)、これらの関係は `relationships` プロパティの下に表示されます。

Reactor API の関係について詳しくは、 [関係に関するガイド](../guides/relationships.md) を参照してください。

### 秘密鍵に関連する環境の検索 {#environment}

GET リクエストのパスに `/environment` を追加することで、秘密鍵を利用する環境を検索できます。

**API 形式**

```http
GET /secrets/{SECRET_ID}/environment
```

| パラメーター | 説明 |
| --- | --- |
| `{SECRET_ID}` | 環境を検索する秘密鍵の ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3/environment \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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

### 秘密鍵に関連するプロパティの検索 {#property}

GET リクエストのパスに `/property` を追加することで、秘密鍵を所有するプロパティを検索できます。

**API 形式**

```http
GET /secrets/{SECRET_ID}/property
```

| パラメーター | 説明 |
| --- | --- |
| `{SECRET_ID}` | プロパティを検索する秘密鍵の ID。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/secrets/SEa3756b962e964fadb61e31df1f7dd5a3/property \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
