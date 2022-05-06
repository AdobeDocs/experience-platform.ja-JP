---
title: 拡張機能エンドポイント
description: Reactor API で /extensions エンドポイントを呼び出す方法を説明します。
exl-id: cc02b2aa-d107-463a-930c-5a9fcc5b4a5a
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '977'
ht-degree: 100%

---

# 拡張機能エンドポイント

Reactor API では、拡張機能は[拡張機能パッケージ](./extension-packages.md)のインストール済みインスタンスを表します。拡張機能は、拡張機能パッケージで定義された機能を[プロパティ](./properties.md)で使用できるようにします。 これらの機能は、[拡張機能](./data-elements.md)と[ルールコンポーネント](./rule-components.md)を作成する際に利用されます。

拡張機能は、1 つのプロパティのみに属します。 プロパティには多くの拡張機能を含めることができますが、特定の拡張機能パッケージにインストールされているインスタンスは 1 つだけです。

## はじめに

このガイドで使用するエンドポイントは、[Reactor API](https://www.adobe.io/experience-platform-apis/references/reactor/) の一部です。続行する前に、API への認証方法に関する重要な情報について、[はじめる前に](../getting-started.md)を確認してください。

## 拡張機能のリストの取得 {#list}

GET リクエストをおこなうことで、プロパティの拡張機能のリストを取得できます。

**API 形式**

```http
GET properties/{PROPERTY_ID}/extensions
```

| パラメーター | 説明 |
| --- | --- |
| `{PROPERTY_ID}` | 拡張機能のリストを取得するプロパティの `id`。 |

{style=&quot;table-layout:auto&quot;}

>[!NOTE]
>
>クエリパラメーターを使用して、リストに表示された拡張機能を、次の属性に基づいてフィルタリングできます。<ul><li>`created_at`</li><li>`dirty`</li><li>`display_name`</li><li>`enabled`</li><li>`name`</li><li>`origin_id`</li><li>`published`</li><li>`published_at`</li><li>`revision_number`</li><li>`updated_at`</li><li>`version`</li></ul>詳しくは、 [応答のフィルタリング](../guides/filtering.md) に関するガイドを参照してください。

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/properties/PRee071cb5b7794f42b74c913e1ad2e325/extensions \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、指定したプロパティで定義されている拡張機能のリストが返されます。

```json
{
  "data": [
    {
      "id": "EXd9d80c87afb6432ba823a58d3e78299b",
      "type": "extensions",
      "attributes": {
        "created_at": "2020-12-14T17:40:21.000Z",
        "deleted_at": null,
        "dirty": false,
        "enabled": true,
        "name": "kessel-test",
        "published": false,
        "published_at": null,
        "revision_number": 0,
        "updated_at": "2020-12-14T17:40:21.000Z",
        "delegate_descriptor_id": null,
        "display_name": "Kessel Test",
        "review_status": "unsubmitted",
        "version": "1.2.0",
        "settings": "{}"
      },
      "relationships": {
        "libraries": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXd9d80c87afb6432ba823a58d3e78299b/libraries"
          }
        },
        "revisions": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXd9d80c87afb6432ba823a58d3e78299b/revisions"
          }
        },
        "notes": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXd9d80c87afb6432ba823a58d3e78299b/notes"
          }
        },
        "property": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXd9d80c87afb6432ba823a58d3e78299b/property"
          },
          "data": {
            "id": "PRee071cb5b7794f42b74c913e1ad2e325",
            "type": "properties"
          }
        },
        "origin": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXd9d80c87afb6432ba823a58d3e78299b/origin"
          },
          "data": {
            "id": "EXd9d80c87afb6432ba823a58d3e78299b",
            "type": "extensions"
          }
        },
        "updated_with_extension_package": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXd9d80c87afb6432ba823a58d3e78299b/updated_with_extension_package"
          },
          "data": {
            "id": "EP75db2452065b44e2b8a38ca883ce369a",
            "type": "extension_packages"
          }
        },
        "extension_package": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXd9d80c87afb6432ba823a58d3e78299b/extension_package"
          },
          "data": {
            "id": "EP75db2452065b44e2b8a38ca883ce369a",
            "type": "extension_packages"
          }
        }
      },
      "links": {
        "property": "https://reactor.adobe.io/properties/PRee071cb5b7794f42b74c913e1ad2e325",
        "origin": "https://reactor.adobe.io/extensions/EXd9d80c87afb6432ba823a58d3e78299b",
        "self": "https://reactor.adobe.io/extensions/EXd9d80c87afb6432ba823a58d3e78299b",
        "extension_package": "https://reactor.adobe.io/extension_packages/EP75db2452065b44e2b8a38ca883ce369a",
        "latest_extension_package": "https://reactor.adobe.io/extension_packages/EP75db2452065b44e2b8a38ca883ce369a"
      },
      "meta": {
        "latest_revision_number": 1
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

## 拡張機能の検索 {#lookup}

GET リクエストのパスで ID を指定することで、拡張機能を検索できます。

>[!NOTE]
>
>拡張機能が削除されると、システムでは削除済みとフラグが付けられますが、実際には削除されません。したがって、削除された拡張機能を取得できます。 削除された拡張機能は、返された拡張機能データの `meta` に `deleted_at` プロパティの存在によって識別できます。

**API 形式**

```http
GET /extensions/{EXTENSION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `EXTENSION_ID` | 検索する拡張機能の `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/extensions/EX2ba586f436ac48e390a1ee7e8c9a8f6e \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、拡張機能の詳細が返されます。

```json
{
  "data": {
    "id": "EX2ba586f436ac48e390a1ee7e8c9a8f6e",
    "type": "extensions",
    "attributes": {
      "created_at": "2020-12-14T17:40:09.823Z",
      "deleted_at": null,
      "dirty": false,
      "enabled": true,
      "name": "kessel-test",
      "published": false,
      "published_at": null,
      "revision_number": 0,
      "updated_at": "2020-12-14T17:40:09.823Z",
      "delegate_descriptor_id": null,
      "display_name": "Kessel Test",
      "review_status": "unsubmitted",
      "version": "1.2.0",
      "settings": "{}"
    },
    "relationships": {
      "libraries": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX2ba586f436ac48e390a1ee7e8c9a8f6e/libraries"
        }
      },
      "revisions": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX2ba586f436ac48e390a1ee7e8c9a8f6e/revisions"
        }
      },
      "notes": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX2ba586f436ac48e390a1ee7e8c9a8f6e/notes"
        }
      },
      "property": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX2ba586f436ac48e390a1ee7e8c9a8f6e/property"
        },
        "data": {
          "id": "PR2254ac6a096c4df38508700093d3e153",
          "type": "properties"
        }
      },
      "origin": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX2ba586f436ac48e390a1ee7e8c9a8f6e/origin"
        },
        "data": {
          "id": "EX2ba586f436ac48e390a1ee7e8c9a8f6e",
          "type": "extensions"
        }
      },
      "updated_with_extension_package": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX2ba586f436ac48e390a1ee7e8c9a8f6e/updated_with_extension_package"
        },
        "data": {
          "id": "EP75db2452065b44e2b8a38ca883ce369a",
          "type": "extension_packages"
        }
      },
      "extension_package": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX2ba586f436ac48e390a1ee7e8c9a8f6e/extension_package"
        },
        "data": {
          "id": "EP75db2452065b44e2b8a38ca883ce369a",
          "type": "extension_packages"
        }
      }
    },
    "links": {
      "property": "https://reactor.adobe.io/properties/PR2254ac6a096c4df38508700093d3e153",
      "origin": "https://reactor.adobe.io/extensions/EX2ba586f436ac48e390a1ee7e8c9a8f6e",
      "self": "https://reactor.adobe.io/extensions/EX2ba586f436ac48e390a1ee7e8c9a8f6e",
      "extension_package": "https://reactor.adobe.io/extension_packages/EP75db2452065b44e2b8a38ca883ce369a",
      "latest_extension_package": "https://reactor.adobe.io/extension_packages/EP75db2452065b44e2b8a38ca883ce369a"
    },
    "meta": {
      "latest_revision_number": 1
    }
  }
}
```

## 拡張機能の作成または更新 {#create}

拡張機能は、[拡張機能パッケージ](./extension-packages.md)を参照し、インストールされた拡張機能をプロパティに追加することで作成されます。インストールタスクが完了すると、拡張機能が正常にインストールされたかどうかを示す応答が返されます。

**API 形式**

```http
POST /properties/{PROPERTY_ID}/extensions
```

| パラメーター | 説明 |
| --- | --- |
| `PROPERTY_ID` | 拡張機能をインストールするプロパティの `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X POST \
  https://reactor.adobe.io/properties/PRee071cb5b7794f42b74c913e1ad2e325/extensions \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
        "data": {
          "attributes": {
            "delegate_descriptor_id": "example-package::extensionConfiguration::config",
            "enabled": true,
            "settings": "{\"elementProperty\":\"html\",\"elementSelector\":\".target-element\"}"
          },
          "relationships": {
            "extension_package": {
              "data": {
                "id": "EP75db2452065b44e2b8a38ca883ce369a",
                "type": "extension_packages"
              }
            }
          },
          "type": "extensions"
        }
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `relationships.extension_package` | **（必須）**&#x200B;インストールする拡張機能パッケージの ID を参照するオブジェクト。 |
| `attributes.delegate_descriptor_id` | 拡張機能にカスタム設定が必要な場合は、デリゲート記述子 ID も必要です。詳しくは、 [デリゲート記述子 ID](../guides/delegate-descriptor-ids.md) に関するガイドを参照してください。 |
| `attributes.enabled` | 拡張機能が有効かどうかを示すブール値。 |
| `attributes.settings` | 文字列として表される設定 JSON オブジェクト。 |

