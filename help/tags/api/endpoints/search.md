---
title: 検索エンドポイント
description: Reactor API で /search エンドポイントを呼び出す方法を説明します。
exl-id: 14eb8d8a-3b42-42f3-be87-f39e16d616f4
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: ht
source-wordcount: '658'
ht-degree: 100%

---

# 検索エンドポイント

Reactor API の `/search` エンドポイントは、クエリとして表現された必要な条件に一致するリソースを検索する方法を提供します。

次の API リソースタイプは、API 全体で返されるリソースベースのドキュメントと同じデータ構造を使用して検索できます。

* `audit_events`
* `builds`
* `callbacks`
* `data_elements`
* `environments`
* `extension_packages`
* `extensions`
* `hosts`
* `libraries`
* `properties`
* `rule_components`
* `rules`

すべてのクエリは、現在の会社およびアクセス可能なプロパティを範囲とします。

>[!IMPORTANT]
>
>この検索機能には、次のような注意事項と例外があります。
>* meta は検索できず、検索結果で返されません。
>* 拡張機能パッケージデリゲート（アクション、条件など）のスキーマフィールドは、ネストしたデータ構造としてではなく、テキストとして検索可能です。
>* 現在、範囲クエリは整数のみをサポートしています。


この機能の使用方法について詳しくは、 [検索ガイド](../guides/search.md) を参照してください。

## はじめに

