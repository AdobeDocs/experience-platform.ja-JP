---
title: メモエンドポイント
description: Reactor APIで/notesエンドポイントを呼び出す方法を説明します。
source-git-commit: 6a1728bd995137a7cd6dc79313762ae6e665d416
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 8%

---

# メモエンドポイント

Reactor APIでは、メモは、特定のリソースに追加できるテキスト注釈です。 メモとは、基本的に、それぞれのリソースに関するコメントです。 メモの内容は、リソースの動作に影響を与えず、次のような様々な使用例に使用できます。

* 背景情報の提供
* TODOリストとしての機能
* リソース使用状況に関するアドバイスを渡す
* 他のチームメンバーに指示を与える
* 履歴コンテキストの記録

Reactor APIの`/notes`エンドポイントを使用すると、これらのノートをプログラムで管理できます。

メモは、次のリソースに適用できます。

* [データ要素](./data-elements.md)
* [拡張機能](./extensions.md)
* [ライブラリ](./libraries.md)
* [プロパティ](./properties.md)
* [ルールコンポーネント](./rule-components.md)
* [ルール](./rules.md)

これら6種類をまとめて「注目すべき」リソースと呼びます。 注目すべきリソースを削除すると、関連付けられたメモも削除されます。

>[!NOTE]
>
>複数のリビジョンを持つことができるリソースの場合は、現在の（ヘッド）リビジョンでメモを作成する必要があります。 他のリビジョンには添付できない場合があります。
>
>ただし、メモは引き続きリビジョンから読み取ることができます。 この場合、APIはリビジョンの作成前に存在したメモのみを返します。 リビジョンが切り取られたときと同じように、アノテーションのスナップショットが表示されます。 これに対し、現在の(head)リビジョンからメモを読むと、すべてのメモが返されます。

## はじめに

このガイドで使用するエンドポイントは、[Reactor API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/reactor.yaml)の一部です。 続行する前に、APIへの認証方法に関する重要な情報について、[はじめにのガイド](../getting-started.md)を参照してください。

## メモのリストの取得 {#list}

リソースのメモのリストを取得するには、該当するリソースのGETリクエストのパスに`/notes`を追加します。

**API 形式**

```http
GET /{RESOURCE_TYPE}/{RESOURCE_ID}/notes
```

| パラメーター | 説明 |
| --- | --- |
| `RESOURCE_TYPE` | メモを取得するリソースのタイプ。 次のいずれかの値にする必要があります。 <ul><li>`data_elements`</li><li>`extensions`</li><li>`libraries`</li><li>`properties`</li><li>`rule_components`</li><li>`rules`</li></ul> |
| `RESOURCE_ID` | メモを一覧表示する特定のリソースの`id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストでは、ライブラリに添付されているメモの一覧を示します。

```shell
curl -X GET \
  https://reactor.adobe.io/libraries/LBcffea1a38c52408cae2398868625a12d/notes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

正常な応答は、指定されたリソースに添付されたメモのリストを返します。

```json
{
  "data": [
    {
      "id": "NTa40de8d76bfd4e40835830900ce83b7b",
      "type": "notes",
      "attributes": {
        "author_display_name": "John Smith",
        "author_email": "jsmith@example.com",
        "created_at": "2020-12-14T17:51:00.411Z",
        "text": "this is a note on a library"
      },
      "relationships": {
        "resource": {
          "links": {
            "related": "https://reactor.adobe.io/libraries/LBcffea1a38c52408cae2398868625a12d"
          },
          "data": {
            "id": "LBcffea1a38c52408cae2398868625a12d",
            "type": "libraries"
          }
        }
      },
      "links": {
        "resource": "https://reactor.adobe.io/libraries/LBcffea1a38c52408cae2398868625a12d",
        "self": "https://reactor.adobe.io/notes/NTa40de8d76bfd4e40835830900ce83b7b"
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

## メモの検索 {#lookup}

メモを検索するには、メモリクエストのパスにIDを指定します。GETリクエストのパスにIDを指定します。

**API 形式**

```http
GET /notes/{NOTE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `NOTE_ID` | 検索するメモの`id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X GET \
  https://reactor.adobe.io/notes/NT550b7a17ab304d49ba289a2978d673e5 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Accept: application/vnd.api+json;revision=1'
```

**応答**

正常な応答は、メモの詳細を返します。

```json
{
  "data": {
    "id": "NT550b7a17ab304d49ba289a2978d673e5",
    "type": "notes",
    "attributes": {
      "author_display_name": "John Smith",
      "author_email": "jsmith@example.com",
      "created_at": "2020-12-14T17:51:10.316Z",
      "text": "this is a note on a property"
    },
    "relationships": {
      "resource": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR4537ac6f1f204ffd864ec47c4b27c2e8"
        },
        "data": {
          "id": "PR4537ac6f1f204ffd864ec47c4b27c2e8",
          "type": "properties"
        }
      }
    },
    "links": {
      "resource": "https://reactor.adobe.io/properties/PR4537ac6f1f204ffd864ec47c4b27c2e8",
      "self": "https://reactor.adobe.io/notes/NT550b7a17ab304d49ba289a2978d673e5"
    }
  }
}
```

## メモの作成 {#create}

>[!WARNING]
>
>新しいメモを作成する前に、メモは編集できないので、メモを削除するには、対応するリソースを削除するだけです。

新しいメモを作成するには、該当するリソースのPOSTリクエストのパスに`/notes`を追加します。

**API 形式**

```http
POST /{RESOURCE_TYPE}/{RESOURCE_ID}/notes
```

| パラメーター | 説明 |
| --- | --- |
| `RESOURCE_TYPE` | メモを作成するリソースのタイプ。 次のいずれかの値にする必要があります。 <ul><li>`data_elements`</li><li>`extensions`</li><li>`libraries`</li><li>`properties`</li><li>`rule_components`</li><li>`rules`</li></ul> |
| `RESOURCE_ID` | メモを作成する特定のリソースの`id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、プロパティの新しいメモを作成します。

```shell
curl -X POST \
  https://reactor.adobe.io/properties/PRb25a704c0b7c4562835ccdf96d3afd31/notes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Content-Type: application/json' \
  -d '{
        "data": {
          "type": "notes",
          "attributes": {
            "text": "this is a note on a property"
          }
        }
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `type` | **（必須）** 更新するリソースのタイプ。このエンドポイントでは、値を`notes`にする必要があります。 |
| `attributes.text` | **（必須）** メモを含むテキスト。各メモは、512文字のUnicode文字に制限されています。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、新しく作成されたメモの詳細を返します。

```json
{
  "data": {
    "id": "NT550b7a17ab304d49ba289a2978d673e5",
    "type": "notes",
    "attributes": {
      "author_display_name": "John Smith",
      "author_email": "jsmith@example.com",
      "created_at": "2020-12-14T17:51:10.316Z",
      "text": "This is a note on a property"
    },
    "relationships": {
      "resource": {
        "links": {
          "related": "https://reactor.adobe.io/properties/PR4537ac6f1f204ffd864ec47c4b27c2e8"
        },
        "data": {
          "id": "PR4537ac6f1f204ffd864ec47c4b27c2e8",
          "type": "properties"
        }
      }
    },
    "links": {
      "resource": "https://reactor.adobe.io/properties/PR4537ac6f1f204ffd864ec47c4b27c2e8",
      "self": "https://reactor.adobe.io/notes/NT550b7a17ab304d49ba289a2978d673e5"
    }
  }
}
```
