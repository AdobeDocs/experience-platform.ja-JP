---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；xdm；エクスペリエンスデータモデル；名前空間；名前空間；互換性モード；xed;
solution: Experience Platform
title: エクスペリエンスデータモデル (XDM) での名前空間
description: Experience Data Model(XDM) の名前空間でスキーマを拡張し、異なるスキーマコンポーネントが統合される際にフィールドの競合を防ぐ方法について説明します。
exl-id: b351dfaf-5219-4750-a7a9-cf4689a5b736
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '630'
ht-degree: 1%

---

# エクスペリエンスデータモデル (XDM) での名前空間

エクスペリエンスデータモデル (XDM) スキーマのすべてのフィールドには、関連する名前空間があります。 これらの名前空間を使用すると、異なるスキーマコンポーネントが統合される際に、スキーマを拡張し、フィールドの競合を防ぐことができます。 このドキュメントでは、XDM の名前空間の概要と、 [スキーマレジストリ API](../api/overview.md).

名前空間を使用すると、ある名前空間でフィールドを、別の名前空間で同じフィールドとは異なる意味として定義できます。 実際には、フィールドの名前空間は、フィールドの作成者 ( 標準 XDM(Adobe)、ベンダー、組織など ) を示します。

例えば、 [[!UICONTROL 個人の連絡先の詳細] フィールドグループ](../field-groups/profile/demographic-details.md)( 標準の `mobilePhone` 次に存在するフィールド `xdm` 名前空間。 同じスキーマ内で、別の `mobilePhone` 別の名前空間 ( [テナント ID](../api/getting-started.md#know-your-tenant_id)) をクリックします。 これらのフィールドは、基本的な意味や制約が異なる間に共存することができます。

## 名前空間の構文

次の節では、XDM 構文で名前空間を割り当てる方法を示します。

### 標準 XDM {#standard}

標準の XDM 構文を使用すると、スキーマで名前空間がどのように表されているか ( [Adobe Experience Platformでの翻訳方法](#compatibility)) をクリックします。

標準 XDM では [JSON-LD](https://json-ld.org/) 名前空間をフィールドに割り当てるための構文です。 この名前空間は URI の形式 ( 例： `https://ns.adobe.com/xdm` の `xdm` 名前空間 )、または `@context` スキーマの属性。

次に、標準の XDM 構文の製品のスキーマの例を示します。 を除き `@id` （JSON-LD 仕様で定義される一意の識別子）、各フィールドは、 `properties` 名前空間で始まり、フィールド名で終わります。 以下で定義された短縮形のプレフィックスを使用する場合 `@context`の場合、名前空間とフィールド名はコロン (`:`) をクリックします。 プレフィックスを使用しない場合、名前空間とフィールド名はスラッシュ (`/`) をクリックします。

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
| `@context` | の下の完全な名前空間 URI の代わりに使用できる短縮形のプレフィックスを定義するオブジェクト `properties`. |
| `@id` | レコードの一意の識別子 ( [JSON-LD 仕様](https://json-ld.org/spec/latest/json-ld/#node-identifiers). |
| `xdm:sku` | 名前空間を表す短縮形のプレフィックスを使用するフィールドの例です。 この場合、 `xdm` は名前空間 (`https://ns.adobe.com/xdm`) および `sku` はフィールド名です。 |
| `https://ns.adobe.com/xdm/channels/application` | 完全な名前空間 URI を使用するフィールドの例。 この場合、 `https://ns.adobe.com/xdm/channels` は名前空間で、 `application` はフィールド名です。 |
| `https://ns.adobe.com/vendorA/product/stockNumber` | ベンダーリソースが提供するフィールドは、独自の一意の名前空間を使用します。 この例では、 `https://ns.adobe.com/vendorA/product` はベンダーの名前空間で、 `stockNumber` はフィールド名です。 |
| `tenantId:internalSku` | 組織で定義されたフィールドでは、一意のテナント ID を名前空間として使用します。 この例では、 `tenantId` はテナントの名前空間 (`https://ns.adobe.com/tenantId`) および `internalSku` はフィールド名です。 |

{style=&quot;table-layout:auto&quot;}

### 互換性モード {#compatibility}

Adobe Experience Platformでは、XDM スキーマは [互換性モード](../api/appendix.md#compatibility) 構文とは異なり、名前空間を表すのに JSON-LD 構文を使用しません。 代わりに、Platform は名前空間を親フィールド（アンダースコアで始まる）に変換し、その下にフィールドをネストします。

例えば、標準の XDM `repo:createdDate` 次に変換： `_repo.createdDate` とは、互換性モードで次の構造の下に表示されます。

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

次を使用するフィールド： `xdm` 名前空間はの下にルートフィールドとして表示されます。 `properties` をクリックし、 `xdm:` に表示されるプレフィックス [標準 XDM 構文](#standard). 例： `xdm:sku` は `sku` 代わりに、

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

このガイドでは、XDM 名前空間の概要と JSON での表現方法を示しました。 API を使用して XDM スキーマを設定する方法について詳しくは、 [スキーマレジストリ API ガイド](../api/overview.md).
