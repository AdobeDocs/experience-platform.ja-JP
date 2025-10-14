---
title: API での推奨値の管理
description: Schema Registry API で文字列フィールドに推奨値を追加する方法を説明します。
exl-id: 96897a5d-e00a-410f-a20e-f77e223bd8c4
source-git-commit: a3140d5216857ef41c885bbad8c69d91493b619d
workflow-type: tm+mt
source-wordcount: '654'
ht-degree: 1%

---

# API での推奨値の管理

エクスペリエンスデータモデル（XDM）の任意の文字列フィールドについて、フィールドが取り込むことができる値を事前定義済みのセットに制限する **enum** を定義できます。 列挙フィールドにデータを取り込もうとすると、その値が設定で定義されている値と一致しない場合、取り込みは拒否されます。

列挙とは異なり、文字列フィールドに **推奨値** を追加しても、取り込むことができる値は制限されません。 代わりに、文字列フィールドを属性として含める場合、推奨値は、[&#x200B; セグメント化 UI](../../segmentation/ui/overview.md) で使用できる事前定義済みの値に影響を与えます。

>[!NOTE]
>
>フィールドの更新された推奨値がセグメント化 UI に反映されるまでに、約 5 分の遅延があります。

このガイドでは、[&#x200B; スキーマレジストリ API](https://developer.adobe.com/experience-platform-apis/references/schema-registry/) を使用して推奨値を管理する方法について説明します。 Adobe Experience Platform ユーザーインターフェイスでこれを行う手順については、[UI ガイドの列挙と推奨値 &#x200B;](../ui/fields/enum.md) を参照してください。

## 前提条件

このガイドは、XDM でのスキーマ構成の要素と、Schema Registry API を使用して XDM リソースを作成および編集する方法について理解していることを前提としています。 説明が必要な場合は、次のドキュメントを参照してください。

* [スキーマ構成の基本](../schema/composition.md)
* [Schema Registry API ガイド](../api/overview.md)

既存のフィールドを更新する場合は、[&#x200B; 列挙と推奨値の進化ルール &#x200B;](../ui/fields/enum.md#evolution) を確認することを強くお勧めします。 結合に参加するスキーマの推奨値を管理している場合は、[&#x200B; 列挙と推奨値の結合ルール &#x200B;](../ui/fields/enum.md#merging) を参照してください。

## 構成

API では、**enum** フィールドの制約値は `enum` 配列で表され、`meta:enum` オブジェクトはそれらの値にわかりやすい表示名を提供します。

```json
"exampleStringField": {
  "type": "string",
  "enum": [
    "value1",
    "value2",
    "value3"
  ],
  "meta:enum": {
    "value1": "Value 1",
    "value2": "Value 2",
    "value3": "Value 3"
  },
  "default": "value1"
}
```

列挙フィールドの場合、スキーマレジストリでは、`enum` で指定された値を超えて `meta:enum` を拡張することはできません。これらの制約の範囲外の文字列値を取り込もうとすると、検証に合格しないためです。

または、`enum` 配列を含まず、`meta:enum` オブジェクトのみを使用して **推奨値** を示す文字列フィールドを定義することもできます。

```json
"exampleStringField": {
  "type": "string",
  "meta:enum": {
    "value1": "Value 1",
    "value2": "Value 2",
    "value3": "Value 3"
  },
  "default": "value1"
}
```

文字列には制約を定義する `enum` 配列がないので、`meta:enum` プロパティを拡張して新しい値を含めることができます。

<!-- ## Manage suggested values for standard fields

For existing standard fields, you can [add suggested values](#add-suggested-standard) or [remove suggested values](#remove-suggested-standard). -->

## 標準フィールドへの推奨値の追加 {#add-suggested-standard}

標準の文字列フィールドの `meta:enum` を拡張するには、特定のスキーマで問題となっているフィールドに [&#x200B; わかりやすい名前記述子 &#x200B;](../api/descriptors.md#friendly-name) を作成します。

>[!NOTE]
>
>文字列フィールドの推奨値は、スキーマレベルでのみ追加できます。 つまり、あるスキーマでの標準フィールドの `meta:enum` を拡張しても、同じ標準フィールドを使用する他のスキーマには影響しません。

次のリクエストは、`sourceSchema` で識別されるスキーマの標準 `eventType` フィールド（[XDM ExperienceEvent クラス &#x200B;](../classes/experienceevent.md) によって提供）に推奨値を追加します。

```curl
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "@type": "xdm:alternateDisplayInfo",
        "sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/fbc52b243d04b5d4f41eaa72a8ba58be",
        "sourceProperty": "/eventType",
        "title": {
            "en_us": "Enum Event Type"
        },
        "description": {
            "en_us": "Event type field with soft enum values"
        },
        "meta:enum": {
          "eventA": {
            "en_us": "Event Type A"
          },
          "eventB": {
            "en_us": "Event Type B"
          }
        }
      }'
```

記述子を適用した後、スキーマレジストリはスキーマを取得する際に次のように応答します（応答がスペースを節約するために切り捨てられます）。

```json
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/fbc52b243d04b5d4f41eaa72a8ba58be",
  "title": "Example Schema",
  "properties": {
    "eventType": {
      "type":"string",
      "title": "Enum Event Type",
      "description": "Event type field with soft enum values.",
      "meta:enum": {
        "customEventA": "Custom Event Type A",
        "customEventB": "Custom Event Type B"
      }
    }
  }
}
```

>[!NOTE]
>
>標準フィールドの `meta:enum` に既に値が含まれている場合、記述子からの新しい値は既存のフィールドを上書きせず、代わりにに追加されます。
>
>```json
>"standardField": {
>   "type":"string",
>   "title": "Example standard enum field",
>   "description": "Standard field with existing enum values.",
>   "meta:enum": {
>       "standardEventA": "Standard Event Type A",
>       "standardEventB": "Standard Event Type B",
>       "customEventA": "Custom Event Type A",
>       "customEventB": "Custom Event Type B"
>   }
>}
>```

<!-- ### Remove suggested values {#remove-suggested-standard}

If a standard string field has predefined suggested values, you can remove any values that you do not wish to see in segmentation. This is done through by creating a [friendly name descriptor](../api/descriptors.md#friendly-name) for the schema that includes an `xdm:excludeMetaEnum` property.

**API format**

```http
POST /tenant/descriptors
```

**Request**

The following request removes the suggested values "[!DNL Web Form Filled Out]" and "[!DNL Media ping]" for `eventType` in a schema based on the [XDM ExperienceEvent class](../classes/experienceevent.md).

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "@type": "xdm:alternateDisplayInfo",
        "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/274f17bc5807ff307a046bab1489fb18",
        "xdm:sourceVersion": 1,
        "xdm:sourceProperty": "/xdm:eventType",
        "xdm:excludeMetaEnum": {
          "web.formFilledOut": "Web Form Filled Out",
          "media.ping": "Media ping"
        }
      }'
```

| Property | Description |
| --- | --- |
| `@type` | The type of descriptor being defined. For a friendly name descriptor, this value must be set to `xdm:alternateDisplayInfo`. |
| `xdm:sourceSchema` | The `$id` URI of the schema where the descriptor is being defined. |
| `xdm:sourceVersion` | The major version of the source schema. |
| `xdm:sourceProperty` | The path to the specific property whose suggested values you want to manage. The path should begin with a slash (`/`) and not end with one. Do not include `properties` in the path (for example, use `/personalEmail/address` instead of `/properties/personalEmail/properties/address`). |
| `meta:excludeMetaEnum` | An object that describes the suggested values that should be excluded for the field in segmentation. The key and value for each entry must match those included in the original `meta:enum` of the field in order for the entry to be excluded.  |

{style="table-layout:auto"}

**Response**

A successful response returns HTTP status 201 (Created) and the details of the newly created descriptor. The suggested values included under `xdm:excludeMetaEnum` will now be hidden from the Segmentation UI.

```json
{
  "@type": "xdm:alternateDisplayInfo",
  "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/274f17bc5807ff307a046bab1489fb18",
  "xdm:sourceVersion": 1,
  "xdm:sourceProperty": "/xdm:eventType",
  "xdm:excludeMetaEnum": {
    "web.formFilledOut": "Web Form Filled Out"
  },
  "meta:containerId": "tenant",
  "@id": "f3a1dfa38a4871cf4442a33074c1f9406a593407"
}
``` -->

## カスタムフィールドの推奨値の管理 {#suggested-custom}

カスタムフィールドの `meta:enum` を管理するには、PATCHリクエストを通じて、フィールドの親クラス、フィールドグループまたはデータタイプを更新できます。

>[!WARNING]
>
>標準フィールドとは異なり、カスタムフィールドの `meta:enum` を更新すると、そのフィールドを使用する他のすべてのスキーマに影響します。 変更がスキーマ間に反映されないようにする場合は、代わりに、新しいカスタムリソースを作成することを検討してください。
>
>* [&#x200B; カスタムクラスの作成 &#x200B;](../api/classes.md#create)
>* [&#x200B; カスタムフィールドグループの作成 &#x200B;](../api/field-groups.md#create)
>* [&#x200B; カスタムデータタイプの作成 &#x200B;](../api/data-types.md#create)

次のリクエストは、カスタムデータタイプが提供する「ロイヤルティレベル」フィールドの `meta:enum` を更新します。

```curl
curl -X PATCH \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/datatypes/_{TENANT_ID}.datatypes.8779fd45d6e4eb074300023a439862bbba359b60d451627a \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '[
        {
          "op": "replace",
          "path": "/loyaltyLevel/meta:enum",
          "value": {
            "ultra-platinum": "Ultra Platinum",
            "platinum": "Platinum",
            "gold": "Gold",
            "silver": "Silver",
            "bronze": "Bronze"
          }
        }
      ]'
```

変更を適用した後、スキーマレジストリはスキーマを取得する際に次のように応答します（応答がスペースを節約するために切り捨てられます）。

```json
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/fbc52b243d04b5d4f41eaa72a8ba58be",
  "title": "Example Schema",
  "properties": {
    "loyaltyLevel": {
      "type":"string",
      "title": "Loyalty Level",
      "description": "The loyalty program tier that this customer qualifies for.",
      "meta:enum": {
        "ultra-platinum": "Ultra Platinum",
        "platinum": "Platinum",
        "gold": "Gold",
        "silver": "Silver",
        "bronze": "Bronze"
      }
    }
  }
}
```

## 次の手順

このガイドでは、Schema Registry API の文字列フィールドの推奨値を管理する方法について説明しました。 様々なフィールドタイプの作成方法について詳しくは、[API でのカスタムフィールドの定義 &#x200B;](./custom-fields-api.md) に関するガイドを参照してください。
