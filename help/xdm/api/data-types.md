---
keywords: Experience Platform；ホーム；人気のあるトピック；API;API;XDM;XDMシステム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；データ型レジストリ；スキーマレジストリ；データ型；データ型；データ型；データ型；作成
solution: Experience Platform
title: データタイプAPIエンドポイント
description: スキーマレジストリAPIの/datatypesエンドポイントを使用すると、エクスペリエンスアプリケーション内のXDMデータ型をプログラムで管理できます。
exl-id: 2a58d641-c681-40cf-acc8-7ad842cd6243
source-git-commit: 8133804076b1c0adf2eae5b748e86a35f3186d14
workflow-type: tm+mt
source-wordcount: '1168'
ht-degree: 14%

---

# データタイプエンドポイント

データ型は、基本リテラルフィールドと同じ方法で、クラスまたはスキーマフィールドグループの参照型フィールドとして使用されます。主な違いは、データ型が複数のサブフィールドを定義できる点です。 フィールドグループは、複数フィールド構造を一貫して使用できる点でフィールドグループと似ていますが、データタイプはスキーマ構造の任意の場所に含めるのに対して、フィールドグループはルートレベルでのみ追加できるので、より柔軟です。 [!DNL Schema Registry] APIの`/datatypes`エンドポイントを使用すると、エクスペリエンスアプリケーション内のデータ型をプログラムで管理できます。

## はじめに

このガイドで使用するエンドポイントは、[[!DNL Schema Registry]  API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) の一部です。続行する前に、[はじめにのガイド](./getting-started.md)を参照して、関連ドキュメントへのリンク、このドキュメントのAPI呼び出し例の読み方、およびExperience PlatformAPIを正しく呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## データ型のリストの取得 {#list}

`global`コンテナまたは`tenant`コンテナの下にあるすべてのGET型をリストするには、それぞれ`/global/datatypes`または`/tenant/datatypes`に対してデータリクエストを実行します。

