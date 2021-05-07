---
keywords: Experience Platform；ホーム；人気の高いトピック；API;XDM;XDM;XDM system；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；スキーマレジストリ；フィールドグループ；フィールドグループ；作成
solution: Experience Platform
title: フィールドグループAPIエンドポイント
description: スキーマレジストリAPIの/fieldgroupsエンドポイントを使用すると、エクスペリエンスアプリケーション内のXDMスキーマフィールドグループをプログラムで管理できます。
topic: 開発者ガイド
translation-type: tm+mt
source-git-commit: b25c545e86c8ffd6b5832893152aef597feaf71f
workflow-type: tm+mt
source-wordcount: '1196'
ht-degree: 9%

---


# スキーマフィールドグループエンドポイント

スキーマフィールドグループは、個人、住所、Webブラウザーの環境など、特定の概念を表す1つ以上のフィールドを定義する再利用可能なコンポーネントです。 フィールドグループは、表すデータの動作（レコードまたは時系列）に応じて、互換性のあるクラスを実装するスキーマの一部として含めることを意図しています。 [!DNL Schema Registry] APIの`/fieldgroups`エンドポイントを使用すると、エクスペリエンスアプリケーション内のフィールドグループをプログラムで管理できます。

## はじめに

このガイドで使用されるエンドポイントは、[[!DNL Schema Registry] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml)の一部です。 先に進む前に、[はじめに](./getting-started.md)を読んで、関連ドキュメントへのリンク、このドキュメントのサンプルAPI呼び出しを読むためのガイド、Experience PlatformAPIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報を確認してください。

## フィールドグループのリストを取得{#list}

`/global/fieldgroups`または`/tenant/fieldgroups`に対してそれぞれGETリクエストを行うことで、`global`コンテナーまたは`tenant`ー下のすべてのフィールドグループをリストできます。

