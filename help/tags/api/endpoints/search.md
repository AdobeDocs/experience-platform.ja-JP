---
title: 検索エンドポイント
description: Reactor API で /search エンドポイントを呼び出す方法を説明します。
source-git-commit: 53612919dc040a8a3ad35a3c5c0991554ffbea7c
workflow-type: tm+mt
source-wordcount: '662'
ht-degree: 98%

---

# 検索エンドポイント

Reactor API の `/search` エンドポイントは、クエリとして表現された、目的の条件に一致するリソースを見つける方法を提供します。

次の API リソースタイプは、API 全体で返されるリソースベースのドキュメントと同じデータ構造を活用して検索できます。

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

すべてのクエリの範囲は、現在の会社およびアクセス可能なプロパティになります。

>[!IMPORTANT]
>
>検索機能には、次のような注意事項と例外があります。
>* meta は検索できず、検索結果にも返されません。
>* 拡張パッケージデリゲートのスキーマフィールド（アクション、条件など）は、ネストされたデータ構造としてではなく、テキストとして検索可能です。
>* 現在、範囲クエリは整数のみをサポートしています。


この機能の使用方法について詳しくは、[検索ガイド](../guides/search.md)を参照してください。

## はじめに

このガイドで使用するエンドポイントは、[Reactor API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/reactor.yaml) の一部です。続行する前に、API への認証方法に関する重要な情報について、[はじめる前に](../getting-started.md)を確認してください。

## 検索の実行 {#perform}

検索は、検索リクエストを実行することでPOSTできます。

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
| `query` | 検索クエリを表すオブジェクト。このオブジェクトの各プロパティでは、キーはクエリを実行するフィールドパスを表し、値はサブプロパティがクエリを実行する内容を決定するオブジェクトである必要があります。<br><br>各フィールドパスで、次のサブプロパティを使用できます。<ul><li>`exists`：リソースにフィールドが存在する場合、true を返します。</li><li>`value`：フィールドの値がこのプロパティの値と一致する場合、true を返します。</li><li>`value_operator`：`value` クエリの処理方法を決定するために使用されるブール論理。指定できる値は、`AND` および `OR` です。除外する場合、`AND` ロジックが想定されます。詳しくは、[値演算子ロジック](#value-operator)の節を参照してください。</li><li>`range` フィールドの値が特定の数値範囲内にある場合、true を返します。範囲自体は、次のサブプロパティによって決定されます。<ul><li>`gt`：指定された値より大きい（指定された値を含まない）。</li><li>`gte`：指定された値以上。</li><li>`lt`:指定された値未満（指定された値を含まない）。</li><li>`lte`：指定された値以下。</li></ul></li></ul> |
| `sort` | 結果を並べ替える順序を示すオブジェクトの配列。各オブジェクトには単一のプロパティが含まれている必要があります。キーは並べ替えるフィールドパスを表し、値は並べ替え順序（昇順の場合は `asc`、降順の場合は `desc`）を表します。 |
| `resource_types` | 検索する特定のリソースタイプを示す文字列の配列。 |

{style=&quot;table-layout:auto&quot;}

**応答**

応答が成功すると、クエリに一致するリソースのリストが返されます。 API が特定の値に対する一致を決定する方法について詳しくは、[一致規則](#conventions)に関する付録の節を参照してください。

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

次の節では、`/search` エンドポイントの使用に関する追加情報を説明します。

### 値演算子ロジック {#value-operator}

検索クエリの値を用語に分割し、インデックス付きのドキュメントと照合します。各用語の間には、`AND` の関係が想定されています。

`AND` を `value_operator` として使用する場合、クエリ値 `My Rule Holiday Sale` は、`My AND Rule AND Holiday AND Sale` を含むフィールドを持つドキュメントとして解釈されます。

`OR` を `value_operator` として使用する場合、クエリ値 `My Rule Holiday Sale` は、`My OR Rule OR Holiday OR Sale` を含むフィールドを持つドキュメントとして解釈されます。 一致する用語が多いほど、`match_score` が高くなります。 用語の部分一致の性質上、目的の値に近いものがない場合、数文字のテキストなど、非常に基本的なレベルでのみ値が一致する結果セットを取得できます。

### 一致規則 {#conventions}

検索は、指定されたクエリに対してドキュメントがどの程度関連しているかに答えることに関係します。ドキュメントデータの分析方法とインデックスの作成方法は、この問題に直接影響します。

次の表に、一般的なフィールドタイプの一致規則を示します。

| フィールドタイプ | 一致規則 |
| --- | --- |
| 文字列 | 部分的な用語分析を含むテキスト（大文字と小文字を区別せず） |
| 列挙値 | 完全一致（大文字と小文字を区別） |
| 整数 | 完全一致 |
| フロート | 完全一致 |
| タイムスタンプ | 完全一致（DateTime 形式） |
| 表示名 | 部分的な用語分析を含むテキスト（大文字と小文字を区別せず） |

API に表示される特定のフィールドには、次のような追加の規則があります。

| フィールド | 一致規則 |
| --- | --- |
| `id` | 完全一致（大文字と小文字を区別） |
| `delegate_descriptor_id` | 完全一致、大文字小文字を区別、`::` で分割された用語を含む |
| `name` | 完全一致（大文字と小文字を区別） |
| `settings` | 部分的な用語分析を含むテキスト（大文字と小文字を区別せず） |
| `type` | 完全一致（大文字と小文字を区別） |