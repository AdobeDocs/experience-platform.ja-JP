---
title: コールバックエンドポイント
description: Reactor APIで/callbacksエンドポイントを呼び出す方法を説明します。
source-git-commit: 6a1728bd995137a7cd6dc79313762ae6e665d416
workflow-type: tm+mt
source-wordcount: '625'
ht-degree: 10%

---

# コールバックエンドポイント

コールバックは、Reactor APIが特定のURL（通常は組織がホストするURL）に送信するメッセージです。

コールバックは、[監査イベント](./audit-events.md)と組み合わせて、Reactor APIのアクティビティを追跡するために使用します。 特定のタイプの監査イベントが生成されるたびに、コールバックは、一致するメッセージを指定のURLに送信できます。

コールバックで指定されたURLの背後にあるサービスは、HTTPステータスコード200(OK)または201（作成済み）で応答する必要があります。 サービスがこれらのステータスコードのいずれかで応答しない場合、メッセージ配信は次の間隔で再試行されます。

* 1 分
* 5 分
* 30 分
* 1 時間
* 12 時間
* 1 日
* 3 日

>[!NOTE]
>
>再試行間隔は前の間隔との相対値です。 例えば、1分間の再試行が失敗した場合、1分間の再試行が失敗してから5分間（メッセージが生成されてから6分後）にスケジュールされます。

すべての配信に失敗した場合、メッセージは破棄されます。

コールバックは、[プロパティ](./properties.md)に1つだけ属します。 プロパティには、多くのコールバックを含めることができます。

## はじめに

このガイドで使用するエンドポイントは、[Reactor API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/reactor.yaml)の一部です。 続行する前に、APIへの認証方法に関する重要な情報について、[はじめにのガイド](../getting-started.md)を参照してください。

## コールバックのリスト {#list}

GETリクエストをおこなうと、プロパティの下にあるすべてのコールバックをリストできます。

**API 形式**

```http
GET  /properties/{PROPERTY_ID}/callbacks
```

| パラメーター | 説明 |
| --- | --- |
| `{PROPERTY_ID}` | コールバックのリストを表示するプロパティの`id`。 |

{style=&quot;table-layout:auto&quot;}

>[!NOTE]
>
>クエリパラメーターを使用して、リストに表示されたコールバックを次の属性に基づいてフィルタリングできます。<ul><li>`created_at`</li><li>`updated_at`</li></ul>詳しくは、[応答のフィルタリング](../guides/filtering.md)に関するガイドを参照してください。

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/properties/PR66a3356c73fc4aabb67ee22caae53d70/callbacks \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

正常な応答は、指定されたプロパティのコールバックのリストを返します。

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

コールバックを検索するには、コールバックリクエストのパスにIDを指定します。GET

**API 形式**

```http
GET /callbacks/{CALLBACK_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `CALLBACK_ID` | 検索するコールバックの`id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/callbacks/CBeef389cee8d84e69acef8665e4dcbef6 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

正常な応答は、コールバックの詳細を返します。

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

新しいコールバックを作成するには、コールバックリクエストをPOSTします。

**API 形式**

```http
POST /properties/{PROPERTY_ID}/callbacks
```

| パラメーター | 説明 |
| --- | --- |
| `PROPERTY_ID` | コールバックを定義する[プロパティ](./properties.md)の`id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X POST \
  https://reactor.adobe.io/properties/PR5e22de986a7c4070965e7546b2bb108d/callbacks \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Content-Type: application/json' \
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
| `url` | コールバックメッセージのURL宛先。 URLは、HTTPSプロトコル拡張を使用する必要があります。 |
| `subscriptions` | コールバックをトリガーする監査イベントタイプを示す、文字列の配列。 使用可能なイベントタイプのリストについては、[監査イベントエンドポイントガイド](./audit-events.md)を参照してください。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、新しく作成されたコールバックの詳細を返します。

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

コールバックを更新するには、コールバックリクエストのパスにIDを含めます。PUT

**API 形式**

```http
PUT /callbacks/{CALLBACK_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `CALLBACK_ID` | 更新するコールバックの`id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、既存のコールバックの`subscriptions`配列を更新します。

```shell
curl -X PUT \
  https://reactor.adobe.io/callbacks/CB4310904d415549888cc9e31ebe1e1e45 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Content-Type: application/json' \
  -d '{
        "data": {
          "attributes": {
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
| `attributes` | コールバック用に更新される属性を表すプロパティを持つオブジェクト。 各キーは、更新する特定のコールバック属性と、更新先の対応する値を表します。<br><br>コールバックで更新できる属性は次のとおりです。<ul><li>`subscriptions`</li><li>`url`</li></ul> |
| `id` | 更新するコールバックの`id`。 これは、リクエストパスで指定された`{CALLBACK_ID}`値と一致する必要があります。 |
| `type` | 更新するリソースのタイプ。 このエンドポイントでは、値を`callbacks`にする必要があります。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、更新されたコールバックの詳細を返します。

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

コールバックを削除するには、コールバックリクエストのパスにIDを含めます。DELETE

**API 形式**

```http
DELETE /callbacks/{CALLBACK_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `CALLBACK_ID` | 削除するコールバックの`id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X DELETE \
  https://reactor.adobe.io/callbacks/CB4310904d415549888cc9e31ebe1e1e45 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
```

**応答**

正常な応答は、応答本文がないHTTPステータス204（コンテンツなし）を返し、コールバックが削除されたことを示します。
