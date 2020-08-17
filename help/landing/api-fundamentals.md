---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform API の基本
topic: getting started
translation-type: tm+mt
source-git-commit: fa439ebb9d02d4a08c8ed92b18f2db819d089174
workflow-type: tm+mt
source-wordcount: '422'
ht-degree: 66%

---


# Adobe Experience Platform API の基本

Adobe Experience Platform APIs employ several underlying technologies and syntaxes that are important to understand in order to effectively manage JSON-based [!DNL Platform] resources. このドキュメントでは、これらのテクノロジーの概要のほか、詳細が記載されている外部ドキュメントへのリンクを提供します。

## JSON ポインター {#json-pointer}

JSON ポインターは、JSON ドキュメント内の特定の値を識別するための標準化された文字列構文（[RFC 6901](https://tools.ietf.org/html/rfc6901)）です。JSON ポインターは、`/` 文字で区切られたトークンの文字列であり、オブジェクトのキーまたは配列のインデックスを指定します。トークンは文字列または数値です。JSON Pointer strings are used in many PATCH operations for [!DNL Platform] APIs, as described later in this document. JSON ポインターの詳細については、[JSON ポインターの概要ドキュメント](https://rapidjson.org/md_doc_pointer.html)を参照してください。

### JSON スキーマオブジェクトの例

```json
{
    "type": "object",
    "title": "Loyalty Member Details",
    "meta:intendedToExtend": [
        "https://ns.adobe.com/xdm/context/profile"
    ],
    "description": "Loyalty Program Mixin.",
    "definitions": {
        "loyalty": {
            "properties": {
                "_{TENANT_ID}": {
                    "type": "object",
                    "properties": {
                        "loyaltyId": {
                            "title": "Loyalty Identifier",
                            "type": "string",
                            "description": "Loyalty Identifier.",
                            "meta:xdmType": "string"
                        },
                        "loyaltyLevel": {
                            "title": "Loyalty Level",
                            "description": "The current loyalty program level to which the individual member belongs.",
                            "type": "string",
                            "enum": [
                                "platinum",
                                "gold",
                                "silver",
                                "bronze"
                            ],
                            "meta:enum": {
                                "platinum": "Platinum",
                                "gold": "Gold",
                                "silver": "Silver",
                                "bronze": "Bronze"
                            },
                            "meta:xdmType": "string"
                        }
                    },
                    "meta:xdmType": "object"
                }
            },
            "type": "object",
            "meta:xdmType": "object"
        }
    }
}
```

### スキーマオブジェクトに基づいた JSON ポインターの例

| JSON ポインター | 解決先 |
|--- | ---|
| `"/title"` | &quot;ロイヤリティーメンバーの詳細&quot; |
| `"/definitions/loyalty"` | （`loyalty` オブジェクトのコンテンツを返します） |
| `"/definitions/loyalty/properties/_{TENANT_ID}/properties/loyaltyLevel/enum"` | `["platinum", "gold", "silver", "bronze"]` |
| `"/definitions/loyalty/properties/_{TENANT_ID}/properties/loyaltyLevel/enum/0"` | `"platinum"` |

>[!NOTE]
>
>When dealing with the `xdm:sourceProperty` and `xdm:destinationProperty` attributes of [!DNL Experience Data Model] (XDM) descriptors, any `properties` keys must be **excluded** from the JSON Pointer string. See the [!DNL Schema Registry] API developer guide sub-guide on [descriptors](../xdm/api/descriptors.md) for more information.

## JSON パッチ

There are many PATCH operations for [!DNL Platform] APIs that accept JSON Patch objects for their request payloads. JSON パッチは、JSON ドキュメントの変更を記述するための標準形式（[RFC 6902](https://tools.ietf.org/html/rfc6902)）です。この標準形式では、リクエスト本文でドキュメント全体を送信する必要なく、JSON の部分的なアップデートを定義できます。

### JSON パッチオブジェクトの例

```json
{
  "op": "remove",
  "path": "/foo"
}
```

* `op`：パッチ操作のタイプ。While JSON Patch supports several different operation types, not all PATCH operations in [!DNL Platform] APIs are compatible with every operation type. 使用可能な操作のタイプは次のとおりです。
   * `add`
   * `remove`
   * `replace`
   * `copy`
   * `move`
   * `test`
* `path`：JSON 構造のアップデートされる部分。[JSON ポインター](#json-pointer)表記を使用して識別されます。

`op` で示されている操作タイプによっては、JSON パッチオブジェクトに追加のプロパティが必要な場合があります。JSON パッチの様々な操作と必要な構文の詳細については、[JSON パッチのドキュメント](http://jsonpatch.com/)を参照してください。

## JSON スキーマ

JSON スキーマは、JSON データの構造を記述して検証するために使用される形式です。[Experience Data Model （XDM）](../xdm/home.md)では、JSON スキーマ機能を利用して、取得される顧客体験データの構造と形式に制約を適用します。JSON スキーマの詳細については、[公式のドキュメント](https://json-schema.org/)を参照してください。

## 次の手順

This document introduced some of the technologies and syntaxes involved with managing JSON-based resources for [!DNL Experience Platform]. For more information on working with [!DNL Platform] APIs, including best practices and answers to frequently asked questions, refer to the [Platform troubleshooting guide](troubleshooting.md).