>[!NOTE]
>
>リソースをリストする場合、スキーマレジストリでは結果セットが300項目に制限されます。 この制限を超えるリソースを返すには、ページングパラメーターを使用する必要があります。 また、結果をフィルターし、返されるリソースの数を減らすために、追加のクエリパラメーターを使用することもお勧めします。 詳しくは、付録ドキュメントの[クエリパラメーター](./appendix.md#query)の節を参照してください。

**API 形式**

```http
GET /{CONTAINER_ID}/fieldgroups?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | フィールドグループを取得するコンテナ:`global`はAdobeが作成したフィールドグループの場合、または`tenant`は組織が所有するフィールドグループの場合です。 |
| `{QUERY_PARAMS}` | 結果をフィルターするオプションのクエリパラメーター。使用可能なパラメーターのリストについては、[付録ドキュメント](./appendix.md#query)を参照してください。 |

**リクエスト**

次のリクエストは、`tenant`コンテナからフィールドグループのリストを取得します。`orderby`クエリパラメーターを使用して、`title`属性でフィールドグループを並べ替えます。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/fieldgroups?orderby=title \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、リクエストで送信される`Accept`ヘッダーに依存します。 フィールドグループのリストには、次の`Accept`ヘッダーを使用できます。

| `Accept` ヘッダー | 説明 |
| --- | --- |
| `application/vnd.adobe.xed-id+json` | 各リソースの短い概要を返します。 リソースのリストを表示する際に推奨されるヘッダーです。 (制限：300) |
| `application/vnd.adobe.xed+json` | 各リソースの完全なJSONフィールドグループを返します。元の`$ref`と`allOf`が含まれます。 (制限：300) |

**応答**

上記のリクエストでは`application/vnd.adobe.xed-id+json` `Accept`ヘッダーが使用されていたので、応答には各フィールドグループの`title`、`$id`、`meta:altId`、`version`属性のみが含まれます。 他の`Accept`ヘッダー(`application/vnd.adobe.xed+json`)を使用すると、各フィールドグループのすべての属性が返されます。 回答で必要な情報に応じて、適切な`Accept`ヘッダーを選択します。

```json
{
  "results": [
    {
      "$id": "https://ns.adobe.com/{TENANT_ID}/fieldgroups/6ece98e9842907c78c651f5b249d9f09",
      "meta:altId": "_{TENANT_ID}.fieldgroups.6ece98e9842907c78c651f5b249d9f09",
      "version": "1.0",
      "title": "CRM Data"
    },
    {
      "$id": "https://ns.adobe.com/{TENANT_ID}/fieldgroups/6386ee478a30914964c6e676ad55603c",
      "meta:altId": "_{TENANT_ID}.fieldgroups.6386ee478a30914964c6e676ad55603c",
      "version": "1.9",
      "title": "Loyalty Member Details"
    },
    {
      "$id": "https://ns.adobe.com/{TENANT_ID}/fieldgroups/67626b2830db3d3ea6c8f9d007aa5797",
      "meta:altId": "_{TENANT_ID}.fieldgroups.67626b2830db3d3ea6c8f9d007aa5797",
      "version": "1.0",
      "title": "Restaurant"
    },
    {
      "$id": "https://ns.adobe.com/{TENANT_ID}/fieldgroups/2583b25b613fec704da6ef70cf527688",
      "meta:altId": "_{TENANT_ID}.fieldgroups.2583b25b613fec704da6ef70cf527688",
      "version": "1.1",
      "title": "Retail Customer Preferences"
    },
  ],
  "_page": {
    "orderby": "title",
    "next": null,
    "count": 3
  },
  "_links": {
    "next": null,
    "global_schemas": {
      "href": "https://platform.adobe.io/data/foundation/schemaregistry/global/fieldgroups"
    }
  }
}
```

## フィールドグループ{#lookup}を検索

GETリクエストのパスにフィールドグループのIDを含めると、特定のフィールドグループを検索できます。

**API 形式**

```http
GET /{CONTAINER_ID}/fieldgroups/{FIELD_GROUP_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | 取得するフィールドグループを格納するコンテナ。Adobeが作成したフィールドグループの場合は`global`、組織が所有するフィールドグループの場合は`tenant`です。 |
| `{FIELD_GROUP_ID}` | 検索するフィールドグループの`meta:altId`またはURLエンコードされた`$id`。 |

**リクエスト**

次のリクエストは、パスに指定された`meta:altId`値を使用してフィールドグループを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/fieldgroups/_{TENANT_ID}.fieldgroups.8779fd45d6e4eb074300023a439862bbba359b60d451627a \
  -H 'Accept: application/vnd.adobe.xed+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、リクエストで送信される`Accept`ヘッダーに依存します。 すべての参照リクエストで、`version`を`Accept`ヘッダーに含める必要があります。 次の`Accept`ヘッダーを使用できます。

| `Accept` ヘッダー | 説明 |
| ------- | ------------ |
| `application/vnd.adobe.xed+json; version={MAJOR_VERSION}` | `$ref` および `allOf` で生、タイトルと説明を含む |
| `application/vnd.adobe.xed-full+json; version={MAJOR_VERSION}` | `$ref` および `allOf` を解決、タイトルと説明を含む |
| `application/vnd.adobe.xed-notext+json; version={MAJOR_VERSION}` | `$ref` および `allOf` で生、タイトルや説明なし |
| `application/vnd.adobe.xed-full-notext+json; version={MAJOR_VERSION}` | `$ref` および `allOf` で解決、タイトルや説明なし |
| `application/vnd.adobe.xed-full-desc+json; version={MAJOR_VERSION}` | `$ref` および `allOf` で解決、説明を含む |

**応答**

成功した応答は、フィールドグループの詳細を返します。 返されるフィールドは、リクエストで送信される`Accept`ヘッダーによって異なります。 異なる`Accept`ヘッダーを試して、回答を比較し、使用事例に最適なヘッダーを判断します。

```json
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/fieldgroups/8779fd45d6e4eb074300023a439862bbba359b60d451627a",
  "meta:altId": "_{TENANT_ID}.fieldgroups.8779fd45d6e4eb074300023a439862bbba359b60d451627a",
  "meta:resourceType": "fieldgroups",
  "version": "1.2",
  "title": "Favorite Hotel",
  "type": "object",
  "description": "",
  "definitions": {
    "customFields": {
      "type": "object",
      "properties": {
        "_{TENANT_ID}": {
          "type": "object",
          "properties": {
            "favoriteHotel": {
              "title": "Favorite Hotel",
              "description": "Reference field for hotel schema.",
              "type": "string",
              "isRequired": false,
              "meta:xdmType": "string"
            }
          },
          "meta:xdmType": "object"
        }
      },
      "meta:xdmType": "object"
    }
  },
  "allOf": [
    {
      "$ref": "#/definitions/customFields",
      "type": "object",
      "meta:xdmType": "object"
    }
  ],
  "imsOrg": "{IMS_ORG}",
  "meta:extensible": true,
  "meta:abstract": true,
  "meta:intendedToExtend": [
    "https://ns.adobe.com/xdm/context/profile"
  ],
  "meta:xdmType": "object",
  "meta:registryMetadata": {
    "repo:createdDate": 1594941263588,
    "repo:lastModifiedDate": 1594941538433,
    "xdm:createdClientId": "{CLIENT_ID}",
    "xdm:lastModifiedClientId": "{CLIENT_ID}",
    "xdm:createdUserId": "{USER_ID}",
    "xdm:lastModifiedUserId": "{USER_ID}",
    "eTag": "5e8a5e508eb2ed344c08cb23ed27cfb60c841bec59a2f7513deda0f7af903021",
    "meta:globalLibVersion": "1.15.4"
  },
  "meta:containerId": "tenant",
  "meta:tenantNamespace": "_{TENANT_ID}"
}
```

## フィールドグループ{#create}を作成

POSTリクエストを行うことで、`tenant`コンテナの下にカスタムフィールドグループを定義できます。

**API 形式**

```http
POST /tenant/fieldgroups
```

**リクエスト**

新しいフィールドグループを定義する場合は、フィールドグループと互換性のあるクラスの`$id`をリストした`meta:intendedToExtend`属性を含める必要があります。 この例では、フィールドグループは、以前に定義した`Property`クラスと互換性があります。 クラスや他のフィールドグループが提供する同様のフィールドとの衝突を回避するため、カスタムフィールドは`_{TENANT_ID}`の下（例に示すように）にネストする必要があります。

>[!NOTE]
>
>フィールドグループに含めるフィールドの種類を定義する方法の詳細については、『[フィールド制約ガイド](../schema/field-constraints.md#define-fields)』を参照してください。

```SHELL
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/fieldgroups \
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

正常な応答は、HTTPステータス201（作成済み）と、新しく作成されたフィールドグループ（`$id`、`meta:altId`、`version`など）の詳細を含むペイロードを返します。 これらの値は読み取り専用で、[!DNL Schema Registry]によって割り当てられます。

```JSON
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/fieldgroups/8779fd45d6e4eb074300023a439862bbba359b60d451627a",
  "meta:altId": "_{TENANT_ID}.fieldgroups.8779fd45d6e4eb074300023a439862bbba359b60d451627a",
  "meta:resourceType": "fieldgroups",
  "version": "1.2",
  "title": "Property Details",
  "type": "object",
  "description": "Detailed information related to the properties owned and operated by the company.",
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
      "$ref": "#/definitions/customFields",
      "type": "object",
      "meta:xdmType": "object"
    }
  ],
  "imsOrg": "{IMS_ORG}",
  "meta:extensible": true,
  "meta:abstract": true,
  "meta:intendedToExtend": [
    "https://ns.adobe.com/xdm/context/profile"
  ],
  "meta:xdmType": "object",
  "meta:registryMetadata": {
    "repo:createdDate": 1594941263588,
    "repo:lastModifiedDate": 1594941538433,
    "xdm:createdClientId": "{CLIENT_ID}",
    "xdm:lastModifiedClientId": "{CLIENT_ID}",
    "xdm:createdUserId": "{USER_ID}",
    "xdm:lastModifiedUserId": "{USER_ID}",
    "eTag": "5e8a5e508eb2ed344c08cb23ed27cfb60c841bec59a2f7513deda0f7af903021",
    "meta:globalLibVersion": "1.15.4"
  },
  "meta:containerId": "tenant",
  "meta:tenantNamespace": "_{TENANT_ID}"
}
```

テナントコンテナ内のすべてのフィールドグループ](#list)に対してGETリクエストを実行すると、プロパティの詳細フィールドグループが含まれるようになりました。また、[URLエンコードされた`$id`URIを使用して、ルックアップ(GET)リクエスト](#lookup)を実行し、新しいフィールドグループを直接表示できます。[

## フィールドグループの更新{#put}

PUT操作を使用してフィールドグループ全体を置き換え、リソースを再書き込みすることができます。 PUT要求を通じてフィールドグループを更新する場合、本文には、POST要求で[新しいフィールドグループ](#create)を作成する際に必要となるすべてのフィールドを含める必要があります。

>[!NOTE]
>
>フィールドグループ全体を置き換える代わりに、フィールドグループの一部だけを更新したい場合は、[フィールドグループ](#patch)の一部を更新するのを参照してください。

**API 形式**

```http
PUT /tenant/fieldgroups/{FIELD_GROUP_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{FIELD_GROUP_ID}` | 再書き込みするフィールドグループの`meta:altId`またはURLエンコードされた`$id`。 |

**リクエスト**

次のリクエストは、既存のフィールドグループを書き直し、新しい`propertyCountry`フィールドを追加します。

```SHELL
curl -X PUT \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/fieldgroups/_{TENANT_ID}.fieldgroups.8779fd45d6e4eb074300023a439862bbba359b60d451627a \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "title": "Property Details",
        "description": "Detailed information related to the properties owned and operated by the company.",
        "type": "object",
        "meta:intendedToExtend": ["https://ns.adobe.com/{TENANT_ID}/classes/19e1d8b5098a7a76e2c10a81cbc99590"],
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
                  "propertyCountry": {
                    "title": "Property Country",
                    "description": "Country where the property is located.",
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

正常に応答すると、更新されたフィールドグループの詳細が返されます。

```JSON
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/fieldgroups/8779fd45d6e4eb074300023a439862bbba359b60d451627a",
  "meta:altId": "_{TENANT_ID}.fieldgroups.8779fd45d6e4eb074300023a439862bbba359b60d451627a",
  "meta:resourceType": "fieldgroups",
  "version": "1.2",
  "title": "Property Details",
  "type": "object",
  "description": "Detailed information related to the properties owned and operated by the company.",
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
            "propertyCountry": {
              "title": "Property Country",
              "description": "Country where the property is located.",
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
      "$ref": "#/definitions/customFields",
      "type": "object",
      "meta:xdmType": "object"
    }
  ],
  "imsOrg": "{IMS_ORG}",
  "meta:extensible": true,
  "meta:abstract": true,
  "meta:intendedToExtend": [
    "https://ns.adobe.com/xdm/context/profile"
  ],
  "meta:xdmType": "object",
  "meta:registryMetadata": {
    "repo:createdDate": 1594941263588,
    "repo:lastModifiedDate": 1594941538433,
    "xdm:createdClientId": "{CLIENT_ID}",
    "xdm:lastModifiedClientId": "{CLIENT_ID}",
    "xdm:createdUserId": "{USER_ID}",
    "xdm:lastModifiedUserId": "{USER_ID}",
    "eTag": "5e8a5e508eb2ed344c08cb23ed27cfb60c841bec59a2f7513deda0f7af903021",
    "meta:globalLibVersion": "1.15.4"
  },
  "meta:containerId": "tenant",
  "meta:tenantNamespace": "_{TENANT_ID}"
}
```

## フィールドグループの一部の更新{#patch}

PATCHリクエストを使用して、フィールドグループの一部を更新できます。 [!DNL Schema Registry]は、`add`、`remove`、`replace`を含む、すべての標準的なJSONパッチ操作をサポートしています。 JSONパッチについて詳しくは、[APIの基本的なガイド](../../landing/api-fundamentals.md#json-patch)を参照してください。

>[!NOTE]
>
>個々のフィールドを更新する代わりに、リソース全体を新しい値に置き換える場合は、[PUT演算](#put)を使用したフィールドグループの置き換えの節を参照してください。

**API 形式**

```http
PATCH /tenant/fieldgroups/{FIELD_GROUP_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{FIELD_GROUP_ID}` | 更新するフィールドグループのURLエンコードされた`$id` URIまたは`meta:altId`です。 |

**リクエスト**

以下のリクエスト例では、既存のフィールドグループの`description`を更新し、新しい`propertyCity`フィールドを追加しています。

リクエスト本体は配列の形をとり、リストに表示された各オブジェクトは個々のフィールドに対する特定の変更を表します。 各オブジェクトは、実行する操作(`op`)、操作を実行するフィールド(`path`)、およびその操作に含める情報(`value`)を含む。

```SHELL
curl -X PATCH \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/fieldgroups/_{TENANT_ID}.fieldgroups.8779fd45d6e4eb074300023a439862bbba359b60d451627a \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'content-type: application/json' \
  -d '[
        {
          "op": "replace",
          "path": "/description",
          "value": "Details relating to a property operated by the company."
        },
        { 
          "op": "add",
          "path": "/definitions/property/properties/_{TENANT_ID}/properties/propertyCity",
          "value": {
            "title": "Property City",
            "description": "City where the property is located.",
            "type": "string"
          }
        }
      ]'
```

**応答**

応答には、両方の操作が正常に実行されたことが示されます。`description`が更新され、`propertyCountry`が`definitions`に追加されました。

```JSON
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/fieldgroups/8779fd45d6e4eb074300023a439862bbba359b60d451627a",
  "meta:altId": "_{TENANT_ID}.fieldgroups.8779fd45d6e4eb074300023a439862bbba359b60d451627a",
  "meta:resourceType": "fieldgroups",
  "version": "1.2",
  "title": "Property Details",
  "type": "object",
  "description": "Details relating to a property operated by the company.",
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
            "propertyCountry": {
              "title": "Property Country",
              "description": "Country where the property is located.",
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
      "$ref": "#/definitions/customFields",
      "type": "object",
      "meta:xdmType": "object"
    }
  ],
  "imsOrg": "{IMS_ORG}",
  "meta:extensible": true,
  "meta:abstract": true,
  "meta:intendedToExtend": [
    "https://ns.adobe.com/xdm/context/profile"
  ],
  "meta:xdmType": "object",
  "meta:registryMetadata": {
    "repo:createdDate": 1594941263588,
    "repo:lastModifiedDate": 1594941538433,
    "xdm:createdClientId": "{CLIENT_ID}",
    "xdm:lastModifiedClientId": "{CLIENT_ID}",
    "xdm:createdUserId": "{USER_ID}",
    "xdm:lastModifiedUserId": "{USER_ID}",
    "eTag": "5e8a5e508eb2ed344c08cb23ed27cfb60c841bec59a2f7513deda0f7af903021",
    "meta:globalLibVersion": "1.15.4"
  },
  "meta:containerId": "tenant",
  "meta:tenantNamespace": "_{TENANT_ID}"
}
```

## フィールドグループ{#delete}の削除

場合によっては、スキーマレジストリからフィールドグループを削除する必要があります。 これは、パスで指定されたフィールドグループIDを使用してDELETEリクエストを実行することで行われます。

**API 形式**

```http
DELETE /tenant/fieldgroups/{FIELD_GROUP_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{FIELD_GROUP_ID}` | 削除するフィールドグループのURLエンコードされた`$id` URIまたは`meta:altId`です。 |

**リクエスト**

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/fieldgroups/_{TENANT_ID}.fieldgroups.d5cc04eb8d50190001287e4c869ebe67 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、空白の本文とともに HTTP ステータス 204（コンテンツなし）を返します。

フィールドグループに対して[ルックアップ(GET)リクエスト](#lookup)を実行して、削除を確認できます。 `Accept`ヘッダーを要求に含める必要がありますが、フィールドグループがスキーマレジストリから削除されたので、HTTPステータス404 （見つかりません）を受け取る必要があります。