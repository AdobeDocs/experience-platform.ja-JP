---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Experience Platform API の基本
topic-legacy: getting started
description: このドキュメントでは、Experience PlatformAPI に関連する基盤となるテクノロジーと構文の概要を簡単に説明します。
exl-id: cd69ba48-f78c-4da5-80d1-efab5f508756
source-git-commit: d425dcd9caf8fccd0cb35e1bac73950a6042a0f8
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 53%

---

# Experience Platform API の基本

Adobe Experience Platform API は、JSON ベースの [!DNL Platform] リソースを効果的に管理するために理解する必要がある、基盤となる複数のテクノロジーと構文を使用しています。 このドキュメントでは、これらのテクノロジーの概要のほか、詳細が記載されている外部ドキュメントへのリンクを提供します。

## JSON ポインター {#json-pointer}

JSON ポインターは、JSON ドキュメント内の特定の値を識別するための標準化された文字列構文（[RFC 6901](https://tools.ietf.org/html/rfc6901)）です。JSON ポインターは、`/` 文字で区切られたトークンの文字列であり、オブジェクトのキーまたは配列のインデックスを指定します。トークンは文字列または数値です。JSON ポインター文字列は、このドキュメントで後述するように、[!DNL Platform] API の多くのPATCH操作で使用されます。 JSON ポインターの詳細については、[JSON ポインターの概要ドキュメント](https://rapidjson.org/md_doc_pointer.html)を参照してください。

### JSON スキーマオブジェクトの例

次の JSON は、JSON ポインター文字列を使用してフィールドを参照できる、シンプルな XDM スキーマを表しています。 カスタムスキーマフィールドグループ（`loyaltyLevel` など）を使用して追加したフィールドはすべて `_{TENANT_ID}` オブジェクトの下に名前空間化されますが、コアフィールドグループ（`fullName` など）を使用して追加したフィールドは名前空間化されません。

```json
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/85a4bdaa168b01bf44384e049fbd3d2e9b2ffaca440d35b9",
  "meta:altId": "_{TENANT_ID}.schemas.85a4bdaa168b01bf44384e049fbd3d2e9b2ffaca440d35b9",
  "meta:resourceType": "schemas",
  "version": "1.0",
  "title": "Example schema",
  "type": "object",
  "description": "This is an example schema.",
  "properties": {
    "_{TENANT_ID}": {
      "type": "object",
      "properties": {
        "loyaltyLevel": {
          "title": "Loyalty Level",
          "description": "",
          "type": "string",
          "isRequired": false,
          "enum": [
            "platinum",
            "gold",
            "silver",
            "bronze"
          ]
        }
      }
    },
    "person": {
      "title": "Person",
      "description": "An individual actor, contact, or owner.",
      "type": "object",
      "properties": {
        "name": {
          "title": "Full name",
          "description": "The person's full name.",
          "type": "object",
          "properties": {
            "fullName": {
              "title": "Full name",
              "type": "string",
              "description": "The full name of the person, in writing order most commonly accepted in the language of the name.",
            },
            "suffix": {
              "title": "Suffix",
              "type": "string",
              "description": "A group of letters provided after a person's name to provide additional information. The `suffix` is used at the end of someones name. For example Jr., Sr., M.D., PhD, I, II, III, etc.",
            }
          },
          "meta:referencedFrom": "https://ns.adobe.com/xdm/context/person-name",
          "meta:xdmField": "xdm:name"
        }
      }
    }
  }
}
```

### スキーマオブジェクトに基づいた JSON ポインターの例

| JSON ポインター | 解決先 |
| --- | --- |
| `"/title"` | `"Example schema"` |
| `"/properties/person/properties/name/properties/fullName"` | （`fullName` フィールドへの参照を返します。このフィールドは、コアフィールドグループによって指定されます）。 |
| `"/properties/_{TENANT_ID}/properties/loyaltyLevel"` | （`loyaltyLevel` フィールドへの参照を返します。このフィールドは、カスタムフィールドグループで指定されます）。 |
| `"/properties/_{TENANT_ID}/properties/loyaltyLevel/enum"` | `["platinum", "gold", "silver", "bronze"]` |
| `"/properties/_{TENANT_ID}/properties/loyaltyLevel/enum/0"` | `"platinum"` |

>[!NOTE]
>
>[!DNL Experience Data Model] (XDM) 記述子の `xdm:sourceProperty` 属性と `xdm:destinationProperty` 属性を扱う場合、 `properties` キーは JSON ポインター文字列から **除外** する必要があります。 詳しくは、[ 記述子 ](../xdm/api/descriptors.md) の [!DNL Schema Registry] API 開発者ガイドを参照してください。

## JSON パッチ {#json-patch}

[!DNL Platform] API のPATCH操作の多くは、リクエストペイロード用の JSON パッチオブジェクトを受け入れます。 JSON パッチは、JSON ドキュメントの変更を記述するための標準形式（[RFC 6902](https://tools.ietf.org/html/rfc6902)）です。この標準形式では、リクエスト本文でドキュメント全体を送信する必要なく、JSON の部分的なアップデートを定義できます。

### JSON パッチオブジェクトの例

```json
{
  "op": "remove",
  "path": "/foo"
}
```

* `op`：パッチ操作のタイプ。JSON パッチは複数の異なる操作タイプをサポートしますが、[!DNL Platform] API のすべてのPATCH操作がすべての操作タイプと互換性があるわけではありません。 使用可能な操作のタイプは次のとおりです。
   * `add`
   * `remove`
   * `replace`
   * `copy`
   * `move`
   * `test`
* `path`：JSON 構造のアップデートされる部分。[JSON ポインター](#json-pointer)表記を使用して識別されます。

`op` で示されている操作タイプによっては、JSON パッチオブジェクトに追加のプロパティが必要な場合があります。JSON パッチの様々な操作と必要な構文の詳細については、[JSON パッチのドキュメント](http://jsonpatch.com/)を参照してください。

## JSON スキーマ {#json-schema}

JSON スキーマは、JSON データの構造を記述して検証するために使用される形式です。[Experience Data Model （XDM）](../xdm/home.md)では、JSON スキーマ機能を利用して、取得される顧客体験データの構造と形式に制約を適用します。JSON スキーマの詳細については、[公式のドキュメント](https://json-schema.org/)を参照してください。

## 次の手順

このドキュメントでは、[!DNL Experience Platform] の JSON ベースのリソース管理に関連するいくつかのテクノロジーと構文について説明しました。 ベストプラクティスを含む、Platform API の使用に関する詳細については、[ 入門ガイド ](api-guide.md) を参照してください。 よくある質問に対する回答は、[Platform トラブルシューティングガイド ](troubleshooting.md) を参照してください。
