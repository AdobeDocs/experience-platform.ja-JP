---
keywords: Experience Platform;home;popular topics;api;API;XDM;XDM system;;experience data model;Experience data model;Experience Data Model;data model;Data Model;class registry;Schema Registry;class;Class;classes;Classes;create
solution: Experience Platform
title: クラスの作成
description: スキーマレジストリAPIの/classesエンドポイントを使用すると、エクスペリエンスアプリケーション内のXDMクラスをプログラムで管理できます。
topic: developer guide
translation-type: tm+mt
source-git-commit: b79482635d87efd5b79cf4df781fc0a3a6eb1b56
workflow-type: tm+mt
source-wordcount: '1470'
ht-degree: 24%

---


# クラスエンドポイント

すべてのExperience Data Model(XDM)スキーマは、クラスに基づいている必要があります。 クラスは、そのクラスに基づくすべてのスキーマが含める必要がある共通プロパティの基本構造を決定し、これらのスキーマで使用できるミックスインを決定します。 また、スキーマのクラスは、スキーマに含めるデータの行動面を決定します。このデータには次の2つのタイプがあります。

* **[!UICONTROL レコード]**:件名の属性に関する情報を提供します。 主体は、組織または個人にすることができます。
* **[!UICONTROL 時系列]**:レコードの件名によって直接または間接的にアクションが実行された時点のシステムのスナップショットを提供します。

>[!NOTE]
>
>データ動作がスキーマ構成に与える影響について詳しくは、スキーマ構成の [基本を参照してください](../schema/composition.md)。

APIの `/classes`[!DNL Schema Registry] エンドポイントを使用すると、エクスペリエンスアプリケーション内のクラスをプログラムで管理できます。

## はじめに

このガイドで使用されるエンドポイントは、 [[!DNL Schema Registry] APIの一部です](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/class-registry.yaml)。 先に進む前に、 [はじめに](./getting-started.md) 、関連ドキュメントへのリンク、このドキュメントのサンプルAPI呼び出しを読むためのガイド、Experience PlatformAPIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報を確認してください。

## Retrieve a list of classes {#list}

また、またはの `global` コンテナ下のすべてのクラスをリストするには、それぞれに対してGETリクエストを作成す `tenant` るか、またはに対してリクエストを作成 `/global/classes``/tenant/classes`します。