{style=&quot;table-layout:auto&quot;}

**応答**

応答が成功すると、新しく作成された拡張機能の詳細が返されます。

```json
{
  "data": {
    "id": "EX8ce7ced633f34bd48d33089ff8fad082",
    "type": "extensions",
    "attributes": {
      "created_at": "2020-12-14T17:32:56.137Z",
      "deleted_at": null,
      "dirty": false,
      "enabled": true,
      "name": "kessel-test",
      "published": false,
      "published_at": null,
      "revision_number": 0,
      "updated_at": "2020-12-14T17:32:56.137Z",
      "delegate_descriptor_id": "example-package::extensionConfiguration::config",
      "display_name": "Kessel Test",
      "review_status": "unsubmitted",
      "version": "1.2.0",
      "settings": "{\"elementProperty\":\"html\",\"elementSelector\":\".target-element\"}"
    },
    "relationships": {
      "libraries": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082/libraries"
        }
      },
      "revisions": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082/revisions"
        }
      },
      "notes": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082/notes"
        }
      },
      "property": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082/property"
        },
        "data": {
          "id": "PRcf1f3e4c218b4caab8191fab003a8355",
          "type": "properties"
        }
      },
      "origin": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082/origin"
        },
        "data": {
          "id": "EX8ce7ced633f34bd48d33089ff8fad082",
          "type": "extensions"
        }
      },
      "updated_with_extension_package": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082/updated_with_extension_package"
        },
        "data": {
          "id": "EP75db2452065b44e2b8a38ca883ce369a",
          "type": "extension_packages"
        }
      },
      "extension_package": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082/extension_package"
        },
        "data": {
          "id": "EP75db2452065b44e2b8a38ca883ce369a",
          "type": "extension_packages"
        }
      }
    },
    "links": {
      "property": "https://reactor.adobe.io/properties/PRcf1f3e4c218b4caab8191fab003a8355",
      "origin": "https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082",
      "self": "https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082",
      "extension_package": "https://reactor.adobe.io/extension_packages/EP75db2452065b44e2b8a38ca883ce369a",
      "latest_extension_package": "https://reactor.adobe.io/extension_packages/EP75db2452065b44e2b8a38ca883ce369a"
    },
    "meta": {
      "latest_revision_number": 1
    }
  }
}
```

