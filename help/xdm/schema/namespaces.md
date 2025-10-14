---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；xdm;experience data model；名前空間；名前空間；互換モード；xed;
solution: Experience Platform
title: エクスペリエンスデータモデル（XDM）の名前空間
description: エクスペリエンスデータモデル（XDM）の名前空間を使用して、スキーマを拡張し、様々なスキーマコンポーネントが統合されたときにフィールドの競合を防ぐ方法について説明します。
exl-id: b351dfaf-5219-4750-a7a9-cf4689a5b736
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '669'
ht-degree: 1%

---

# エクスペリエンスデータモデル（XDM）の名前空間

>[!IMPORTANT]
>
>XDM では、名前空間（このページのトピック）をスキーマ内のフィールドの区別に使用します。 これは、名前空間を使用して ID 値を区別する ID サービスの ID 名前空間の概念とは異なります。 詳しくは、[ID サービスの名前空間 &#x200B;](../../identity-service/features/namespaces.md) に関するドキュメントを参照してください。

エクスペリエンスデータモデル（XDM）スキーマのすべてのフィールドには、名前空間が関連付けられています。 これらの名前空間を使用すると、スキーマを拡張し、様々なスキーマコンポーネントが統合されてフィールドの競合を防ぐことができます。 このドキュメントでは、XDM の名前空間の概要と、[&#x200B; スキーマレジストリ API](../api/overview.md) での表現方法について説明します。

名前空間を使用すると、ある名前空間のフィールドを、別の名前空間の同じフィールドとは異なるものを意味するように定義できます。 実際には、フィールドの名前空間は、フィールドを作成したユーザー（標準 XDM （Adobe）、ベンダー、組織など）を示します。

例えば、[[!UICONTROL &#x200B; 個人の連絡先の詳細 &#x200B;] フィールドグループ &#x200B;](../field-groups/profile/demographic-details.md) を使用する XDM スキーマについて考えてみます。このフィールドグループには、`xdm` 名前空間に存在する標準の `mobilePhone` フィールドがあります。 同じスキーマ内で、別の名前空間（[&#x200B; テナント ID](../api/getting-started.md#know-your-tenant_id)）の下に別の `mobilePhone` フィールドを自由に作成することもできます。 これらのフィールドは両方とも混在でき、基盤となる様々な意味や制約を持つことができます。

## 名前空間の構文

以下の節では、XDM 構文での名前空間の割り当て方法について説明します。

### 標準 XDM {#standard}

標準 XDM 構文は、スキーマでの名前空間の表現方法（[Adobe Experience Platformでの名前空間の翻訳方法 &#x200B;](#compatibility) など）を示すinsightを提供します。

標準 XDM では、[JSON-LD](https://www.w3.org/TR/json-ld11/#basic-concepts) 構文を使用して名前空間をフィールドに割り当てます。 この名前空間は、URI の形式（`xdm` 名前空間の `https://ns.adobe.com/xdm` など）、またはスキーマの `@context` 属性に設定される短縮形のプレフィックスとして提供されます。

標準 XDM 構文での製品のスキーマ例を次に示します。 `@id` （JSON-LD 仕様で定義された一意の ID）を除き、`properties` の下の各フィールドは名前空間で始まり、フィールド名で終わります。 `@context` で定義された短縮形のプレフィックスを使用する場合、名前空間とフィールド名はコロン（`:`）で区切られます。 プレフィックスを使用しない場合、名前空間とフィールド名はスラッシュ（`/`）で区切られます。

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
| `@context` | `properties` の下で、完全な名前空間 URI の代わりに使用できる短縮形のプレフィックスを定義するオブジェクト。 |
| `@id` | [JSON-LD 仕様 &#x200B;](https://www.w3.org/TR/json-ld11/#node-identifiers) で定義されたレコードの一意の ID。 |
| `xdm:sku` | 短縮形のプレフィックスを使用して名前空間を示すフィールドの例。 この場合、`xdm` は名前空間（`https://ns.adobe.com/xdm`）、`sku` はフィールド名です。 |
| `https://ns.adobe.com/xdm/channels/application` | 完全な名前空間 URI を使用するフィールドの例です。 この場合、`https://ns.adobe.com/xdm/channels` は名前空間で、`application` はフィールド名です。 |
| `https://ns.adobe.com/vendorA/product/stockNumber` | ベンダーリソースによって提供されるフィールドは、独自の一意の名前空間を使用します。 この例では、`https://ns.adobe.com/vendorA/product` はベンダーの名前空間で、`stockNumber` はフィールド名です。 |
| `tenantId:internalSku` | 組織で定義されたフィールドでは、一意のテナント ID を名前空間として使用しています。 この例では、`tenantId` はテナント名前空間（`https://ns.adobe.com/tenantId`）で、`internalSku` はフィールド名です。 |

{style="table-layout:auto"}

### 互換性モード {#compatibility}

Adobe Experience Platformでは、XDM スキーマは、JSON-LD 構文を使用して名前空間を表さない [&#x200B; 互換モード &#x200B;](../api/appendix.md#compatibility) 構文で表されます。 代わりに、Experience Platformは（アンダースコアで始まる）名前空間を親フィールドに変換し、その下にフィールドをネストします。

例えば、標準 XDM `repo:createdDate` は `_repo.createdDate` に変換され、互換性モードでは次の構造で表示されます。

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

`xdm` 名前空間を使用するフィールドは、`properties` の下のルートフィールドとして表示され、[&#x200B; 標準 XDM 構文 &#x200B;](#standard) に表示される `xdm:` プレフィックスをドロップします。 例えば、`xdm:sku` は代わりに `sku` として単にリストされます。

次の JSON は、上記の標準 XDM 構文の例が互換性モードにどのように翻訳されるかを表しています。

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

このガイドでは、XDM 名前空間の概要と JSON での表現方法を説明しました。 API を使用して XDM スキーマを設定する方法について詳しくは、『 [&#x200B; スキーマレジストリ API ガイド &#x200B;](../api/overview.md) 』を参照してください。