>[!NOTE]
>
>リソースをリストする場合、スキーマレジストリでは結果セットが300項目に制限されます。 この制限を超えるリソースを返すには、ページングパラメーターを使用する必要があります。 また、結果をフィルターし、返されるリソースの数を減らすために、追加のクエリパラメーターを使用することもお勧めします。 詳しくは、付録ドキュメントの [クエリパラメータ](./appendix.md#query) の節を参照してください。

**API 形式**

```http
GET /{CONTAINER_ID}/classes?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | クラスを取得するコンテナ: `global` adobeが作成したクラス、または組織が所有するクラス `tenant` 用。 |
| `{QUERY_PARAMS}` | 結果をフィルターするオプションのクエリパラメーター。 使用可能なパラメーターのリストについては、 [付録ドキュメント](./appendix.md#query) を参照してください。 |

**リクエスト**

次のリクエストは、 `tenant` コンテナからクラスのリストを取得し、 `orderby``title` クエリパラメーターを使用して、クラスを属性で並べ替えます。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/classes?orderby=title \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

The response format depends on the `Accept` header sent in the request. The following `Accept` headers are available for listing classes:

| `Accept` ヘッダー | 説明 |
| --- | --- |
| `application/vnd.adobe.xed-id+json` | 各リソースの短い概要を返します。 リソースのリストを表示する際に推奨されるヘッダーです。 (制限：300) |
| `application/vnd.adobe.xed+json` | Returns full JSON class for each resource, with original `$ref` and `allOf` included. (制限：300) |

**応答** 

The request above used the `application/vnd.adobe.xed-id+json` `Accept` header, therefore the response includes only the `title`, `$id`, `meta:altId`, and `version` attributes for each class. もう一方の `Accept` ヘッダー(`application/vnd.adobe.xed+json`)を使用すると、各クラスのすべての属性が返されます。 Select the appropriate `Accept` header depending on the information you require in your response.

```json
{
  "results": [
    {
      "$id": "https://ns.adobe.com/{TENANT_ID}/classes/01b7b1745e8ac4ed1e8784ec91b6afa7",
      "meta:altId": "_{TENANT_ID}.classes.01b7b1745e8ac4ed1e8784ec91b6afa7",
      "version": "1.0",
      "title": "Hotel"
    },
    {
      "$id": "https://ns.adobe.com/{TENANT_ID}/classes/d43b86253676af50da3f671ecdd26ff9",
      "meta:altId": "_{TENANT_ID}.classes.d43b86253676af50da3f671ecdd26ff9",
      "version": "1.1",
      "title": "Property"
    },
    {
      "$id": "https://ns.adobe.com/{TENANT_ID}/classes/366f015dbfea802455fbc46c3b27f771",
      "meta:altId": "_{TENANT_ID}.classes.366f015dbfea802455fbc46c3b27f771",
      "version": "1.0",
      "title": "Subscription"
    }
  ],
  "_page": {
    "orderby": "title",
    "next": null,
    "count": 3
  },
  "_links": {
    "next": null,
    "global_schemas": {
      "href": "https://platform.adobe.io/data/foundation/schemaregistry/global/classes"
    }
  }
}
```

## クラスの検索 {#lookup}

GETリクエストのパスにクラスのIDを含めると、特定のクラスを検索できます。

**API 形式**

```http
GET /{CONTAINER_ID}/classes/{CLASS_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | 取得するクラスを格納するコンテナ。 `global` adobeが作成したクラス、または組織が所有するクラス `tenant` の場合。 |
| `{CLASS_ID}` | 検索するクラス `meta:altId` のURLエンコード `$id` またはURLエンコードです。 |

**リクエスト**

次のリクエストは、パスに指定された `meta:altId` 値でクラスを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/classes/_{TENANT_ID}.classes.f579a0b5f992c69458ea408ec36571f7da9de15901bab116 \
  -H 'Accept: application/vnd.adobe.xed+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

The response format depends on the `Accept` header sent in the request. All lookup requests require a `version` be included in the `Accept` header. The following `Accept` headers are available:

| `Accept` ヘッダー | 説明 |
| ------- | ------------ |
| `application/vnd.adobe.xed+json; version={MAJOR_VERSION}` | `$ref` および `allOf` で生、タイトルと説明を含む |
| `application/vnd.adobe.xed-full+json; version={MAJOR_VERSION}` | `$ref` および `allOf` を解決、タイトルと説明を含む |
| `application/vnd.adobe.xed-notext+json; version={MAJOR_VERSION}` | `$ref` および `allOf` で生、タイトルや説明なし |
| `application/vnd.adobe.xed-full-notext+json; version={MAJOR_VERSION}` | `$ref` および `allOf` で解決、タイトルや説明なし |
| `application/vnd.adobe.xed-full-desc+json; version={MAJOR_VERSION}` | `$ref` および `allOf` で解決、説明を含む |

**応答** 

成功した応答は、クラスの詳細を返します。 The fields that are returned depend on the `Accept` header sent in the request. Experiment with different `Accept` headers to compare the responses and determine which header is best for your use case.

```json
{
  "$id":"https://ns.adobe.com/{TENANT_ID}/classes/f8bbdc3c49d49eae62d1c17e867230ac3de6b5b63b0615ce",
  "meta:altId":"_{TENANT_ID}.classes.f8bbdc3c49d49eae62d1c17e867230ac3de6b5b63b0615ce",
  "meta:resourceType":"classes",
  "version":"1.1",
  "title":"Hotel",
  "type":"object",
  "description":"Base class for the Hotels schema",
  "definitions":{
    "customFields":{
      "type":"object",
      "properties":{
        "_{TENANT_ID}":{
          "type":"object",
          "properties":{
            "Address":{
              "title":"Address",
              "description":"",
              "isRequired":false,
              "$ref":"https://ns.adobe.com/xdm/common/address",
              "type":"object",
              "meta:xdmType":"object"
            },
            "phoneNumber":{
              "title":"Phone Number",
              "description":"",
              "isRequired":false,
              "$ref":"https://ns.adobe.com/xdm/context/phonenumber",
              "type":"object",
              "meta:xdmType":"object"
            },
            "brand":{
              "title":"Brand",
              "description":"",
              "type":"string",
              "isRequired":false,
              "meta:xdmType":"string"
            },
            "hotelId":{
              "title":"Hotel ID",
              "description":"",
              "type":"string",
              "isRequired":false,
              "meta:xdmType":"string"
            }
          },
          "meta:xdmType":"object"
        }
      },
      "meta:xdmType":"object"
    }
  },
  "allOf":[
    {
      "$ref":"https://ns.adobe.com/xdm/data/record",
      "type":"object",
      "meta:xdmType":"object"
    },
    {
      "$ref":"#/definitions/customFields",
      "type":"object",
      "meta:xdmType":"object"
    }
  ],
  "imsOrg":"{IMS_ORG}",
  "meta:extensible":true,
  "meta:abstract":true,
  "meta:extends":[
    "https://ns.adobe.com/xdm/data/record"
  ],
  "meta:xdmType":"object",
  "meta:registryMetadata":{
    "repo:createdDate":1593643258779,
    "repo:lastModifiedDate":1597246362579,
    "xdm:createdClientId":"{CLIENT_ID}",
    "xdm:lastModifiedClientId":"{CLIENT_ID}",
    "xdm:createdUserId":"{USER_ID}",
    "xdm:lastModifiedUserId":"{USER_ID}",
    "eTag":"502f89ee16b8ab2e6b4ea09ecf0ab1e5614907db755051c1f3c65a273001d725",
    "meta:globalLibVersion":"1.15.4"
  },
  "meta:containerId":"tenant",
  "meta:tenantNamespace":"_{TENANT_ID}"
}
```

## クラスの作成 {#create}

POSTリクエストを行うことで、 `tenant` コンテナの下にカスタムクラスを定義できます。

>[!IMPORTANT]
>
>独自に定義したカスタムクラスに基づいてスキーマを構成する場合、標準のミックスインは使用できません。 各 mixin は、`meta:intendedToExtend` 属性で互換性のあるクラスを定義します。新しいクラスと互換性のある mixin の定義を開始すると（mixin の `meta:intendedToExtend` フィールドで新しいクラスの `$id` を使用）、定義したクラスを実装するスキーマを定義するたびに、それらの mixin を再利用できます。詳しくは、それぞれのエンドポイントガイドでのミックスイン [の](./mixins.md#create) 作成とスキーマの [作成に関する節を参照してください](./schemas.md#create) 。
>
>リアルタイム顧客プロファイルでカスタムクラスに基づくスキーマを使用する場合は、和集合スキーマは同じクラスを共有するスキーマに基づいてのみ構築されることに留意することも重要です。 XDM Individual Everience [!UICONTROL 、] XDM ExperienceEventなどの別のクラスの和集合にカスタムクラススキーマを含める場合は、そのクラスを使用する別のスキーマとの関係を確立する必要があります 。 See the tutorial on [establishing a relationship between two schemas in the API](../tutorials/relationship-api.md) for more information.

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

Performing a GET request to [list all classes](#list) in the `tenant` container would now include the Property class. You can also [perform a lookup (GET) request](#lookup) using the URL-encoded `$id` to view the new class directly.

## クラスの更新 {#put}

PUT操作を使用してクラス全体を置き換え、基本的にリソースを書き直すことができます。 PUT要求を通じてクラスを更新する場合、本文には、POST要求で新しいクラスを [作成する際に必要となるすべてのフィールドを含める必要があります](#create) 。

>[!NOTE]
>
>If you only want to update part of a class instead of replacing it entirely, see the section on [updating a portion of a class](#patch).

**API 形式**

```http
PUT /tenant/classes/{CLASS_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CLASS_ID}` | 再書き込みす `meta:altId` るクラス `$id` のURLエンコードまたはURL。 |

**リクエスト**

次のリクエストは、既存のクラスを書き直し、そのクラスの `description` とフィールドの1つ `title` を変更します。

```SHELL
curl -X PUT \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/classes/_{TENANT_ID}.classes.19e1d8b5098a7a76e2c10a81cbc99590 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "title": "Property",
        "description": "Base class for properties operated by a company.",
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
                        "title": "Property ID",
                        "type": "string",
                        "description": "Unique Property ID string."
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

**応答** 

正常に応答すると、更新されたクラスの詳細が返されます。

```JSON
{
  "title": "Property",
  "description": "Base class for properties operated by a company.",
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
                  "title": "Property ID",
                  "type": "string",
                  "description": "Unique Property ID string",
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

## Update a portion of a class {#patch}

PATCHリクエストを使用して、クラスの一部を更新できます。 の標準的なJSONパッチ操作(、、な [!DNL Schema Registry] ど)はすべて `add`サポートされ `remove``replace`ます。 JSONパッチについて詳しくは、 [APIの基本ガイドを参照してください](../../landing/api-fundamentals.md#json-patch)。

>[!NOTE]
>
>If you want to replace an entire resource with new values instead of updating individual fields, see the section on [replacing a class using a PUT operation](#put).

**API 形式**

```http
PATCH /tenant/class/{CLASS_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{CLASS_ID}` | The URL-encoded `$id` URI or `meta:altId` of the class you want to update. |

**リクエスト**

次の例のリクエストでは、既存のクラス `description` と、そのいずれかのフィールド `title` を更新します。

リクエスト本体は配列の形をとり、リストに表示された各オブジェクトは個々のフィールドに対する特定の変更を表します。 各オブジェクトは、実行する操作(`op`)、操作を実行するフィールド(`path`)、およびその操作に含める情報(`value`)を含む。

```SHELL
curl -X PATCH \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/classes/_{TENANT_ID}.classes.19e1d8b5098a7a76e2c10a81cbc99590 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'content-type: application/json' \
  -d '[
        { "op": "replace", "path": "/description", "value":  "Base class for properties operated by a company."},
        { "op": "replace", "path": "/definitions/property/properties/_{TENANT_ID}/properties/property/properties/propertyId/title", "value": "Unique Property ID string" }
      ]'
```

**応答**

応答には、両方の操作が正常に実行されたことが示されます。フィールド `description` の内容と共に、が更新 `title` され `propertyId` ました。

```JSON
{
  "title": "Property",
  "description": "Base class for properties operated by a company.",
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
                  "title": "Property ID",
                  "type": "string",
                  "description": "Unique Property ID string",
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

## クラスの削除 {#delete}

場合によっては、スキーマレジストリからクラスを削除する必要があります。 これは、パスで指定されたクラスIDを使用してDELETEリクエストを実行することで行われます。

**API 形式**

```http
DELETE /tenant/classes/{CLASS_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CLASS_ID}` | The URL-encoded `$id` URI or `meta:altId` of the class you want to delete. |

**リクエスト**

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/classes/_{TENANT_ID}.classes.d5cc04eb8d50190001287e4c869ebe67 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、空白の本文とともに HTTP ステータス 204（コンテンツなし）を返します。

You can confirm the deletion by attempting a [lookup (GET) request](#lookup) for the class. You will need to include an `Accept` header in the request, but should receive an HTTP status 404 (Not Found) because the class has been removed from the Schema Registry.