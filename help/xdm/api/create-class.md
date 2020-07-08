---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: クラスの作成
topic: developer guide
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 0%

---


# クラスの作成

スキーマの主要な構成要素はクラスです。 このクラスには、スキーマのコアデータを取り込むために定義する必要があるフィールドの最小セットが含まれます。 例えば、車やトラックのスキーマを設計する場合、Vehicleというクラスを使用して、すべての車の基本的な共通性を説明していると考えられます。

アドビや他のExperience Platformパートナーが提供するいくつかの標準クラスがありますが、独自のクラスを定義してスキーマレジストリに保存することもできます。 その後、作成したクラスを実装するスキーマを作成し、新しく定義したクラスと互換性のあるミックスインを定義できます。

>[!NOTE]
>
>定義したクラスに基づいてスキーマを構成する場合、標準のミックスインは使用できません。 各ミックスインは、属性内で互換性のあるクラスを定義し `meta:intendedToExtend` ます。 新しいクラスと互換性のあるミックスインの定義を開始すると(mixinの `$id``meta:intendedToExtend` フィールドで新しいクラスのを使用して)、定義したクラスを実装するスキーマを定義するたびに、それらのミックスインを再利用できます。 詳しくは、ミックスインの [作成とスキーマの](create-mixin.md) 作成に関する節を参照してください [](create-schema.md) 。

**API形式**

```http
POST /tenant/classes
```

**リクエスト**

クラスを作成(POST)するリクエストには、次の2つの値のいずれかを含む `allOf` 属性を含める必要があ `$ref` ります。 `https://ns.adobe.com/xdm/data/record` または `https://ns.adobe.com/xdm/data/time-series`。 これらの値は、クラスが基にする動作（レコードまたは時系列）を表します。 レコードデータと時系列データの違いについて詳しくは、スキーマ構成の [基本事項に含まれる動作の種類に関する節を参照してください](../schema/composition.md)。

クラスを定義する際に、クラス定義にミックスインやカスタムフィールドを含めることもできます。 これにより、追加されたミックスインとフィールドが、クラスを実装するすべてのスキーマに含まれます。 次のリクエスト例は、会社が所有および操作する様々なプロパティに関する情報を取り込む、「Property」という名前のクラスを定義します。 クラスが使用されるたびに含める `propertyId` フィールドが含まれます。

```SHELL
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/classes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "title":"Property",
        "description":"Properties owned and operated by the company.",
        "type":"object",
        "definitions": {
          "property": {
            "properties": {
              "_{TENANT_ID}": {
                "type": "object",
                "properties": {
                  "property": {
                    "title": "Property Information",
                    "type": "object",
                    "description": "Information about different owned and operated properties.",
                    "properties": {
                      "propertyId": {
                        "title": "Property Identification Number",
                        "type": "string",
                        "description": "Unique Property identification number"
                      }
                    }
                  }
                }
              }
            },
            "type": "object"
          }
        },
        "allOf": [
          {
            "$ref": "https://ns.adobe.com/xdm/data/record"
          },
          {
            "$ref": "#/definitions/property"
          }
        ]
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `_{TENANT_ID}` | 組織の `TENANT_ID` 名前空間。 スキーマレジストリ内の他のリソースとの競合を回避するために、組織で作成するすべてのリソースにこのプロパティを含める必要があります。 |
| `allOf` | プロパティが新しいクラスに継承されるリソースのリストです。 配列内の `$ref` オブジェクトの1つがクラスの動作を定義します。 この例では、クラスは「record」動作を継承します。 |

**応答**

正常な応答は、HTTPステータス201（作成済み）と、新しく作成されたクラスの詳細（、、およびを含む）を含むペイロード `$id`を返し `meta:altId`ま `version`す。 これらの3つの値は読み取り専用で、スキーマレジストリによって割り当てられます。

```JSON
{
    "title": "Property",
    "description": "Properties owned and operated by the company.",
    "type": "object",
    "definitions": {
        "property": {
            "properties": {
                "_{TENANT_ID}": {
                    "type": "object",
                    "properties": {
                        "property": {
                            "title": "Property Information",
                            "type": "object",
                            "description": "Information about different owned and operated properties.",
                            "properties": {
                                "propertyId": {
                                    "title": "Property Identification Number",
                                    "type": "string",
                                    "description": "Unique Property identification number",
                                    "meta:xdmType": "string"
                                }
                            },
                            "meta:xdmType": "object"
                        }
                    },
                    "meta:xdmType": "object"
                }
            },
            "type": "object",
            "meta:xdmType": "object"
        }
    },
    "allOf": [
        {
            "$ref": "https://ns.adobe.com/xdm/data/record"
        },
        {
            "$ref": "#/definitions/property"
        }
    ],
    "meta:abstract": true,
    "meta:extensible": true,
    "meta:extends": [
        "https://ns.adobe.com/xdm/data/record"
    ],
    "meta:containerId": "tenant",
    "imsOrg": "{IMS_ORG}",
    "meta:altId": "_{TENANT_ID}.classes.19e1d8b5098a7a76e2c10a81cbc99590",
    "meta:xdmType": "object",
    "$id": "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590",
    "version": "1.0",
    "meta:resourceType": "classes",
    "meta:registryMetadata": {
        "repo:createDate": 1552086405448,
        "repo:lastModifiedDate": 1552086405448,
        "xdm:createdClientId": "{CREATED_CLIENT}",
        "xdm:repositoryCreatedBy": "{CREATED_BY}"
    }
}
```

テナントコンテナ内のすべてのクラスに対してリストへのGET要求を実行すると、Propertyクラスが含まれるようになりました。 また、URLエンコードされた `$id` URIを使用してルックアップ(GET)リクエストを実行し、新しいクラスを直接表示することもできます。 ルックアップリクエストを実行する場合は、必ずAcceptヘッダー `version` にを含めてください。
