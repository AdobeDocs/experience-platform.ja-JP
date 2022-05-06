---
title: フィールドへの推奨値の追加
description: スキーマレジストリ API の文字列フィールドに推奨値を追加する方法を説明します。
exl-id: 96897a5d-e00a-410f-a20e-f77e223bd8c4
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 2%

---

# フィールドに推奨値を追加する

エクスペリエンスデータモデル (XDM) では、列挙フィールドは、値の事前定義サブセットに制限される文字列フィールドを表します。 列挙フィールドでは、取り込んだデータが一連の受け入れられた値に準拠していることを確認する検証を提供できます。 ただし、制約として強制せずに、文字列フィールドの推奨値のセットを定義することもできます。

内 [スキーマレジストリ API](https://developer.adobe.com/experience-platform-apis/references/schema-registry/)列挙フィールドの制約された値は、 `enum` 配列、 `meta:enum` オブジェクトは、これらの値にわかりやすい表示名を提供します。

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

または、 `enum` 配列で、 `meta:enum` オブジェクトは、推奨値を示します。

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

文字列には `enum` 制約を定義する配列 `meta:enum` プロパティを拡張して、新しい値を含めることができます。 このチュートリアルでは、スキーマレジストリ API で標準およびカスタム文字列フィールドに推奨値を追加する方法について説明します。

## 前提条件

このガイドは、XDM のスキーマ構成の要素と、スキーマレジストリ API を使用して XDM リソースを作成および編集する方法について詳しいことを前提としています。 紹介が必要な場合は、次のドキュメントを参照してください。

* [スキーマ構成の基本](../schema/composition.md)
* [Schema Registry API ガイド](../api/overview.md)

## 標準フィールドに推奨値を追加する

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

## カスタムフィールドに推奨値を追加する

を拡張するには、以下を実行します。 `meta:enum` カスタムフィールドの場合は、PATCHリクエストを通じて、フィールドの親クラス、フィールドグループ、またはデータ型を更新できます。

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

このガイドでは、スキーマレジストリ API で文字列フィールドに推奨値を追加する方法について説明しました。 詳しくは、 [API でのカスタムフィールドの定義](./custom-fields-api.md) 様々なフィールドタイプの作成方法の詳細については、を参照してください。
