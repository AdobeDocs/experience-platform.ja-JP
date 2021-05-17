---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;xdm；エクスペリエンスデータモデル；名前空間;名前空間；互換モード；xed;
solution: Experience Platform
title: エクスペリエンスデータモデル(XDM)での名前空間
topic-legacy: overviews
description: Experience Data Model(XDM)での名前空間設定によって、異なるスキーマコンポーネントが結合される際に、スキーマを拡張し、フィールドの衝突を防ぐ方法を説明します。
source-git-commit: b4c4f8f7e428d27f389bff5591a03925b6afa6d8
workflow-type: tm+mt
source-wordcount: '630'
ht-degree: 1%

---


# エクスペリエンスデータモデル(XDM)での名前空間

エクスペリエンスデータモデル(XDM)スキーマのすべてのフィールドには、名前空間が関連付けられています。 これらの名前空間を使用すると、スキーマを拡張し、異なるスキーマコンポーネントが統合される際に、フィールドの衝突を防ぐことができます。 このドキュメントでは、
XDMと、[スキーマレジストリAPI](../api/overview.md)での表現方法。

名前空間を使用すると、1つの名前空間で、別の名前空間の同じフィールドとは異なる意味を持つフィールドを定義できます。 実際には、フィールドの名前空間はフィールドの作成者(標準のXDM(Adobe)、ベンダー、組織など)を示します。

例えば、[[!UICONTROL 個人連絡先の詳細]フィールドグループ](../field-groups/profile/demographic-details.md)を使用するXDMスキーマを考えてみましょう。このフィールドグループは、`xdm`名前空間に存在する標準の`mobilePhone`フィールドを持ちます。 同じスキーマでは、別の名前空間（[テナントID](../api/getting-started.md#know-your-tenant_id)）の下に別の`mobilePhone`フィールドを自由に作成することもできます。 これらのフィールドは両方とも共存できますが、基本的な意味や制約は異なります。

## 名前空間の構文

XDM構文での名前空間の割り当て方法を以下の節で説明します。

### 標準 XDM {#standard}

標準的なXDM構文は、スキーマでの名前空間の表現方法([Adobe Experience Platform](#compatibility)での変換方法を含む)を理解するうえで役立ちます。

標準のXDMは、[JSON-LD](https://json-ld.org/)構文を使用して、名前空間をフィールドに割り当てます。 この名前空間は、URI(`xdm`名前空間の`https://ns.adobe.com/xdm`など)、またはスキーマの`@context`属性で設定された略記名プレフィックスの形式で提供されます。

標準のXDM構文での製品のスキーマ例を以下に示します。 `@id`（JSON-LD仕様で定義される一意の識別子）を除き、`properties`開始の下の各フィールドは名前空間を持ち、フィールド名で終わります。 `@context`の下に定義された略記法の接頭辞を使用する場合、名前空間とフィールド名はコロン(`:`)で区切られます。 プレフィックスを使用しない場合、名前空間とフィールド名はスラッシュ(`/`)で区切られます。

```json
{
  "$id": "https://ns.adobe.com/xdm/schemas/mySchema",
  "title": "Product",
  "description": "Represents the definition of a Project",
  "@context": {
    "xdm": "https://ns.adobe.com/xdm",
    "repo": "http://ns.adobe.com/adobecloud/core/1.0/",
    "schema": "http://schema.org",
    "tenantId": "https://ns.adobe.com/tenantId"
  },
  "properties": {
    "@id": {
      "type": "string"
    },
    "xdm:sku": {
      "type": "string"
    },
    "xdm:name": {
      "type": "string"
    },
    "repo:createdDate": {
      "type": "string",
      "format": "datetime"
    },
    "https://ns.adobe.com/xdm/channels/application": {
      "type": "string"
    },
    "schema:latitude": {
      "type": "number"
    },
    "https://ns.adobe.com/vendorA/product/stockNumber": {
      "type": "string"
    },
    "tenantId:internalSku": {
      "type": "number"
    }
  }
}
```

| プロパティ | 説明 |
| --- | --- |
| `@context` | `properties`の下の完全な名前空間URIの代わりに使用できる略記名接頭辞を定義するオブジェクトです。 |
| `@id` | [JSON-LD仕様](https://json-ld.org/spec/latest/json-ld/#node-identifiers)で定義される、レコードの一意の識別子。 |
| `xdm:sku` | 名前空間を表す略記法の接頭辞を使用するフィールドの例です。 この場合、`xdm`は名前空間(`https://ns.adobe.com/xdm`)、`sku`はフィールド名です。 |
| `https://ns.adobe.com/xdm/channels/application` | 完全な名前空間URIを使用するフィールドの例です。 この場合、`https://ns.adobe.com/xdm/channels`は名前空間、`application`はフィールド名です。 |
| `https://ns.adobe.com/vendorA/product/stockNumber` | ベンダーリソースから提供されるフィールドは、独自の固有名前空間を使用します。 この例では、`https://ns.adobe.com/vendorA/product`がベンダー名前空間、`stockNumber`がフィールド名です。 |
| `tenantId:internalSku` | 組織で定義されたフィールドでは、固有のテナントIDを名前空間として使用します。 この例では、`tenantId`がテナント名前空間(`https://ns.adobe.com/tenantId`)、`internalSku`がフィールド名です。 |

{style=&quot;table-layout:auto&quot;}

### 互換性モード {#compatibility}

Adobe Experience Platformでは、XDMスキーマは[互換モード](../api/appendix.md#compatibility)構文で表されますが、名前空間を表すのにJSON-LD構文を使用しません。 代わりに、名前空間が親フィールド（アンダースコアで始まる）に変換され、その下にフィールドがネストされます。

例えば、標準のXDM `repo:createdDate`は`_repo.createdDate`に変換され、互換モードでは次の構造の下に表示されます。

```json
"_repo": {
  "type": "object",
  "properties": {
    "createdDate": {
      "type": "string",
      "format": "datetime"
    }
  }
}
```

`xdm`名前空間を使用するフィールドは、`properties`の下にルートフィールドとして表示され、[標準のXDM構文](#standard)に表示される`xdm:`プレフィックスを削除します。 例えば、`xdm:sku`は、代わりに`sku`と表示されます。

次のJSONは、上に示した標準のXDM構文の例が互換モードに変換される方法を表しています。

```json
{
  "$id": "https://ns.adobe.com/xdm/schemas/mySchema",
  "title": "Product",
  "description": "Represents the definition of a Project",
  "properties": {
    "_id": {
      "type": "string"
    },
    "sku": {
      "type": "string"
    },
    "name": {
      "type": "string"
    },
    "_repo": {
      "type": "object",
      "properties": {
        "createdDate": {
          "type": "string",
          "format": "datetime"
        }
      }
    },
    "_channels": {
      "type": "object",
      "properties": {
        "application": {
          "type": "string"
        }
      }
    },
    "_schema": {
      "type": "object",
      "properties": {
        "application": {
          "type": "string"
        }
      }
    },
    "_vendorA": {
      "type": "object",
      "properties": {
        "product": {
          "type": "object",
          "properties": {
            "stockNumber": {
              "type": "string"
            }
          }
        }
      }
    },
    "_tenantId": {
      "type": "object",
      "properties": {
        "internalSku": {
          "type": "number"
        }
      }
    }
  }
}
```

## 次の手順

このガイドでは、XDM名前空間の概要と、JSONでのXDM要素の表現方法を説明しました。 APIを使用したXDMスキーマの設定方法について詳しくは、[スキーマレジストリAPIガイド](../api/overview.md)を参照してください。
