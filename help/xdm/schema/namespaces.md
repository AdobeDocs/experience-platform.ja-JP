---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；スキーマ；xdm；エクスペリエンスデータモデル；名前空間；名前空間；互換性モード；xed;
solution: Experience Platform
title: エクスペリエンスデータモデル (XDM) の名前空間
topic-legacy: overviews
description: エクスペリエンスデータモデル (XDM) で名前空間を使用してスキーマを拡張し、異なるスキーマコンポーネントを統合する際にフィールドの競合を防ぐ方法について説明します。
exl-id: b351dfaf-5219-4750-a7a9-cf4689a5b736
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '630'
ht-degree: 1%

---

# エクスペリエンスデータモデル (XDM) の名前空間

エクスペリエンスデータモデル (XDM) スキーマのすべてのフィールドには、関連する名前空間があります。 これらの名前空間を使用すると、異なるスキーマコンポーネントが統合される際に、スキーマを拡張し、フィールドの競合を防ぐことができます。 このドキュメントでは、
XDM と、[ スキーマレジストリ API](../api/overview.md) での XDM の表現方法。

名前空間を使用すると、ある名前空間のフィールドを、別の名前空間の同じフィールドとは異なる意味として定義できます。 実際には、フィールドの名前空間は、フィールドの作成者 ( 標準 XDM(Adobe)、ベンダー、組織など ) を示します。

例えば、[[!UICONTROL  個人の連絡先の詳細 ] フィールドグループ ](../field-groups/profile/demographic-details.md) を使用する XDM スキーマについて考えてみましょう。このグループには、`xdm` 名前空間に存在する標準の `mobilePhone` フィールドがあります。 同じスキーマ内で、別の名前空間（[ テナント ID](../api/getting-started.md#know-your-tenant_id)）の下に別の `mobilePhone` フィールドを自由に作成することもできます。 これらのフィールドは、基本的な意味や制約が異なる一方で、共存することができます。

## 名前空間の構文

次の節では、XDM 構文で名前空間を割り当てる方法を示します。

### 標準 XDM {#standard}

標準の XDM 構文は、スキーマで名前空間がどのように表されるか ([Adobe Experience Platform](#compatibility) での名前空間の変換方法を含む ) に関するインサイトを提供します。

標準の XDM は、[JSON-LD](https://json-ld.org/) 構文を使用して、名前空間をフィールドに割り当てます。 この名前空間は、URI（`xdm` 名前空間の場合は `https://ns.adobe.com/xdm` など）の形式で、またはスキーマの `@context` 属性で設定される短縮形のプレフィックスとして使用されます。

次に、標準の XDM 構文の製品のスキーマの例を示します。 `@id` （JSON-LD 仕様で定義される一意の識別子）を除き、`properties` の下の各フィールドは名前空間で始まり、フィールド名で終わります。 `@context` の下に定義された短縮形のプレフィックスを使用する場合、名前空間とフィールド名はコロン (`:`) で区切られます。 プレフィックスを使用しない場合、名前空間とフィールド名はスラッシュ (`/`) で区切られます。

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
| `@context` | `properties` の下の完全な名前空間 URI の代わりに使用できる短縮形の接頭辞を定義するオブジェクト。 |
| `@id` | [JSON-LD 仕様 ](https://json-ld.org/spec/latest/json-ld/#node-identifiers) で定義された、レコードの一意の識別子。 |
| `xdm:sku` | 短縮形のプレフィックスを使用して名前空間を表すフィールドの例です。 この場合、 `xdm` は名前空間 (`https://ns.adobe.com/xdm`)、 `sku` はフィールド名です。 |
| `https://ns.adobe.com/xdm/channels/application` | 完全な名前空間 URI を使用するフィールドの例。 この場合、 `https://ns.adobe.com/xdm/channels` が名前空間で、 `application` がフィールド名です。 |
| `https://ns.adobe.com/vendorA/product/stockNumber` | ベンダーリソースが提供するフィールドは、独自の一意の名前空間を使用します。 この例では、 `https://ns.adobe.com/vendorA/product` がベンダーの名前空間で、 `stockNumber` がフィールド名です。 |
| `tenantId:internalSku` | 組織で定義されたフィールドでは、一意のテナント ID を名前空間として使用します。 この例では、 `tenantId` がテナント名前空間 (`https://ns.adobe.com/tenantId`)、 `internalSku` がフィールド名です。 |

{style=&quot;table-layout:auto&quot;}

### 互換性モード {#compatibility}

Adobe Experience Platformでは、XDM スキーマは [ 互換性モード ](../api/appendix.md#compatibility) 構文で表され、名前空間を表すのに JSON-LD 構文は使用されません。 代わりに、Platform は名前空間を親フィールド（アンダースコアで始まる）に変換し、その下にフィールドをネストします。

例えば、標準の XDM `repo:createdDate` は `_repo.createdDate` に変換され、互換性モードでは次の構造の下に表示されます。

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

`xdm` 名前空間を使用するフィールドは、`properties` の下にルートフィールドとして表示され、[ 標準の XDM 構文 ](#standard) に表示される `xdm:` プレフィックスをドロップします。 例えば、`xdm:sku` は、代わりに `sku` と表示されます。

次の JSON は、上記の標準 XDM 構文の例が互換性モードに変換される方法を表しています。

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

このガイドでは、XDM 名前空間の概要と JSON での表現方法を示しました。 API を使用して XDM スキーマを設定する方法について詳しくは、『[ スキーマレジストリ API ガイド ](../api/overview.md)』を参照してください。