## 拡張機能の改訂 {#revise}

PATCH リクエストのパスに ID を含めることで、拡張機能を改訂できます。

**API 形式**

```http
PATCH /extensions/{EXTENSION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `EXTENSION_ID` | 改訂する拡張機能の `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

[拡張機能の作成](#create)と同様に、改訂されたパッケージのローカルバージョンをフォームデータを介してアップロードする必要があります。

```shell
curl -X PATCH \
  https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
        "data": {
          "attributes": {
            "enabled": false,
          },
          "meta": {
            "action": "revise"
          },
          "id": "EX85509bc7baea4f8599bc44024ed3f110",
          "type": "extensions"
        }
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `attributes` | 改訂する属性。 拡張機能の場合は、 `delegate_descriptor_id`、 `enabled` および `settings` 属性を変更できます。 |
| `meta.action` | リビジョンを作成する場合は、`revise` の値を含める必要があります。 |

{style=&quot;table-layout:auto&quot;}

**応答**

応答が成功すると、増えた改訂後の拡張機能の詳細が返され、`meta.latest_revision_number` プロパティの値が 1 増分されます。

```json
{
  "data": {
    "id": "EX8ce7ced633f34bd48d33089ff8fad082",
    "type": "extensions",
    "attributes": {
      "created_at": "2020-12-14T17:32:56.137Z",
      "deleted_at": null,
      "dirty": false,
      "enabled": false,
      "name": "kessel-test",
      "published": false,
      "published_at": null,
      "revision_number": 0,
      "updated_at": "2020-12-14T17:32:56.137Z",
      "delegate_descriptor_id": "example-package::extensionConfiguration::config",
      "display_name": "Kessel Test",
      "review_status": "unsubmitted",
      "version": "1.2.0",
      "settings": "{\"elementProperty\":\"html\",\"elementSelector\":\".target-element\"}"
    },
    "relationships": {
      "libraries": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082/libraries"
        }
      },
      "revisions": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082/revisions"
        }
      },
      "notes": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082/notes"
        }
      },
      "property": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082/property"
        },
        "data": {
          "id": "PRcf1f3e4c218b4caab8191fab003a8355",
          "type": "properties"
        }
      },
      "origin": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082/origin"
        },
        "data": {
          "id": "EX8ce7ced633f34bd48d33089ff8fad082",
          "type": "extensions"
        }
      },
      "updated_with_extension_package": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082/updated_with_extension_package"
        },
        "data": {
          "id": "EP75db2452065b44e2b8a38ca883ce369a",
          "type": "extension_packages"
        }
      },
      "extension_package": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082/extension_package"
        },
        "data": {
          "id": "EP75db2452065b44e2b8a38ca883ce369a",
          "type": "extension_packages"
        }
      }
    },
    "links": {
      "property": "https://reactor.adobe.io/properties/PRcf1f3e4c218b4caab8191fab003a8355",
      "origin": "https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082",
      "self": "https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082",
      "extension_package": "https://reactor.adobe.io/extension_packages/EP75db2452065b44e2b8a38ca883ce369a",
      "latest_extension_package": "https://reactor.adobe.io/extension_packages/EP75db2452065b44e2b8a38ca883ce369a"
    },
    "meta": {
      "latest_revision_number": 2
    }
  }
}
```

## 拡張機能の削除 {#private-release}

DELETE リクエストのパスに ID を含めることで、拡張機能を削除できます。

**API 形式**

```http
DELETE /extensions/{EXTENSION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `EXTENSION_ID` | 削除する拡張機能の `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X DELETE \
  https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}'
```

**応答**

応答が成功すると、応答本文なしで HTTP ステータス 204（コンテンツなし）が返され、拡張機能が削除されたことを示します。

## 拡張機能のメモの管理 {#notes}

拡張機能は「メモ機能のある」リソースのため、個々のリソースでテキストベースのメモを作成し、取得することができます。拡張機能や他の互換性のあるリソースのメモを管理する方法について詳しくは、 [メモエンドポイントガイド](./notes.md) を参照してください。

## 拡張機能の関連リソースの取得 {#related}

次の呼び出しは、拡張機能の関連リソースを取得する方法を示しています。 [拡張機能を検索](#lookup)すると、これらの関係は `relationships` プロパティの下にリストされます。

Reactor API の関係について詳しくは、 [関係に関するガイド](../guides/relationships.md) を参照してください。

### 拡張機能に関連するライブラリのリスト {#libraries}

検索リクエストのパスに `/libraries` を追加すると、拡張機能を利用するライブラリをリストできます。

**API 形式**

```http
GET  /extensions/{EXTENSION_ID}/libraries
```

| パラメーター | 説明 |
| --- | --- |
| `{EXTENSION_ID}` | ライブラリのリストを取得する拡張機能の `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082/libraries \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、指定した拡張機能を使用しているライブラリのリストが返されます。

```json
{
  "data": [
    {
      "id": "LB62d20ad807a949e6b13b0a2c7299eb65",
      "type": "libraries",
      "attributes": {
        "created_at": "2020-12-14T17:50:19.589Z",
        "name": "My Library",
        "published_at": null,
        "state": "development",
        "updated_at": "2020-12-14T17:50:19.589Z",
        "build_required": true
      },
      "relationships": {
        "builds": {
          "links": {
            "related": "https://reactor.adobe.io/libraries/LB62d20ad807a949e6b13b0a2c7299eb65/builds"
          }
        },
        "environment": {
          "links": {
            "self": "https://reactor.adobe.io/libraries/LB62d20ad807a949e6b13b0a2c7299eb65/relationships/environment"
          },
          "data": null
        },
        "data_elements": {
          "links": {
            "related": "https://reactor.adobe.io/libraries/LB62d20ad807a949e6b13b0a2c7299eb65/data_elements",
            "self": "https://reactor.adobe.io/libraries/LB62d20ad807a949e6b13b0a2c7299eb65/relationships/data_elements"
          }
        },
        "extensions": {
          "links": {
            "related": "https://reactor.adobe.io/libraries/LB62d20ad807a949e6b13b0a2c7299eb65/extensions",
            "self": "https://reactor.adobe.io/libraries/LB62d20ad807a949e6b13b0a2c7299eb65/relationships/extensions"
          }
        },
        "notes": {
          "links": {
            "related": "https://reactor.adobe.io/libraries/LB62d20ad807a949e6b13b0a2c7299eb65/notes"
          }
        },
        "rules": {
          "links": {
            "related": "https://reactor.adobe.io/libraries/LB62d20ad807a949e6b13b0a2c7299eb65/rules",
            "self": "https://reactor.adobe.io/libraries/LB62d20ad807a949e6b13b0a2c7299eb65/relationships/rules"
          }
        },
        "upstream_library": {
          "data": null
        },
        "property": {
          "links": {
            "related": "https://reactor.adobe.io/libraries/LB62d20ad807a949e6b13b0a2c7299eb65/property"
          },
          "data": {
            "id": "PR241ba9cd56324ac192de68d658f20cb0",
            "type": "properties"
          }
        },
        "last_build": {
          "links": {
            "related": "https://reactor.adobe.io/libraries/LB62d20ad807a949e6b13b0a2c7299eb65/last_build"
          },
          "data": null
        }
      },
      "links": {
        "property": "https://reactor.adobe.io/properties/PR241ba9cd56324ac192de68d658f20cb0",
        "self": "https://reactor.adobe.io/libraries/LB62d20ad807a949e6b13b0a2c7299eb65"
      },
      "meta": {
        "build_status": null,
        "build_required_detail": "No build found since last state change"
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

### 拡張機能に関連するリビジョンのリスト {#revisions}

検索リクエストのパスに `/revisions` を追加すると、拡張機能の以前のリビジョンをリストできます。

**API 形式**

```http
GET  /extensions/{EXTENSION_ID}/revisions
```

| パラメーター | 説明 |
| --- | --- |
| `{EXTENSION_ID}` | リビジョンのリストを取得する拡張機能の `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082/revisions \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、指定した拡張機能のリビジョンのリストが返されます。

```json
{
  "data": [
    {
      "id": "EXbfcca9f3ce9d40318b9115159a951e09",
      "type": "extensions",
      "attributes": {
        "created_at": "2020-12-14T17:41:09.023Z",
        "deleted_at": null,
        "dirty": false,
        "enabled": true,
        "name": "kessel-test",
        "published": false,
        "published_at": null,
        "revision_number": 1,
        "updated_at": "2020-12-14T17:41:09.023Z",
        "delegate_descriptor_id": null,
        "display_name": "Kessel Test",
        "review_status": "unsubmitted",
        "version": "1.2.0",
        "settings": "{}"
      },
      "relationships": {
        "libraries": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXbfcca9f3ce9d40318b9115159a951e09/libraries"
          }
        },
        "revisions": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXbfcca9f3ce9d40318b9115159a951e09/revisions"
          }
        },
        "notes": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXbfcca9f3ce9d40318b9115159a951e09/notes"
          }
        },
        "property": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXbfcca9f3ce9d40318b9115159a951e09/property"
          },
          "data": {
            "id": "PR92de152ae31e48a692142ea65c1efeb9",
            "type": "properties"
          }
        },
        "origin": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXbfcca9f3ce9d40318b9115159a951e09/origin"
          },
          "data": {
            "id": "EXd485e07fb3d3429b997768ae40de8f02",
            "type": "extensions"
          }
        },
        "updated_with_extension_package": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXbfcca9f3ce9d40318b9115159a951e09/updated_with_extension_package"
          },
          "data": {
            "id": "EP75db2452065b44e2b8a38ca883ce369a",
            "type": "extension_packages"
          }
        },
        "extension_package": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXbfcca9f3ce9d40318b9115159a951e09/extension_package"
          },
          "data": {
            "id": "EP75db2452065b44e2b8a38ca883ce369a",
            "type": "extension_packages"
          }
        }
      },
      "links": {
        "property": "https://reactor.adobe.io/properties/PR92de152ae31e48a692142ea65c1efeb9",
        "origin": "https://reactor.adobe.io/extensions/EXd485e07fb3d3429b997768ae40de8f02",
        "self": "https://reactor.adobe.io/extensions/EXbfcca9f3ce9d40318b9115159a951e09",
        "extension_package": "https://reactor.adobe.io/extension_packages/EP75db2452065b44e2b8a38ca883ce369a",
        "latest_extension_package": "https://reactor.adobe.io/extension_packages/EP75db2452065b44e2b8a38ca883ce369a"
      },
      "meta": {
        "latest_revision_number": 1
      }
    },
    {
      "id": "EXd485e07fb3d3429b997768ae40de8f02",
      "type": "extensions",
      "attributes": {
        "created_at": "2020-12-14T17:41:09.002Z",
        "deleted_at": null,
        "dirty": false,
        "enabled": true,
        "name": "kessel-test",
        "published": false,
        "published_at": null,
        "revision_number": 0,
        "updated_at": "2020-12-14T17:41:09.002Z",
        "delegate_descriptor_id": null,
        "display_name": "Kessel Test",
        "review_status": "unsubmitted",
        "version": "1.2.0",
        "settings": "{}"
      },
      "relationships": {
        "libraries": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXd485e07fb3d3429b997768ae40de8f02/libraries"
          }
        },
        "revisions": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXd485e07fb3d3429b997768ae40de8f02/revisions"
          }
        },
        "notes": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXd485e07fb3d3429b997768ae40de8f02/notes"
          }
        },
        "property": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXd485e07fb3d3429b997768ae40de8f02/property"
          },
          "data": {
            "id": "PR92de152ae31e48a692142ea65c1efeb9",
            "type": "properties"
          }
        },
        "origin": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXd485e07fb3d3429b997768ae40de8f02/origin"
          },
          "data": {
            "id": "EXd485e07fb3d3429b997768ae40de8f02",
            "type": "extensions"
          }
        },
        "updated_with_extension_package": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXd485e07fb3d3429b997768ae40de8f02/updated_with_extension_package"
          },
          "data": {
            "id": "EP75db2452065b44e2b8a38ca883ce369a",
            "type": "extension_packages"
          }
        },
        "extension_package": {
          "links": {
            "related": "https://reactor.adobe.io/extensions/EXd485e07fb3d3429b997768ae40de8f02/extension_package"
          },
          "data": {
            "id": "EP75db2452065b44e2b8a38ca883ce369a",
            "type": "extension_packages"
          }
        }
      },
      "links": {
        "property": "https://reactor.adobe.io/properties/PR92de152ae31e48a692142ea65c1efeb9",
        "origin": "https://reactor.adobe.io/extensions/EXd485e07fb3d3429b997768ae40de8f02",
        "self": "https://reactor.adobe.io/extensions/EXd485e07fb3d3429b997768ae40de8f02",
        "extension_package": "https://reactor.adobe.io/extension_packages/EP75db2452065b44e2b8a38ca883ce369a",
        "latest_extension_package": "https://reactor.adobe.io/extension_packages/EP75db2452065b44e2b8a38ca883ce369a"
      },
      "meta": {
        "latest_revision_number": 1
      }
    }
  ],
  "meta": {
    "pagination": {
      "current_page": 1,
      "next_page": null,
      "prev_page": null,
      "total_pages": 1,
      "total_count": 2
    }
  }
}
```

### 拡張機能に関連する拡張機能パッケージを検索する {#extension}

GET リクエストのパスに `/extension_package` を追加することで、拡張機能のベースとなる拡張機能パッケージを検索できます。

**API 形式**

```http
GET  /extensions/{EXTENSION_ID}/extension_package
```

| パラメーター | 説明 |
| --- | --- |
| `{EXTENSION_ID}` | 検索する拡張機能の `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082/extension \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、指定された拡張機能がベースとなっている拡張機能パッケージの詳細が返されます。次の応答の例は、スペースを節約するために切り捨てられています。

