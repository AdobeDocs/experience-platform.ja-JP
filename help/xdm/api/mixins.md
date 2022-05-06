---
keywords: Experience Platform；ホーム；人気のトピック；API;XDM;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；Mixin レジストリ；Mixin;Mixin;Mixin;Mixin；作成
solution: Experience Platform
title: Mixins API エンドポイント
description: スキーマレジストリ API の/mixins エンドポイントを使用すると、エクスペリエンスアプリケーション内で XDM Mixin をプログラムで管理できます。
topic-legacy: developer guide
exl-id: 93ba2fe3-0277-4c06-acf6-f236cd33252e
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '1210'
ht-degree: 18%

---


# Mixin エンドポイント（廃止）

>[!IMPORTANT]
>
>Mixin の名前は、スキーマフィールドグループに変更されたので、 `/mixins` エンドポイントは非推奨 ( 代わりに `/fieldgroups` endpoint.
>
>While `/mixins` は従来のエンドポイントとして引き続き維持されます。を使用することを強くお勧めします。 `/fieldgroups` を参照してください。 詳しくは、 [フィールドグループエンドポイントガイド](./field-groups.md) を参照してください。

Mixin は、個人、郵送先住所、Web ブラウザー環境など、特定の概念を表す 1 つ以上のフィールドを定義する再利用可能なコンポーネントです。 Mixin は、表すデータ（レコードまたは時系列）の動作に応じて、互換性のあるクラスを実装するスキーマの一部として含まれることを意図しています。 この `/mixins` エンドポイント [!DNL Schema Registry] API を使用すると、エクスペリエンスアプリケーション内の Mixin をプログラムで管理できます。

## はじめに

このガイドで使用するエンドポイントは、[[!DNL Schema Registry]  API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) の一部です。先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## Mixin のリストの取得 {#list}

すべての mixin は、 `global` または `tenant` コンテナを作成するには、次に対してGETリクエストを実行します。 `/global/mixins` または `/tenant/mixins`、それぞれ。

>[!NOTE]
>
>リソースをリストする場合、スキーマレジストリでは結果セットを 300 項目に制限します。 この制限を超えるリソースを返すには、ページングパラメーターを使用する必要があります。 また、追加のクエリパラメーターを使用して結果をフィルタリングし、返されるリソースの数を減らすことをお勧めします。 詳しくは、 [クエリパラメーター](./appendix.md#query) 詳しくは、付録のドキュメントを参照してください。

**API 形式**

```http
GET /{CONTAINER_ID}/mixins?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | Mixin を取得するコンテナ： `global` (Adobeで作成された mixin またはの ) `tenant` 組織が所有する mixin の場合。 |
| `{QUERY_PARAMS}` | 結果をフィルターするオプションのクエリパラメーター。詳しくは、 [付録文書](./appendix.md#query) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、 `tenant` コンテナ、使用 `orderby` mixin を `title` 属性。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/mixins?orderby=title \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、 `Accept` ヘッダーがリクエストで送信されました。 以下 `Accept` ヘッダーを mixin のリストに使用できます。

| `Accept` ヘッダー | 説明 |
| --- | --- |
| `application/vnd.adobe.xed-id+json` | 各リソースの短い概要を返します。 リソースを一覧表示する場合は、これが推奨されるヘッダーです。 ( 制限：300) |
| `application/vnd.adobe.xed+json` | 各リソースの完全な JSON mixin を元のと共に返します `$ref` および `allOf` 含まれる ( 制限：300) |

{style=&quot;table-layout:auto&quot;}

**応答**

上記のリクエストでは、 `application/vnd.adobe.xed-id+json` `Accept` したがって、応答には `title`, `$id`, `meta:altId`、および `version` 各 mixin の属性。 他の `Accept` ヘッダー (`application/vnd.adobe.xed+json`) は各 mixin のすべての属性を返します。 適切な `Accept` ヘッダーを作成します。

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
      "href": "https://platform.adobe.io/data/foundation/schemaregistry/global/mixins"
    }
  }
}
```

## Mixin の検索 {#lookup}

特定の mixin を検索するには、mixin の ID をGETリクエストのパスに含めます。

**API 形式**

```http
GET /{CONTAINER_ID}/mixins/{MIXIN_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | 取得する mixin を格納するコンテナ： `global` Adobeが作成した mixin または `tenant` 組織が所有する Mixin の場合。 |
| `{MIXIN_ID}` | この `meta:altId` または URL エンコード済み `$id` 検索する mixin の数を指定します。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストでは、 `meta:altId` の値がパスに指定されました。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/mixins/_{TENANT_ID}.mixins.8779fd45d6e4eb074300023a439862bbba359b60d451627a \
  -H 'Accept: application/vnd.adobe.xed+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、 `Accept` ヘッダーがリクエストで送信されました。 すべての検索リクエストには `version` 含まれる `Accept` ヘッダー。 以下 `Accept` ヘッダーを使用できます。

| `Accept` ヘッダー | 説明 |
| ------- | ------------ |
| `application/vnd.adobe.xed+json; version=1` | `$ref` および `allOf` で生、タイトルと説明を含む |
| `application/vnd.adobe.xed-full+json; version=1` | `$ref` および `allOf` を解決、タイトルと説明を含む |
| `application/vnd.adobe.xed-notext+json; version=1` | `$ref` および `allOf` で生、タイトルや説明なし |
| `application/vnd.adobe.xed-full-notext+json; version=1` | `$ref` および `allOf` で解決、タイトルや説明なし |
| `application/vnd.adobe.xed-full-desc+json; version=1` | `$ref` および `allOf` で解決、説明を含む |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、mixin の詳細を返します。 返されるフィールドは、 `Accept` ヘッダーがリクエストで送信されました。 異なるを使用した実験 `Accept` ヘッダーを使用して、応答を比較し、使用事例に最適なヘッダーを決定します。

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

## Mixin の作成 {#create}

カスタム mixin は、 `tenant` コンテナを含める必要があります。POSTリクエストを実行します。

**API 形式**

```http
POST /tenant/mixins
```

**リクエスト**

新しい mixin を定義する場合は、`meta:intendedToExtend` 属性を含めて、その mixin と互換性があるクラスの `$id` をリストする必要があります。この例では、mixin は `Property` 以前に定義されたクラス。 カスタムフィールドはの下にネストする必要があります `_{TENANT_ID}` （例で示すように）を使用して、クラスや他の mixin によって提供される類似のフィールドとの競合を回避します。

>[!NOTE]
>
>mixin に含める様々なフィールドタイプを定義する方法について詳しくは、 [フィールド制約ガイド](../schema/field-constraints.md#define-fields).

```SHELL
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/mixins \
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

成功した応答は、HTTP ステータス 201 （Created）と、新しく作成された mixin の詳細（`$id`、`meta:altId`、`version` など）を含むペイロードを返します。これらの値は読み取り専用で、 [!DNL Schema Registry].

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

へのGETリクエストの実行 [すべての mixin のリスト](#list) テナントコンテナにプロパティの詳細の mixin が含まれるようになります。または、 [ルックアップ (GET) リクエストの実行](#lookup) URL エンコードされたを使用 `$id` 新しい mixin を直接表示する URI。

## Mixin の更新 {#put}

mixin 全体をPUT操作で置き換え、基本的にリソースを書き直すことができます。 PUTリクエストを通じて mixin を更新する場合、本文には、 [新しい mixin の作成](#create) POST。

>[!NOTE]
>
>完全に置き換えるのではなく、mixin の一部のみを更新する場合は、 [mixin の一部の更新](#patch).

**API 形式**

```http
PUT /tenant/mixins/{MIXIN_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MIXIN_ID}` | この `meta:altId` または URL エンコード済み `$id` の値を指定します。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、新しい `propertyCountry` フィールドに入力します。

```SHELL
curl -X PUT \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/mixins/_{TENANT_ID}.mixins.8779fd45d6e4eb074300023a439862bbba359b60d451627a \
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

正常な応答は、更新された mixin の詳細を返します。

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

## Mixin の一部の更新 {#patch}

mixin の一部を更新するには、PATCHリクエストを使用します。 この [!DNL Schema Registry] は、以下を含むすべての標準的な JSON パッチ操作をサポートしています。 `add`, `remove`、および `replace`. JSON パッチの詳細については、 [API の基本事項ガイド](../../landing/api-fundamentals.md#json-patch).

>[!NOTE]
>
>個々のフィールドを更新する代わりに、リソース全体を新しい値に置き換える場合は、 [PUT操作を使用した mixin の置き換え](#put).

**API 形式**

```http
PATCH /tenant/mixin/{MIXIN_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{MIXIN_ID}` | URL エンコードされた `$id` URI or `meta:altId` 更新する mixin の。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

以下のリクエスト例は、 `description` を追加し、新しい `propertyCity` フィールドに入力します。

リクエスト本文は配列の形式をとり、リストに表示される各オブジェクトは個々のフィールドに対する特定の変更を表します。 各オブジェクトには、実行される操作 (`op`) を呼び出し、操作を実行する必要があるフィールド (`path`) と、その操作に含める必要のある情報 (`value`) をクリックします。

```SHELL
curl -X PATCH \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/mixins/_{TENANT_ID}.mixins.8779fd45d6e4eb074300023a439862bbba359b60d451627a \
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

応答には、両方の操作が正常に実行されたことが示されます。この `description` が更新され、 `propertyCountry` は以下に追加されています： `definitions`.

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

## Mixin の削除 {#delete}

スキーマレジストリから mixin を削除する必要が生じる場合があります。 これは、パスで指定された mixin ID を使用してDELETEリクエストを実行することでおこなわれます。

**API 形式**

```http
DELETE /tenant/mixins/{MIXIN_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{MIXIN_ID}` | URL エンコードされた `$id` URI or `meta:altId` 削除する mixin の数を指定します。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/mixins/_{TENANT_ID}.mixins.d5cc04eb8d50190001287e4c869ebe67 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、HTTP ステータス 204（コンテンツなし）が空白の本文とともに返されます。

削除を確認するには、 [参照 (GET) リクエスト](#lookup) を mixin に追加します。 次を含める必要があります： `Accept` ヘッダーを使用しますが、mixin がスキーマレジストリから削除されているので、HTTP ステータス 404(Not Found) を受け取る必要があります。
