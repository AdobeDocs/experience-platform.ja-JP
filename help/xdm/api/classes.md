---
keywords: Experience Platform；ホーム；人気のトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；クラスレジストリ；スキーマレジストリ；クラス；クラス；クラス；create
solution: Experience Platform
title: クラス API エンドポイント
description: Schema Registry API の/classes エンドポイントを使用すると、エクスペリエンスアプリケーション内の XDM クラスをプログラムで管理できます。
exl-id: 7beddb37-0bf2-4893-baaf-5b292830f368
source-git-commit: 983682489e2c0e70069dbf495ab90fc9555aae2d
workflow-type: tm+mt
source-wordcount: '1510'
ht-degree: 22%

---

# クラスのエンドポイント

すべてのエクスペリエンスデータモデル（XDM）スキーマは、クラスに基づいている必要があります。 クラスは、そのクラスに基づくすべてのスキーマに含める必要がある共通プロパティの基本構造、およびこれらのスキーマで使用できるスキーマフィールドグループを決定します。 さらに、スキーマのクラスは、スキーマに含まれるデータの行動面を決定します。次の 2 つのタイプがあります。

* **[!UICONTROL レコード]**：主体の属性に関する情報を提供します。主体は、組織または個人にすることができます。
* **[!UICONTROL 時系列]**：レコードの主体によって直接または間接的にアクションが実行された時点のシステムのスナップショットを提供します。

>[!NOTE]
>
>データの動作がスキーマ構成に与える影響について詳しくは、[ スキーマ構成の基本 ](../schema/composition.md) を参照してください。

[!DNL Schema Registry] API の `/classes` エンドポイントを使用すると、エクスペリエンスアプリケーション内のクラスをプログラムで管理できます。

## はじめに

