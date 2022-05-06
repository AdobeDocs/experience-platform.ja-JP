---
keywords: Experience Platform；ホーム；人気のトピック；API;XDM;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；スキーマレジストリ；スキーマレジストリ；アドホック；アドホック；アドホック；Ad hoc；チュートリアル；作成；スキーマ；スキーマ
solution: Experience Platform
title: アドホックスキーマの作成
description: 特定の状況において、1 つのデータセットでのみ使用するために名前空間が使用されたフィールドを持つ Experience Data Model（XDM）スキーマを作成する必要が出ることがあります。これは「アドホック」スキーマと呼ばれます。アドホックスキーマは、CSV ファイルの取り込みや特定の種類のソース接続の作成など、Experience Platform の様々なデータ取り込みワークフローで使用されます。
topic-legacy: tutorial
type: Tutorial
exl-id: bef01000-909a-4594-8cf4-b9dbe0b358d5
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '828'
ht-degree: 76%

---

# アドホックスキーマの作成

特定の状況では、 [!DNL Experience Data Model] 単一のデータセットでのみ使用するために名前空間が設定されたフィールドを持つ (XDM) スキーマ。 これは「アドホック」スキーマと呼ばれます。アドホックスキーマは、 [!DNL Experience Platform]（CSV ファイルの取り込みや特定の種類のソース接続の作成を含む）

このドキュメントでは、[スキーマレジストリ API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) を使用してアドホックスキーマを作成する一般的な手順を示します。他の [!DNL Experience Platform] ワークフローの一部としてアドホックスキーマを作成する必要があるチュートリアルです。 これらの各ドキュメントには、特定の使用例に合わせてアドホックスキーマを適切に設定する方法に関する詳細が記載されています。

## はじめに

このチュートリアルでは、 [!DNL Experience Data Model] (XDM) システム。 このチュートリアルを開始する前に、次の XDM ドキュメントを確認してください。

- [XDM システムの概要](../home.md):XDM と、 [!DNL Experience Platform].
- [スキーマ構成の基本](../schema/composition.md)：XDM スキーマの基本的なコンポーネントの概要。

このチュートリアルを開始する前に、 [開発者ガイド](../api/getting-started.md) を正しく呼び出すために知っておく必要がある重要な情報については、を参照してください。 [!DNL Schema Registry] API これには、`{TENANT_ID}`、「コンテナ」の概念、リクエストをおこなうために必要なヘッダー（Accept ヘッダーとその可能な値に特に注意）が含まれます。

## アドホッククラスの作成

XDM データの動作は、基になるスキーマによって決まります。アドホックスキーマを作成する最初の手順は、`adhoc` 動作に基づいてクラスを作成することです。これは、`/tenant/classes` エンドポイントに POST リクエストを送信することでおこなわれます。

**API 形式**

```http
POST /tenant/classes
```

**リクエスト**

次のリクエストは、ペイロードで指定された属性で設定された新しい XDM クラスを作成します。`allOf` 配列内で `https://ns.adobe.com/xdm/data/adhoc` に設定された `$ref` プロパティを指定することで、このクラスは `adhoc` 動作を継承します。リクエストは、クラスのカスタムフィールドを含む `_adhoc` オブジェクトも定義します。

>[!NOTE]
>
> `_adhoc` の下に定義されるカスタムフィールドは、アドホックスキーマの使用例によって異なります。使用例に基づく必須のカスタムフィールドについては、該当するチュートリアルの特定のワークフローを参照してください。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/classes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "title":"New ad-hoc class",
        "description": "New ad-hoc class description",
        "type":"object",
        "allOf": [
          {
            "$ref":"https://ns.adobe.com/xdm/data/adhoc"
          },
          {
            "properties": {
              "_adhoc": {
                "type":"object",
                "properties": {
                  "field1": {
                    "type":"string"
                  },
                  "field2": {
                    "type":"string"
                  }
                }
              }
            }
          }
        ]
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `$ref` | 新しいクラスのデータの動作。アドホッククラスの場合、この値を `https://ns.adobe.com/xdm/data/adhoc` に設定する必要があります。 |
| `properties._adhoc` | クラスのカスタムフィールドを含むオブジェクトで、フィールド名とデータ型のキーと値のペアとして表現されます。 |

