---
keywords: Experience Platform；ホーム；人気のあるトピック；API;API;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；データモデル；クラスレジストリ；クラス；クラス；作成
solution: Experience Platform
title: クラス API エンドポイント
description: スキーマレジストリ API の/classes エンドポイントを使用すると、エクスペリエンスアプリケーション内の XDM クラスをプログラムで管理できます。
topic-legacy: developer guide
exl-id: 7beddb37-0bf2-4893-baaf-5b292830f368
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '1536'
ht-degree: 19%

---

# クラスエンドポイント

すべてのエクスペリエンスデータモデル (XDM) スキーマは、クラスに基づいている必要があります。 クラスは、そのクラスに基づくすべてのスキーマに含める必要がある共通プロパティの基本構造と、それらのスキーマで使用する資格のあるスキーマフィールドグループを決定します。 さらに、スキーマのクラスは、スキーマに含まれるデータの動作面を決定します。そのうち 2 つのタイプがあります。

* **[!UICONTROL レコード]**:主体の属性に関する情報を提供します。主体は、組織または個人にすることができます。
* **[!UICONTROL 時系列]**:レコードの主体によって直接または間接的にアクションが実行された時点のシステムのスナップショットを提供します。

>[!NOTE]
>
>データビヘイビアーがスキーマ構成に与える影響の詳細については、[ スキーマ構成の基本 ](../schema/composition.md) を参照してください。

[!DNL Schema Registry] API の `/classes` エンドポイントを使用すると、エクスペリエンスアプリケーション内のクラスをプログラムで管理できます。

## はじめに

このガイドで使用するエンドポイントは、[[!DNL Schema Registry]  API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/class-registry.yaml) の一部です。続行する前に、関連ドキュメントへのリンク、このドキュメントの API 呼び出し例の読み方、およびExperience PlatformAPI を正しく呼び出すために必要なヘッダーに関する重要な情報については、[ はじめに ](./getting-started.md) を参照してください。

## クラスのリストの取得 {#list}

`/global/classes` または `/tenant/classes` に対してGETリクエストを実行すると、 `global` コンテナまたは `tenant` コンテナの下にあるすべてのクラスをリストできます。

