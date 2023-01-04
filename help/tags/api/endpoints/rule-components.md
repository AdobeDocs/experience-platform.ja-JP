---
title: ルールコンポーネントエンドポイント
description: Reactor API で /rule_components エンドポイントを呼び出す方法を説明します。
exl-id: 8a878a89-7f41-45fc-88f3-17f0f743e29c
source-git-commit: e602f78470fe4eeb2a42e6333ba52096d8a9fe8a
workflow-type: tm+mt
source-wordcount: '1190'
ht-degree: 95%

---

# ルールコンポーネントエンドポイント

データ収集タグでは、[ルール](./rules.md)は、デプロイ済みの[ライブラリ](./libraries.md)内のリソースの動作を制御します。 **ルールコンポーネント**&#x200B;は、ルールを構成する個々のパーツです。ルールがレシピだとすると、ルールコンポーネントは食材の一つです。Reactor API の `/rule_components` エンドポイントを使用すると、ルールコンポーネントをプログラムで管理できます。

>[!NOTE]
>
>このドキュメントでは、Reactor API のルールコンポーネントを管理する方法について説明します。 UI でルールやルールコンポーネントを操作する方法について詳しくは、 [UI ガイド](../../ui/managing-resources/rules.md).

ルールコンポーネントには、次の 3 つの基本的なタイプがあります。

| ルールコンポーネントタイプ | 説明 |
| --- | --- |
| イベント | イベントは、ルールのトリガーです。ルールは、クライアントデバイスで実行時にイベントが発生すると開始します。「[!UICONTROL ライブラリ読み込み]」、「[!UICONTROL ページ上部]」、「[!UICONTROL クリック]」などがイベントの例です。 |
| 条件 | 条件とは、アクションが実行される前に特定の条件を満たしているどうかを評価するものです。 イベントが発生すると、条件が評価されます。ルールのアクションは、すべての条件が満たされた場合にのみ実行されます。 |
| アクション | これらは、Adobe Analytics ビーコンの送信、カスタム訪問者 ID の取得、特定の mbox の実行など、ルールで実際に実行するアクションです。 |

{style=&quot;table-layout:auto&quot;}

 ルールコンポーネントは、1 つのルールのみに属します。ルールには、多くのルールコンポーネントを含めることができます（含める必要があります）。

ルールコンポーネントは、1 つの [拡張機能](./extensions.md)のみによって提供されます。 拡張機能では、多くのルールコンポーネントタイプを提供できます。

## はじめに