{style=&quot;table-layout:auto&quot;}

**応答**

成功した応答は、新しいクラスの詳細を返し、`properties._adhoc` オブジェクトの名前を、システムで生成された読み取り専用クラスの一意の ID である GUID に置き換えます。`meta:datasetNamespace` 属性も自動的に生成され、応答に含まれます。

```json
{
    "$id": "https://ns.adobe.com/{TENANT_ID}/classes/6395cbd58812a6d64c4e5344f7b9120f",
    "meta:altId": "_{TENANT_ID}.classes.6395cbd58812a6d64c4e5344f7b9120f",
    "meta:resourceType": "classes",
    "version": "1.0",
    "title": "New Class",
    "description": "New class description",
    "type": "object",
    "allOf": [
        {
            "$ref": "https://ns.adobe.com/xdm/data/adhoc"
        },
        {
            "properties": {
                "_6395cbd58812a6d64c4e5344f7b9120f": {
                    "type": "object",
                    "properties": {
                        "field1": {
                            "type": "string",
                            "meta:xdmType": "string"
                        },
                        "field2": {
                            "type": "string",
                            "meta:xdmType": "string"
                        }
                    },
                    "meta:xdmType": "object"
                }
            },
            "type": "object",
            "meta:xdmType": "object"
        }
    ],
    "meta:abstract": true,
    "meta:extensible": true,
    "meta:extends": [
        "https://ns.adobe.com/xdm/data/adhoc"
    ],
    "meta:containerId": "tenant",
    "meta:datasetNamespace": "_6395cbd58812a6d64c4e5344f7b9120f",
    "imsOrg": "{ORG_ID}",
    "meta:xdmType": "object",
    "meta:registryMetadata": {
        "repo:createdDate": 1557527784822,
        "repo:lastModifiedDate": 1557527784822,
        "xdm:createdClientId": "{CREATED_CLIENT}",
        "xdm:lastModifiedClientId": "{MODIFIED_CLIENT}",
        "eTag": "Jggrlh4PQdZUvDUhQHXKx38iTQo="
    }
}
```

| プロパティ | 説明 |
| --- | --- |
| `$id` | 読み取り専用の、新しいアドホッククラスに対してシステムによって生成された一意の ID として機能する URI。この値は、アドホックスキーマ作成の次の手順で使用されます。 |

{style=&quot;table-layout:auto&quot;}

## アドホックスキーマの作成

アドホッククラスを作成したら、`/tenant/schemas` エンドポイントに対する POST リクエストを作成することで、そのクラスを実装する新しいスキーマを作成できます。

**API 形式**

```http
POST /tenant/schemas
```

**リクエスト**

次のリクエストは、新しいスキーマを作成し、前に作成したアドホッククラスの `$id` に対する参照（`$ref`）をペイロードに提供します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "title":"New Schema",
        "description": "New schema description.",
        "type":"object",
        "allOf": [
          {
            "$ref":"https://ns.adobe.com/{TENANT_ID}/classes/6395cbd58812a6d64c4e5344f7b9120f"
          }
        ]
      }'
