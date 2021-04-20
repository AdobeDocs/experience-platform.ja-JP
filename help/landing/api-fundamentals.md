---
keywords: Experience Platform；ホーム；人気の高いトピック
solution: Experience Platform
title: Experience Platform API の基本
topic: getting started
description: このドキュメントでは、Experience PlatformAPIに関連する基盤となるテクノロジーと構文の一部について簡単に説明します。
translation-type: tm+mt
source-git-commit: ca5c8527b1b54856aa1e762a06ddbe404f30ec42
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 52%

---


# Experience Platform API の基本

Adobe Experience PlatformのAPIは、JSONベースの[!DNL Platform]リソースを効果的に管理するために理解する必要のある、基礎となるテクノロジーと構文をいくつか採用しています。 このドキュメントでは、これらのテクノロジーの概要のほか、詳細が記載されている外部ドキュメントへのリンクを提供します。

## JSON ポインター {#json-pointer}

JSON ポインターは、JSON ドキュメント内の特定の値を識別するための標準化された文字列構文（[RFC 6901](https://tools.ietf.org/html/rfc6901)）です。JSON ポインターは、`/` 文字で区切られたトークンの文字列であり、オブジェクトのキーまたは配列のインデックスを指定します。トークンは文字列または数値です。JSONポインタ文字列は、このドキュメントで後述するように、[!DNL Platform] APIの多くのPATCH操作で使用されます。 JSON ポインターの詳細については、[JSON ポインターの概要ドキュメント](https://rapidjson.org/md_doc_pointer.html)を参照してください。

### JSON スキーマオブジェクトの例

以下のJSONは、JSONポインタ文字列を使用して参照できるフィールドを持つシンプルなXDMスキーマを表しています。 カスタムミックスイン（`loyaltyLevel`など）を使用して追加されたすべてのフィールドは`_{TENANT_ID}`オブジェクトの下に名前が割り当てられますが、コアミックスイン（`fullName`など）を使用して追加されたフィールドは割り当てられません。

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
| `"/properties/person/properties/name/properties/fullName"` | （コアミックスインが提供する`fullName`フィールドへの参照を返します）。 |
| `"/properties/_{TENANT_ID}/properties/loyaltyLevel"` | （カスタムミックスインで提供される`loyaltyLevel`フィールドへの参照を返します）。 |
| `"/properties/_{TENANT_ID}/properties/loyaltyLevel/enum"` | `["platinum", "gold", "silver", "bronze"]` |
| `"/properties/_{TENANT_ID}/properties/loyaltyLevel/enum/0"` | `"platinum"` |

>[!NOTE]
>
>[!DNL Experience Data Model] (XDM)記述子の`xdm:sourceProperty`および`xdm:destinationProperty`属性を処理する場合、`properties`キーはJSONポインタ文字列から&#x200B;**除外**&#x200B;する必要があります。 詳しくは、[descriptors](../xdm/api/descriptors.md)の[!DNL Schema Registry] API開発者ガイドのサブガイドを参照してください。

## JSON パッチ {#json-patch}

[!DNL Platform] APIのPATCH操作には、要求ペイロードにJSONパッチオブジェクトを受け入れるものが多数あります。 JSON パッチは、JSON ドキュメントの変更を記述するための標準形式（[RFC 6902](https://tools.ietf.org/html/rfc6902)）です。この標準形式では、リクエスト本文でドキュメント全体を送信する必要なく、JSON の部分的なアップデートを定義できます。

### JSON パッチオブジェクトの例

```json
{
  "op": "remove",
  "path": "/foo"
}
```

* `op`：パッチ操作のタイプ。JSONパッチは複数の異なる操作タイプをサポートしますが、[!DNL Platform] APIのPATCH操作はすべての操作タイプと互換性があるわけではありません。 使用可能な操作のタイプは次のとおりです。
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

このドキュメントでは、[!DNL Experience Platform]のJSONベースのリソースの管理に関するテクノロジーと構文の一部を紹介しました。 プラットフォームAPIの操作に関する詳細（ベストプラクティスを含む）については、[はじめに](api-guide.md)を参照してください。 よくある質問に対する回答は、[プラットフォームのトラブルシューティングガイド](troubleshooting.md)を参照してください。