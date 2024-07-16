---
keywords: Experience Platform；ホーム；人気のトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；スキーマレジストリ；フィールドグループ；フィールドグループ；create
solution: Experience Platform
title: フィールドグループ API エンドポイント
description: Schema Registry API の/fieldgroups エンドポイントを使用すると、エクスペリエンスアプリケーション内の XDM スキーマフィールドグループをプログラムで管理できます。
exl-id: d26257e4-c7d5-4bff-b555-7a2997c88c74
source-git-commit: 983682489e2c0e70069dbf495ab90fc9555aae2d
workflow-type: tm+mt
source-wordcount: '1197'
ht-degree: 12%

---

# スキーマフィールドグループエンドポイント

スキーマフィールドグループは、個人、住所、web ブラウザー環境など、特定の概念を表す 1 つ以上のフィールドを定義する、再利用可能なコンポーネントです。 フィールドグループは、表現するデータ（レコードまたは時系列）の動作に応じて、互換性のあるクラスを実装するスキーマの一部として含めることを目的としています。 [!DNL Schema Registry] API の `/fieldgroups` エンドポイントを使用すると、エクスペリエンスアプリケーション内のフィールドグループをプログラムで管理できます。

## はじめに

このガイドで使用するエンドポイントは、[[!DNL Schema Registry] API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) の一部です。 先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## フィールドグループのリストの取得 {#list}

`/global/fieldgroups` または `/tenant/fieldgroups` に対してそれぞれGETリクエストをおこなうことで、`global` コンテナまたは `tenant` コンテナの下のすべてのフィールドグループをリストできます。

