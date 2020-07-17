---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience PlatformAPIの基本事項
topic: getting started
translation-type: tm+mt
source-git-commit: f910351d49de9c4a18a444b99b7f102f4ce3ed5b
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Adobe Experience PlatformAPIの基本事項

Adobe Experience PlatformAPIは、JSONベースのリソースを効果的に管理するために理解する必要がある、いくつかの基礎となるテクノロジーと構文を採用してい [!DNL Platform] ます。 このドキュメントでは、これらのテクノロジーの概要を簡単に説明し、詳細については外部ドキュメントへのリンクを提供します。

## JSONポインタ {#json-pointer}

JSONポインターは、JSONドキュメント内の特定の値を識別するための標準化された文字列構文([RFC 6901](https://tools.ietf.org/html/rfc6901))です。 JSONポインターは、オブジェクトのキーまたは配列のインデックスを指定する、 `/` 文字で区切られたトークンの文字列です。トークンは文字列または数値です。 JSONポインタ文字列は、このドキュメントで後述するように、多くの [!DNL Platform] APIのPATCH操作で使用されます。 JSONポインターについて詳しくは、 [JSONポインターの概要に関するドキュメントを参照してください](https://rapidjson.org/md_doc_pointer.html)。

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
| `"/definitions/loyalty"` | ( `loyalty` オブジェクトの内容を返します) |
| `"/definitions/loyalty/properties/_{TENANT_ID}/properties/loyaltyLevel/enum"` | `["platinum", "gold", "silver", "bronze"]` |
| `"/definitions/loyalty/properties/_{TENANT_ID}/properties/loyaltyLevel/enum/0"` | `"platinum"` |

>[!N注意]
>
>
>( `xdm:sourceProperty` XDM)記述子の `xdm:destinationProperty` および [!DNL Experience Data Model] 属性を処理する場合、すべての `properties` キーはJSONポインタ文字列から **** 除外する必要があります。 詳しくは、 [!DNL Schema Registry] API開発者ガイドの [記述子を参照してください](../xdm/api/descriptors.md) 。

## JSONパッチ

APIには、要求ペイロードにJSONパッチオブジェクトを受け入れる多くのPATCH操作が [!DNL Platform] あります。 JSONパッチは、JSONドキュメントの変更を記述する標準形式([RFC 6902](https://tools.ietf.org/html/rfc6902))です。 これにより、リクエスト本文でドキュメント全体を送信する必要なく、JSONに部分的な更新を定義できます。

### JSONパッチオブジェクトの例

```json
{
  "op": "remove",
  "path": "/foo"
}
```

* `op`: パッチ操作のタイプ。 JSONパッチは複数の異なる操作タイプをサポートしますが、APIのすべてのPATCH操作が各操作タイプと互換性があるとは限りま [!DNL Platform] せん。 次の操作タイプを使用できます。
   * `add`
   * `remove`
   * `replace`
   * `copy`
   * `move`
   * `test`
* `path`: JSON構造の更新対象部分。 [JSONポインタ](#json-pointer) 表記を使用して識別されます。

に示す操作タイプに応じて、JSONパッチオブジェクト `op`に追加のプロパティが必要な場合があります。 様々なJSONパッチ操作と必要な構文について詳しくは、 [JSONパッチのドキュメントを参照してください](http://jsonpatch.com/)。

## JSON スキーマ

JSONスキーマは、JSONデータの構造を記述し、検証するために使用される形式です。 [Experience Data Model(XDM)](../xdm/home.md) は、JSONスキーマ機能を利用して、取り込む顧客体験データの構造と形式に制約を課します。 JSONのスキーマについて詳しくは、 [公式ドキュメントを参照してください](https://json-schema.org/)。

## 次の手順

このドキュメントでは、のJSONベースのリソースの管理に関連するテクノロジーと構文の一部を紹介し [!DNL Experience Platform]ました。 よくある質問へのベストプラクティスや回答を含む、 [!DNL Platform] APIの操作について詳しくは、 [Platformのトラブルシューティングガイドを参照してください](troubleshooting.md)。