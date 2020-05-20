---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リソースの更新
topic: developer guide
translation-type: tm+mt
source-git-commit: 0d3bee939226d9ef4ac1672b71e0d240f32c5dcf
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 2%

---


# リソースの更新

PATCH要求を使用して、テナントコンテナ内のリソースを変更または更新できます。 スキーマレジストリでは、追加、削除、置換など、すべての標準的なJSONパッチ操作がサポートされています。

使用可能な操作を含むJSONパッチについて詳しくは、公式の [JSONパッチドキュメントを参照してください](http://jsonpatch.com/)。

>[!NOTE] 個々のフィールドを更新する代わりに、リソース全体を新しい値に置き換える場合は、「PUT操作を使用したリソースの [置き換えに関するドキュメント](replace-resource.md)」を参照してください。

## スキーマ追加へのミックスイン

最も一般的なPATCH操作の1つは、以下の例に示すように、XDMスキーマに以前に定義したミックスインを追加することです。

**API形式**

```http
PATCH /tenant/{RESOURCE_TYPE}/{RESOURCE_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{RESOURCE_TYPE}` | スキーマライブラリから更新するリソースの種類です。 有効なタイプは、 `datatypes`、、、お `mixins`よび `schemas``classes`です。 |
| `{RESOURCE_ID}` | URLエンコードされた `$id` URIまたはリソース `meta:altId` のURIです。 |

**リクエスト**

PATCH操作を使用すると、スキーマを更新して、以前に作成したMixinで定義されたフィールドを含めることができます。 これを行うには、スキーマまたはURLエンコードされた `meta:altId``$id` URIを使用して、に対してPATCHリクエストを実行する必要があります。

リクエスト本体には、実行する操作(`op`)、操作を実行する操作(`path`)、操作に含める情報(`value`)が含まれます。 この例では、ターゲットスキーマのとの両方のフィールドにmixinの `$id` 値が追加され `meta:extends``allOf` ています。

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

この応答は、両方の操作が正常に実行されたことを示します。 mixin `$id` が配列に追加され、mixinへの参照( `meta:extends` )が`$ref`配列に表示され `$id``allOf` ます。

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

## リソースの個々のフィールドを更新する

また、スキーマレジストリリソース内の個々のフィールドに複数の変更を加えるPATCH要求を送信することもできます。

**API形式**

```SHELL
PATCH /tenant/{RESOURCE_TYPE}/{RESOURCE_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{RESOURCE_TYPE}` | スキーマライブラリから更新するリソースの種類です。 有効なタイプは、 `datatypes`、、、お `mixins`よび `schemas``classes`です。 |
| `{RESOURCE_ID}` | URLエンコードされた `$id` URIまたはリソース `meta:altId` のURIです。 |

**リクエスト**

リクエスト本体には、ミックスインの更新に必要な操作(`op`)、場所(`path`)、および情報(`value`)が含まれます。 この要求は、「プロパティの詳細」ミックスインを更新して「propertyCity」フィールドを削除し、住所情報を含む標準データ型を参照する新しい「propertyAddress」フィールドを追加します。 また、電子メール情報を含む標準データ型を参照する新しい「emailAddress」フィールドも追加します。

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

正常に完了した場合は、新しいフィールドが存在し、「propertyCity」フィールドが削除されたので、操作が正常に完了したことが示されます。

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