>[!NOTE]
>
>リソースをリストする場合、スキーマレジストリでは結果セットが 300 項目に制限されます。 この制限を超えるリソースを返すには、ページングパラメーターを使用する必要があります。 また、追加のクエリパラメーターを使用して結果をフィルタリングし、返されるリソースの数を減らすことをお勧めします。 詳しくは、付録のドキュメントの [ クエリパラメータ ](./appendix.md#query) の節を参照してください。

**API 形式**

```http
GET /{CONTAINER_ID}/classes?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | クラスの取得元のコンテナ：`global` はAdobeが作成したクラスの場合、`tenant` は組織が所有するクラスの場合です。 |
| `{QUERY_PARAMS}` | 結果をフィルターするオプションのクエリパラメーター。使用可能なパラメータのリストについては、付録の [ ドキュメント ](./appendix.md#query) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、`tenant` コンテナからクラスのリストを取得し、`orderby` クエリパラメーターを使用して、`title` 属性でクラスを並べ替えます。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/classes?orderby=title \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、リクエストで送信される `Accept` ヘッダーによって異なります。 次の `Accept` ヘッダーを使用して、クラスを一覧表示できます。

| `Accept` ヘッダー | 説明 |
| --- | --- |
| `application/vnd.adobe.xed-id+json` | 各リソースの短い概要を返します。 リソースを一覧表示する際に推奨されるヘッダーです。 ( 制限：300) |
| `application/vnd.adobe.xed+json` | 元の `$ref` と `allOf` を含む、各リソースの完全な JSON クラスを返します。 ( 制限：300) |

{style=&quot;table-layout:auto&quot;}

**応答**

上記のリクエストでは `application/vnd.adobe.xed-id+json` `Accept` ヘッダーが使用されていたので、応答には各クラスの `title`、`$id`、`meta:altId` および `version` 属性のみが含まれています。 他の `Accept` ヘッダー (`application/vnd.adobe.xed+json`) を使用すると、各クラスのすべての属性が返されます。 応答で必要な情報に応じて、適切な `Accept` ヘッダーを選択します。

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

クラスの ID をGETリクエストのパスに含めることで、特定のクラスを検索できます。

**API 形式**

```http
GET /{CONTAINER_ID}/classes/{CLASS_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | 取得するクラスを格納するコンテナ：`global` はAdobeが作成したクラスの場合、または `tenant` は組織が所有するクラスの場合です。 |
| `{CLASS_ID}` | 検索するクラスの `meta:altId` または URL エンコードされた `$id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、パスで指定された `meta:altId` 値でクラスを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/classes/_{TENANT_ID}.classes.f579a0b5f992c69458ea408ec36571f7da9de15901bab116 \
  -H 'Accept: application/vnd.adobe.xed+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、リクエストで送信される `Accept` ヘッダーによって異なります。 すべての検索リクエストでは、`version` を `Accept` ヘッダーに含める必要があります。 次の `Accept` ヘッダーを使用できます。

| `Accept` ヘッダー | 説明 |
| ------- | ------------ |
| `application/vnd.adobe.xed+json; version=1` | `$ref` および `allOf` で生、タイトルと説明を含む |
| `application/vnd.adobe.xed-full+json; version=1` | `$ref` および `allOf` を解決、タイトルと説明を含む |
| `application/vnd.adobe.xed-notext+json; version=1` | `$ref` および `allOf` で生、タイトルや説明なし |
| `application/vnd.adobe.xed-full-notext+json; version=1` | `$ref` および `allOf` で解決、タイトルや説明なし |
| `application/vnd.adobe.xed-full-desc+json; version=1` | `$ref` および `allOf` で解決、説明を含む |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、クラスの詳細を返します。 返されるフィールドは、リクエストで送信される `Accept` ヘッダーによって異なります。 異なる `Accept` ヘッダーを試して、応答を比較し、使用事例に最適なヘッダーを判断します。

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

POSTリクエストを実行することで、`tenant` コンテナの下にカスタムクラスを定義できます。

>[!IMPORTANT]
>
>定義したカスタムクラスに基づいてスキーマを作成する場合、標準のフィールドグループは使用できません。 各フィールドグループは、 `meta:intendedToExtend` 属性で互換性のあるクラスを定義します。 新しいクラスと互換性のあるフィールドグループの定義を開始すると（フィールドグループの `meta:intendedToExtend` フィールドで新しいクラスの `$id` を使用）、定義したクラスを実装するスキーマを定義するたびに、それらのフィールドグループを再利用できます。 詳しくは、それぞれのエンドポイントガイドの [ フィールドグループの作成 ](./field-groups.md#create) と [ スキーマの作成 ](./schemas.md#create) の節を参照してください。
>
>リアルタイム顧客プロファイルでカスタムクラスに基づくスキーマを使用する場合は、和集合スキーマは同じクラスを共有するスキーマに基づいてのみ構築されることに注意する必要があります。 [!UICONTROL XDM Individual Profile] や [!UICONTROL XDM ExperienceEvent] など、別のクラスの和集合にカスタムクラスのスキーマを含める場合は、そのクラスを使用する別のスキーマとの関係を確立する必要があります。 詳しくは、API](../tutorials/relationship-api.md) での 2 つのスキーマ間の関係の確立に関するチュートリアル [ を参照してください。

**API 形式**

```http
POST /tenant/classes
```

**リクエスト**

クラスを作成する（POST）リクエストには、`https://ns.adobe.com/xdm/data/record` または `https://ns.adobe.com/xdm/data/time-series` の 2 つの値のいずれかへの `$ref` を含む `allOf` 属性を含める必要があります。これらの値は、クラスの基となる動作（レコードまたは時系列）を表します。レコードデータと時系列データの違いについて詳しくは、「[スキーマ構成の基本](../schema/composition.md)」内の動作タイプに関する節を参照してください。

クラスを定義する際に、クラス定義にフィールドグループやカスタムフィールドを含めることもできます。 これにより、追加したフィールドグループとフィールドが、クラスを実装するすべてのスキーマに含まれます。 次のリクエスト例では、「Property」と呼ばれるクラスを定義しています。このクラスは、会社が所有および操作する様々なプロパティに関する情報を取得します。クラスが使用されるたびに含める `propertyId` フィールドが含まれます。

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
| `_{TENANT_ID}` | 組織の `TENANT_ID` 名前空間。[!DNL Schema Registry] 内の他のリソースとの競合を避けるために、組織で作成されるすべてのリソースにこのプロパティを含める必要があります。 |
| `allOf` | 新しいリストによってプロパティが継承されるリソースのクラス。配列内の `$ref` オブジェクトの 1 つが、クラスの動作を定義します。この例では、クラスは「レコード」動作を継承します。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、HTTP ステータス 201（作成済み）と、新しく作成されたクラスの詳細（`$id`、`meta:altId`、`version`）を含むペイロードを返します。これらの 3 つの値は読み取り専用で、[!DNL Schema Registry] によって割り当てられます。

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

[ に対してGETリクエストを実行すると、`tenant` コンテナ内のすべてのクラス ](#list) が Property クラスに含まれるようになります。 また、URL エンコードされた `$id` を使用して [ 検索 (GET) リクエスト ](#lookup) を実行し、新しいクラスを直接表示することもできます。

## クラスの更新 {#put}

クラス全体を置き換えるには、PUT操作を使用します。つまり、リソースを書き直します。 PUTリクエストを通じてクラスを更新する場合、本文には、POSTリクエストで新しいクラス ](#create) を作成する際に必要となるすべてのフィールドを含める必要があります。[

>[!NOTE]
>
>クラス全体を置き換えるのではなく、クラスの一部のみを更新する場合は、[ クラス ](#patch) の一部を更新する節を参照してください。

**API 形式**

```http
PUT /tenant/classes/{CLASS_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CLASS_ID}` | 書き直すクラスの `meta:altId` または URL エンコードされた `$id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、既存のクラスを書き換え、その `description` と `title` のいずれかのフィールドを変更します。

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

正常な応答は、更新されたクラスの詳細を返します。

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

## クラスの一部の更新 {#patch}

クラスの一部を更新するには、PATCHリクエストを使用します。 [!DNL Schema Registry] は、`add`、`remove`、`replace` を含む、すべての標準的な JSON パッチ操作をサポートしています。 JSON パッチの詳細については、[API の基本ガイド ](../../landing/api-fundamentals.md#json-patch) を参照してください。

>[!NOTE]
>
>個々のフィールドを更新する代わりに、リソース全体を新しい値に置き換える場合は、[PUT操作 ](#put) を使用したクラスの置き換えの節を参照してください。

**API 形式**

```http
PATCH /tenant/class/{CLASS_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{CLASS_ID}` | 更新するクラスの URL エンコードされた `$id` URI または `meta:altId`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次の例のリクエストでは、既存のクラスの `description` と、そのフィールドの 1 つ `title` が更新されます。

リクエスト本文は配列の形式をとり、リストされた各オブジェクトは個々のフィールドに対する特定の変更を表します。 各オブジェクトは、実行する操作 (`op`)、操作を実行するフィールド (`path`)、およびその操作に含める情報 (`value`) を含む。

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

応答には、両方の操作が正常に実行されたことが示されます。`description` が更新され、`propertyId` フィールドの `title` も更新されました。

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

場合によっては、クラスをスキーマレジストリから削除する必要があります。 これは、パスで指定されたクラス ID を使用してDELETEリクエストを実行することでおこなわれます。

**API 形式**

```http
DELETE /tenant/classes/{CLASS_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CLASS_ID}` | 削除するクラスの URL エンコードされた `$id` URI または `meta:altId`。 |

{style=&quot;table-layout:auto&quot;}

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

クラスの [ ルックアップ (GET) リクエスト ](#lookup) を試みて、削除を確認できます。 リクエストに `Accept` ヘッダーを含める必要がありますが、クラスがスキーマレジストリから削除されたので、HTTP ステータス 404(Not Found) を受け取る必要があります。
