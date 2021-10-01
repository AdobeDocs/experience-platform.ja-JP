---
title: ホストエンドポイント
description: Reactor API で /hosts エンドポイントを呼び出す方法を説明します。
exl-id: 9d0d2a65-49e9-429c-a665-754b59a11cf1
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: ht
source-wordcount: '765'
ht-degree: 100%

---

# ホストエンドポイント

>[!NOTE]
>
>このドキュメントでは、Reactor API でホストを管理する方法について説明します。タグのホストの一般的な情報について詳しくは、公開ドキュメントの [ホストの概要](../../ui/publishing/hosts/hosts-overview.md) に関するガイドを参照してください。

Reactor API では、ホストが[ビルド](./builds.md)を配信できる宛先を定義します。

Adobe Experience Platform のタグユーザーがビルドを要求すると、システムはライブラリをチェックし、ライブラリをビルドする[環境](./environments.md)を決定します。各環境はホストとの関係があり、ビルドの配信先を示します。

ホストは 1 つの[プロパティ](./properties.md)のみに属しますが、プロパティは複数のホストを持つことができます。公開するには、プロパティに 1 つ以上のホストが必要です。

ホストは、プロパティ内の複数の環境で使用できます。プロパティに単一のホストがあり、そのプロパティのすべての環境で同じホストを使用するのが一般的です。

## はじめに