>[!NOTE]
>
>リソースをリストする場合、スキーマレジストリでは結果セットを 300 項目に制限しています。 この制限を超えるリソースを返すには、ページングパラメーターを使用する必要があります。 また、追加のクエリパラメーターを使用して結果をフィルタリングし、返されるリソースの数を減らすこともお勧めします。 詳しくは、付録のドキュメントの [ クエリパラメーター ](./appendix.md#query) に関する節を参照してください。

**API 形式**

```http
GET /{CONTAINER_ID}/fieldgroups?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | 取得するフィールドグループのコンテナ：Adobeが作成したフィールドグループの `global` または組織が所有するフィールドグループの `tenant`。 |
| `{QUERY_PARAMS}` | 結果をフィルタリングするオプションのクエリパラメーター。 使用可能なパラメーターのリストについては、[ 付録ドキュメント ](./appendix.md#query) を参照してください。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、`orderby` クエリパラメーターを使用してフィールドグループを `title` 属性で並べ替え、`tenant` コンテナからフィールドグループのリストを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/fieldgroups?orderby=title \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、リクエストで送信される `Accept` ヘッダーによって異なります。 フィールドグループのリストに使用できる `Accept` ヘッダーは次のとおりです。

| `Accept` ヘッダー | 説明 |
| --- | --- |
| `application/vnd.adobe.xed-id+json` | 各リソースの短い概要を返します。 リソースをリストする場合は、このヘッダーをお勧めします。 （上限：300） |
| `application/vnd.adobe.xed+json` | 各リソースの完全な JSON フィールドグループを、元の `$ref` と `allOf` を含めて返します。 （上限：300） |

{style="table-layout:auto"}

**応答**

上記のリクエストでは `application/vnd.adobe.xed-id+json` `Accept` ヘッダーを使用しているので、応答には各フィールドグループの `title`、`$id`、`meta:altId` および `version` 属性のみが含まれています。 もう一方の `Accept` ヘッダー（`application/vnd.adobe.xed+json`）を使用すると、各フィールドグループのすべての属性を返します。 応答で必要な情報に応じて、適切な `Accept` ヘッダーを選択します。

```json
{
  "results": [
    {
      "$id": "https://ns.adobe.com/{TENANT_ID}/mixins/6ece98e9842907c78c651f5b249d9f09",
      "meta:altId": "_{TENANT_ID}.mixins.6ece98e9842907c78c651f5b249d9f09",
      "version": "1.0",
      "title": "CRM Data"
    },
    {
      "$id": "https://ns.adobe.com/{TENANT_ID}/mixins/6386ee478a30914964c6e676ad55603c",
      "meta:altId": "_{TENANT_ID}.mixins.6386ee478a30914964c6e676ad55603c",
      "version": "1.9",
      "title": "Loyalty Member Details"
    },
    {
      "$id": "https://ns.adobe.com/{TENANT_ID}/mixins/67626b2830db3d3ea6c8f9d007aa5797",
      "meta:altId": "_{TENANT_ID}.mixins.67626b2830db3d3ea6c8f9d007aa5797",
      "version": "1.0",
      "title": "Restaurant"
    },
    {
      "$id": "https://ns.adobe.com/{TENANT_ID}/mixins/2583b25b613fec704da6ef70cf527688",
      "meta:altId": "_{TENANT_ID}.mixins.2583b25b613fec704da6ef70cf527688",
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

## フィールドグループの検索 {#lookup}

GETリクエストのパスにフィールドグループの ID を含めることで、特定のフィールドグループを検索できます。

**API 形式**

```http
GET /{CONTAINER_ID}/fieldgroups/{FIELD_GROUP_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | 取得するフィールドグループを格納するコンテナ：Adobeが作成したフィールドグループの場合は `global`、組織が所有するフィールドグループの場合は `tenant`。 |
| `{FIELD_GROUP_ID}` | 検索するフィールドグループの `meta:altId` または URL エンコードされた `$id`。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、パスで指定された `meta:altId` 値によってフィールドグループを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/fieldgroups/_{TENANT_ID}.mixins.8779fd45d6e4eb074300023a439862bbba359b60d451627a \
  -H 'Accept: application/vnd.adobe.xed+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、リクエストで送信される `Accept` ヘッダーによって異なります。 すべての参照リクエストには、`Accept` ヘッダーに `version` を含める必要があります。 次の `Accept` ヘッダーを使用できます。

| `Accept` ヘッダー | 説明 |
| ------- | ------------ |
| `application/vnd.adobe.xed+json; version={MAJOR_VERSION}` | `$ref` および `allOf` で生、タイトルと説明を含む |
| `application/vnd.adobe.xed-full+json; version={MAJOR_VERSION}` | `$ref` および `allOf` を解決、タイトルと説明を含む |
| `application/vnd.adobe.xed-notext+json; version={MAJOR_VERSION}` | `$ref` および `allOf` で生、タイトルや説明なし |
| `application/vnd.adobe.xed-full-notext+json; version={MAJOR_VERSION}` | `$ref` および `allOf` で解決、タイトルや説明なし |
| `application/vnd.adobe.xed-full-desc+json; version={MAJOR_VERSION}` | `$ref` および `allOf` で解決、説明を含む |

{style="table-layout:auto"}

**応答**

応答が成功すると、フィールドグループの詳細が返されます。 返されるフィールドは、リクエストで送信される `Accept` ヘッダーによって異なります。 様々な `Accept` ヘッダーを試して、応答を比較し、ユースケースに最適なヘッダーを判断します。

```json
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/mixins/8779fd45d6e4eb074300023a439862bbba359b60d451627a",
  "meta:altId": "_{TENANT_ID}.mixins.8779fd45d6e4eb074300023a439862bbba359b60d451627a",
  "meta:resourceType": "mixins",
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
  "imsOrg": "{ORG_ID}",
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

## フィールドグループの作成 {#create}

POSTのリクエストを行うことで、`tenant` コンテナの下にカスタムフィールドグループを定義できます。

**API 形式**

```http
POST /tenant/fieldgroups
```

**リクエスト**

新しいフィールドグループを定義する際は、フィールドグループと互換性のあるクラスの `$id` をリストする `meta:intendedToExtend` 属性を含める必要があります。 この例では、フィールドグループは、以前に定義された `Property` クラスと互換性があります。 クラスや他のフィールドグループが提供する類似フィールドとの競合を避けるために、カスタムフィールドは（例に示すように） `_{TENANT_ID}` の下にネストする必要があります。

>[!NOTE]
>
>フィールドグループに含める様々なフィールドタイプを定義する方法について詳しくは、[API でのカスタムフィールドの定義 ](../tutorials/custom-fields-api.md#define-fields) に関するガイドを参照してください。

```SHELL
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/fieldgroups \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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

応答が成功すると、HTTP ステータス 201 （作成済み）と、新しく作成されたフィールドグループの詳細（`$id`、`meta:altId`、`version` など）を含むペイロードが返されます。 これらの値は読み取り専用で、[!DNL Schema Registry] によって割り当てられます。

```JSON
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/mixins/8779fd45d6e4eb074300023a439862bbba359b60d451627a",
  "meta:altId": "_{TENANT_ID}.mixins.8779fd45d6e4eb074300023a439862bbba359b60d451627a",
  "meta:resourceType": "mixins",
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
  "imsOrg": "{ORG_ID}",
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

テナントコンテナで [ すべてのフィールドグループをリスト ](#list) する」というGETリクエストを実行すると、プロパティの詳細フィールドグループが含まれるようになりました。または、URL エンコードされた `$id` URI を使用して [ 検索（GET）リクエストを実行 ](#lookup) し、新しいフィールドグループを直接表示できます。

## フィールドグループの更新 {#put}

フィールドグループ全体を置き換えるには、PUTの操作を行います。つまり、リソースを書き換えます。 PUTリクエストを通じてフィールドグループを更新する場合、本文には、POSTリクエストで [ 新しいフィールドグループの作成 ](#create) を行う際に必要なすべてのフィールドを含める必要があります。

>[!NOTE]
>
>フィールドグループを完全に置き換えるのではなく、一部のみを更新する場合は、[ フィールドグループの一部の更新 ](#patch) の節を参照してください。

**API 形式**

```http
PUT /tenant/fieldgroups/{FIELD_GROUP_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{FIELD_GROUP_ID}` | 書き直すフィールドグループの `meta:altId` または URL エンコードされた `$id`。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、既存のフィールドグループを書き直し、新しい `propertyCountry` フィールドを追加します。

```SHELL
curl -X PUT \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/fieldgroups/_{TENANT_ID}.mixins.8779fd45d6e4eb074300023a439862bbba359b60d451627a \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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

応答が成功すると、更新されたフィールドグループの詳細が返されます。

```JSON
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/mixins/8779fd45d6e4eb074300023a439862bbba359b60d451627a",
  "meta:altId": "_{TENANT_ID}.mixins.8779fd45d6e4eb074300023a439862bbba359b60d451627a",
  "meta:resourceType": "mixins",
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
  "imsOrg": "{ORG_ID}",
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

## フィールドグループの一部を更新 {#patch}

フィールドグループの一部を更新するには、PATCHリクエストを使用します。 [!DNL Schema Registry] は、`add`、`remove`、`replace` など、標準の JSON パッチ操作をすべてサポートしています。 JSON パッチについて詳しくは、[API の基本ガイド](../../landing/api-fundamentals.md#json-patch)を参照してください。

>[!NOTE]
>
>個々のフィールドを更新するのではなく、リソース全体を新しい値で置き換える場合は、「[PUT処理を使用したフィールドグループの置き換え ](#put)」の節を参照してください。

**API 形式**

```http
PATCH /tenant/fieldgroups/{FIELD_GROUP_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{FIELD_GROUP_ID}` | 更新するフィールドグループの URL エンコードされた `$id` URI または `meta:altId`。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエスト例では、既存のフィールドグループの `description` を更新し、新しい `propertyCity` フィールドを追加します。

リクエスト本文は配列の形式をとり、リストされた各オブジェクトは、個々のフィールドに対する特定の変更を表します。 各オブジェクトには、実行する操作（`op`）、操作を実行するフィールド（`path`）、その操作に含める情報（`value`）が含まれます。

```SHELL
curl -X PATCH \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/fieldgroups/_{TENANT_ID}.mixins.8779fd45d6e4eb074300023a439862bbba359b60d451627a \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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

応答には、両方の操作が正常に実行されたことが示されます。`description` が更新され、`definitions` に `propertyCountry` が追加されました。

```JSON
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/mixins/8779fd45d6e4eb074300023a439862bbba359b60d451627a",
  "meta:altId": "_{TENANT_ID}.mixins.8779fd45d6e4eb074300023a439862bbba359b60d451627a",
  "meta:resourceType": "mixins",
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
  "imsOrg": "{ORG_ID}",
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

## フィールドグループの削除 {#delete}

場合によっては、スキーマレジストリからフィールドグループを削除する必要があります。 これをおこなうには、パスで指定されたフィールドグループ ID を使用してDELETEリクエストを実行します。

**API 形式**

```http
DELETE /tenant/fieldgroups/{FIELD_GROUP_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{FIELD_GROUP_ID}` | 削除するフィールドグループの URL エンコードされた `$id` URI または `meta:altId`。 |

{style="table-layout:auto"}

**リクエスト**

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/fieldgroups/_{TENANT_ID}.mixins.d5cc04eb8d50190001287e4c869ebe67 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、HTTP ステータス 204（コンテンツなし）が空白の本文とともに返されます。

フィールドグループに対して [ ルックアップ（GET）リクエスト ](#lookup) を実行することで、削除を確認できます。 リクエストには `Accept` ヘッダーを含める必要がありますが、フィールドグループがスキーマレジストリから削除されたので、HTTP ステータス 404 （見つかりません）を受け取ります。