```json
{
  "data": {
    "id": "EP75db2452065b44e2b8a38ca883ce369a",
    "type": "extension_packages",
    "attributes": {
      "actions": [
        {
          "id": "kessel-test::actions::custom-code",
          "name": "custom-code",
          "schema": {
            "type": "object",
            "oneOf": [
              {
                "required": [
                  "language",
                  "source"
                ],
                "properties": {
                  "global": {
                    "type": "boolean"
                  },
                  "source": {
                    "type": "string",
                    "minLength": 1
                  },
                  "language": {
                    "enum": [
                      "javascript"
                    ]
                  }
                },
                "additionalProperties": false
              },
              {
                "required": [
                  "language",
                  "source"
                ],
                "properties": {
                  "source": {
                    "type": "string",
                    "minLength": 1
                  },
                  "language": {
                    "enum": [
                      "html"
                    ]
                  }
                },
                "additionalProperties": false
              }
            ],
            "$schema": "http://json-schema.org/draft-04/schema#"
          },
          "libPath": "src/lib/actions/customCode.js",
          "viewPath": "actions/customCode.html",
          "displayName": "Custom Code"
        }
      ],
      "author": {
        "url": "http://adobe.com",
        "name": "Adobe Systems",
        "email": "reactor@adobe.com"
      },
      "availability": "private",
      "cdn_path": "https://assets.adobedtm.com/staging/extensions/EP75db2452065b44e2b8a38ca883ce369a",
      "conditions": [
        {
          "id": "kessel-test::conditions::browser",
          "name": "browser",
          "schema": {
            "type": "object",
            "$schema": "http://json-schema.org/draft-04/schema#",
            "required": [
              "browsers"
            ],
            "properties": {
              "browsers": {
                "type": "array",
                "items": {
                  "enum": [
                    "Chrome",
                    "Firefox",
                    "IE",
                    "Edge",
                    "Safari",
                    "Mobile Safari"
                  ],
                  "type": "string"
                }
              }
            },
            "additionalProperties": false
          },
          "libPath": "src/lib/conditions/browser.js",
          "viewPath": "conditions/browser.html",
          "displayName": "Browser",
          "categoryName": "Technology"
        }
      ],
      "configuration": null,
      "created_at": "2020-11-09T17:51:59.433Z",
      "data_elements": [
        {
          "id": "kessel-test::dataElements::cookie",
          "name": "cookie",
          "schema": {
            "type": "object",
            "$schema": "http://json-schema.org/draft-04/schema#",
            "required": [
              "name"
            ],
            "properties": {
              "name": {
                "type": "string",
                "minLength": 1
              }
            },
            "additionalProperties": false
          },
          "libPath": "src/lib/dataElements/cookie.js",
          "viewPath": "dataElements/cookie.html",
          "displayName": "Cookie"
        }
      ],
      "description": "Provides default event, condition, and data element types available to all tags users.",
      "discontinued": false,
      "display_name": "Kessel Test",
      "events": [
        {
          "id": "kessel-test::events::blur",
          "name": "blur",
          "schema": {
            "type": "object",
            "$schema": "http://json-schema.org/draft-04/schema#",
            "properties": {
              "bubbleStop": {
                "type": "boolean"
              },
              "elementSelector": {
                "type": "string",
                "minLength": 1
              },
              "elementProperties": {
                "type": "array",
                "items": {
                  "type": "object",
                  "required": [
                    "name",
                    "value"
                  ],
                  "properties": {
                    "name": {
                      "type": "string",
                      "minLength": 1
                    },
                    "value": {
                      "type": "string"
                    },
                    "valueIsRegex": {
                      "type": "boolean"
                    }
                  },
                  "additionalProperties": false
                }
              },
              "bubbleFireIfParent": {
                "type": "boolean"
              },
              "bubbleFireIfChildFired": {
                "type": "boolean"
              }
            },
            "additionalProperties": false
          },
          "libPath": "src/lib/events/blur.js",
          "viewPath": "events/blur.html",
          "displayName": "Blur",
          "categoryName": "Form"
        }
      ],
      "exchange_url": null,
      "hosted_lib_files": null,
      "icon_path": "resources/icons/core.svg",
      "main": null,
      "name": "kessel-test",
      "owner_org_id": "08364A825824E04F0A494115@AdobeOrg",
      "resources": null,
      "shared_modules": null,
      "status": "succeeded",
      "platform": "web",
      "updated_at": "2020-11-09T17:54:01.487Z",
      "version": "1.2.0",
      "view_base_path": "dist/"
    },
    "links": {
      "self": "https://reactor.adobe.io/extension_packages/EP75db2452065b44e2b8a38ca883ce369a"
    }
  }
}
```