このガイドで使用するエンドポイントは、[Reactor API](https://www.adobe.io/experience-platform-apis/references/reactor/) の一部です。続行する前に、API への認証方法に関する重要な情報について、[はじめる前に](../getting-started.md)を確認してください。

## ホストのリストの取得 {#list}

GET リクエストのパスにプロパティの ID を含めることで、プロパティのホストのリストを取得できます。

**API 形式**

```http
GET /properties/{PROPERTY_ID}/hosts
```

| パラメーター | 説明 |
| --- | --- |
| `PROPERTY_ID` | ホストを所有するプロパティの `id`。 |

{style=&quot;table-layout:auto&quot;}

>[!NOTE]
>
>クエリーパラメーターを使用して、リストされているホストを次の属性に基づいてフィルタリングできます。<ul><li>`created_at`</li><li>`name`</li><li>`type_of`</li><li>`updated_at`</li></ul>詳しくは、 [応答のフィルタリング](../guides/filtering.md) に関するガイドを参照してください。

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/properties/PRd428c2a25caa4b32af61495f5809b737/hosts \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、指定されたプロパティのホストのリストが返されます。

```json
{
  "data": [
    {
      "id": "HT405b8d9306004eb38106e66c8a4afc09",
      "type": "hosts",
      "attributes": {
        "created_at": "2020-12-14T17:42:35.239Z",
        "server": null,
        "name": "Example Akamai Host",
        "path": null,
        "port": null,
        "status": "succeeded",
        "type_of": "akamai",
        "updated_at": "2020-12-14T17:42:35.239Z",
        "username": null
      },
      "relationships": {
        "property": {
          "links": {
            "related": "https://reactor.adobe.io/hosts/HT405b8d9306004eb38106e66c8a4afc09/property"
          },
          "data": {
            "id": "PRd428c2a25caa4b32af61495f5809b737",
            "type": "properties"
          }
        }
      },
      "links": {
        "property": "https://reactor.adobe.io/properties/PRd428c2a25caa4b32af61495f5809b737",
        "self": "https://reactor.adobe.io/hosts/HT405b8d9306004eb38106e66c8a4afc09"
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

## ホストの検索 {#lookup}

GET リクエストのパスで ID を指定することで、ホストを検索できます。

**API 形式**

```http
GET /hosts/{HOST_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `HOST_ID` | 検索するホストの `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/hosts/HT5d90148e72224224aac9bc0b01498b84 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、ホストの詳細が返されます。

```json
{
  "data": {
    "id": "HT5d90148e72224224aac9bc0b01498b84",
    "type": "hosts",
    "attributes": {
      "created_at": "2020-12-14T17:42:25.353Z",
      "server": "https://server.example.com",
      "name": "Example Akamai Host",
      "path": "/akamai",
      "port": 8000,
      "status": "succeeded",
      "type_of": "akamai",
      "updated_at": "2020-12-14T17:42:25.353Z",
      "username": null
    },
    "relationships": {
      "property": {
        "links": {
          "related": "https://reactor.adobe.io/hosts/HT5d90148e72224224aac9bc0b01498b84/property"
        },
        "data": {
          "id": "PRd7cf174259f34057b5c435ef873a79bf",
          "type": "properties"
        }
      }
    },
    "links": {
      "property": "https://reactor.adobe.io/properties/PRd7cf174259f34057b5c435ef873a79bf",
      "self": "https://reactor.adobe.io/hosts/HT5d90148e72224224aac9bc0b01498b84"
    }
  }
}
```

## ホストの作成 {#create}

POST リクエストをおこなうことで、新しいホストを作成できます。

**API 形式**

```http
POST /properties/{PROPERTY_ID}/hosts
```

| パラメーター | 説明 |
| --- | --- |
| `PROPERTY_ID` | ホストを定義している[プロパティ](./properties.md)の `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、指定されたプロパティの新しいホストを作成します。また、この呼び出しは、 `relationships` プロパティを通じてホストを既存の拡張機能と関連付けます。 詳しくは、 [関係](../guides/relationships.md) に関するガイドを参照してください。

```shell
curl -X POST \
  https://reactor.adobe.io/properties/PRb25a704c0b7c4562835ccdf96d3afd31/hosts \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Content-Type: application/json' \
  -d '{
        "data": {
          "attributes": {
            "name": "Example SFTP Host",
            "type_of": "sftp",
            "username": "John Doe",
            "encrypted_private_key": "{PRIVATE_KEY}",
            "server": "https://example.com",
            "path": "assets",
            "port": 22
          },
          "type": "hosts"
        }
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `attributes.name` | **（必須）**&#x200B;人間が判読できるホスト名。 |
| `attributes.type_of` | **（必須）**&#x200B;ホストのタイプ。次の 2 つのオプションのいずれかを指定できます。 <ul><li>`akamai`[アドビが管理するホスト](../../ui/publishing/hosts/managed-by-adobe-host.md)の場合  </li><li>`sftp`：[SFTP ホスト](../../ui/publishing/hosts/sftp-host.md)の場合 </li></ul> |
| `attributes.encrypted_private_key` | ホスト認証に使用するオプションの秘密鍵。 |
| `attributes.path` | `server` URL に追加するパス。 |
| `attributes.port` | 使用する特定のサーバーポートを示す整数。 |
| `attributes.server` | サーバーのホスト URL。 |
| `attributes.username` | 認証用のオプションのユーザー名。 |
| `type` | 更新するリソースのタイプ。 このエンドポイントの場合は、値を `hosts` にする必要があります。 |

{style=&quot;table-layout:auto&quot;}

**応答**

応答が成功すると、新しく作成されたホストの詳細が返されます。

```json
{
  "data": {
    "id": "HT69bfe634dead4a9a8c659f5d4d94cecd",
    "type": "hosts",
    "attributes": {
      "created_at": "2020-12-14T17:42:07.033Z",
      "server": "//example.com",
      "name": "Example SFTP Host",
      "path": "assets",
      "port": 22,
      "status": "pending",
      "type_of": "sftp",
      "updated_at": "2020-12-14T17:42:07.033Z",
      "username": "John Doe"
    },
    "relationships": {
      "property": {
        "links": {
          "related": "https://reactor.adobe.io/hosts/HT69bfe634dead4a9a8c659f5d4d94cecd/property"
        },
        "data": {
          "id": "PRb25a704c0b7c4562835ccdf96d3afd31",
          "type": "properties"
        }
      }
    },
    "links": {
      "property": "https://reactor.adobe.io/properties/PRb25a704c0b7c4562835ccdf96d3afd31",
      "self": "https://reactor.adobe.io/hosts/HT69bfe634dead4a9a8c659f5d4d94cecd"
    }
  }
}
```

## ホストの更新 {#update}

>[!NOTE]
>
>SFTP ホストのみを更新できます。

PATCH リクエストのパスに ID を含めることで、ホストを更新できます。

**API 形式**

```http
PATCH /hosts/{HOST_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `HOST_ID` | 更新するホストの `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、既存のホストの `name` を更新します。

```shell
curl -X PATCH \
  https://reactor.adobe.io/hosts/HT5d90148e72224224aac9bc0b01498b84 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Content-Type: application/json' \
  -d '{
        "data": {
          "attributes": {
            "name": "New host Name"
          },
          "id": "HT5d90148e72224224aac9bc0b01498b84",
          "type": "hosts"
        }
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `attributes` | プロパティがホストの更新される属性を表すオブジェクト。ホストに対して更新できる属性は次のとおりです。 <ul><li>`encrypted_private_key`</li><li>`name`</li><li>`path`</li><li>`port`</li><li>`server`</li><li>`type_of`</li><li>`username`</li></ul> |
| `id` | 更新するホストの `id` 。この値は、リクエストパスで指定された `{HOST_ID}` 値と一致する必要があります。 |
| `type` | 更新するリソースのタイプ。 このエンドポイントの場合は、値を `hosts` にする必要があります。 |

{style=&quot;table-layout:auto&quot;}

**応答**

応答が成功すると、更新されたホストの詳細が返されます。

```json
{
  "data": {
    "id": "HTb14e136a6fe147459b298a4645d2a6f5",
    "type": "hosts",
    "attributes": {
      "created_at": "2020-12-14T17:42:45.087Z",
      "server": null,
      "name": "My SFTP host",
      "path": null,
      "port": null,
      "status": "succeeded",
      "type_of": "sftp",
      "updated_at": "2020-12-14T17:42:45.696Z",
      "username": null
    },
    "relationships": {
      "property": {
        "links": {
          "related": "https://reactor.adobe.io/hosts/HTb14e136a6fe147459b298a4645d2a6f5/property"
        },
        "data": {
          "id": "PR8f240526f7b54a4dbd46965e79519fde",
          "type": "properties"
        }
      }
    },
    "links": {
      "property": "https://reactor.adobe.io/properties/PR8f240526f7b54a4dbd46965e79519fde",
      "self": "https://reactor.adobe.io/hosts/HTb14e136a6fe147459b298a4645d2a6f5"
    }
  }
}
```

## ホストの削除

DELETE リクエストのパスに ID を含めることで、ホストを削除できます。

**API 形式**

```http
DELETE /hosts/{HOST_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `HOST_ID` | 削除するホストの `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X DELETE \
  https://reactor.adobe.io/hosts/HT5d90148e72224224aac9bc0b01498b84 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
```

**応答**

応答が成功すると、応答本文なしで HTTP ステータス 204（コンテンツなし）が返され、ホストが削除されたことを示します。

## ホストの関連リソースの取得 {#related}

次の呼び出しは、ホストの関連リソースを取得する方法を示しています。 [ホストを検索](#lookup)すると、これらの関係が `relationships` プロパティの下に表示されます。

Reactor API の関係について詳しくは、 [関係に関するガイド](../guides/relationships.md) を参照してください。

### ホストの関連プロパティの検索 {#property}

検索リクエストのパスに `/property` を追加することで、ホストを所有するプロパティを検索できます。

**API 形式**

```http
GET /hosts/{HOST_ID}/property
```

| パラメーター | 説明 |
| --- | --- |
| `{HOST_ID}` | プロパティを検索するホストの `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/hosts/HT5d90148e72224224aac9bc0b01498b84/property \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、指定したホストのプロパティの詳細が返されます。

```json
{
  "data": {
    "id": "PRbdfaffb7bf374b87be50e672f0cf9309",
    "type": "properties",
    "attributes": {
      "created_at": "2020-12-14T17:52:05.900Z",
      "enabled": true,
      "name": "Kessel Example Property",
      "updated_at": "2020-12-14T17:52:05.900Z",
      "platform": "web",
      "development": false,
      "token": "47d65c7c98db",
      "domains": [
        "example.com"
      ],
      "undefined_vars_return_empty": false,
      "rule_component_sequencing_enabled": false
    },
    "relationships": {
      "company": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PRbdfaffb7bf374b87be50e672f0cf9309/company"
        },
        "data": {
          "id": "CO2bf094214ffd4785bb4bcf88c952a7c1",
          "type": "companies"
        }
      },
      "callbacks": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PRbdfaffb7bf374b87be50e672f0cf9309/callbacks"
        }
      },
      "hosts": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PRbdfaffb7bf374b87be50e672f0cf9309/hosts"
        }
      },
      "environments": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PRbdfaffb7bf374b87be50e672f0cf9309/environments"
        }
      },
      "libraries": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PRbdfaffb7bf374b87be50e672f0cf9309/libraries"
        }
      },
      "data_elements": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PRbdfaffb7bf374b87be50e672f0cf9309/data_elements"
        }
      },
      "extensions": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PRbdfaffb7bf374b87be50e672f0cf9309/extensions"
        }
      },
      "rules": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PRbdfaffb7bf374b87be50e672f0cf9309/rules"
        }
      },
      "notes": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PRbdfaffb7bf374b87be50e672f0cf9309/notes"
        }
      }
    },
    "links": {
      "company": "https://reactor.adobe.io/companies/CO2bf094214ffd4785bb4bcf88c952a7c1",
      "data_elements": "https://reactor.adobe.io/properties/PRbdfaffb7bf374b87be50e672f0cf9309/data_elements",
      "environments": "https://reactor.adobe.io/properties/PRbdfaffb7bf374b87be50e672f0cf9309/environments",
      "extensions": "https://reactor.adobe.io/properties/PRbdfaffb7bf374b87be50e672f0cf9309/extensions",
      "rules": "https://reactor.adobe.io/properties/PRbdfaffb7bf374b87be50e672f0cf9309/rules",
      "self": "https://reactor.adobe.io/properties/PRbdfaffb7bf374b87be50e672f0cf9309"
    },
    "meta": {
      "rights": [
        "approve",
        "develop",
        "manage_environments",
        "manage_extensions",
        "publish"
      ]
    }
  }
}
```
