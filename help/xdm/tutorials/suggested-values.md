---
title: APIでの推奨値の管理
description: Schema Registry APIの文字列フィールドに推奨値を追加する方法を説明します。
exl-id: 96897a5d-e00a-410f-a20e-f77e223bd8c4
source-git-commit: 82e41af32468febeda2dce6b471d72ef74359ea9
workflow-type: tm+mt
source-wordcount: '654'
ht-degree: 1%

---

# APIでの推奨値の管理

Experience Data Model （XDM）の任意の文字列フィールドに対して、フィールドが事前定義されたセットに取り込むことができる値を制限する&#x200B;**enum**&#x200B;を定義できます。 列挙フィールドにデータを取り込もうとし、値がその設定で定義されているどれにも一致しない場合、取り込みは拒否されます。

列挙とは対照的に、文字列フィールドに&#x200B;**推奨値**&#x200B;を追加しても、取り込み可能な値は制限されません。 代わりに、推奨される値は、文字列フィールドを属性として含める場合、[&#x200B; セグメント化UI](../../segmentation/ui/overview.md)で使用できる定義済みの値に影響します。

>[!NOTE]
>
>フィールドの更新された推奨値がセグメント UIに反映されるまでに、おおよその5分の遅延があります。

このガイドでは、[Schema Registry API](https://developer.adobe.com/experience-platform-apis/references/schema-registry/)を使用して推奨値を管理する方法について説明します。 Adobe Experience Platform ユーザーインターフェイスでこれを行う手順については、列挙と推奨値に関する[UI ガイド &#x200B;](../ui/fields/enum.md)を参照してください。

## 前提条件

このガイドでは、XDMのスキーマ構成の要素と、Schema Registry APIを使用してXDM リソースを作成および編集する方法について理解していることを前提としています。 導入が必要な場合は、次のドキュメントを参照してください。

* [スキーマ構成の基本](../schema/composition.md)
* [Schema Registry API ガイド](../api/overview.md)

また、既存のフィールドを更新する場合は、列挙および推奨値[の](../ui/fields/enum.md#evolution)進化ルールを確認することを強くお勧めします。 和集合に参加するスキーマの推奨値を管理する場合は、列挙と推奨値のマージに関する[&#x200B; ルール &#x200B;](../ui/fields/enum.md#merging)を参照してください。

## 構成

APIでは、**enum** フィールドの制約された値は`enum`配列で表され、`meta:enum` オブジェクトはこれらの値に対してわかりやすい表示名を提供します。

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

列挙フィールドの場合、これらの制約の外の文字列値を取り込もうとしても検証を渡さないため、スキーマレジストリでは`meta:enum`を`enum`で指定された値を超えて拡張することはできません。

または、`enum`配列を含まず、`meta:enum` オブジェクトのみを使用して&#x200B;**推奨値**&#x200B;を示す文字列フィールドを定義することもできます。

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

文字列には制約を定義する`enum`配列がないため、その`meta:enum` プロパティを拡張して新しい値を含めることができます。

<!-- 
## Manage suggested values for standard fields

For existing standard fields, you can [add suggested values](#add-suggested-standard) or [remove suggested values](#remove-suggested-standard). 
-->

## 標準フィールドへの推奨値の追加 {#add-suggested-standard}

標準文字列フィールドの`meta:enum`を拡張するには、特定のスキーマで対象となるフィールドの[わかりやすい名前の記述子](../api/descriptors.md#friendly-name)を作成できます。

>[!NOTE]
>
>文字列フィールドの推奨値は、スキーマレベルでのみ追加できます。 つまり、1つのスキーマ内の標準フィールドの`meta:enum`を拡張しても、同じ標準フィールドを使用する他のスキーマには影響しません。

次のリクエストは、`eventType`で識別されるスキーマの標準[&#x200B; フィールド（](../classes/experienceevent.md)XDM ExperienceEvent クラス `sourceSchema`によって提供）に推奨値を追加します。

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

ディスクリプタを適用すると、スキーマを取得する際に、スキーマレジストリは次のように応答します（応答はスペース用に切り捨てられます）。

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
>標準フィールドに既に`meta:enum`未満の値が含まれている場合、記述子の新しい値は既存のフィールドを上書きせず、代わりに次の場所に追加されます。
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

<!-- 
### Remove suggested values {#remove-suggested-standard}

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
``` 
-->

## カスタムフィールドの推奨値の管理 {#suggested-custom}

カスタムフィールドの`meta:enum`を管理するには、PATCH リクエストを使用して、フィールドの親クラス、フィールドグループ、またはデータタイプを更新できます。

>[!WARNING]
>
>標準フィールドとは異なり、カスタムフィールドの`meta:enum`を更新すると、そのフィールドを使用する他のすべてのスキーマに影響します。 スキーマ間で変更を反映させたくない場合は、代わりに新しいカスタムリソースを作成することを検討してください。
>
>* [&#x200B; カスタムクラスを作成](../api/classes.md#create)
>* [&#x200B; カスタムフィールドグループを作成](../api/field-groups.md#create)
>* [&#x200B; カスタムデータタイプを作成](../api/data-types.md#create)

次のリクエストは、カスタムデータタイプによって提供される「ロイヤルティレベル」フィールドの`meta:enum`を更新します。

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

変更を適用すると、スキーマを取得する際に、スキーマレジストリは次のように応答します（応答はスペース用に切り捨てられます）。

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

このガイドでは、Schema Registry APIで文字列フィールドの推奨値を管理する方法について説明しました。 さまざまなフィールドタイプを作成する方法について詳しくは、[APIでのカスタムフィールドの定義](./custom-fields-api.md)に関するガイドを参照してください。