```

**応答**

成功応答は、新たに作成されたスキーマの詳細（システム生成の読み取り専用 `$id` を含む）を返します。

```json
{
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/26f6833e55db1dd8308aa07a64f2042d",
    "meta:altId": "_{TENANT_ID}.schemas.26f6833e55db1dd8308aa07a64f2042d",
    "meta:resourceType": "schemas",
    "version": "1.0",
    "title": "New Schema",
    "description": "New schema description.",
    "type": "object",
    "allOf": [
        {
            "$ref": "https://ns.adobe.com/{TENANT_ID}/classes/6395cbd58812a6d64c4e5344f7b9120f"
        }
    ],
    "meta:datasetNamespace": "_6395cbd58812a6d64c4e5344f7b9120f",
    "meta:class": "https://ns.adobe.com/{TENANT_ID}/classes/6395cbd58812a6d64c4e5344f7b9120f",
    "meta:abstract": false,
    "meta:extensible": false,
    "meta:extends": [
        "https://ns.adobe.com/{TENANT_ID}/classes/6395cbd58812a6d64c4e5344f7b9120f",
        "https://ns.adobe.com/xdm/data/adhoc"
    ],
    "meta:containerId": "tenant",
    "imsOrg": "{ORG_ID}",
    "meta:xdmType": "object",
    "meta:registryMetadata": {
        "repo:createdDate": 1557528570542,
        "repo:lastModifiedDate": 1557528570542,
        "xdm:createdClientId": "{CREATED_CLIENT}",
        "xdm:lastModifiedClientId": "{MODIFIED_CLIENT}",
        "eTag": "Jggrlh4PQdZUvDUhQHXKx38iTQo="
    }
}
```

## 完全なアドホックスキーマを表示する

>[!NOTE]
>
>この手順はオプションです。アドホックスキーマのフィールド構造を調べたくない場合は、このチュートリアルの最後にある[次の手順](#next-steps)の節に進んでください。

アドホックスキーマが作成されたら、拡張されたフォームでスキーマを表示するための参照（GET）リクエストを作成できます。これは、以下に示すように、GET リクエストで適切な Accept ヘッダーを使用しておこないます。

**API 形式**

```http
GET /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | アクセスするアドホックスキーマの URL エンコード `$id` URI または `meta:altId`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストでは、Accept ヘッダー`application/vnd.adobe.xed-full+json; version=1`を使用して、拡張形式のスキーマを返します。特定のリソースを [!DNL Schema Registry]の場合、リクエストの Accept ヘッダーには、該当するリソースのメジャーバージョンを含める必要があります。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.26f6833e55db1dd8308aa07a64f2042d \
  -H 'Accept: application/vnd.adobe.xed-full+json; version=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

成功応答は、スキーマの詳細（`properties` の下にネストされたすべてのフィールドを含む）を返します。

```json
{
    "$id": "https://ns.adobe.com/{TENANT_ID}/schemas/26f6833e55db1dd8308aa07a64f2042d",
    "meta:altId": "_{TENANT_ID}.schemas.26f6833e55db1dd8308aa07a64f2042d",
    "meta:resourceType": "schemas",
    "version": "1.0",
    "title": "New Schema",
    "description": "New schema description.",
    "type": "object",
    "meta:datasetNamespace": "_6395cbd58812a6d64c4e5344f7b9120f",
    "meta:class": "https://ns.adobe.com/{TENANT_ID}/classes/6395cbd58812a6d64c4e5344f7b9120f",
    "meta:abstract": false,
    "meta:extensible": false,
    "meta:extends": [
        "https://ns.adobe.com/{TENANT_ID}/classes/6395cbd58812a6d64c4e5344f7b9120f",
        "https://ns.adobe.com/xdm/data/adhoc"
    ],
    "meta:containerId": "tenant",
    "imsOrg": "{ORG_ID}",
    "meta:xdmType": "object",
    "properties": {
        "_6395cbd58812a6d64c4e5344f7b9120f": {
            "type": "object",
            "meta:xdmType": "object",
            "properties": {
                "field1": {
                    "type": "string",
                    "meta:xdmType": "string"
                },
                "field2": {
                    "type": "string",
                    "meta:xdmType": "string"
                }
            }
        }
    },
    "meta:registryMetadata": {
        "repo:createdDate": 1557528570542,
        "repo:lastModifiedDate": 1557528570542,
        "xdm:createdClientId": "{CREATED_CLIENT}",
        "xdm:lastModifiedClientId": "{MODIFIED_CLIENT}",
        "eTag": "bTogM1ON2LO/F7rlcc1iOWmNVy0="
    }
}
```

## 次の手順 {#next-steps}

このチュートリアルに従い、新しいアドホックスキーマを作成しました。別のチュートリアルの一部としてこのドキュメントにたどり着いた場合は、アドホックスキーマの `$id` を使用し、指示に従ってワークフローを完了することができるようになりました。

の操作の詳細については、 [!DNL Schema Registry] API（を参照） [開発者ガイド](../api/getting-started.md).
