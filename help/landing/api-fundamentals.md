---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Experience Platform API の基本事項
description: このドキュメントでは、Experience Platform API に関連する基盤となるテクノロジーと構文のいくつかについて簡単に説明します。
role: Developer
feature: API
exl-id: cd69ba48-f78c-4da5-80d1-efab5f508756
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 50%

---

# Experience Platform API の基本事項

Adobe Experience Platform API では、JSON ベースの [!DNL Experience Platform] リソースを効果的に管理するために理解することが重要な、いくつかの基本となるテクノロジーと構文を使用します。 このドキュメントでは、これらのテクノロジーの概要のほか、詳細が記載されている外部ドキュメントへのリンクを提供します。

## JSON ポインター {#json-pointer}

JSON ポインターは、JSON ドキュメント内の特定の値を識別するための標準化された文字列構文（[RFC 6901](https://tools.ietf.org/html/rfc6901)）です。JSON ポインターは、`/` 文字で区切られたトークンの文字列であり、オブジェクトのキーまたは配列のインデックスを指定します。トークンは文字列または数値です。JSON ポインター文字列は、このドキュメントで後述するように、[!DNL Experience Platform] API の多くのPATCH操作で使用されます。 JSON ポインターの詳細については、[JSON ポインターの概要ドキュメント](https://rapidjson.org/md_doc_pointer.html)を参照してください。

### JSON スキーマオブジェクトの例

次の JSON は、JSON ポインター文字列を使用してフィールドを参照できる、簡略化された XDM スキーマを表しています。 カスタムスキーマフィールドグループ（`loyaltyLevel` など）を使用して追加されたすべてのフィールドは `_{TENANT_ID}` オブジェクトの下に名前空間が設定され、コアフィールドグループ（`fullName` など）を使用して追加されたフィールドは名前空間が設定されないことに注意してください。

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
| `"/properties/person/properties/name/properties/fullName"` | （コアフィールドグループが提供する `fullName` フィールドへの参照を返します）。 |
| `"/properties/_{TENANT_ID}/properties/loyaltyLevel"` | （カスタムフィールドグループが提供する `loyaltyLevel` フィールドへの参照を返します）。 |
| `"/properties/_{TENANT_ID}/properties/loyaltyLevel/enum"` | `["platinum", "gold", "silver", "bronze"]` |
| `"/properties/_{TENANT_ID}/properties/loyaltyLevel/enum/0"` | `"platinum"` |

>[!NOTE]
>
>[!DNL Experience Data Model] （XDM）記述子の `xdm:sourceProperty` および `xdm:destinationProperty` 属性を処理する場合、`properties` キーはすべて JSON ポインター文字列から **除外** する必要があります。 詳しくは、[!DNL Schema Registry] API 開発者ガイドの [&#x200B; 記述子 &#x200B;](../xdm/api/descriptors.md) に関するサブガイドを参照してください。

## JSON パッチ {#json-patch}

リクエストペイロードに対して JSON Patch オブジェクトを受け入れる [!DNL Experience Platform] API には、多くのPATCH操作があります。 JSON パッチは、JSON ドキュメントの変更を記述するための標準形式（[RFC 6902](https://tools.ietf.org/html/rfc6902)）です。この標準形式では、リクエスト本文でドキュメント全体を送信する必要なく、JSON の部分的なアップデートを定義できます。

### JSON パッチオブジェクトの例

```json
{
  "op": "remove",
  "path": "/foo"
}
```

* `op`：パッチ操作のタイプ。JSON パッチでは複数の異なる操作タイプがサポートされますが、[!DNL Experience Platform] API のすべてのPATCH操作がすべての操作タイプと互換性があるわけではありません。 使用可能な操作のタイプは次のとおりです。
   * `add`
   * `remove`
   * `replace`
   * `copy`
   * `move`
   * `test`
* `path`：JSON 構造のアップデートされる部分。[JSON ポインター](#json-pointer)表記を使用して識別されます。

`op` で示されている操作タイプによっては、JSON パッチオブジェクトに追加のプロパティが必要な場合があります。JSON パッチの様々な操作と必要な構文の詳細については、[JSON パッチのドキュメント](https://datatracker.ietf.org/doc/html/rfc6902)を参照してください。

## JSON スキーマ {#json-schema}

JSON スキーマは、JSON データの構造を記述して検証するために使用される形式です。[Experience Data Model （XDM）](../xdm/home.md)では、JSON スキーマ機能を利用して、取得される顧客体験データの構造と形式に制約を適用します。JSON スキーマの詳細については、[公式のドキュメント](https://json-schema.org/)を参照してください。

## 次の手順

このドキュメントでは、[!DNL Experience Platform] 用の JSON ベースのリソースの管理に関連するテクノロジーと構文の一部を紹介しました。 ベストプラクティスなど、Experience Platform API の操作について詳しくは、[&#x200B; はじめる前に &#x200B;](api-guide.md) を参照してください。 よくある質問への回答については、[Experience Platform トラブルシューティングガイド &#x200B;](troubleshooting.md) を参照してください。