このガイドで使用するエンドポイントは、[Reactor API](https://www.adobe.io/experience-platform-apis/references/reactor/) の一部です。続行する前に、 [はじめる前に](../getting-started.md) で、API への認証方法に関する重要な情報を確認してください。

## 検索の実行 {#perform}

POST リクエストをおこなうことで検索を実行できます。

**API 形式**

```http
POST /search
```

**リクエスト**

```shell
curl -X POST \
  https://reactor.adobe.io/search \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H "Content-Type: application/vnd.api+json" \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -d '{
        "data" : {
          "from": 0,
          "size": 25,
          "query": {
            "attributes.name": {
              "value": "Performance"
            },
            "attributes.revision_number": {
              "range": {
                "lte": "2",
                "gt": "0"
              }
            }
          },
          "sort": [
            {
              "attributes.revision_number": "desc"
            }
          ],
          "resource_types": [
            "data_elements",
            "rule_components"
          ]
        }
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `from` | 応答をオフセットする結果の数。 |
| `size` | 返される結果の最大数。結果は 100 項目を超えることはできません。 |
| `query` | 検索クエリを表すオブジェクト。このオブジェクトの各プロパティでは、キーはクエリを実行するフィールドパスを表し、値はサブプロパティがクエリを実行する内容を決定するオブジェクトである必要があります。<br><br>各フィールドパスで、次のサブプロパティを使用できます。<ul><li>`exists`：リソースにフィールドが存在する場合は、true を返します。</li><li>`value`：フィールドの値がこのプロパティの値と一致する場合は、true を返します。</li><li>`value_operator`：クエリの処理方法を決定するために使用するために使用されるブール論理`value`。指定できる値は、`AND` および `OR` です。除外されている場合、`AND` ロジックが想定されます。 詳しくは、 [値演算子ロジック](#value-operator) の節を参照してください。</li><li>`range` フィールドの値が特定の数値範囲内にある場合は true を返します。範囲自体は、次のサブプロパティによって決まります。<ul><li>`gt`：指定された値より大きい（その値は含まない）。</li><li>`gte`：指定された値以上。</li><li>`lt`：指定された値より小さい（その値は含まない）。</li><li>`lte`：指定された値以下。</li></ul></li></ul> |
| `sort` | 結果を並べ替える順序を示すオブジェクトの配列。 各オブジェクトには、1 つのプロパティを含める必要があります。キーは並べ替えるフィールドのパスを表し、値は並べ替え順（昇順の場合は `asc`、降順の場合は `desc`）を表します。 |
| `resource_types` | 検索する特定のリソースタイプを示す、文字列の配列。 |

{style=&quot;table-layout:auto&quot;}

**応答**

応答が成功すると、クエリに一致するリソースのリストが返されます。API が特定の値に対する一致を決定する方法について詳しくは、 [一致規則](#conventions) に関する付録の節を参照してください。

```json
{
  "data": [
    {
      "id": "DE5d11b3ed301d4ce99b530a5121e392b2",
      "type": "data_elements",
      "attributes": {
        "created_at": "2020-12-14T17:36:09.045Z",
        "deleted_at": null,
        "dirty": true,
        "enabled": true,
        "name": "Performance Indicator",
        "published": false,
        "published_at": null,
        "revision_number": 1,
        "updated_at": "2020-12-14T17:36:09.045Z",
        "clean_text": false,
        "default_value": null,
        "delegate_descriptor_id": "kessel-test::dataElements::dom-attribute",
        "force_lower_case": false,
        "review_status": "unsubmitted",
        "storage_duration": null,
        "settings": "{\"elementProperty\":\"html\",\"elementSelector\":\".target-element\"}"
      },
      "relationships": {
        "libraries": {
          "links": {
            "related": "https://reactor.adobe.io/data_elements/DE5d11b3ed301d4ce99b530a5121e392b2/libraries"
          }
        },
        "revisions": {
          "links": {
            "related": "https://reactor.adobe.io/data_elements/DE5d11b3ed301d4ce99b530a5121e392b2/revisions"
          }
        },
        "notes": {
          "links": {
            "related": "https://reactor.adobe.io/data_elements/DE5d11b3ed301d4ce99b530a5121e392b2/notes"
          }
        },
        "property": {
          "links": {
            "related": "https://reactor.adobe.io/data_elements/DE5d11b3ed301d4ce99b530a5121e392b2/property"
          },
          "data": {
            "id": "PR97d92a379a5f48758947cdf44f607a0d",
            "type": "properties"
          }
        },
        "origin": {
          "links": {
            "related": "https://reactor.adobe.io/data_elements/DE5d11b3ed301d4ce99b530a5121e392b2/origin"
          },
          "data": {
            "id": "DE5d11b3ed301d4ce99b530a5121e392b2",
            "type": "data_elements"
          }
        },
        "extension": {
          "links": {
            "related": "https://reactor.adobe.io/data_elements/DE5d11b3ed301d4ce99b530a5121e392b2/extension"
          },
          "data": {
            "id": "EX0348d463358c4c89afe726245576f112",
            "type": "extensions"
          }
        },
        "updated_with_extension_package": {
          "links": {
            "related": "https://reactor.adobe.io/data_elements/DE5d11b3ed301d4ce99b530a5121e392b2/updated_with_extension_package"
          },
          "data": {
            "id": "EP75db2452065b44e2b8a38ca883ce369a",
            "type": "extension_packages"
          }
        },
        "updated_with_extension": {
          "links": {
            "related": "https://reactor.adobe.io/data_elements/DE5d11b3ed301d4ce99b530a5121e392b2/updated_with_extension"
          },
          "data": {
            "id": "EX1cc78b39339242da82a0e0752fa53375",
            "type": "extensions"
          }
        }
      },
      "links": {
        "property": "https://reactor.adobe.io/properties/PR97d92a379a5f48758947cdf44f607a0d",
        "origin": "https://reactor.adobe.io/data_elements/DE5d11b3ed301d4ce99b530a5121e392b2",
        "self": "https://reactor.adobe.io/data_elements/DE5d11b3ed301d4ce99b530a5121e392b2",
        "extension": "https://reactor.adobe.io/extensions/EX0348d463358c4c89afe726245576f112"
      },
      "meta": {
        "latest_revision_number": 1
      }
    }
  ],
  "meta": {
    "total_hits": 1
  }
}
```

## 付録

次の節では、`/search` エンドポイントの使用に関する追加情報を示します。

### 値演算子ロジック {#value-operator}

検索クエリの値は単語に分割され、インデックス付きのドキュメントと照合されます。各単語の間に `AND` 関係が想定されます。

`AND` を `value_operator` として使用する場合、クエリ値 `My Rule Holiday Sale` は、`My AND Rule AND Holiday AND Sale` を含むフィールドを持つドキュメントとして解釈されます。

`OR` を `value_operator` として使用する場合、クエリ値 `My Rule Holiday Sale` は、`My OR Rule OR Holiday OR Sale` を含むフィールドを持つドキュメントとして解釈されます。一致する用語が多いほど、`match_score` が高くなります。 用語の部分一致の性質上、目的の値に近いものがない場合、文字のテキストなど、非常に基本的なレベルでのみ値が一致する結果セットを取得できます。

### 一致規則 {#conventions}

検索は、指定されたクエリに対してドキュメントがどの程度関連しているかに答えることに関係します。ドキュメントデータを分析し、インデックスを作成する方法は、この問題に直接影響します。

次の表は、一般的なフィールドタイプの一致規則を示しています。

| フィールドタイプ | 一致規則 |
| --- | --- |
| 文字列 | 部分的に用語を分析したテキスト、大文字と小文字を区別しない |
| 列挙値 | 完全一致、大文字と小文字を区別 |
| 整数 | 完全一致 |
| フロート | 完全一致 |
| タイムスタンプ | 完全一致（DateTime 形式） |
| 表示名 | 部分的に用語を分析したテキスト、大文字と小文字を区別しない |

API に表示される特定のフィールドには、追加の規則があります。

| フィールド | 一致規則 |
| --- | --- |
| `id` | 完全一致、大文字と小文字を区別 |
| `delegate_descriptor_id` | 完全一致、大文字小文字を区別、用語を `::` で分割 |
| `name` | 完全一致、大文字と小文字を区別 |
| `settings` | 部分的に用語を分析したテキスト、大文字と小文字を区別しない |
| `type` | 完全一致、大文字と小文字を区別 |
