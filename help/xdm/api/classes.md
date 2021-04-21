---
keywords: Experience Platform；ホーム；人気の高いトピック；API;XDM;XDM;XDMシステム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；クラス；スキーマレジストリ；クラス；クラス；作成
solution: Experience Platform
title: クラスAPIエンドポイント
description: スキーマレジストリAPIの/classesエンドポイントを使用すると、エクスペリエンスアプリケーション内のXDMクラスをプログラムで管理できます。
topic-legacy: developer guide
exl-id: 7beddb37-0bf2-4893-baaf-5b292830f368
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1502'
ht-degree: 23%

---

# クラスエンドポイント

すべてのExperience Data Model(XDM)スキーマは、クラスに基づいている必要があります。 クラスは、そのクラスに基づくすべてのスキーマが含める必要がある共通プロパティの基本構造を決定し、これらのスキーマで使用できるミックスインを決定します。 また、スキーマのクラスは、スキーマに含めるデータの行動面を決定します。このデータには次の2つのタイプがあります。

* **[!UICONTROL レコード]**:件名の属性に関する情報を提供します。主体は、組織または個人にすることができます。
* **[!UICONTROL 時系列]**:レコードの件名によって直接または間接的にアクションが実行された時点のシステムのスナップショットを提供します。

>[!NOTE]
>
>データ動作がスキーマ構成に与える影響について詳しくは、[スキーマ構成の基本](../schema/composition.md)を参照してください。

[!DNL Schema Registry] APIの`/classes`エンドポイントを使用すると、エクスペリエンスアプリケーション内のクラスをプログラムで管理できます。

## はじめに

このガイドで使用されるエンドポイントは、[[!DNL Schema Registry] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/class-registry.yaml)の一部です。 先に進む前に、[はじめに](./getting-started.md)を読んで、関連ドキュメントへのリンク、このドキュメントのサンプルAPI呼び出しを読むためのガイド、Experience PlatformAPIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報を確認してください。

## クラスのリストを取得{#list}

`/global/classes`または`/tenant/classes`に対してGETリクエストを行うことで、`global`コンテナまたは`tenant`リスト下のすべてのクラスをそれぞれできます。

