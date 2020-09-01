---
keywords: Experience Platform;home;popular topics;api;API;XDM;XDM system;;experience data model;Experience data model;Experience Data Model;data model;Data Model;schema registry;Schema Registry;class;Class;classes;Classes;create
solution: Experience Platform
title: クラスの作成
description: スキーマの主な構成要素はクラスです。クラスには、スキーマのコアデータをキャプチャするために定義する必要があるフィールドの最小セットが含まれています。例えば、車やトラック用のスキーマをデザインしている場合、すべての車の基本的な共通の特性を説明した Vehicle クラスを使用することがよくあります。
topic: developer guide
translation-type: tm+mt
source-git-commit: 74a4a3cc713cc068be30379e8ee11572f8bb0c63
workflow-type: tm+mt
source-wordcount: '539'
ht-degree: 90%

---


# クラスの作成

スキーマの主な構成要素はクラスです。クラスには、スキーマのコアデータをキャプチャするために定義する必要があるフィールドの最小セットが含まれています。例えば、車やトラック用のスキーマをデザインしている場合、すべての車の基本的な共通の特性を説明した Vehicle クラスを使用することがよくあります。

There are several standard classes provided by Adobe and other [!DNL Experience Platform] partners, but you may also define your own classes and save them to the [!DNL Schema Registry]. 作成したクラスを実装するスキーマを使用例し、新しく定義したクラスと互換性のある mixin を定義できます。

>[!NOTE]
>
> 定義したクラスに基づいてスキーマを作成する場合、標準の mixin は使用できません。各 mixin は、`meta:intendedToExtend` 属性で互換性のあるクラスを定義します。新しいクラスと互換性のある mixin の定義を開始すると（mixin の `meta:intendedToExtend` フィールドで新しいクラスの `$id` を使用）、定義したクラスを実装するスキーマを定義するたびに、それらの mixin を再利用できます。詳しくは、「[mixin の作成](create-mixin.md)」と「[スキーマの作成](create-schema.md) 」の節を参照してください。

**API 形式**

```http
POST /tenant/classes
```

**リクエスト**

クラスを作成する（POST）リクエストには、`https://ns.adobe.com/xdm/data/record` または `https://ns.adobe.com/xdm/data/time-series` の 2 つの値のいずれかへの `$ref` を含む `allOf` 属性を含める必要があります。これらの値は、クラスの基となる動作（レコードまたは時系列）を表します。レコードデータと時系列データの違いについて詳しくは、「[スキーマ構成の基本](../schema/composition.md)」内の動作タイプに関する節を参照してください。

クラスを定義する際に、クラス定義に mixin やカスタムフィールドを含めることもできます。これにより、追加した mixin とフィールドが、クラスを実装するすべてのスキーマに含まれます。次のリクエスト例では、「Property」と呼ばれるクラスを定義しています。このクラスは、会社が所有および操作する様々なプロパティに関する情報を取得します。クラスが使用されるたびに含める `propertyId` フィールドが含まれます。

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
| `_{TENANT_ID}` | 組織の `TENANT_ID` 名前空間。All resources created by your organization must include this property to avoid collisions with other resources in the [!DNL Schema Registry]. |
| `allOf` | 新しいリストによってプロパティが継承されるリソースのクラス。配列内の `$ref` オブジェクトの 1 つが、クラスの動作を定義します。この例では、クラスは「レコード」動作を継承します。 |

**応答**

正常な応答は、HTTP ステータス 201（作成済み）と、新しく作成されたクラスの詳細（`$id`、`meta:altId`、`version`）を含むペイロードを返します。These three values are read-only and are assigned by the [!DNL Schema Registry].

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

GET リクエストを実行してテナントコンテナ内のすべてのクラスを一覧表示すると、Property クラスが含まれるようになります。また、URL エンコードされた `$id` URI を使用して検索（GET）リクエストを実行し、新しいクラスを直接表示することもできます。検索リクエストを実行する際は、必ず Accept ヘッダーに `version` を含めてください。