このガイドで使用するエンドポイントは、[Reactor API](https://www.adobe.io/experience-platform-apis/references/reactor/) の一部です。続行する前に、 [はじめる前に](../getting-started.md) で、API への認証方法に関する重要な情報を確認してください。

## ルールコンポーネントのリストの取得 {#list}

GET リクエストのパスにルールの ID を含めることで、ルールに属するルールコンポーネントのリストを取得できます。

**API 形式**

```http
GET /rules/{RULE_ID}/rule_components
```

| パラメーター | 説明 |
| --- | --- |
| `RULE_ID` | コンポーネントのリストを取得するルールの `id`。 |

{style=&quot;table-layout:auto&quot;}

>[!NOTE]
>
>クエリパラメーターを使用して、リストされたルールコンポーネントを次の属性に基づいてフィルタリングできます。<ul><li>`created_at`</li><li>`dirty`</li><li>`enabled`</li><li>`name`</li><li>`negate`</li><li>`origin_id`</li><li>`published`</li><li>`published_at`</li><li>`revision_number`</li><li>`updated_at`</li></ul>詳しくは、 [応答のフィルタリング](../guides/filtering.md) に関するガイドを参照してください。

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/rules/RL14dc6a8c37b14b619ddb2b3ba489a1f51/rule_components \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、指定したルールのルールコンポーネントのリストが返されます。

```json
{
  "data": [
    {
      "id": "RC45944086902c4828b6e14ffbb40017f4",
      "type": "rule_components",
      "attributes": {
        "created_at": "2020-12-14T17:54:34.976Z",
        "delegate_descriptor_id": "kessel-test::events::click",
        "deleted_at": null,
        "dirty": true,
        "name": "My Example Click Event",
        "negate": false,
        "order": 0,
        "rule_order": 50.0,
        "timeout": 2000,
        "delay_next": true,
        "published": false,
        "published_at": null,
        "revision_number": 0,
        "updated_at": "2020-12-14T17:54:34.976Z",
        "settings": "{\"elementSelector\":\".accordion\",\"bubbleFireIfChildFired\":true}"
      },
      "relationships": {
        "updated_with_extension_package": {
          "links": {
            "related": "https://reactor.adobe.io/rule_components/RC45944086902c4828b6e14ffbb40017f4/updated_with_extension_package"
          },
          "data": {
            "id": "EP75db2452065b44e2b8a38ca883ce369a",
            "type": "extension_packages"
          }
        },
        "updated_with_extension": {
          "links": {
            "related": "https://reactor.adobe.io/rule_components/RC45944086902c4828b6e14ffbb40017f4/updated_with_extension"
          },
          "data": {
            "id": "EX6312cea676de47ad9f70b42f7c0fbf02",
            "type": "extensions"
          }
        },
        "extension": {
          "links": {
            "related": "https://reactor.adobe.io/rule_components/RC45944086902c4828b6e14ffbb40017f4/extension"
          },
          "data": {
            "id": "EXbfd099788024423ebdd49cf06b52e50a",
            "type": "extensions"
          }
        },
        "notes": {
          "links": {
            "related": "https://reactor.adobe.io/rule_components/RC45944086902c4828b6e14ffbb40017f4/notes"
          }
        },
        "origin": {
          "links": {
            "related": "https://reactor.adobe.io/rule_components/RC45944086902c4828b6e14ffbb40017f4/origin"
          },
          "data": {
            "id": "RC45944086902c4828b6e14ffbb40017f4",
            "type": "rule_components"
          }
        },
        "rule component": {
          "links": {
            "related": "https://reactor.adobe.io/properties/PRb1090b7443e948ac91650964b490e622"
          },
          "data": {
            "id": "PRb1090b7443e948ac91650964b490e622",
            "type": "properties"
          }
        },
        "rules": {
          "links": {
            "related": "https://reactor.adobe.io/rule_components/RC45944086902c4828b6e14ffbb40017f4/rules"
          }
        }
      },
      "links": {
        "extension": "https://reactor.adobe.io/extensions/EXbfd099788024423ebdd49cf06b52e50a",
        "origin": "https://reactor.adobe.io/rule_components/RC45944086902c4828b6e14ffbb40017f4",
        "rules": "https://reactor.adobe.io/rule_components/RC45944086902c4828b6e14ffbb40017f4/rules",
        "self": "https://reactor.adobe.io/rule_components/RC45944086902c4828b6e14ffbb40017f4"
      },
      "meta": {
        "latest_revision_number": 0
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

## ルールコンポーネントの検索 {#lookup}

GET リクエストのパスで ID を指定することで、ルールコンポーネントを検索できます。

**API 形式**

```http
GET /rule_components/{RULE_COMPONENT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `RULE_COMPONENT_ID` | 検索するルールコンポーネントの `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/rule_components/RC7be169fcfd534ffc82acc7bffdc50128 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、ルールコンポーネントの詳細が返されます。

```json
{
  "data": {
    "id": "RC7be169fcfd534ffc82acc7bffdc50128",
    "type": "rule_components",
    "attributes": {
      "created_at": "2020-12-14T17:54:18.551Z",
      "delegate_descriptor_id": "kessel-test::events::click",
      "deleted_at": null,
      "dirty": true,
      "name": "My Example Click Event",
      "negate": false,
      "order": 0,
      "rule_order": 50.0,
      "timeout": 2000,
      "delay_next": true,
      "published": false,
      "published_at": null,
      "revision_number": 0,
      "updated_at": "2020-12-14T17:54:18.551Z",
      "settings": "{\"elementSelector\":\".accordion\",\"bubbleFireIfChildFired\":true}"
    },
    "relationships": {
      "updated_with_extension_package": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC7be169fcfd534ffc82acc7bffdc50128/updated_with_extension_package"
        },
        "data": {
          "id": "EP75db2452065b44e2b8a38ca883ce369a",
          "type": "extension_packages"
        }
      },
      "updated_with_extension": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC7be169fcfd534ffc82acc7bffdc50128/updated_with_extension"
        },
        "data": {
          "id": "EXa11e168f2ff2485197a493095269f964",
          "type": "extensions"
        }
      },
      "extension": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC7be169fcfd534ffc82acc7bffdc50128/extension"
        },
        "data": {
          "id": "EXa76eb16dd86849318b743494e75c33a1",
          "type": "extensions"
        }
      },
      "notes": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC7be169fcfd534ffc82acc7bffdc50128/notes"
        }
      },
      "origin": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC7be169fcfd534ffc82acc7bffdc50128/origin"
        },
        "data": {
          "id": "RC7be169fcfd534ffc82acc7bffdc50128",
          "type": "rule_components"
        }
      },
      "property": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR00a35a74381443dc994e6b30b7152106"
        },
        "data": {
          "id": "PR00a35a74381443dc994e6b30b7152106",
          "type": "properties"
        }
      },
      "rules": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC7be169fcfd534ffc82acc7bffdc50128/rules"
        }
      }
    },
    "links": {
      "extension": "https://reactor.adobe.io/extensions/EXa76eb16dd86849318b743494e75c33a1",
      "origin": "https://reactor.adobe.io/rule_components/RC7be169fcfd534ffc82acc7bffdc50128",
      "rules": "https://reactor.adobe.io/rule_components/RC7be169fcfd534ffc82acc7bffdc50128/rules",
      "self": "https://reactor.adobe.io/rule_components/RC7be169fcfd534ffc82acc7bffdc50128"
    },
    "meta": {
      "latest_revision_number": 0
    }
  }
}
```

## ルールコンポーネントの作成 {#create}

POST リクエストをおこなうことで、新しいルールコンポーネントを作成できます。

**API 形式**

```http
POST /properties/{PROPERTY_ID}/rule_components
```

| パラメーター | 説明 |
| --- | --- |
| `PROPERTY_ID` | この `id` ルールコンポーネントを定義するプロパティの。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、新しいルールコンポーネントを作成します。 ペイロードでは、 `relationships` プロパティは、コンポーネントを特定のルールおよび既存の拡張機能に関連付けます。 詳しくは、 [関係](../guides/relationships.md) に関するガイドを参照してください。

```shell
curl -X POST \
  https://reactor.adobe.io/properties/PR97596432a82549ceb8e2a5d9df05c0e1/rule_components \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/json' \
  -d '{
        "data": {
          "attributes": {
            "delegate_descriptor_id": "kessel-test::events::click",
            "name": "My Example Click Event",
            "delay_next": true,
            "order": 0,
            "rule_order": 50.0,
            "settings": "{\"elementSelector\":\".accordion\",\"bubbleFireIfChildFired\":true}",
            "timeout": 2000
          },
          "relationships": {
            "extension": {
              "data": {
                "id": "EX31b8c49f134b4307924d71a64204099e",
                "type": "extensions"
              }
            },
            "rules": {
              "data": [
                {
                  "id": "RLf7b4f416b2e04ae1ba857ae681fee5bc",
                  "type": "rules"
                }
              ]
            }
          },
          "type": "rule_components"
        }
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `attributes.delegate_descriptor_id` | **（必須）**&#x200B;定義できるルールコンポーネントのタイプは、[拡張機能パッケージ](./extension-packages.md)によ って提供されます。新しいルールコンポーネントを作成する際には、このルールコンポーネントがどの拡張機能パッケージに基づいているかを示すデリゲート記述子 ID、コンポーネントのタイプ（イベント、条件、アクション）、拡張機能で定義されている特定のコンポーネントの名前（コア拡張機能の「クリック」イベントなど）を指定する必要があります。<br><br>詳しくは、 [デリゲート記述子 ID](../guides/delegate-descriptor-ids.md) に関するガイドを参照してください。 |
| `attributes.name` | **（必須）**&#x200B;ルールコンポーネントの人間が判読できる名前。 |
| `attributes.delay_next` | 後のアクションを遅らせるかどうかを示すブール値。 |
| `attributes.order` | タイプ別にコンポーネントを読み込む順序を示す整数。 |
| `attributes.rule_order` | 関連付けられたルールが起動する優先度を示す整数。 |
| `attributes.settings` | 文字列として表される設定 JSON オブジェクト。 |
| `attributes.timeout` | 順に実行されるアクションのタイムアウトを示す整数。 |
| `relationships` | ルールコンポーネントに必要な関係を確立するオブジェクト。次の 2 つの関係を確立する必要があります。 <ol><li>`extension`：このルールコンポーネントを定義する拡張機能。これは、`delegate_descriptor_id` で示される拡張機能のパッケージと同じにする必要があります。</li><li>`rules`：このコンポーネントが定義されているルール。</li></ol>関係に関する一般的な情報について詳しくは、 [関係に関するガイド](../guides/relationships.md) を参照してください。 |
| `type` | 作成するリソースのタイプ。このエンドポイントの場合は、値を `rule_components` にする必要があります。 |