>[!NOTE]
>
>リソースをリストする場合、スキーマレジストリでは結果セットが300項目に制限されます。 この制限を超えるリソースを返すには、ページングパラメーターを使用する必要があります。 また、追加のクエリパラメーターを使用して結果をフィルタリングし、返されるリソースの数を減らすこともお勧めします。 詳しくは、付録ドキュメントの[クエリパラメーター](./appendix.md#query)の節を参照してください。

**API 形式**

```http
GET /{CONTAINER_ID}/datatypes?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | データ型の取得元のコンテナ：`global`(Adobeが作成したデータ型の場合)または`tenant`（組織が所有するデータ型の場合）。 |
| `{QUERY_PARAMS}` | 結果をフィルターするオプションのクエリパラメーター。使用可能なパラメーターのリストについては、付録の[ドキュメント](./appendix.md#query)を参照してください。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストでは、`tenant`コンテナからデータ型のリストを取得し、`orderby`クエリパラメーターを使用して`title`属性でデータ型を並べ替えます。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/datatypes?orderby=title \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、リクエストで送信される`Accept`ヘッダーに応じて異なります。 データタイプのリストには、次の`Accept`ヘッダーを使用できます。

| `Accept` ヘッダー | 説明 |
| --- | --- |
| `application/vnd.adobe.xed-id+json` | 各リソースの短い概要を返します。 これは、リソースをリストする際に推奨されるヘッダーです。 (上限：300) |
| `application/vnd.adobe.xed+json` | 各リソースの完全なJSONデータ型を返します。元の`$ref`と`allOf`が含まれます。 (上限：300) |

{style=&quot;table-layout:auto&quot;}

**応答**

上記のリクエストでは`application/vnd.adobe.xed-id+json` `Accept`ヘッダーが使用されていたので、応答には各データタイプの`title`、`$id`、`meta:altId`および`version`属性のみが含まれています。 他の`Accept`ヘッダー(`application/vnd.adobe.xed+json`)を使用すると、各データ型のすべての属性が返されます。 応答で必要な情報に応じて、適切な`Accept`ヘッダーを選択します。

```json
{
  "results": [
    {
      "$id": "https://ns.adobe.com/{TENANT_ID}/datatypes/78570e371092c032260714dd8bfd6d44",
      "meta:altId": "_{TENANT_ID}.datatypes.78570e371092c032260714dd8bfd6d44",
      "version": "1.0",
      "title": "Loyalty"
    },
    {
      "$id": "https://ns.adobe.com/{TENANT_ID}/datatypes/4b0329b5573cbb7cb757db667d7fdf66",
      "meta:altId": "_{TENANT_ID}.datatypes.4b0329b5573cbb7cb757db667d7fdf66",
      "version": "1.0",
      "title": "Property Details"
    }
  ],
  "_page": {
    "orderby": "title",
    "next": null,
    "count": 2
  },
  "_links": {
    "next": null,
    "global_schemas": {
      "href": "https://platform.adobe.io/data/foundation/schemaregistry/global/datatypes?orderby=title"
    }
  }
}
```

## データタイプの検索 {#lookup}

データ型のIDをデータリクエストのパスに含めることで、特定のGET型を検索できます。

**API 形式**

```http
GET /{CONTAINER_ID}/datatypes/{DATA_TYPE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | 取得するデータ型を格納するコンテナ：`global`(Adobeが作成したデータ型の場合)または`tenant`（組織が所有するデータ型の場合）。 |
| `{DATA_TYPE_ID}` | 検索するデータ型の`meta:altId`またはURLエンコードされた`$id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストでは、パスで指定された`meta:altId`値でデータ型を取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/datatypes/_{TENANT_ID}.datatypes.78570e371092c032260714dd8bfd6d44 \
  -H 'Accept: application/vnd.adobe.xed+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、リクエストで送信される`Accept`ヘッダーに応じて異なります。 すべての検索リクエストでは、`version`を`Accept`ヘッダーに含める必要があります。 次の`Accept`ヘッダーを使用できます。

| `Accept` ヘッダー | 説明 |
| ------- | ------------ |
| `application/vnd.adobe.xed+json; version=1` | `$ref` および `allOf` で生、タイトルと説明を含む |
| `application/vnd.adobe.xed-full+json; version=1` | `$ref` および `allOf` を解決、タイトルと説明を含む |
| `application/vnd.adobe.xed-notext+json; version=1` | `$ref` および `allOf` で生、タイトルや説明なし |
| `application/vnd.adobe.xed-full-notext+json; version=1` | `$ref` および `allOf` で解決、タイトルや説明なし |
| `application/vnd.adobe.xed-full-desc+json; version=1` | `$ref` および `allOf` で解決、説明を含む |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、データ型の詳細を返します。 返されるフィールドは、リクエストで送信される`Accept`ヘッダーに応じて異なります。 様々な`Accept`ヘッダーを試して、応答を比較し、使用事例に最適なヘッダーを判断します。

```json
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/datatypes/78570e371092c032260714dd8bfd6d44",
  "meta:altId": "_{TENANT_ID}.datatypes.78570e371092c032260714dd8bfd6d44",
  "meta:resourceType": "datatypes",
  "version": "1.0",
  "title": "Loyalty",
  "type": "object",
  "description": "Loyalty object containing loyalty-specific fields.",
  "definitions": {
    "customFields": {
      "properties": {
        "loyaltyId": {
          "title": "Loyalty ID",
          "description": "Unique loyalty program member ID. Should be in the format of an email address.",
          "type": "string",
          "meta:xdmType": "string"
        },
        "memberSince": {
          "title": "Member Since",
          "description": "Date person joined loyalty program.",
          "type": "string",
          "format": "date",
          "meta:xdmType": "date"
        },
        "points": {
          "title": "Points",
          "description": "Accumulated loyalty points",
          "type": "integer",
          "meta:xdmType": "int"
        },
        "loyaltyLevel": {
          "title": "Loyalty Level",
          "description": "The current loyalty program level to which the individual member belongs.",
          "type": "string",
          "enum": [
            "platinum",
            "gold",
            "silver",
            "bronze"
          ],
          "meta:enum": {
            "platinum": "Platinum",
            "gold": "Gold",
            "silver": "Silver",
            "bronze": "Bronze"
          },
          "meta:xdmType": "string"
        }
      },
      "type": "object",
      "meta:xdmType": "object"
    }
  },
  "allOf": [
    {
      "$ref": "#/definitions/customFields"
    }
  ],
  "imsOrg": "{IMS_ORG}",
  "meta:extensible": true,
  "meta:abstract": true,
  "meta:xdmType": "object",
  "meta:registryMetadata": {
    "repo:createdDate": 1557529442681,
    "repo:lastModifiedDate": 1557529442681,
    "xdm:createdClientId": "{CLIENT_ID}",
    "xdm:lastModifiedClientId": "{CLIENT_ID}",
    "xdm:lastModifiedUserId": "{USER_ID}",
    "eTag": "50b8008b588e911314f9685240dd4c23a247f37179a6d9ff6ba3877dc11ca504",
    "meta:globalLibVersion": "1.15.4"
  },
  "meta:containerId": "tenant",
  "meta:tenantNamespace": "_{TENANT_ID}"
}
```

## データタイプの作成 {#create}

POSTリクエストを実行することで、`tenant`コンテナの下にカスタムデータ型を定義できます。

**API 形式**

```http
POST /tenant/datatypes
```

**リクエスト**

データ型を定義する際に、`meta:extends` フィールドや `meta:intendedToExtend` フィールドは必要なく、衝突を避けるためにフィールドを入れ子にする必要もありません。

```SHELL
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/datatypes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "title":"Property Construction",
        "description":"Information related to the property construction",
        "type":"object",
        "properties": {
          "yearBuilt": {
            "type":"integer",
            "title": "Year Built",
            "description": "The year the property was constructed."
          },
          "propertyType": {
            "type":"string",
            "title": "Property Type",
            "description": "Type of building or structure in which the property exists.",
            "enum": [
              "freeStanding",
              "mall",
              "shoppingCenter"
            ],
            "meta:enum": {
              "freeStanding": "Free Standing Building",
              "mall": "Mall Space",
              "shoppingCenter": "Shopping Center"
            }
          }
        } 
      }'
