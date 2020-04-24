---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: アドホックスキーマ
topic: tutorials
translation-type: tm+mt
source-git-commit: 956d1e5b4a994c9ea52d818f3dd6d3ff88cb16b6

---


# アドホックスキーマ

特定の状況では、単一のデータセットでのみ使用するために名前が付けられたフィールドを持つExperience Data Model(XDM)スキーマを作成する必要がある場合があります。 これは「アドホック」スキーマと呼ばれます。 アドホックスキーマは、CSVファイルの取り込みや特定の種類のソース接続の作成など、Experience Platformの様々なデータ取り込みワークフローで使用されます。

このドキュメントでは、 [スキーマレジストリAPIを使用してアドホックスキーマを作成する一般的な手順を示します](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml)。 ワークフローの一部としてアドホックスキーマを作成する必要がある他のエクスペリエンスプラットフォームチュートリアルと組み合わせて使用することを目的としています。 これらの各ドキュメントには、特定の使用事例に合わせてアドホックスキーマを適切に設定する方法に関する詳細が記載されています。

## はじめに

このチュートリアルでは、Experience Data Model(XDM)システムに関する十分な理解が必要です。 このチュートリアルを開始する前に、次のXDMドキュメントを確認してください。

- [XDMシステム概要](../home.md):XDMとExperience Platformでの実装の概要を示します。
- [スキーマ構成の基本](../schema/composition.md):XDMコンポーネントの基本的なコンポーネントの概要をスキーマします。

このチュートリアルを開始する前に、開発者 [ガイドを参照し](../api/getting-started.md) 、スキーマレジストリAPIを正しく呼び出すために知っておく必要がある重要な情報を確認してください。 これには、ユーザ `{TENANT_ID}`ー、「コンテナ」の概念、リクエストを行うために必要なヘッダー（Acceptヘッダーとその可能な値に特に注意）が含まれます。

## アドホッククラスの作成

XDMデータの動作は、基になるスキーマによって決まります。 アドホックスキーマを作成する最初の手順は、動作に基づいてクラスを作成するこ `adhoc` とです。 これは、エンドポイントにPOSTリクエストを行うことで行わ `/tenant/classes` れます。

**API形式**

```http
POST /tenant/classes
```

**リクエスト**

次のリクエストは、ペイロードで指定された属性で設定された新しいXDMクラスを作成します。 配列内でに設定さ `$ref` れたプロパティを `https://ns.adobe.com/xdm/data/adhoc` 指定するこ `allOf` とで、このクラスは動作を継承 `adhoc` します。 リクエストは、クラスのカス `_adhoc` タムフィールドを含むオブジェクトも定義します。

>[!NOTE] の下に定義されるカスタムフ `_adhoc` ィールドは、アドホックスキーマの使用例によって異なります。 使用事例に基づく必須のカスタムフィールドについては、該当するチュートリアルの特定のワークフローを参照してください。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/classes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `$ref` | 新しいクラスのデータ動作。 アドホッククラスの場合、この値をに設定する必要がありま `https://ns.adobe.com/xdm/data/adhoc`す。 |
| `properties._adhoc` | クラスのカスタムフィールドを含むオブジェクトで、フィールド名とデータ型のキーと値のペアとして表現されます。 |

**応答**

成功した応答は、新しいクラスの詳細を返し、オブジェクトの名前を、システム生成の読み取り専用のクラスの一意の識別子である `properties._adhoc` GUIDに置き換えます。 属性も `meta:datasetNamespace` 自動的に生成され、応答に含まれます。

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
    "imsOrg": "{IMS_ORG}",
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
| `$id` | 読み取り専用の、新しいアドホッククラスに対してシステムによって生成された一意の識別子として機能するURI。 この値は、アドホックスキーマの次の作成手順で使用されます。 |

## アドホックスキーマ

アドホッククラスを作成したら、エンドポイントにPOSTリクエストを作成することで、そのクラスを実装する新しいスキーマを作成で `/tenant/schemas` きます。

**API形式**

```http
POST /tenant/schemas
```

**リクエスト**

次のリクエストは、新しいスキーマを作成し、前に作成した`$ref`ad hocクラスの `$id` 参照()をペイロードに提供します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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

正常に完了すると、新たに作成されたスキーマの詳細（システム生成の読み取り専用を含む）が返されま `$id`す。

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
    "imsOrg": "{IMS_ORG}",
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

## 表示 — アドホックスキーマ

>[!NOTE] この手順はオプションです。 アドホックスキーマのフィールド構造を調べたくない場合は、このチュートリアルの最後にある [次の手順](#next-steps) の節に進んでください。

アドホックスキーマが作成されたら、拡張されたフォームでスキーマを表示するための参照(GET)リクエストを作成できます。 これは、以下に示すように、GETリクエストの適切なAcceptヘッダを使用して行います。

**API形式**

```http
GET /tenant/schemas/{SCHEMA_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{SCHEMA_ID}` | アクセスするア `$id` ドホッ `meta:altId` クスキーマのURLエンコードURI。 |

**リクエスト**

次の要求では、Acceptヘッダーを使用し `application/vnd.adobe.xed-full+json; version=1`て、拡張されたフォームのスキーマを返します。 スキーマレジストリから特定のリソースを取得する場合、要求のAcceptヘッダーには、該当するリソースのメジャーバージョンが含まれている必要があります。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.26f6833e55db1dd8308aa07a64f2042d \
  -H 'Accept: application/vnd.adobe.xed-full+json; version=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

成功した応答は、下にネストされたすべてのフィールドを含むスキーマの詳細を返しま `properties`す。

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
    "imsOrg": "{IMS_ORG}",
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

このチュートリアルに従うと、新しいアドホックスキーマが 別のチュートリアルでこのドキュメントに移動した場合は、アドホックスキーマのを使用して、指示に従っ `$id` てワークフローを完了することができます。

スキーマレジストリAPIの使用に関する詳細は、開発者ガイドを参照して [ください](../api/getting-started.md)。