{style=&quot;table-layout:auto&quot;}

**応答**

応答が成功すると、新しく作成したルールコンポーネントの詳細が返されます。

```json
{
  "data": {
    "id": "RC78c44af3cf7644e5927fc0ad61e88940",
    "type": "rule_components",
    "attributes": {
      "created_at": "2020-12-14T17:54:00.232Z",
      "delegate_descriptor_id": "kessel-test::events::click",
      "deleted_at": null,
      "dirty": true,
      "name": "My Example Click Event",
      "negate": false,
      "order": 0,
      "rule_order": 50.0,
      "timeout": 2000,
      "delay_next": true,
      "published": false,
      "published_at": null,
      "revision_number": 0,
      "updated_at": "2020-12-14T17:54:00.232Z",
      "settings": "{\"elementSelector\":\".accordion\",\"bubbleFireIfChildFired\":true}"
    },
    "relationships": {
      "updated_with_extension_package": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC78c44af3cf7644e5927fc0ad61e88940/updated_with_extension_package"
        },
        "data": {
          "id": "EP75db2452065b44e2b8a38ca883ce369a",
          "type": "extension_packages"
        }
      },
      "updated_with_extension": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC78c44af3cf7644e5927fc0ad61e88940/updated_with_extension"
        },
        "data": {
          "id": "EX0019a115a74f401fa0b9bb8f57a0196b",
          "type": "extensions"
        }
      },
      "extension": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC78c44af3cf7644e5927fc0ad61e88940/extension"
        },
        "data": {
          "id": "EX31b8c49f134b4307924d71a64204099e",
          "type": "extensions"
        }
      },
      "notes": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC78c44af3cf7644e5927fc0ad61e88940/notes"
        }
      },
      "origin": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC78c44af3cf7644e5927fc0ad61e88940/origin"
        },
        "data": {
          "id": "RC78c44af3cf7644e5927fc0ad61e88940",
          "type": "rule_components"
        }
      },
      "property": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR97596432a82549ceb8e2a5d9df05c0e1"
        },
        "data": {
          "id": "PR97596432a82549ceb8e2a5d9df05c0e1",
          "type": "properties"
        }
      },
      "rules": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC78c44af3cf7644e5927fc0ad61e88940/rules"
        }
      }
    },
    "links": {
      "extension": "https://reactor.adobe.io/extensions/EX31b8c49f134b4307924d71a64204099e",
      "origin": "https://reactor.adobe.io/rule_components/RC78c44af3cf7644e5927fc0ad61e88940",
      "rules": "https://reactor.adobe.io/rule_components/RC78c44af3cf7644e5927fc0ad61e88940/rules",
      "self": "https://reactor.adobe.io/rule_components/RC78c44af3cf7644e5927fc0ad61e88940"
    },
    "meta": {
      "latest_revision_number": 0
    }
  }
}
```

