---
title: コールバックエンドポイント
description: Reactor API で /callbacks エンドポイントを呼び出す方法を説明します。
exl-id: dd980f91-89e3-4ba0-a6fc-64d66b288a22
source-git-commit: 7f3b9ef9270b7748bc3366c8c39f503e1aee2100
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 97%

---

# コールバックエンドポイント

コールバックは、Reactor API が特定の URL（通常は組織がホストする URL）に送信するメッセージです。

コールバックは、Reactor API のアクティビティを追跡するために[監査イベント](./audit-events.md)と組み合わせて使用することを目的としています。特定のタイプの監査イベントが生成されるたびに、コールバックは指定された URL に一致するメッセージを送信できます。

コールバックで指定した URL の背後にあるサービスは、HTTP ステータスコード 200（OK）または 201（作成済み）で応答する必要があります。サービスがこれらのステータスコードのいずれかで応答しない場合、メッセージ配信は次の間隔で再試行されます。

* 1 分
* 5 分
* 30 分
* 1 時間
* 12 時間
* 1 日
* 3 日

>[!NOTE]
>
>再試行の間隔は、前回の間隔を基準にしています。例えば、1 分間の再試行が失敗した場合、次の試行は 1 分間の試行が失敗してから 5 分後（メッセージが生成されてから 6 分後）にスケジュールされます。

すべての配信に失敗した場合、メッセージは破棄されます。

コールバックは、1 つの[プロパティ](./properties.md)のみに属します。 プロパティには、多くのコールバックを含めることができます。

## はじめに