### 拡張機能に関連するオリジンを検索する {#origin}

GET リクエストのパスに `/origin` を追加して、拡張機能のオリジンを調べることができます。拡張機能のオリジンは、現在のリビジョンを作成するために更新された以前のリビジョンです。

**API 形式**

```http
GET  /extensions/{EXTENSION_ID}/origin
```

| パラメーター | 説明 |
| --- | --- |
| `{EXTENSION_ID}` | オリジンを検索する拡張機能の `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082/origin \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、指定された拡張機能のオリジンの詳細が返されます。

```json
{
  "data": {
    "id": "EX524f2cd22ae4490eaf73cc9214eb217d",
    "type": "extensions",
    "attributes": {
      "created_at": "2020-12-14T17:41:21.397Z",
      "deleted_at": null,
      "dirty": false,
      "enabled": true,
      "name": "kessel-test",
      "published": false,
      "published_at": null,
      "revision_number": 0,
      "updated_at": "2020-12-14T17:41:21.397Z",
      "delegate_descriptor_id": null,
      "display_name": "Kessel Test",
      "review_status": "unsubmitted",
      "version": "1.2.0",
      "settings": "{}"
    },
    "relationships": {
      "libraries": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX524f2cd22ae4490eaf73cc9214eb217d/libraries"
        }
      },
      "revisions": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX524f2cd22ae4490eaf73cc9214eb217d/revisions"
        }
      },
      "notes": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX524f2cd22ae4490eaf73cc9214eb217d/notes"
        }
      },
      "property": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX524f2cd22ae4490eaf73cc9214eb217d/property"
        },
        "data": {
          "id": "PR88d6b8ee423b44a49de1dee26391e25b",
          "type": "properties"
        }
      },
      "origin": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX524f2cd22ae4490eaf73cc9214eb217d/origin"
        },
        "data": {
          "id": "EX524f2cd22ae4490eaf73cc9214eb217d",
          "type": "extensions"
        }
      },
      "updated_with_extension_package": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX524f2cd22ae4490eaf73cc9214eb217d/updated_with_extension_package"
        },
        "data": {
          "id": "EP75db2452065b44e2b8a38ca883ce369a",
          "type": "extension_packages"
        }
      },
      "extension_package": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX524f2cd22ae4490eaf73cc9214eb217d/extension_package"
        },
        "data": {
          "id": "EP75db2452065b44e2b8a38ca883ce369a",
          "type": "extension_packages"
        }
      }
    },
    "links": {
      "property": "https://reactor.adobe.io/properties/PR88d6b8ee423b44a49de1dee26391e25b",
      "origin": "https://reactor.adobe.io/extensions/EX524f2cd22ae4490eaf73cc9214eb217d",
      "self": "https://reactor.adobe.io/extensions/EX524f2cd22ae4490eaf73cc9214eb217d",
      "extension_package": "https://reactor.adobe.io/extension_packages/EP75db2452065b44e2b8a38ca883ce369a",
      "latest_extension_package": "https://reactor.adobe.io/extension_packages/EP75db2452065b44e2b8a38ca883ce369a"
    },
    "meta": {
      "latest_revision_number": 1
    }
  }
}
```

### 拡張機能に関連するプロパティを検索する {#property}

GET リクエストのパスに `/property` を追加して、拡張機能を所有するプロパティを検索できます。

**API 形式**

```http
GET  /extensions/{EXTENSION_ID}/property
```

| パラメーター | 説明 |
| --- | --- |
| `{EXTENSION_ID}` | プロパティを検索する拡張機能の `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/extensions/EX8ce7ced633f34bd48d33089ff8fad082/property \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、指定した拡張機能を所有するプロパティの詳細が返されます。