このガイドで使用するエンドポイントは、[[!DNL Schema Registry] API](https://developer.adobe.com/experience-platform-apis/references/schema-registry/) の一部です。 先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## クラスのリストの取得 {#list}

`/global/classes` または `/tenant/classes` に対してそれぞれGETリクエストをおこなうことで、`global` コンテナまたは `tenant` コンテナの下のすべてのクラスをリストできます。

>[!NOTE]
>
>リソースをリストする場合、スキーマレジストリでは結果セットを 300 項目に制限しています。 この制限を超えるリソースを返すには、ページングパラメーターを使用する必要があります。 また、追加のクエリパラメーターを使用して結果をフィルタリングし、返されるリソースの数を減らすこともお勧めします。 詳しくは、付録のドキュメントの [ クエリパラメーター ](./appendix.md#query) に関する節を参照してください。

**API 形式**

```http
GET /{CONTAINER_ID}/classes?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | クラスを取得するコンテナ：Adobeが作成したクラスの場合は `global`、組織が所有するクラスの場合は `tenant`。 |
| `{QUERY_PARAMS}` | 結果をフィルタリングするオプションのクエリパラメーター。 使用可能なパラメーターのリストについては、[ 付録ドキュメント ](./appendix.md#query) を参照してください。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、`orderby` クエリパラメーターを使用して `title` 属性でクラスを並べ替え、`tenant` コンテナからクラスのリストを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/classes?orderby=title \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、リクエストで送信される `Accept` ヘッダーによって異なります。 クラスのリストには、次の `Accept` ヘッダーを使用できます。

| `Accept` ヘッダー | 説明 |
| --- | --- |
| `application/vnd.adobe.xed-id+json` | 各リソースの短い概要を返します。 リソースをリストする場合は、このヘッダーをお勧めします。 （上限：300） |
| `application/vnd.adobe.xed+json` | 各リソースの完全な JSON クラスを、元の `$ref` と `allOf` を含めて返します。 （上限：300） |

{style="table-layout:auto"}

**応答**

上記のリクエストでは `application/vnd.adobe.xed-id+json` `Accept` ヘッダーが使用されているので、応答には各クラスの `title`、`$id`、`meta:altId` および `version` 属性のみが含まれています。 もう一方の `Accept` ヘッダー（`application/vnd.adobe.xed+json`）を使用すると、各クラスのすべての属性が返されます。 応答で必要な情報に応じて、適切な `Accept` ヘッダーを選択します。

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

GETリクエストのパスにクラスの ID を含めることで、特定のクラスを検索できます。

**API 形式**

```http
GET /{CONTAINER_ID}/classes/{CLASS_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | 取得するクラスを格納するコンテナ：Adobeが作成したクラスの場合は `global`、組織が所有するクラスの場合は `tenant`。 |
| `{CLASS_ID}` | 検索するクラスの `meta:altId` または URL エンコードされた `$id`。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、パスで指定された `meta:altId` 値によってクラスを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/classes/_{TENANT_ID}.classes.f579a0b5f992c69458ea408ec36571f7da9de15901bab116 \
  -H 'Accept: application/vnd.adobe.xed+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、リクエストで送信される `Accept` ヘッダーによって異なります。 すべての参照リクエストには、`Accept` ヘッダーに `version` を含める必要があります。 次の `Accept` ヘッダーを使用できます。

| `Accept` ヘッダー | 説明 |
| ------- | ------------ |
| `application/vnd.adobe.xed+json; version=1` | `$ref` および `allOf` で生、タイトルと説明を含む |
| `application/vnd.adobe.xed-full+json; version=1` | `$ref` および `allOf` を解決、タイトルと説明を含む |
| `application/vnd.adobe.xed-notext+json; version=1` | `$ref` および `allOf` で生、タイトルや説明なし |
| `application/vnd.adobe.xed-full-notext+json; version=1` | `$ref` および `allOf` で解決、タイトルや説明なし |
| `application/vnd.adobe.xed-full-desc+json; version=1` | `$ref` および `allOf` で解決、説明を含む |

{style="table-layout:auto"}

**応答**

応答が成功すると、クラスの詳細が返されます。 返されるフィールドは、リクエストで送信される `Accept` ヘッダーによって異なります。 様々な `Accept` ヘッダーを試して、応答を比較し、ユースケースに最適なヘッダーを判断します。

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
  "imsOrg":"{ORG_ID}",
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

POSTリクエストを行うことで、`tenant` コンテナの下にカスタムクラスを定義できます。

>[!IMPORTANT]
>
>定義したカスタムクラスに基づいてスキーマを構成する場合、標準フィールドグループを使用できません。 各フィールドグループは、`meta:intendedToExtend` 属性で互換性のあるクラスを定義します。 新しいクラスに適合するフィールドグループの定義を開始すると（フィールドグループの `meta:intendedToExtend` フィールドで新しいクラスの `$id` を使用すると）、定義したクラスを実装するスキーマを定義するたびに、それらのフィールドグループを再利用できます。 詳しくは、それぞれのエンドポイントガイドの [ フィールドグループの作成 ](./field-groups.md#create) および [ スキーマの作成 ](./schemas.md#create) の節を参照してください。
>
>リアルタイム顧客プロファイルのカスタムクラスに基づくスキーマを使用する場合、結合スキーマは同じクラスを共有するスキーマに基づいてのみ構築されることに注意してください。 [!UICONTROL XDM 個人プロファイル &#x200B;] または [!UICONTROL XDM エクスペリエンスイベント &#x200B;] などの別のクラスの結合にカスタムクラススキーマを含める場合は、そのクラスを採用する別のスキーマとの関係を確立する必要があります。 詳しくは、[API での 2 つのスキーマ間の関係の確立 ](../tutorials/relationship-api.md) に関するチュートリアルを参照してください。

**API 形式**

```http
POST /tenant/classes
```

**リクエスト**

クラスを作成する（POST）リクエストには、`https://ns.adobe.com/xdm/data/record` または `https://ns.adobe.com/xdm/data/time-series` の 2 つの値のいずれかへの `$ref` を含む `allOf` 属性を含める必要があります。これらの値は、クラスの基となる動作（レコードまたは時系列）を表します。レコードデータと時系列データの違いについて詳しくは、「[スキーマ構成の基本](../schema/composition.md)」内の動作タイプに関する節を参照してください。

クラスを定義するときに、クラス定義内にフィールドグループやカスタムフィールドを含めることもできます。 これにより、追加されたフィールドグループとフィールドが、クラスを実装するすべてのスキーマに含まれることになります。 次のリクエスト例では、「Property」と呼ばれるクラスを定義しています。このクラスは、会社が所有および操作する様々なプロパティに関する情報を取得します。クラスが使用されるたびに含める `propertyId` フィールドが含まれます。

```SHELL
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/classes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `_{TENANT_ID}` | 組織の `TENANT_ID` 名前空間。組織で作成されるすべてのリソースは、[!DNL Schema Registry] 内の他のリソースとの競合を避けるために、このプロパティを含める必要があります。 |
| `allOf` | 新しいリストによってプロパティが継承されるリソースのクラス。配列内の `$ref` オブジェクトの 1 つが、クラスの動作を定義します。この例では、クラスは「レコード」動作を継承します。 |

{style="table-layout:auto"}

**応答**

正常な応答は、HTTP ステータス 201（作成済み）と、新しく作成されたクラスの詳細（`$id`、`meta:altId`、`version`）を含むペイロードを返します。これら 3 つの値は読み取り専用で、[!DNL Schema Registry] によって割り当てられます。

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
  "imsOrg": "{ORG_ID}",
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

`tenant` コンテナで [ すべてのクラスをリスト ](#list) するGETリクエストを実行すると、Property クラスが含まれるようになりました。 また、URL エンコードされた `$id` を使用して参照 [GET）リクエストを実行 ](#lookup) し、新しいクラスを直接表示することもできます。

## クラスの更新 {#put}

PUT処理によってクラス全体を置き換えることができ、基本的にリソースを書き換えます。 PUTリクエストを通じてクラスをアップデートする場合、本文には、POSTリクエストで [ 新しいクラスの作成 ](#create) を行う際に必要なすべてのフィールドを含める必要があります。

>[!NOTE]
>
>クラスを完全に置換するのではなく、一部のみを更新する場合は、[ クラスの一部の更新 ](#patch) の節を参照してください。

**API 形式**

```http
PUT /tenant/classes/{CLASS_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CLASS_ID}` | 再書き込みするクラスの `meta:altId` または URL エンコードされた `$id`。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、既存のクラスを書き換えて、クラスの `description` と、フィールドの `title` を変更します。

```SHELL
curl -X PUT \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/classes/_{TENANT_ID}.classes.19e1d8b5098a7a76e2c10a81cbc99590 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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

応答が成功すると、更新されたクラスの詳細が返されます。

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
  "imsOrg": "{ORG_ID}",
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

## クラスの一部を更新 {#patch}

PATCHリクエストを使用して、クラスの一部を更新できます。 [!DNL Schema Registry] は、`add`、`remove`、`replace` など、標準の JSON パッチ操作をすべてサポートしています。 JSON パッチについて詳しくは、[API の基本ガイド](../../landing/api-fundamentals.md#json-patch)を参照してください。

>[!NOTE]
>
>個々のフィールドを更新する代わりに、リソース全体を新しい値で置き換える場合は、[PUT処理を使用したクラスの置き換え ](#put) の節を参照してください。

**API 形式**

```http
PATCH /tenant/class/{CLASS_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{CLASS_ID}` | 更新するクラスの URL エンコードされた `$id` URI または `meta:altId`。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエスト例では、既存のクラスの `description` と、そのクラスのいずれかのフィールドの `title` を更新しています。

リクエスト本文は配列の形式をとり、リストされた各オブジェクトは、個々のフィールドに対する特定の変更を表します。 各オブジェクトには、実行する操作（`op`）、操作を実行するフィールド（`path`）、その操作に含める情報（`value`）が含まれます。

```SHELL
curl -X PATCH \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/classes/_{TENANT_ID}.classes.19e1d8b5098a7a76e2c10a81cbc99590 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'content-type: application/json' \
  -d '[
        { "op": "replace", "path": "/description", "value":  "Base class for properties operated by a company."},
        { "op": "replace", "path": "/definitions/property/properties/_{TENANT_ID}/properties/property/properties/propertyId/title", "value": "Unique Property ID string" }
      ]'
```

**応答**

応答には、両方の操作が正常に実行されたことが示されます。`description` が `propertyId` フィールドの `title` と共に更新されました。

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
  "imsOrg": "{ORG_ID}",
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

場合によっては、スキーマレジストリからクラスを削除する必要があります。 これを行うには、パスで指定されたクラス ID を使用してDELETEリクエストを実行します。

**API 形式**

```http
DELETE /tenant/classes/{CLASS_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CLASS_ID}` | 削除するクラスの URL エンコードされた `$id` URI または `meta:altId`。 |

{style="table-layout:auto"}

**リクエスト**

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/classes/_{TENANT_ID}.classes.d5cc04eb8d50190001287e4c869ebe67 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、HTTP ステータス 204（コンテンツなし）が空白の本文とともに返されます。

削除を確認するには、クラスに対して [ ルックアップ（GET）リクエスト ](#lookup) を試みます。 リクエストには `Accept` ヘッダーを含める必要がありますが、クラスがスキーマレジストリから削除されたので、HTTP ステータス 404 （見つかりません）を受け取ります。
