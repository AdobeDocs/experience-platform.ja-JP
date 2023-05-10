---
title: API での推奨値の管理
description: スキーマレジストリ API の文字列フィールドに推奨値を追加する方法を説明します。
exl-id: 96897a5d-e00a-410f-a20e-f77e223bd8c4
source-git-commit: a3140d5216857ef41c885bbad8c69d91493b619d
workflow-type: tm+mt
source-wordcount: '658'
ht-degree: 4%

---

# API での推奨値の管理

エクスペリエンスデータモデル (XDM) の任意の文字列フィールドに対して、 **enum** フィールドが取り込むことができる値を事前定義済みのセットに制限する データを列挙フィールドに取り込もうとしたが、その値が設定で定義された値と一致しない場合、取り込みは拒否されます。

列挙とは異なり、 **推奨値** 文字列フィールドに取り込み可能な値が制限されていない。 代わりに、推奨値は、 [セグメント化 UI](../../segmentation/ui/overview.md) 文字列フィールドを属性として含める場合。

>[!NOTE]
>
>フィールドの更新された推奨値がセグメント化 UI に反映されるまでに、およそ 5 分の遅延があります。

このガイドでは、 [スキーマレジストリ API](https://developer.adobe.com/experience-platform-apis/references/schema-registry/). Adobe Experience Platformユーザーインターフェイスでこれをおこなう手順については、 [列挙と推奨値に関する UI ガイド](../ui/fields/enum.md).

## 前提条件

このガイドは、XDM のスキーマ構成の要素と、スキーマレジストリ API を使用して XDM リソースを作成および編集する方法について詳しいことを前提としています。 紹介が必要な場合は、次のドキュメントを参照してください。

* [スキーマ構成の基本](../schema/composition.md)
* [Schema Registry API ガイド](../api/overview.md)

また、 [列挙と推奨値の進化ルール](../ui/fields/enum.md#evolution) 既存のフィールドを更新する場合。 和集合に参加するスキーマの推奨値を管理している場合は、 [列挙と推奨値の結合ルール](../ui/fields/enum.md#merging).

## 構成

API では、 **enum** フィールドは、 `enum` 配列、 `meta:enum` オブジェクトは、これらの値にわかりやすい表示名を提供します。

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

列挙フィールドの場合、スキーマレジストリでは許可されていません `meta:enum` 以下に規定される値を超えて拡張される `enum`の代わりに、これらの制約の外部で文字列値を取り込もうとしても検証に合格しません。

または、 `enum` 配列で、 `meta:enum` 示すオブジェクト **推奨値**:

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

文字列には `enum` 制約を定義する配列 `meta:enum` プロパティを拡張して、新しい値を含めることができます。

<!-- ## Manage suggested values for standard fields

For existing standard fields, you can [add suggested values](#add-suggested-standard) or [remove suggested values](#remove-suggested-standard). -->

## 標準フィールドに推奨値を追加する {#add-suggested-standard}

を拡張するには、以下を実行します。 `meta:enum` 標準の文字列フィールドの [わかりやすい名前記述子](../api/descriptors.md#friendly-name) 特定のスキーマの問題になっているフィールドに対して。

>[!NOTE]
>
>文字列フィールドの推奨値は、スキーマレベルでのみ追加できます。 つまり、 `meta:enum` あるスキーマ内の標準フィールドのは、同じ標準フィールドを使用する他のスキーマには影響しません。

次のリクエストでは、標準の `eventType` フィールド ( [XDM ExperienceEvent クラス](../classes/experienceevent.md)) で指定されたスキーマの `sourceSchema`:

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

記述子を適用した後、スキーマを取得する際に、スキーマレジストリは次の応答を返します（領域を節約するために切り捨てられた応答）。

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
>標準フィールドに、既に `meta:enum`の場合、記述子の新しい値は既存のフィールドを上書きせず、代わりにに追加されます。
>
>
```json
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

次の手順で `meta:enum` カスタムフィールドの場合は、PATCHリクエストを通じて、フィールドの親クラス、フィールドグループ、またはデータ型を更新できます。

>[!WARNING]
>
>標準フィールドとは異なり、 `meta:enum` カスタムフィールドのは、そのフィールドを使用する他のすべてのスキーマに影響します。 変更をスキーマ間で反映しない場合は、代わりに新しいカスタムリソースを作成することを検討してください。
>
>* [カスタムクラスの作成](../api/classes.md#create)
>* [カスタムフィールドグループの作成](../api/field-groups.md#create)
>* [カスタムデータタイプの作成](../api/data-types.md#create)


次のリクエストは、 `meta:enum` の値は、カスタムデータ型で提供される「ロイヤルティレベル」フィールドです。

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

変更を適用した後、スキーマの取得時に、スキーマレジストリは次の応答を返します（スペースを節約するために切り捨てられた応答）。

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

このガイドでは、スキーマレジストリ API で文字列フィールドの推奨値を管理する方法について説明しました。 詳しくは、 [API でのカスタムフィールドの定義](./custom-fields-api.md) 様々なフィールドタイプの作成方法の詳細については、を参照してください。
