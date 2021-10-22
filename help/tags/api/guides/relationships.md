---
title: Reactor API における関係
description: 各リソースの関係要件など、Reactor API でリソースの関係が確立される方法を説明します。
exl-id: 23976978-a639-4eef-91b6-380a29ec1c14
source-git-commit: 7e4bc716e61b33563e0cb8059cb9f1332af7fd36
workflow-type: tm+mt
source-wordcount: '807'
ht-degree: 99%

---

# Reactor API における関係

Reactor API のリソースは、多くの場合、相互に関連しています。このドキュメントでは、API でのリソースの関係の確立方法と、各リソースタイプの関係要件の概要を説明します。

問題のリソースのタイプによっては、いくつかの関係が必要です。必須の関係は、親リソースがその関係なしでは存在できないことを意味します。その他の関係はオプションです。

関係は、必須かオプションかに関係なく、関連するリソースが作成されるときにシステムによって自動的に確立されるか、手動で作成する必要があります。関係を手動で作成する場合、問題のリソースに応じて 2 つの方法が考えられます。

* [ペイロード別に作成](#payload)
* [URL で作成](#url)（ライブラリのみ）

各リソースタイプの互換性のある関係のリストと、該当する場合はそれらの関係を確立するために必要な方法については、 [関係要件](#requirements) の節を参照してください。

## ペイロード別の関係の作成 {#payload}

リソースを最初に作成する際に、手動で関係を確立する必要がある場合があります。これを実現するには、最初に親リソースを作成する際に、リクエストペイロードに `relationship` オブジェクトを指定する必要があります。これらの関係の例を次に示します。

* 必要な拡張機能を持つ[データ要素の作成](../endpoints/data-elements.md#create)
* 必要なホストの関係を持つ[環境の作成](../endpoints/environments.md#create)

**API 形式**

```http
POST /properties/{PROPERTY_ID}/{RESOURCE_TYPE}
```

| パラメーター | 説明 |
| --- | --- |
| `{PROPERTY_ID}` | リソースが属するプロパティの ID。 |
| `{RESOURCE_TYPE}` | 作成するリソースのタイプ。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、新しい `rule_component` を作成し、`rules` と `extension` との関係を確立します。

```shell
curl -X POST \
  https://reactor.adobe.io/properties/PRf606dbddfbdc44f580fc6f342b5ff9be/rule_components \
  -H 'Authorization: Bearer [TOKEN]' \
  -H 'x-api-key: [KEY]' \
  -H 'x-gw-ims-org-id: [ORG_ID]' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
        "data": {
          "attributes": {
            "delegate_descriptor_id": "kessel-test::events::click",
            "name": "My Example Click Event",
            "settings": "{\"elementSelector\":\".accordion\",\"bubbleFireIfChildFired\":true}"
          },
          "relationships": {
            "extension": {
              "data": {
                "id": "EXa2865f4d14204fa094f247406424371b",
                "type": "extensions"
              }
            },
            "rules": {
              "data": [
                {
                  "id": "RLd53598e3f1884e63bbc8e9c95e463dcf",
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
| `relationships` | ペイロードで関係を作成する際に指定する必要があるオブジェクト。このオブジェクトの各キーは、特定の関係タイプを表します。 上記の例では、`rule_components` に固有の `extension` と `rules` の関係が確立されています。様々なリソースの互換性のある関係タイプの詳細については、 [リソース別の関係要件](#relationship-requirements-by-resource) の節を参照してください。 |
| `data` | `relationship` オブジェクトの下で提供される各関係タイプには、`data` プロパティが必要です。このプロパティは、関係が確立されるリソースの `id` と `type` を参照します。`data` プロパティをオブジェクトの配列としてフォーマットし、各オブジェクトに `id` と `type` を含めることで、同じタイプの複数のリソースとの関係を作成できます。 |
| `id` | リソースの一意の ID。 各 `id` には、問題のリソースのタイプを示す兄弟 `type` プロパティを付ける必要があります。 |
| `type` | 兄弟 `id` フィールドによって参照されるリソースのタイプ。 許容値としては、`data_elements`、`rules`、`extensions`、`environments` などがあります。 |

{style = &quot;テーブル-layout: auto&quot;}

## URL による関係の作成 {#url}

他のリソースとは異なり、ライブラリは独自の専用の `/relationship` エンドポイントを通じて関係を確立します。以下に例を示します。

* [ライブラリへの拡張機能、データ要素、ルールの追加](../endpoints/libraries.md#add-resources)
* [環境へのライブラリの割り当て](../endpoints/libraries.md#environment)

**API 形式**

```http
POST /properties/{PROPERTY_ID}/libraries/{LIBRARY_ID}/relationships/{RESOURCE_TYPE}
```

| パラメーター | 説明 |
| --- | --- |
| `{PROPERTY_ID}` | ライブラリが属するプロパティの ID。 |
| `{LIBRARY_ID}` | 関係を作成するライブラリの ID。 |
| `{RESOURCE_TYPE}` | 関係がターゲットとするリソースのタイプ。 使用可能な値は、`environment`、`data_elements`、`extensions`、`rules` などです。 |

**リクエスト**

次のリクエストでは、ライブラリの `/relationships/environment` エンドポイントを使用して、環境との関係を作成します。

```shell
curl -X POST \
  https://reactor.adobe.io/properties/PRf606dbddfbdc44f580fc6f342b5ff9be/libraries/LB10c1fd171cd347f19fcb8659a8d679ef/relationships/environment \
  -H 'Authorization: Bearer [TOKEN]' \
  -H 'x-api-key: [KEY]' \
  -H 'x-gw-ims-org-id: [ORG_ID]' \
  -H 'Accept: application/vnd.api+json;revision=1' \
  -H 'Content-Type: application/vnd.api+json' \
  -d '{
        "data": {
          "id": "ENf395a477d2b24ad696d65b901055b9dc",
          "type": "environments",
        }
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `data` | 関係のターゲットリソースの `id` と `type` を参照するオブジェクト。同じタイプの複数のリソース（`extensions` や `rules` など）との関係を作成する場合は、`data` プロパティをオブジェクトの配列としてフォーマットし、各オブジェクトに適用可能なリソースの `id` と `type` を含める必要があります。 |
| `id` | リソースの一意の ID。 各 `id` には、問題のリソースのタイプを示す兄弟 `type` プロパティを付ける必要があります。 |
| `type` | 兄弟 `id` フィールドによって参照されるリソースのタイプ。 許容値としては、`data_elements`、`rules`、`extensions`、`environments` などがあります。 |

{style = &quot;テーブル-layout: auto&quot;}

## リソース別の関係要件 {#requirements}

次の表は、各リソースタイプで使用可能な関係、それらの関係が必要かどうか、および該当する場合は手動で関係を作成するための許可された方法の概要を示しています。

>[!NOTE]
>
>関係がペイロードまたは URL で作成されたものとしてリストされない場合、システムによって自動的に割り当てられます。

### 監査イベント

| 関係 | 必須 | ペイロード別に作成 | URL で作成 |
| :--- | :---: | :---: | :---: |
| `property` | ✓ |  |  |
| `entity` | ✓ |  |  |

{style = &quot;テーブル-layout: auto&quot;}

### ビルド

| 関係 | 必須 | ペイロード別に作成 | URL で作成 |
| :--- | :---: | :---: | :---: |
| `data_elements` |  |  |  |
| `extensions` |  |  |  |
| `rules` |  |  |  |
| `environment` | ✓ |  |  |
| `library` | ✓ |  |  |
| `property` | ✓ |  |  |

{style = &quot;テーブル-layout: auto&quot;}

### コールバック

| 関係 | 必須 | ペイロード別に作成 | URL で作成 |
| :--- | :---: | :---: | :---: |
| `property` | ✓ |  |  |

{style = &quot;テーブル-layout: auto&quot;}

### 会社

| 関係 | 必須 | ペイロード別に作成 | URL で作成 |
| :--- | :---: | :---: | :---: |
| `properties` |  |  |  |

{style = &quot;テーブル-layout: auto&quot;}

### データ要素

| 関係 | 必須 | ペイロード別に作成 | URL で作成 |
| :--- | :---: | :---: | :---: |
| `libraries` |  |  |  |
| `revisions` | ✓ |  |  |
| `notes` |  |  |  |
| `property` | ✓ |  |  |
| `origin` | ✓ |  |  |
| `extension` | ✓ | ✓ |  |
| `updated_with_extension` | ✓ |  |  |
| `updated_with_extension_package` | ✓ |  |  |

{style = &quot;テーブル-layout: auto&quot;}

### 環境

| 関係 | 必須 | ペイロード別に作成 | URL で作成 |
| :--- | :---: | :---: | :---: |
| `library` |  |  |  |
| `builds` |  |  |  |
| `host` | ✓ | ✓ |  |
| `property` | ✓ |  |  |

{style = &quot;テーブル-layout: auto&quot;}

### 拡張機能

| 関係 | 必須 | ペイロード別に作成 | URL で作成 |
| :--- | :---: | :---: | :---: |
| `libraries` |  |  |  |
| `revisions` | ✓ |  |  |
| `notes` |  |  |  |
| `property` | ✓ |  |  |
| `origin` | ✓ |  |  |
| `extension_package` | ✓ | ✓ |  |
| `updated_with_extension_package` | ✓ |  |  |

{style = &quot;テーブル-layout: auto&quot;}

### ホスト

| 関係 | 必須 | ペイロード別に作成 | URL で作成 |
| :--- | :---: | :---: | :---: |
| `property` | ✓ |  |  |

{style = &quot;テーブル-layout: auto&quot;}

### ライブラリ

| 関係 | 必須 | ペイロード別に作成 | URL で作成 |
| :--- | :---: | :---: | :---: |
| `builds` |  |  |  |
| `environment` |  |  | ✓ |
| `data_elements` |  |  | ✓ |
| `extensions` |  |  | ✓ |
| `rules` |  |  | ✓ |
| `notes` |  |  |  |
| `upstream_library` | ✓ |  |  |
| `property` | ✓ |  |  |
| `last_build` |  |  |  |

{style = &quot;テーブル-layout: auto&quot;}

### 備考

| 関係 | 必須 | ペイロード別に作成 | URL で作成 |
| :--- | :---: | :---: | :---: |
| `resource` | ✓ |  |  |

{style = &quot;テーブル-layout: auto&quot;}

### プロパティ

| 関係 | 必須 | ペイロード別に作成 | URL で作成 |
| :--- | :---: | :---: | :---: |
| `company` | ✓ |  |  |
| `callbacks` |  |  |  |
| `environments` |  |  |  |
| `libraries` |  |  |  |
| `data_elements` |  |  |  |
| `extensions` |  |  |  |
| `extensions` |  |  |  |

{style = &quot;テーブル-layout: auto&quot;}

### ルールコンポーネント

| 関係 | 必須 | ペイロード別に作成 | URL で作成 |
| :--- | :---: | :---: | :---: |
| `updated_with_extensions_package` | ✓ |  |  |
| `updated_with_extension` | ✓ |  |  |
| `extension` | ✓ | ✓ |  |
| `notes` |  |  |  |
| `origin` | ✓ |  |  |
| `property` | ✓ |  |  |
| `rules` | ✓ | ✓ |  |
| `revisions` | ✓ |  |  |

{style = &quot;テーブル-layout: auto&quot;}

### ルール

| 関係 | 必須 | ペイロード別に作成 | URL で作成 |
| :--- | :---: | :---: | :---: |
| `libraries` |  |  |  |
| `revisions` | ✓ |  |  |
| `notes` |  |  |  |
| `property` | ✓ |  |  |
| `origin` | ✓ |  |  |
| `rule_components` |  |  |  |

### 機密

| 関係 | 必須 | ペイロード別に作成 | URL で作成 |
| :--- | :---: | :---: | :---: |
| `property` | ✓ |  | ✓ |
| `environment` | ✓ | ✓ |  |

