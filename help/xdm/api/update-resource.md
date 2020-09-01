---
keywords: Experience Platform;home;popular topics;api;API;XDM;XDM system;;experience data model;Experience data model;Experience Data Model;data model;Data Model;schema registry;Schema Registry;update;Update;patch;PATCH
solution: Experience Platform
title: リソースのアップデート
description: 'PATCH リクエストを使用して、テナントコンテナ内のリソースを変更またはアップデートできます。スキーマレジストリは、追加、削除、置換など、すべての標準 JSON パッチ操作をサポートします。 '
topic: developer guide
translation-type: tm+mt
source-git-commit: 74a4a3cc713cc068be30379e8ee11572f8bb0c63
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 92%

---


# リソースのアップデート

PATCH リクエストを使用して、テナントコンテナ内のリソースを変更またはアップデートできます。The [!DNL Schema Registry] supports all standard JSON Patch operations, including add, remove, and replace.

使用可能な操作など、JSON パッチの詳細については、[JSON パッチの公式ドキュメント](http://jsonpatch.com/)を参照してください。

>[!NOTE]
>
>個々のフィールドをアップデートする代わりに、リソース全体を新しい値に置き換える場合は、[PUT 操作を使用したリソースの置き換え](replace-resource.md)に関するドキュメントを参照してください。

## スキーマへの mixin の追加

最も一般的な PATCH 操作の 1 つでは、以下の例で示すように、以前に定義した mixin を XDM スキーマに追加します。

**API 形式**

```http
PATCH /tenant/{RESOURCE_TYPE}/{RESOURCE_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{RESOURCE_TYPE}` | The type of resource to be updated from the [!DNL Schema Library]. 有効なタイプは、`datatypes`、`mixins`、`schemas` および `classes` です。 |
| `{RESOURCE_ID}` | リソースの URL エンコードされた `$id` URI または `meta:altId`。 |

**リクエスト**

PATCH 操作を使用すると、スキーマをアップデートして、以前に作成した mixin で定義されたフィールドを含めることができます。この操作を行うには、`meta:altId` または URL エンコードされた `$id` URI を使用して、スキーマに対して PATCH リクエストを実行する必要があります。

リクエスト本文には、実行する操作（`op`）、操作を実行する場所（`path`）、操作に含める情報（`value`）が含まれます。この例では、 mixin の `$id` 値がターゲットスキーマの `meta:extends` フィールドと `allOf` フィールドの両方に追加されます。

```SHELL
curl -X PATCH\
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'content-type: application/json' \
  -d '[
        { "op": "add", "path": "/meta:extends/-", "value":  "https://ns.adobe.com/{TENANT_ID}/mixins/e49cbb2eec33618f686b8344b4597ecf"},
        { "op": "add", "path": "/allOf/-", "value":  {"$ref": "https://ns.adobe.com/{TENANT_ID}/mixins/e49cbb2eec33618f686b8344b4597ecf"}}
      ]'
```

**応答**

応答には、両方の操作が正常に実行されたことが示されます。Mixin `$id` が `meta:extends` 配列に追加され、mixin `$id` への参照（`$ref`）が `allOf` 配列に表示されます。

```JSON
{
    "title": "Property Information",
    "description": "Property-related information.",
    "type": "object",
    "allOf": [
        {
            "$ref": "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590"
        },
        {
            "$ref": "https://ns.adobe.com/{TENANT_ID}/mixins/e49cbb2eec33618f686b8344b4597ecf"
        }
    ],
    "meta:class": "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590",
    "meta:abstract": false,
    "meta:extensible": false,
    "meta:extends": [
        "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590",
        "https://ns.adobe.com/xdm/data/record",
        "https://ns.adobe.com/{TENANT_ID}/mixins/e49cbb2eec33618f686b8344b4597ecf"
    ],
    "meta:containerId": "tenant",
    "imsOrg": "{IMS_ORG}",
    "meta:altId": "_{TENANT_ID}.schemas.d5cc04eb8d50190001287e4c869ebe67",
    "meta:xdmType": "object",
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/d5cc04eb8d50190001287e4c869ebe67",
    "version": "1.1",
    "meta:resourceType": "schemas",
    "meta:registryMetadata": {
        "repo:createDate": 1552088461236,
        "repo:lastModifiedDate": 1552088649634,
        "xdm:createdClientId": "{CREATED_CLIENT}",
        "xdm:repositoryCreatedBy": "{CREATED_BY}"
    }
}
```

## リソースの個々のフィールドのアップデート

また、PATCH リクエストを送信して、スキーマレジストリリソース内の個々のフィールドに複数の変更を加えることができます。

**API 形式**

```SHELL
PATCH /tenant/{RESOURCE_TYPE}/{RESOURCE_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{RESOURCE_TYPE}` | The type of resource to be updated from the [!DNL Schema Library]. 有効なタイプは、`datatypes`、`mixins`、`schemas` および `classes` です。 |
| `{RESOURCE_ID}` | リソースの URL エンコードされた `$id` URI または `meta:altId`。 |

**リクエスト**

リクエスト本文には、操作（`op`）、場所（`path`）、 mixin のアップデートに必要な情報（`value`）が含まれます。このリクエストは、プロパティ詳細の mixin をアップデートして &quot;propertyCity&quot; フィールドを削除し、住所情報を含む標準のデータ型を参照する新しい &quot;propertyAddress&quot; フィールドを追加します。また、電子メール情報を含む標準のデータ型を参照する新しい &quot;emailAddress&quot; フィールドも追加します。

```SHELL
curl -X PATCH \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/mixins/_{TENANT_ID}.mixins.e49cbb2eec33618f686b8344b4597ecf \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'content-type: application/json' \
  -d '[
          { "op": "remove", "path": "/definitions/vehicles/properties/_{TENANT_ID}/properties/propertyCity"},
          { "op": "add", "path": "/definitions/property/properties/_{TENANT_ID}/properties/propertyAddress", "value":
            {
              "title": "Property Address",
              "description": "Address of the Property",
              "$ref": "https://ns.adobe.com/xdm/common/address"
            } 
          },
          { "op": "add", "path": "/definitions/property/properties/_{TENANT_ID}/properties/emailAddress", "value":
            {
              "title": "Property Email Address",
              "description": "Email address of the Property",
              "$ref": "https://ns.adobe.com/xdm/context/emailaddress"
            } 
          },
      ]'
```

**応答**

新しいフィールドが追加され、&quot;propertyCity&quot; フィールドが削除されたため、成功した応答には、操作が正常に完了したことが示されます。

```JSON
{
    "title": "Property Details",
    "description": "Detailed information related to the properties owned and operated by the company.",
    "type": "object",
    "meta:intendedToExtend": [
        "https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590"
    ],
    "definitions": {
        "property": {
            "properties": {
                "_{TENANT_ID}": {
                    "type": "object",
                    "properties": {
                        "propertyName": {
                            "type": "string",
                            "title": "Property Name",
                            "description": "Name of the property",
                            "meta:xdmType": "string"
                        },
                        "phoneNumber": {
                            "title": "Phone Number",
                            "description": "Primary phone number for the property.",
                            "type": "string",
                            "meta:xdmType": "string"
                        },
                        "propertyType": {
                            "type": "string",
                            "title": "Property Type",
                            "description": "Type and primary use of property.",
                            "enum": [
                                "retail",
                                "yoga",
                                "fitness"
                            ],
                            "meta:enum": {
                                "retail": "Retail Store",
                                "yoga": "Yoga Studio",
                                "fitness": "Fitness Center"
                            },
                            "meta:xdmType": "string"
                        },
                        "propertyConstruction": {
                            "$ref": "https://ns.adobe.com/{TENANT_ID}/datatypes/24c643f618647344606222c494bd0102"
                        },
                        "propertyAddress": {
                            "title": "Property Address",
                            "description": "Address of the Property",
                            "$ref": "https://ns.adobe.com/xdm/common/address"
                        },
                        "emailAddress": {
                            "title": "Property Email Address",
                            "description": "Email address of the Property",
                            "$ref": "https://ns.adobe.com/xdm/context/emailaddress"
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
            "$ref": "#/definitions/property"
        }
    ],
    "meta:abstract": true,
    "meta:extensible": true,
    "meta:containerId": "tenant",
    "imsOrg": "{IMS_ORG}",
    "meta:altId": "_{TENANT_ID}.mixins.e49cbb2eec33618f686b8344b4597ecf",
    "meta:xdmType": "object",
    "$id": "https://ns.adobe.com/{TENANT_ID}/mixins/e49cbb2eec33618f686b8344b4597ecf",
    "version": "1.1",
    "meta:resourceType": "mixins",
    "meta:registryMetadata": {
        "repo:createDate": 1552088205144,
        "repo:lastModifiedDate": 1552089776535,
        "xdm:createdClientId": "{CREATED_CLIENT}",
        "xdm:repositoryCreatedBy": "{CREATED_BY}"
    }
}
```
