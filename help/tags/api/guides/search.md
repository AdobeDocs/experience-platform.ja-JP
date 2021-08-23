---
title: Reactor API でリソースを検索する
description: Reactor API でリソースを検索する方法について説明します。
source-git-commit: 59592154eeb8592fa171b5488ecb0385e0e59f39
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 100%

---

# Reactor API でリソースを検索する

Reactor API の `/search` エンドポイントを使用すると、保存されたリソースに対して構造化クエリを実行できます。このドキュメントでは、一般的なユースケースにおける検索クエリの例を多数紹介しています。

>[!NOTE]
>
>このガイドをお読みになる前に、[検索エンドポイントガイド](../endpoints/search.md)を参照して、受け入れ可能なクエリ構文とその他の使用ガイドラインを確認してください。

## 基本的なクエリ戦略

以下の例は、API の検索機能を使用する際の基本的な概念を示しています。

### 複数のフィールドをまたいだ検索

複数のフィールドをまたいだ検索を実行するには、フィールド名にワイルドカードを使用します。例えば、複数の属性フィールドをまたいで検索するには、フィールド名として `attributes.*` を使用します。

```json
{
  "data" : {
    "query": {
      "attributes.*": {
        "value": "evar7"
      }
    }
  }
}
```

>[!IMPORTANT]
>
>通常、検索値は、検索するデータのタイプと一致している必要があります。例えば、整数フィールドに対してクエリ値 `evar7` を指定すると失敗します。複数のフィールドをまたいで検索する場合、エラーを避けるためにクエリタイプの要件を緩やかにしますが、望ましくない結果が生じる可能性があります。

### クエリの範囲を特定のリソースタイプに限定する

検索の範囲を特定のリソースタイプに限定するには、リクエストで `resource_types` を指定します。例えば、`data_elements`と `rule_components` で検索するには、次のように指定します。

```json
{
  "data" : {
    "from": 0,
    "size": 25,
    "query": {
      "attributes.display_name": {
        "value": "Performance"
      }
    },
    "resource_types": [
      "data_elements",
      "rule_components"
    ]
  }
}
```

### 応答の並べ替え

`sort` プロパティを使用して応答を並べ替えることができます。例えば、`created_at` で新しい順に並べ替えるには、次のように指定します。

```json
{
  "data" : {
    "from": 0,
    "size": 25,
    "query": {
      "attributes.display_name": {
        "value": "Performance"
      }
    },
    "sort": [
      {
        "attributes.created_at": "desc"
      }
    ],
    "resource_types": [
      "data_elements",
      "rule_components"
    ]
  }
}
```

## 一般的な検索例

次の例は、その他の一般的な検索パターンを示しています。

### 特定の名前を持つリソース

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
          "query": {
            "attributes.name": {
              "value": "Adobe"
            }
          }
        }
      }'
```

### 「evar7」を参照するリソース

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
          "query": {
            "attributes.*": {
              "value": "evar7"
            }
          }
        }
      }'
```

### 「custom-code」デリゲート型のデータ要素

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
          "query": {
            "attributes.delegate_descriptor_id": {
              "value": "custom-code"
            }
          },
          "resource_types": ["data_elements"]
        }
      }'
```

### 特定のデータ要素を参照するルールコンポーネント

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
          "query": {
            "attributes.settings": {
              "value": "myDataElement8"
            }
          },
          "resource_types": ["rule_components"]
        }
      }'
```

### 特定のプロパティ内のルール

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
          "query": {
            "relationships.property.data.id": {
              "value": "PR3cab070a9eb3423894e4a3038ef0e7b7"
            }
          },
          "resource_types": ["rules"]
        }
      }'
```

### ID によるリソースの検索

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
          "query": {
            "id": {
              "value": "PR3cab070a9eb3423894e4a3038ef0e7b7"
            }
          }
        }
      }'
```

### 「OR」用語ロジックを使用した検索の実行

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
          "query": {
            "attributes.display_name": {
              "value": "My Rule Holiday Sale",
              "value_operator: "OR"
            }
          }
        }
      }'
```