```

**応答**

正常な応答は、HTTP ステータス 201（作成済み）と、新しく作成されたデータタイプの詳細（`$id`、`meta:altId`、`version`を含む）を含むペイロードを返します。これらの3つの値は読み取り専用で、[!DNL Schema Registry]によって割り当てられます。

```JSON
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/datatypes/7602bc6e97e5786a31c95d9e6531a1596687433451d97bc1",
  "meta:altId": "_{TENANT_ID}.datatypes.7602bc6e97e5786a31c95d9e6531a1596687433451d97bc1",
  "meta:resourceType": "datatypes",
  "version": "1.0",
  "title": "Property Construction",
  "type": "object",
  "description": "Information related to the property construction",
  "properties": {
    "yearBuilt": {
      "type": "integer",
      "title": "Year Built",
      "description": "The year the property was constructed.",
      "meta:xdmType": "int"
    },
    "propertyType": {
      "type": "string",
      "title": "Property Type",
      "description": "Type of building or structure in which the property exists.",
      "enum": [
        "freeStanding",
        "mall",
        "shoppingCenter"
      ],
      "meta:enum": {
        "freeStanding": "Free Standing Building",
        "mall": "Mall Space",
        "shoppingCenter": "Shopping Center"
      },
      "meta:xdmType": "string"
    }
  },
  "refs": [],
  "imsOrg": "{IMS_ORG}",
  "meta:extensible": true,
  "meta:abstract": true,
  "meta:xdmType": "object",
  "meta:registryMetadata": {
    "repo:createdDate": 1604524729435,
    "repo:lastModifiedDate": 1604524729435,
    "xdm:createdClientId": "{CLIENT_ID}",
    "xdm:lastModifiedClientId": "{CLIENT_ID}",
    "xdm:createdUserId": "{USER_ID}",
    "xdm:lastModifiedUserId": "{USER_ID}",
    "eTag": "1c838764342756868ca1297869f582a38d15f03ed0acfc97fda7532d22e942c7",
    "meta:globalLibVersion": "1.15.4"
  },
  "meta:containerId": "tenant",
  "meta:sandboxId": "ff0f6870-c46d-11e9-8ca3-036939a64204",
  "meta:sandboxType": "production",
  "meta:tenantNamespace": "_{TENANT_ID}"
}
```

テナントコンテナ内の[すべてのGET型](#list)に対してGETリクエストを実行すると、Property Detailsデータ型が含まれます。または、URLエンコードされた`$id` URIを使用して[ルックアップ（データ）リクエスト](#lookup)を実行し、新しいデータ型を直接表示できます。

## データタイプの更新 {#put}

データ型全体を置き換えるには、PUT操作を使用します。つまり、リソースを書き直します。 PUTリクエストを通じてデータ型を更新する場合、本文には、POSTリクエストで[新しいデータ型](#create)を作成する際に必要となるすべてのフィールドを含める必要があります。

>[!NOTE]
>
>データ型全体を置き換えるのではなく、データ型の一部のみを更新する場合は、[データ型](#patch)の一部の更新の節を参照してください。

**API 形式**

```http
PUT /tenant/datatypes/{DATA_TYPE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DATA_TYPE_ID}` | 書き直すデータ型の`meta:altId`またはURLエンコードされた`$id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、既存のデータ型を書き換え、新しい`floorSize`フィールドを追加します。