```json
{
  "data": {
    "id": "PR96fd3675be144ddc8c4540d79430355a",
    "type": "properties",
    "attributes": {
      "created_at": "2020-12-14T17:53:06.993Z",
      "enabled": true,
      "name": "Kessel Example Property",
      "updated_at": "2020-12-14T17:53:06.993Z",
      "platform": "web",
      "development": false,
      "token": "bf75a40b3c34",
      "domains": [
        "example.com"
      ],
      "undefined_vars_return_empty": false,
      "rule_component_sequencing_enabled": false
    },
    "relationships": {
      "company": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR96fd3675be144ddc8c4540d79430355a/company"
        },
        "data": {
          "id": "CO2bf094214ffd4785bb4bcf88c952a7c1",
          "type": "companies"
        }
      },
      "callbacks": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR96fd3675be144ddc8c4540d79430355a/callbacks"
        }
      },
      "hosts": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR96fd3675be144ddc8c4540d79430355a/hosts"
        }
      },
      "environments": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR96fd3675be144ddc8c4540d79430355a/environments"
        }
      },
      "libraries": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR96fd3675be144ddc8c4540d79430355a/libraries"
        }
      },
      "data_elements": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR96fd3675be144ddc8c4540d79430355a/data_elements"
        }
      },
      "extensions": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR96fd3675be144ddc8c4540d79430355a/extensions"
        }
      },
      "rules": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR96fd3675be144ddc8c4540d79430355a/rules"
        }
      },
      "notes": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR96fd3675be144ddc8c4540d79430355a/notes"
        }
      }
    },
    "links": {
      "company": "https://reactor.adobe.io/companies/CO2bf094214ffd4785bb4bcf88c952a7c1",
      "data_elements": "https://reactor.adobe.io/properties/PR96fd3675be144ddc8c4540d79430355a/data_elements",
      "environments": "https://reactor.adobe.io/properties/PR96fd3675be144ddc8c4540d79430355a/environments",
      "extensions": "https://reactor.adobe.io/properties/PR96fd3675be144ddc8c4540d79430355a/extensions",
      "rules": "https://reactor.adobe.io/properties/PR96fd3675be144ddc8c4540d79430355a/rules",
      "self": "https://reactor.adobe.io/properties/PR96fd3675be144ddc8c4540d79430355a"
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
