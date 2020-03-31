---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: クラスの作成
topic: developer guide
translation-type: tm+mt
source-git-commit: 60911e32fd9235be2a258e60818011a42cd5ceba

---


# クラスの作成

スキーマの主要な構成要素はクラスです。 クラスには、スキーマのコアデータを取り込むために定義する必要があるフィールドの最小セットが含まれます。 例えば、車やトラック用のスキーマを設計している場合、すべての車の基本的な共通の特性を説明したVehicleというクラスを使用することがよくあります。

アドビやその他のExperience Platformパートナーが提供するいくつかの標準クラスがありますが、独自のクラスを定義してスキーマレジストリに保存することもできます。 作成したクラスを実装するスキーマを作成し、新しく定義したクラスと互換性のあるミックスインを定義できます。

>[!NOTE] 定義したクラスに基づいてスキーマを作成する場合、標準のミックスインは使用できません。 各ミックスインは、属性で互換性のあるクラスを定義 `meta:intendedToExtend` します。 新しいクラスと互換性のあるミックスインの定義を開始すると（ミックスインのフィールドの新しいクラスのを使用して）、定義したクラスを実装するスキーマを定義するたびに、それらのミックスインを再利用できます。 `$id``meta:intendedToExtend` 詳しくは、ミックスインの作成 [とスキーマの作成](create-mixin.md)[(英語](create-schema.md) )の節を参照してください。

**API形式**

```http
POST /tenant/classes
```

**リクエスト**

クラスを作成(POST)するリクエストには、次の2つの値のいずれかを含 `allOf` む属性を含め `$ref` る必要があります。ま `https://ns.adobe.com/xdm/data/record` た `https://ns.adobe.com/xdm/data/time-series`は これらの値は、クラスの基となる動作（レコードまたは時系列）を表します。 レコードデータと時系列データの違いについて詳しくは、スキーマ構成の基本内の動作タイプに関する節を [参照してください](../schema/composition.md)。

クラスを定義する際に、クラス定義にミックスインやカスタムフィールドを含めることもできます。 これにより、追加したミックスインとフィールドが、クラスを実装するすべてのスキーマに含まれます。 次のリクエスト例では、「プロパティ」と呼ばれるクラスを定義しています。このクラスは、会社が所有および操作する様々なプロパティに関する情報を取得します。 クラスが使用さ `propertyId` れるたびに含めるフィールドが含まれます。

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
| `_{TENANT_ID}` | 組織の `TENANT_ID` 名前空間です。 組織で作成されるすべてのリソースは、このプロパティを含めて、リソースレジストリ内の他のリソースとの競合を避ける必要があるスキーマです。 |
| `allOf` | 新しいリストによってプロパティが継承されるリソースのクラスです。 配列内のオブ `$ref` ジェクトの1つが、クラスの動作を定義します。 この例では、クラスは「record」動作を継承します。 |

**応答**

成功した応答は、HTTPステータス201（作成済み）と、、およびを含む新しく作成されたクラスの詳細を含むペイロ `$id`ードを `meta:altId`返しま `version`す。 これらの3つの値は読み取り専用で、割り当てはスキーマレジストリです。

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

テナントコンテナ内のすべてのクラスにリストに対してGET要求を実行すると、Propertyクラスが含まれるようになりました。 また、URLエンコードされたURIを使用して参照(GET)リクエストを実行し、新しいクラスを直接 `$id` 表示することもできます。 参照リクエストを実行する際は、必 `version` ず「承認」ヘッダーにを含めてください。