```SHELL
curl -X PUT \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/datatypes/_{TENANT_ID}.datatypes.7602bc6e97e5786a31c95d9e6531a1596687433451d97bc1 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "title": "Property Construction",
        "description": "Information related to the property construction",
        "type": "object",
        "properties": {
          "yearBuilt": {
            "type":"integer",
            "title": "Year Built",
            "description": "The year the property was constructed."
          },
          "propertyType": {
            "type":"string",
            "title": "Property Type",
            "description": "Type of building or structure in which the property exists.",
            "enum": [
              "freeStanding",
              "mall",
              "shoppingCenter"
            ],
            "meta:enum": {
              "freeStanding": "Free Standing Building",
              "mall": "Mall Space",
              "shoppingCenter": "Shopping Center"
            }
          },
          "floorSize" {
            "type": "integer",
            "title": "Floor Size",
            "description": "The floor size of the property, in square feet."
          }
        } 
      }'
```

**応答**

正常な応答は、更新されたデータ型の詳細を返します。

```JSON
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/datatypes/7602bc6e97e5786a31c95d9e6531a1596687433451d97bc1",
  "meta:altId": "_{TENANT_ID}.datatypes.7602bc6e97e5786a31c95d9e6531a1596687433451d97bc1",
  "meta:resourceType": "datatypes",
  "version": "1.0",
  "title": "Property Construction",
  "type": "object",
  "description": "Information related to the property construction",
  "properties": {
    "yearBuilt": {
      "type": "integer",
      "title": "Year Built",
      "description": "The year the property was constructed.",
      "meta:xdmType": "int"
    },
    "propertyType": {
      "type": "string",
      "title": "Property Type",
      "description": "Type of building or structure in which the property exists.",
      "enum": [
        "freeStanding",
        "mall",
        "shoppingCenter"
      ],
      "meta:enum": {
        "freeStanding": "Free Standing Building",
        "mall": "Mall Space",
        "shoppingCenter": "Shopping Center"
      },
      "meta:xdmType": "string"
    },
    "floorSize" {
      "type":  "integer",
      "title":  "Floor Size",
      "description":  "The floor size of the property, in square feet.",
      "meta:xdmType": "int"
    }
  },
  "refs": [],
  "imsOrg": "{IMS_ORG}",
  "meta:extensible": true,
  "meta:abstract": true,
  "meta:xdmType": "object",
  "meta:registryMetadata": {
    "repo:createdDate": 1604524729435,
    "repo:lastModifiedDate": 1604524729435,
    "xdm:createdClientId": "{CLIENT_ID}",
    "xdm:lastModifiedClientId": "{CLIENT_ID}",
    "xdm:createdUserId": "{USER_ID}",
    "xdm:lastModifiedUserId": "{USER_ID}",
    "eTag": "1c838764342756868ca1297869f582a38d15f03ed0acfc97fda7532d22e942c7",
    "meta:globalLibVersion": "1.15.4"
  },
  "meta:containerId": "tenant",
  "meta:sandboxId": "ff0f6870-c46d-11e9-8ca3-036939a64204",
  "meta:sandboxType": "production",
  "meta:tenantNamespace": "_{TENANT_ID}"
}
```

