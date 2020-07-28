---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Mixin の作成
topic: developer guide
translation-type: tm+mt
source-git-commit: d04bf35e49488ab7d5e07de91eb77d0d9921b6fa
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 92%

---


# Mixin の作成

mixin は、「住所」や「プロファイル環境設定」など、特定の概念を説明するために使用される一連のフィールドです。多数の標準 mixin を使用できます。また、組織に固有の情報を取得する場合、独自の mixin を定義することもできます。各 mixin には、mixin と互換性があるクラスをリストする `meta:intendedToExtend` フィールドが含まれています。

使用可能なすべての mixin を確認し、各 mixin に含まれるフィールドについて理解しておくと役に立ちます。「グローバル」および「テナント」コンテナのそれぞれに対してリクエストを実行し、&quot;meta:intendedToExtend&quot; フィールドが（使用中の）クラスと一致する mixin のみを返すことにより、特定のクラスと互換性のあるすべての mixin をリスト（GET）できます。The examples below will return all mixins that can be used with the [!DNL XDM Individual Profile] class:

```http
GET /global/mixins?property=meta:intendedToExtend==https://ns.adobe.com/xdm/context/profile
GET /tenant/mixins?property=meta:intendedToExtend==https://ns.adobe.com/xdm/context/profile
```

以下の例の API リクエストは、テナントコンテナに新しい mixin を作成します。

**API 形式**

```http
POST /tenant/mixins
```

**リクエスト**

新しい mixin を定義する場合は、`meta:intendedToExtend` 属性を含めて、その mixin と互換性があるクラスの `$id` をリストする必要があります。この例では、 mixin は、以前に定義した Property クラスと互換性があります。クラススキーマの他の mixin やフィールドとの競合を避けるために、カスタムフィールドは `_{TENANT_ID}` の下にネストする必要があります（例で示されているように）。`propertyConstruction` フィールドは、前の呼び出しで作成されたデータ型への参照であることに注意してください。

```SHELL
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/mixins \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "title":"Property Details",
        "description":"Detailed information related to the properties owned and operated by the company.",
        "type":"object",
        "meta:intendedToExtend":["https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590"],
        "definitions": {
          "property": {
            "properties": {
              "_{TENANT_ID}": {
              "type":"object",
              "properties": {
                  "propertyName": {
                    "type": "string",
                    "title": "Property Name",
                    "description": "Name of the property"
                  },
                  "propertyCity": {
                    "title": "Property City",
                    "description": "City where the property is located.",
                    "type": "string"
                  },
                  "phoneNumber": {
                    "title": "Phone Number",
                    "description": "Primary phone number for the property.",
                    "type": "string"
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
                    }
                  },
                  "propertyConstruction": {
                    "$ref": "https://ns.adobe.com/{TENANT_ID}/datatypes/24c643f618647344606222c494bd0102"
                  }
                }
              }
            }
          }
        },
        "allOf": [
            {
                "$ref": "#/definitions/property"
            }
        ]
}'
```

**応答**

成功した応答は、HTTP ステータス 201 （Created）と、新しく作成された mixin の詳細（`$id`、`meta:altId`、`version` など）を含むペイロードを返します。These values are read-only and are assigned by the [!DNL Schema Registry].

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
                        "propertyCity": {
                            "title": "Property City",
                            "description": "City where the property is located.",
                            "type": "string",
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
    "version": "1.0",
    "meta:resourceType": "mixins",
    "meta:registryMetadata": {
        "repo:createDate": 1552088205144,
        "repo:lastModifiedDate": 1552088205144,
        "xdm:createdClientId": "{CREATED_CLIENT}",
        "xdm:repositoryCreatedBy": "{CREATED_BY}"
    }
}
```

GET リクエストを実行して、テナントコンテナ内のすべての mixin をリストすると、Vehicle Details mixin が含まれます。または、URL エンコードされた `$id` URI を使用してルックアップ（GET）リクエストを実行して、新しい mixin を直接表示できます。すべてのルックアップリクエストの Accept ヘッダーに `version` を必ず含めてください。