このガイドで使用するエンドポイントは、[Reactor API](https://www.adobe.io/experience-platform-apis/references/reactor/) の一部です。続行する前に、 [はじめる前に](../getting-started.md) で、API への認証方法に関する重要な情報を確認してください。

## コールバックのリスト {#list}

GET リクエストをおこなうと、プロパティ配下のすべてのコールバックをリストできます。

**API 形式**

```http
GET  /properties/{PROPERTY_ID}/callbacks
```

| パラメーター | 説明 |
| --- | --- |
| `{PROPERTY_ID}` | コールバックのリストを取得するプロパティの `id`。 |

{style="table-layout:auto"}

>[!NOTE]
>
>クエリパラメーターを使用し、次の属性に基づいてリストされた callbacks をフィルタリングできます。<ul><li>`created_at`</li><li>`updated_at`</li></ul>詳しくは、 [応答のフィルタリング](../guides/filtering.md) に関するガイドを参照してください。

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/properties/PR66a3356c73fc4aabb67ee22caae53d70/callbacks \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、指定したプロパティのコールバックのリストが返されます。

```json
{
  "data": [
    {
      "id": "CB26edef8d709243579589107bcda034da",
      "type": "callbacks",
      "attributes": {
        "created_at": "2020-12-14T17:34:47.082Z",
        "subscriptions": [
          "rule.created"
        ],
        "updated_at": "2020-12-14T17:34:47.082Z",
        "url": "https://www.example.com"
      },
      "relationships": {
        "property": {
          "links": {
            "related": "https://reactor.adobe.io/callbacks/CB26edef8d709243579589107bcda034da/property"
          },
          "data": {
            "id": "PR66a3356c73fc4aabb67ee22caae53d70",
            "type": "properties"
          }
        }
      },
      "links": {
        "property": "https://reactor.adobe.io/properties/PR66a3356c73fc4aabb67ee22caae53d70",
        "self": "https://reactor.adobe.io/callbacks/CB26edef8d709243579589107bcda034da"
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

## コールバックの検索 {#lookup}

GET リクエストのパスで ID を指定することで、コールバックを検索できます。

**API 形式**

```http
GET /callbacks/{CALLBACK_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `CALLBACK_ID` | 検索するコールバックの `id`。 |

{style="table-layout:auto"}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/callbacks/CBeef389cee8d84e69acef8665e4dcbef6 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、コールバックの詳細が返されます。

```json
{
  "data": {
    "id": "CBeef389cee8d84e69acef8665e4dcbef6",
    "type": "callbacks",
    "attributes": {
      "created_at": "2020-12-14T17:34:36.872Z",
      "subscriptions": [
        "rule.created"
      ],
      "updated_at": "2020-12-14T17:34:36.872Z",
      "url": "https://www.example.com"
    },
    "relationships": {
      "property": {
        "links": {
          "related": "https://reactor.adobe.io/callbacks/CBeef389cee8d84e69acef8665e4dcbef6/property"
        },
        "data": {
          "id": "PRb513bbab52114573ac87f9848eea6ead",
          "type": "properties"
        }
      }
    },
    "links": {
      "property": "https://reactor.adobe.io/properties/PRb513bbab52114573ac87f9848eea6ead",
      "self": "https://reactor.adobe.io/callbacks/CBeef389cee8d84e69acef8665e4dcbef6"
    }
  }
}
```

## コールバックの作成 {#create}

POST リクエストをおこなうことで、新しいコールバックを作成できます。

**API 形式**

```http
POST /properties/{PROPERTY_ID}/callbacks
```

| パラメーター | 説明 |
| --- | --- |
| `PROPERTY_ID` | コールバックを定義する[プロパティ](./properties.md)の `id`。 |

{style="table-layout:auto"}

**リクエスト**

```shell
curl -X POST \
  https://reactor.adobe.io/properties/PR5e22de986a7c4070965e7546b2bb108d/callbacks \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -d '{
        "data": {
          "attributes": {
            "url": "https://www.example.com",
            "subscriptions": [
              "rule.created"
            ]
          }
        }
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `url` | コールバックメッセージの URL 宛先。 URL は、HTTPS プロトコル拡張機能を使用する必要があります。 |
| `subscriptions` | コールバックをトリガーする監査イベントタイプを示す、文字列の配列。 使用可能なイベントタイプのリストについては、[監査イベントエンドポイントのガイド](./audit-events.md)を参照してください。 |

{style="table-layout:auto"}

**応答**

応答が成功すると、新しく作成したコールバックの詳細が返されます。

```json
{
  "data": {
    "id": "CB32d8f23d5ee548278d32076af4c442a0",
    "type": "callbacks",
    "attributes": {
      "created_at": "2020-12-14T17:34:27.059Z",
      "subscriptions": [
        "rule.created"
      ],
      "updated_at": "2020-12-14T17:34:27.059Z",
      "url": "https://www.example.com"
    },
    "relationships": {
      "property": {
        "links": {
          "related": "https://reactor.adobe.io/callbacks/CB32d8f23d5ee548278d32076af4c442a0/property"
        },
        "data": {
          "id": "PR5e22de986a7c4070965e7546b2bb108d",
          "type": "properties"
        }
      }
    },
    "links": {
      "property": "https://reactor.adobe.io/properties/PR5e22de986a7c4070965e7546b2bb108d",
      "self": "https://reactor.adobe.io/callbacks/CB32d8f23d5ee548278d32076af4c442a0"
    }
  }
}
```

## コールバックの更新

コールバックを更新するには、コールバックリクエストのパスに ID を含めるPATCHを更新します。

**API 形式**

```http
PATCH /callbacks/{CALLBACK_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `CALLBACK_ID` | 更新するコールバックの `id`。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、既存のコールバックの `subscriptions` 配列を更新します。

```shell
curl -X PATCH \
  https://reactor.adobe.io/callbacks/CB4310904d415549888cc9e31ebe1e1e45 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -d '{
        "data": {
          "attributes": {
            "url": "https://www.example.net",
            "subscriptions": [
              "rule.created",
              "build.created"
            ]
          },
          "type": "callbacks",
          "id": "CB4310904d415549888cc9e31ebe1e1e45"
        }
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `attributes` | コールバック用に更新される属性を表すプロパティを持つオブジェクト。各キーは、更新する特定のコールバック属性と、それに対応して更新される値を表します。<br><br>コールバックで更新できる属性は次のとおりです。<ul><li>`subscriptions`</li><li>`url`</li></ul> |
| `id` | 更新するコールバックの `id`。この値は、リクエストパスで指定された `{CALLBACK_ID}` 値と一致する必要があります。 |
| `type` | 更新するリソースのタイプ。 このエンドポイントの場合は、値を `callbacks` にする必要があります。 |

{style="table-layout:auto"}

**応答**

応答が成功すると、更新されたコールバックの詳細が返されます。

```json
{
  "data": {
    "id": "CB4310904d415549888cc9e31ebe1e1e45",
    "type": "callbacks",
    "attributes": {
      "created_at": "2020-12-14T17:34:56.884Z",
      "subscriptions": [
        "rule.created",
        "build.created"
      ],
      "updated_at": "2020-12-14T17:34:57.614Z",
      "url": "https://www.example.net"
    },
    "relationships": {
      "property": {
        "links": {
          "related": "https://reactor.adobe.io/callbacks/CB4310904d415549888cc9e31ebe1e1e45/property"
        },
        "data": {
          "id": "PR0a8ef3ca31dc456a8566e9288960bd79",
          "type": "properties"
        }
      }
    },
    "links": {
      "property": "https://reactor.adobe.io/properties/PR0a8ef3ca31dc456a8566e9288960bd79",
      "self": "https://reactor.adobe.io/callbacks/CB4310904d415549888cc9e31ebe1e1e45"
    }
  }
}
```

## コールバックの削除

DELETE リクエストのパスに ID を含めることで、コールバックを削除できます。

**API 形式**

```http
DELETE /callbacks/{CALLBACK_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `CALLBACK_ID` | 削除するコールバックの `id`。 |

{style="table-layout:auto"}

**リクエスト**

```shell
curl -X DELETE \
  https://reactor.adobe.io/callbacks/CB4310904d415549888cc9e31ebe1e1e45 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

応答が成功すると、応答本文なしで HTTP ステータス 204（コンテンツなし）が返され、コールバックが削除されたことを示します。