## データタイプの一部の更新 {#patch}

データタイプの一部を更新するには、PATCHリクエストを使用します。 [!DNL Schema Registry]は、`add`、`remove`、`replace`を含む、すべての標準的なJSONパッチ操作をサポートします。 JSONパッチの詳細については、『[APIの基本ガイド](../../landing/api-fundamentals.md#json-patch)』を参照してください。

>[!NOTE]
>
>個々のフィールドを更新する代わりに、リソース全体を新しい値に置き換える場合は、「PUT」操作](#put)を使用したデータ型の置き換え[の節を参照してください。

**API 形式**

```http
PATCH /tenant/data type/{DATA_TYPE_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{DATA_TYPE_ID}` | 更新するデータ型のURLエンコードされた`$id` URIまたは`meta:altId`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次の例のリクエストでは、既存のデータ型の`description`を更新し、新しい`floorSize`フィールドを追加します。

リクエスト本文は配列の形式をとり、リストされた各オブジェクトは個々のフィールドに対する特定の変更を表します。 各オブジェクトには、実行する操作(`op`)、操作を実行するフィールド(`path`)、およびその操作に含める情報(`value`)が含まれます。

```SHELL
curl -X PATCH \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/datatypes/_{TENANT_ID}.datatypes.8779fd45d6e4eb074300023a439862bbba359b60d451627a \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'content-type: application/json' \
  -d '[
        {
          "op": "replace",
          "path": "/description",
          "value": "Construction-related information for a company-operated property."
        },
        { 
          "op": "add",
          "path": "/properties/floorSize",
          "value": {
            "type": "integer",
            "title": "Floor Size",
            "description": "The floor size of the property, in square feet."
          }
        }
      ]'
```

**応答**

応答には、両方の操作が正常に実行されたことが示されます。`description`が更新され、`floorSize`が`definitions`に追加されました。

```JSON
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/datatypes/8779fd45d6e4eb074300023a439862bbba359b60d451627a",
  "meta:altId": "_{TENANT_ID}.datatypes.8779fd45d6e4eb074300023a439862bbba359b60d451627a",
  "meta:resourceType": "datatypes",
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

## データ型の削除 {#delete}

スキーマレジストリからデータ型を削除する必要が生じる場合があります。 これは、パスで指定されたDELETEタイプIDを使用してデータリクエストを実行することでおこなわれます。

**API 形式**

```http
DELETE /tenant/datatypes/{DATA_TYPE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DATA_TYPE_ID}` | 削除するデータ型のURLエンコードされた`$id` URIまたは`meta:altId`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/datatypes/_{TENANT_ID}.datatypes.d5cc04eb8d50190001287e4c869ebe67 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、空白の本文とともに HTTP ステータス 204（コンテンツなし）を返します。

[ルックアップ(GET)リクエスト](#lookup)をデータ型に対して試行すると、削除を確認できます。 リクエストに`Accept`ヘッダーを含める必要がありますが、データ型がスキーマレジストリから削除されているので、HTTPステータス404（見つかりません）を受け取る必要があります。