>[!NOTE]
>
>リソースをリストする場合、スキーマレジストリでは結果セットが300項目に制限されます。 この制限を超えるリソースを返すには、ページングパラメーターを使用する必要があります。 また、結果をフィルターし、返されるリソースの数を減らすために、追加のクエリパラメーターを使用することもお勧めします。 詳しくは、付録ドキュメントの[クエリパラメーター](./appendix.md#query)の節を参照してください。

**API 形式**

```http
GET /{CONTAINER_ID}/classes?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | クラスを取得するコンテナ:Adobeが作成したクラスの場合は`global`、組織が所有するクラスの場合は`tenant`です。 |
| `{QUERY_PARAMS}` | 結果をフィルターするオプションのクエリパラメーター。使用可能なパラメーターのリストについては、[付録ドキュメント](./appendix.md#query)を参照してください。 |

**リクエスト**

次のリクエストは、`tenant`コンテナからクラスのリストを取得し、`orderby`クエリパラメーターを使用して`title`属性でクラスを並べ替えます。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/classes?orderby=title \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、リクエストで送信される`Accept`ヘッダーに依存します。 クラスのリストには、次の`Accept`ヘッダーを使用できます。

| `Accept` ヘッダー | 説明 |
| --- | --- |
| `application/vnd.adobe.xed-id+json` | 各リソースの短い概要を返します。 リソースのリストを表示する際に推奨されるヘッダーです。 (制限：300) |
| `application/vnd.adobe.xed+json` | 各リソースの完全なJSONクラスを返し、元の`$ref`と`allOf`が含まれます。 (制限：300) |

**応答**

上記のリクエストでは`application/vnd.adobe.xed-id+json` `Accept`ヘッダーを使用していたので、応答には各クラスの`title`、`$id`、`meta:altId`、`version`属性のみが含まれます。 他の`Accept`ヘッダー(`application/vnd.adobe.xed+json`)を使用すると、各クラスのすべての属性が返されます。 回答で必要な情報に応じて、適切な`Accept`ヘッダーを選択します。

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

## クラス{#lookup}を検索

GETリクエストのパスにクラスのIDを含めると、特定のクラスを検索できます。

**API 形式**

```http
GET /{CONTAINER_ID}/classes/{CLASS_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | 取得するクラスを格納するコンテナ。Adobeが作成したクラスの場合は`global`、組織が所有するクラスの場合は`tenant`です。 |
| `{CLASS_ID}` | 検索するクラスの`meta:altId`またはURLエンコードされた`$id`です。 |

**リクエスト**

次のリクエストは、パスに指定された`meta:altId`値を使用してクラスを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/classes/_{TENANT_ID}.classes.f579a0b5f992c69458ea408ec36571f7da9de15901bab116 \
  -H 'Accept: application/vnd.adobe.xed+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、リクエストで送信される`Accept`ヘッダーに依存します。 すべての参照リクエストで、`version`を`Accept`ヘッダーに含める必要があります。 次の`Accept`ヘッダーを使用できます。

| `Accept` ヘッダー | 説明 |
| ------- | ------------ |
| `application/vnd.adobe.xed+json; version=1` | `$ref` および `allOf` で生、タイトルと説明を含む |
| `application/vnd.adobe.xed-full+json; version=1` | `$ref` および `allOf` を解決、タイトルと説明を含む |
| `application/vnd.adobe.xed-notext+json; version=1` | `$ref` および `allOf` で生、タイトルや説明なし |
| `application/vnd.adobe.xed-full-notext+json; version=1` | `$ref` および `allOf` で解決、タイトルや説明なし |
| `application/vnd.adobe.xed-full-desc+json; version=1` | `$ref` および `allOf` で解決、説明を含む |

**応答**

成功した応答は、クラスの詳細を返します。 返されるフィールドは、リクエストで送信される`Accept`ヘッダーによって異なります。 異なる`Accept`ヘッダーを試して、回答を比較し、使用事例に最適なヘッダーを判断します。

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

POSTリクエストを行うことで、`tenant`コンテナの下にカスタムクラスを定義できます。

>[!IMPORTANT]
>
>独自に定義したカスタムクラスに基づいてスキーマを構成する場合、標準のミックスインは使用できません。 各 mixin は、`meta:intendedToExtend` 属性で互換性のあるクラスを定義します。新しいクラスと互換性のある mixin の定義を開始すると（mixin の `meta:intendedToExtend` フィールドで新しいクラスの `$id` を使用）、定義したクラスを実装するスキーマを定義するたびに、それらの mixin を再利用できます。詳しくは、それぞれのエンドポイントガイドの[ミックスイン](./mixins.md#create)と[スキーマの作成](./schemas.md#create)の節を参照してください。
>
>リアルタイム顧客プロファイルでカスタムクラスに基づくスキーマを使用する場合は、和集合スキーマは同じクラスを共有するスキーマに基づいてのみ構築されることに留意することも重要です。 [!UICONTROL XDM Individualプロファイル]や[!UICONTROL XDM ExperienceEvent]など、別のクラスの和集合にカスタムクラススキーマを含める場合は、そのクラスを使用する別のスキーマとの関係を確立する必要があります。 詳しくは、API](../tutorials/relationship-api.md)の2つのスキーマ間の関係の確立に関するチュートリアル[を参照してください。

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
| `_{TENANT_ID}` | 組織の `TENANT_ID` 名前空間。[!DNL Schema Registry]内の他のリソースとの競合を回避するために、組織で作成するすべてのリソースにこのプロパティを含める必要があります。 |
| `allOf` | 新しいリストによってプロパティが継承されるリソースのクラス。配列内の `$ref` オブジェクトの 1 つが、クラスの動作を定義します。この例では、クラスは「レコード」動作を継承します。 |

**応答**

正常な応答は、HTTP ステータス 201（作成済み）と、新しく作成されたクラスの詳細（`$id`、`meta:altId`、`version`）を含むペイロードを返します。これらの3つの値は読み取り専用で、[!DNL Schema Registry]によって割り当てられます。

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

[リストに対してGETリクエストを実行すると、`tenant`コンテナ内のすべてのクラス](#list)にPropertyクラスが含まれるようになりました。 また、URLエンコードされた`$id`を使用して、[ルックアップ(GET)リクエスト](#lookup)を実行し、新しいクラスを直接表示することもできます。

## クラス{#put}を更新

PUT操作を使用してクラス全体を置き換え、基本的にリソースを書き直すことができます。 PUT要求を通じてクラスを更新する場合、本文には、POST要求で[新しいクラス](#create)を作成する際に必要となるすべてのフィールドを含める必要があります。

>[!NOTE]
>
>クラス全体を置き換えるのではなく、クラスの一部だけを更新したい場合は、[クラス](#patch)の一部を更新するのを参照してください。

**API 形式**

```http
PUT /tenant/classes/{CLASS_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CLASS_ID}` | 再書き込みするクラスの`meta:altId`またはURLエンコードされた`$id`。 |

**リクエスト**

次のリクエストは、既存のクラスを書き直し、そのフィールドの`description`と`title`を変更します。

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

## クラス{#patch}の一部を更新

PATCHリクエストを使用して、クラスの一部を更新できます。 [!DNL Schema Registry]は、`add`、`remove`、`replace`を含む、すべての標準的なJSONパッチ操作をサポートしています。 JSONパッチについて詳しくは、[APIの基本的なガイド](../../landing/api-fundamentals.md#json-patch)を参照してください。

>[!NOTE]
>
>個々のフィールドを更新する代わりに、リソース全体を新しい値に置き換える場合は、[PUT演算](#put)を使用したクラスの置き換えの節を参照してください。

**API 形式**

```http
PATCH /tenant/class/{CLASS_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{CLASS_ID}` | 更新するクラスのURLエンコードされた`$id` URIまたは`meta:altId`。 |

**リクエスト**

以下のリクエスト例では、既存のクラスの`description`と、そのフィールドの1つの`title`を更新します。

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

応答には、両方の操作が正常に実行されたことが示されます。`description`が`propertyId`フィールドの`title`と共に更新されました。

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

## クラス{#delete}を削除

場合によっては、スキーマレジストリからクラスを削除する必要があります。 これは、パスで指定されたクラスIDを使用してDELETEリクエストを実行することで行われます。

**API 形式**

```http
DELETE /tenant/classes/{CLASS_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CLASS_ID}` | 削除するクラスのURLエンコードされた`$id` URIまたは`meta:altId`です。 |

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

クラスの[ルックアップ(GET)リクエスト](#lookup)を試みると、削除を確認できます。 リクエストに`Accept`ヘッダーを含める必要がありますが、HTTPステータス404 （見つかりません）を受け取る必要があります。これは、クラスがスキーマレジストリから削除されたためです。
