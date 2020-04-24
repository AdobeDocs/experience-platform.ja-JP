---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform APIの基本事項
topic: getting started
translation-type: tm+mt
source-git-commit: c94f065a5d56ac495dd2d541531aaec94c187612

---


# Adobe Experience Platform APIの基本事項

Adobe Experience Platform APIは、JSONベースのプラットフォームリソースを効果的に管理するために理解する必要がある、基盤となる複数のテクノロジーと構文を採用しています。 このドキュメントでは、これらのテクノロジーの概要と、詳細についての外部ドキュメントへのリンクを示します。

## JSONポインタ {#json-pointer}

JSONポインターは、JSONドキュメント内の特定の値を識別するための標準化された文字列構文([RFC 6901](https://tools.ietf.org/html/rfc6901))です。 JSONポインターはトークンの文字列で、オブジェク `/` トのキーまたは配列のインデックスを指定します。トークンは文字列または数値です。 JSONポインタ文字列は、このドキュメントで後述するように、Platform APIの多くのPATCH操作で使用されます。 JSONポインターの詳細については、 [JSONポインターの概要ドキュメントを参照してください](https://rapidjson.org/md_doc_pointer.html)。

### JSONスキーマオブジェクトの例

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

### スキーマオブジェクトに基づくJSONポインタの例

| JSONポインタ | 解決先 |
|--- | ---|
| `"/title"` | 忠誠度メンバーの詳細 |
| `"/definitions/loyalty"` | (オブジェクトの内容を返 `loyalty` します) |
| `"/definitions/loyalty/properties/_{TENANT_ID}/properties/loyaltyLevel/enum"` | `["platinum", "gold", "silver", "bronze"]` |
| `"/definitions/loyalty/properties/_{TENANT_ID}/properties/loyaltyLevel/enum/0"` | `"platinum"` |

>[!N注意]
>エクスペリエンスデー `xdm:sourceProperty` タモデ `xdm:destinationProperty` ル(XDM)記述子の属性と属性を扱う場合、JSON `properties` ポインタ文字列か **らキーを除外する必要があります** 。 詳しくは、スキーマレジストリAPI開発者ガイドを参照し [てください](../xdm/api/descriptors.md) 。

## JSONパッチ

リクエストペイロード用のJSONパッチオブジェクトを受け入れるプラットフォームAPIのPATCH操作は多数存在します。 JSONパッチは、JSONドキュメントの変更を記述する[標準形式(RFC 6902](https://tools.ietf.org/html/rfc6902))です。 これにより、リクエスト本文でドキュメント全体を送信する必要なく、JSONの部分的な更新を定義できます。

### JSONパッチオブジェクトの例

```json
{
  "op": "remove",
  "path": "/foo"
}
```

* `op`:パッチ操作のタイプ。 JSONパッチは複数の異なる操作タイプをサポートしますが、プラットフォームAPIのすべてのPATCH操作がすべての操作タイプと互換性があるわけではありません。 使用可能な操作のタイプは次のとおりです。
   * `add`
   * `remove`
   * `replace`
   * `copy`
   * `move`
   * `test`
* `path`:JSON構造の更新対象の部分で、 [JSONポインター表記を使用して識別されます](#json-pointer) 。

に示す操作のタイプに応じて、JSONパッチオ `op`ブジェクトに追加のプロパティが必要な場合があります。 JSONパッチの様々な操作とその必要な構文について詳しくは、 [JSONパッチのドキュメントを参照してください](http://jsonpatch.com/)。

## JSON スキーマ

JSONスキーマは、JSONデータの構造を記述し、検証するために使用される形式です。 [エクスペリエンスデータモデル(XDM)は](../xdm/home.md) 、JSONスキーマ機能を利用して、取り込む顧客エクスペリエンスデータの構造と形式に制約を課します。 JSONのスキーマについて詳しくは、公式のドキュメントを参照し [てください](https://json-schema.org/)。

## 次の手順

このドキュメントでは、エクスペリエンスプラットフォームのJSONベースのリソースの管理に関連するテクノロジーと構文の一部が導入されました。 よくある質問に対するベストプラクティスや回答を含む、プラットフォームAPIの使用に関する詳細は、プラットフォームのトラブルシューティングガイドを参 [照してくださ](troubleshooting.md)い。