## ルールコンポーネントの更新 {#update}

PATCH リクエストのパスに ID を含めることで、ルールコンポーネントを更新できます。

>[!NOTE]
>
>ルールコンポーネントを更新すると、親ルールの `updated_at` タイムスタンプも更新されます。

**API 形式**

```http
PATCH /rule_components/{RULE_COMPONENT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `RULE_COMPONENT_ID` | 更新するルールコンポーネントの `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、既存のルールコンポーネントの `order` 属性と `settings` 属性を更新します。

```shell
curl -X PATCH \
  https://reactor.adobe.io/rule_components/RC9af052ee231346f28d1e44865ab62c04 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/json' \
  -d '{
        "data": {
          "attributes": {
            "order": 1,
            "settings": "{\"elementSelector\":\".accordion\",\"bubbleFireIfChildFired\":false}"
          },
          "type": "rule_components",
          "id": "RC9af052ee231346f28d1e44865ab62c04"
        }
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `attributes` | ルールコンポーネントが、ルールコンポーネント用に更新される属性を表すオブジェクト。ルールコンポーネントの次の属性を更新できます。 <ul><li>`delay_next`</li><li>`delegate_descriptor_id`</li><li>`name`</li><li>`order`</li><li>`rule_order`</li><li>`settings`</li><li>`timeout`</li></ul> |
| `id` | 更新するルールコンポーネントの `id`。この値は、リクエストパスで指定された `{RULE_COMPONENT_ID}` 値と一致する必要があります。 |
| `type` | 更新するリソースのタイプ。 このエンドポイントの場合は、値を `rule_components` にする必要があります。 |

{style=&quot;table-layout:auto&quot;}

**応答**

応答が成功すると、更新されたルールコンポーネントの詳細が返されます。

```json
{
  "data": {
    "id": "RC9af052ee231346f28d1e44865ab62c04",
    "type": "rule_components",
    "attributes": {
      "created_at": "2020-12-14T17:54:50.887Z",
      "delegate_descriptor_id": "kessel-test::events::click",
      "deleted_at": null,
      "dirty": true,
      "name": "My Example Click Event",
      "negate": false,
      "order": 1,
      "rule_order": 50.0,
      "timeout": 2000,
      "delay_next": true,
      "published": false,
      "published_at": null,
      "revision_number": 0,
      "updated_at": "2020-12-14T17:54:52.553Z",
      "settings": "{\"elementSelector\":\".accordion\",\"bubbleFireIfChildFired\":false}"
    },
    "relationships": {
      "updated_with_extension_package": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC9af052ee231346f28d1e44865ab62c04/updated_with_extension_package"
        },
        "data": {
          "id": "EP75db2452065b44e2b8a38ca883ce369a",
          "type": "extension_packages"
        }
      },
      "updated_with_extension": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC9af052ee231346f28d1e44865ab62c04/updated_with_extension"
        },
        "data": {
          "id": "EX468796dd09d743858f17d4c5ca52f3e0",
          "type": "extensions"
        }
      },
      "extension": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC9af052ee231346f28d1e44865ab62c04/extension"
        },
        "data": {
          "id": "EXcedb08a8265c488e8bb98b46245b2486",
          "type": "extensions"
        }
      },
      "notes": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC9af052ee231346f28d1e44865ab62c04/notes"
        }
      },
      "origin": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC9af052ee231346f28d1e44865ab62c04/origin"
        },
        "data": {
          "id": "RC9af052ee231346f28d1e44865ab62c04",
          "type": "rule_components"
        }
      },
      "property": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR986402dc07834fbeb4789c56060dbf41"
        },
        "data": {
          "id": "PR986402dc07834fbeb4789c56060dbf41",
          "type": "properties"
        }
      },
      "rules": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC9af052ee231346f28d1e44865ab62c04/rules"
        }
      }
    },
    "links": {
      "extension": "https://reactor.adobe.io/extensions/EXcedb08a8265c488e8bb98b46245b2486",
      "origin": "https://reactor.adobe.io/rule_components/RC9af052ee231346f28d1e44865ab62c04",
      "rules": "https://reactor.adobe.io/rule_components/RC9af052ee231346f28d1e44865ab62c04/rules",
      "self": "https://reactor.adobe.io/rule_components/RC9af052ee231346f28d1e44865ab62c04"
    },
    "meta": {
      "latest_revision_number": 0
    }
  }
}
```

## ルールコンポーネントの削除

DELETE リクエストのパスに ID を含めることで、ルールコンポーネントを削除できます。

**API 形式**

```http
DELETE /rule_components/{RULE_COMPONENT_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `RULE_COMPONENT_ID` | 削除するルールコンポーネントの `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X DELETE \
  https://reactor.adobe.io/rule_components/RC9af052ee231346f28d1e44865ab62c04 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}'
```

**応答**

応答が成功すると、応答本文なしで HTTP ステータス 204（コンテンツなし）が返され、ルールコンポーネントが削除されたことを示します。

## ルールコンポーネントのメモの管理 {#notes}

ルールコンポーネントは「メモ機能のある」リソースのため、個々のリソースでテキストベースのメモを作成し、取得することができます。ルールコンポーネントや他の互換性のあるリソースのメモを管理する方法について詳しくは、 [メモエンドポイントガイド](./notes.md) を参照してください。

## ルールコンポーネントに関連するリソースの取得 {#related}

次の呼び出しは、ルールコンポーネントに関連するリソースの取得方法を示しています。 [ルールコンポーネントを検索](#lookup)すると、これらの関係は `relationships` ルールコンポーネントの下に表示されます。

Reactor API の関係について詳しくは 、[関係に関するガイド](../guides/relationships.md) を参照してください。

### ルールコンポーネントに関連するルールのリスト {#rules}

検索リクエストのパスに `/rules` を追加することで、特定のルールコンポーネントを利用するルールをリストできます。

**API 形式**

```http
GET  /rule_components/{RULE_COMPONENT_ID}/rules
```

| パラメーター | 説明 |
| --- | --- |
| `{RULE_COMPONENT_ID}` | ルールのリストを取得するルールコンポーネントの `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/rule_components/RC9af052ee231346f28d1e44865ab62c04/rules \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、指定したルールコンポーネントを使用するルールのリストが返されます。

```json
{
  "data": [
    {
      "id": "RLf1baa571748941db88f54de8efd119aa",
      "type": "rules",
      "attributes": {
        "created_at": "2020-12-14T17:58:36.072Z",
        "deleted_at": null,
        "dirty": true,
        "enabled": true,
        "name": "Example Rule",
        "published": false,
        "published_at": null,
        "revision_number": 0,
        "updated_at": "2020-12-14T17:58:37.452Z",
        "review_status": "unsubmitted"
      },
      "relationships": {
        "libraries": {
          "links": {
            "related": "https://reactor.adobe.io/rules/RLf1baa571748941db88f54de8efd119aa/libraries"
          }
        },
        "revisions": {
          "links": {
            "related": "https://reactor.adobe.io/rules/RLf1baa571748941db88f54de8efd119aa/revisions"
          }
        },
        "notes": {
          "links": {
            "related": "https://reactor.adobe.io/rules/RLf1baa571748941db88f54de8efd119aa/notes"
          }
        },
        "property": {
          "links": {
            "related": "https://reactor.adobe.io/rules/RLf1baa571748941db88f54de8efd119aa/property"
          },
          "data": {
            "id": "PR966c4a501e1a43a48cb55e104b4de935",
            "type": "properties"
          }
        },
        "origin": {
          "links": {
            "related": "https://reactor.adobe.io/rules/RLf1baa571748941db88f54de8efd119aa/origin"
          },
          "data": {
            "id": "RLf1baa571748941db88f54de8efd119aa",
            "type": "rules"
          }
        },
        "rule_components": {
          "links": {
            "related": "https://reactor.adobe.io/rules/RLf1baa571748941db88f54de8efd119aa/rule_components"
          }
        }
      },
      "links": {
        "property": "https://reactor.adobe.io/properties/PR966c4a501e1a43a48cb55e104b4de935",
        "origin": "https://reactor.adobe.io/rules/RLf1baa571748941db88f54de8efd119aa",
        "self": "https://reactor.adobe.io/rules/RLf1baa571748941db88f54de8efd119aa",
        "rule_components": "https://reactor.adobe.io/rules/RLf1baa571748941db88f54de8efd119aa/rule_components"
      },
      "meta": {
        "latest_revision_number": 0
      }
    }
  ]
}
```

### ルールコンポーネントに関連する拡張機能の検索 {#extension}

検索リクエストのパスに `/extension` を追加することで、ルールコンポーネントを提供する拡張機能を検索できます。

**API 形式**

```http
GET /rule_components/{RULE_COMPONENT_ID}/extension
```

| パラメーター | 説明 |
| --- | --- |
| `{RULE_COMPONENT_ID}` | 検索する拡張機能を持つルールコンポーネントの `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/rule_components/RC9af052ee231346f28d1e44865ab62c04/extension \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、指定したルールコンポーネントの拡張機能の詳細が返されます。

```json
{
  "data": {
    "id": "EX5644c3eed97d46b39cb2279ea11dde29",
    "type": "extensions",
    "attributes": {
      "created_at": "2020-12-14T17:55:22.634Z",
      "deleted_at": null,
      "dirty": false,
      "enabled": true,
      "name": "kessel-test",
      "published": false,
      "published_at": null,
      "revision_number": 0,
      "updated_at": "2020-12-14T17:55:22.634Z",
      "delegate_descriptor_id": null,
      "display_name": "Kessel Test",
      "review_status": "unsubmitted",
      "version": "1.2.0",
      "settings": "{}"
    },
    "relationships": {
      "libraries": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX5644c3eed97d46b39cb2279ea11dde29/libraries"
        }
      },
      "revisions": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX5644c3eed97d46b39cb2279ea11dde29/revisions"
        }
      },
      "notes": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX5644c3eed97d46b39cb2279ea11dde29/notes"
        }
      },
      "property": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX5644c3eed97d46b39cb2279ea11dde29/property"
        },
        "data": {
          "id": "PRcdb3d12504ce48ecbfa4fbbe5b80b6dd",
          "type": "properties"
        }
      },
      "origin": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX5644c3eed97d46b39cb2279ea11dde29/origin"
        },
        "data": {
          "id": "EX5644c3eed97d46b39cb2279ea11dde29",
          "type": "extensions"
        }
      },
      "updated_with_extension_package": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX5644c3eed97d46b39cb2279ea11dde29/updated_with_extension_package"
        },
        "data": {
          "id": "EP75db2452065b44e2b8a38ca883ce369a",
          "type": "extension_packages"
        }
      },
      "extension_package": {
        "links": {
          "related": "https://reactor.adobe.io/extensions/EX5644c3eed97d46b39cb2279ea11dde29/extension_package"
        },
        "data": {
          "id": "EP75db2452065b44e2b8a38ca883ce369a",
          "type": "extension_packages"
        }
      }
    },
    "links": {
      "property": "https://reactor.adobe.io/properties/PRcdb3d12504ce48ecbfa4fbbe5b80b6dd",
      "origin": "https://reactor.adobe.io/extensions/EX5644c3eed97d46b39cb2279ea11dde29",
      "self": "https://reactor.adobe.io/extensions/EX5644c3eed97d46b39cb2279ea11dde29",
      "extension_package": "https://reactor.adobe.io/extension_packages/EP75db2452065b44e2b8a38ca883ce369a",
      "latest_extension_package": "https://reactor.adobe.io/extension_packages/EP75db2452065b44e2b8a38ca883ce369a"
    },
    "meta": {
      "latest_revision_number": 1
    }
  }
}
```

### ルールコンポーネントに関連する起点の検索 {#origin}

検索リクエストのパスに `/origin` を追加することで、ルールコンポーネントの起点（以前のリビジョン）を検索することができます。

**API 形式**

```http
GET /rule_components/{RULE_COMPONENT_ID}/origin
```

| パラメーター | 説明 |
| --- | --- |
| `{RULE_COMPONENT_ID}` | 検索する起点を持つルールコンポーネントの `id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/rule_components/RC3d0805fde85d42db8988090bc074bb44/origin \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、指定したルールコンポーネントの起点の詳細が返されます。

```json
{
  "data": {
    "id": "RC3d0805fde85d42db8988090bc074bb44",
    "type": "rule_components",
    "attributes": {
      "created_at": "2020-12-14T17:55:40.016Z",
      "delegate_descriptor_id": "kessel-test::events::click",
      "deleted_at": null,
      "dirty": false,
      "name": "My Example Click Event",
      "negate": false,
      "order": 0,
      "rule_order": 50.0,
      "timeout": 2000,
      "delay_next": true,
      "published": false,
      "published_at": null,
      "revision_number": 0,
      "updated_at": "2020-12-14T17:55:40.016Z",
      "settings": "{\"elementSelector\":\".accordion\",\"bubbleFireIfChildFired\":true}"
    },
    "relationships": {
      "updated_with_extension_package": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC3d0805fde85d42db8988090bc074bb44/updated_with_extension_package"
        },
        "data": {
          "id": "EP75db2452065b44e2b8a38ca883ce369a",
          "type": "extension_packages"
        }
      },
      "updated_with_extension": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC3d0805fde85d42db8988090bc074bb44/updated_with_extension"
        },
        "data": {
          "id": "EXb713fc209ce344c996bdeb377685e2c4",
          "type": "extensions"
        }
      },
      "extension": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC3d0805fde85d42db8988090bc074bb44/extension"
        },
        "data": {
          "id": "EXd6e1dce006b2412f874301e24d58ce24",
          "type": "extensions"
        }
      },
      "notes": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC3d0805fde85d42db8988090bc074bb44/notes"
        }
      },
      "origin": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC3d0805fde85d42db8988090bc074bb44/origin"
        },
        "data": {
          "id": "RC3d0805fde85d42db8988090bc074bb44",
          "type": "rule_components"
        }
      },
      "property": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR89c66a560ec44928889b439333255efe"
        },
        "data": {
          "id": "PR89c66a560ec44928889b439333255efe",
          "type": "properties"
        }
      },
      "rules": {
        "links": {
          "related": "https://reactor.adobe.io/rule_components/RC3d0805fde85d42db8988090bc074bb44/rules"
        }
      }
    },
    "links": {
      "extension": "https://reactor.adobe.io/extensions/EXd6e1dce006b2412f874301e24d58ce24",
      "origin": "https://reactor.adobe.io/rule_components/RC3d0805fde85d42db8988090bc074bb44",
      "rules": "https://reactor.adobe.io/rule_components/RC3d0805fde85d42db8988090bc074bb44/rules",
      "self": "https://reactor.adobe.io/rule_components/RC3d0805fde85d42db8988090bc074bb44"
    },
    "meta": {
      "latest_revision_number": 1
    }
  }
}
```
