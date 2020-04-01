---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ミックスインの作成
topic: developer guide
translation-type: tm+mt
source-git-commit: b2ceac3de73ac622dc885eb388e46e93551f43a8

---


# ミックスインの作成

ミックスインは、「住所」や「プロファイル環境設定」など、特定の概念を説明するために使用される一連のフィールドです。 多数の標準ミックスインを使用できます。また、組織に固有の情報を取り込む場合に独自のミックスインを定義することもできます。 各ミックスインには、ミックスイ `meta:intendedToExtend` ンが互換性を持つクラスをリストするフィールドが含まれています。

使用可能なすべてのミックスインを確認し、各ミックスインに含まれるフィールドについて理解しておくと役に立ちます。 「グローバル」コンテナと「テナント」要素のそれぞれに対してリクエストを実行し、「meta:intendedToExtend」フィールドが使用しているクラスに一致するミックスインのみを返すことで、特定のクラスと互換性のあるすべてのミックスインをリスト(GET)できます。 以下の例は、XDM Individualプロファイルクラスで使用できるすべてのミックスインを返します。

```http
GET /global/mixins?property=meta:intendedToExtend==https://ns.adobe.com/xdm/context/profile
GET /tenant/mixins?property=meta:intendedToExtend==https://ns.adobe.com/xdm/context/profile
```

以下の例のAPIリクエストは、テナントコンテナに新しいmixinを作成します。

**API形式**

```http
POST /tenant/mixins
```

**リクエスト**

新しいミックスインを定義する場合は、そのミックスインに `meta:intendedToExtend` 互換性のあるクラスの一 `$id` 覧を示す属性を含める必要があります。 この例では、ミックスインは、以前に定義したPropertyクラスと互換性があります。 クラススキーマの他のミックスインやフ `_{TENANT_ID}` ィールドとの衝突を避けるために、カスタムフィールドは（例に示すように）下にネストする必要があります。 このフィールド `propertyConstruction` は、前の呼び出しで作成されたデータ型への参照です。

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

成功した応答は、HTTPステータス201（作成済み）と、新しく作成されたミックスインの詳細（、およびを含む）を含むペ `$id`イロード `meta:altId`を返しま `version`す。 これらの値は読み取り専用で、割り当てはスキーマレジストリです。

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

テナントコンテナ内のすべてのミックスインをリストに対してGET要求を実行すると、車両の詳細ミックスインが含まれるようになりました。また、URLエンコードされた `$id` URIを使用してルックアップ(GET)要求を実行し、新しいミックスインを直接表示できます。 すべての参照リクエストの「受 `version` け入れ」ヘッダーにを必ず含めてください。
