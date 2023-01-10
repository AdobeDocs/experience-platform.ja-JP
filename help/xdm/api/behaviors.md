---
keywords: Experience Platform；ホーム；人気のトピック；API;XDM;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；スキーマレジストリ；スキーマレジストリ；動作；動作；動作；動作；
solution: Experience Platform
title: ビヘイビアー API エンドポイント
description: スキーマレジストリ API の/behaviors エンドポイントを使用すると、グローバルコンテナ内の使用可能なすべてのビヘイビアーを取得できます。
exl-id: 3b45431f-1d55-4279-8b62-9b27863885ec
source-git-commit: 983682489e2c0e70069dbf495ab90fc9555aae2d
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 28%

---

# 動作エンドポイント

エクスペリエンスデータモデル (XDM) では、ビヘイビアーは、スキーマが記述するデータの特性を定義します。 各 XDM クラスは、特定の動作を参照する必要があります。この動作は、そのクラスを使用するすべてのスキーマが継承します。 Platform のほとんどの使用例に対して、次の 2 つの動作を使用できます。

* **[!UICONTROL レコード]**：主体の属性に関する情報を提供します。主体は、組織または個人にすることができます。
* **[!UICONTROL 時系列]**：レコードの主体によって直接または間接的にアクションが実行された時点のシステムのスナップショットを提供します。

>[!NOTE]
>
>Platform の一部の使用例では、上記のいずれの動作も使用しないスキーマを使用する必要があります。 この場合、3 つ目の「アドホック」動作を使用できます。 に関するチュートリアルを参照してください。 [アドホックスキーマの作成](../tutorials/ad-hoc.md) を参照してください。
>
>スキーマの構成に対するデータ動作の影響について詳しくは、 [スキーマ構成の基本](../schema/composition.md).

この `/behaviors` エンドポイント [!DNL Schema Registry] API を使用すると、 `global` コンテナ。

## はじめに

このガイドで使用するエンドポイントは、[[!DNL Schema Registry]  API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/behavior-registry.yaml) の一部です。先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## 動作のリストの取得 {#list}

使用可能なすべての動作のリストを取得するには、GETリクエストを `/behaviors` endpoint.

**API 形式**

```http
GET /global/behaviors
```

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/global/behaviors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/vnd.adobe.xed-id+json'
```

**応答**

```json
{
    "results": [
        {
            "$id": "https://ns.adobe.com/xdm/data/record",
            "meta:altId": "_xdm.data.record",
            "version": "1.16.4",
            "title": "Record Schema"
        },
        {
            "$id": "https://ns.adobe.com/xdm/data/adhoc",
            "meta:altId": "_xdm.data.adhoc",
            "version": "1.16.4",
            "title": "Ad Hoc Schema"
        },
        {
            "$id": "https://ns.adobe.com/xdm/data/time-series",
            "meta:altId": "_xdm.data.time-series",
            "version": "1.16.4",
            "title": "Time-series Schema"
        }
    ],
    "_page": {
        "orderby": "updated",
        "next": null,
        "count": 3
    },
    "_links": {
        "next": null
    }
}
```

## 動作の検索 {#lookup}

特定の動作を検索するには、 `/behaviors` endpoint.

**API 形式**

```http
GET /global/behaviors/{BEHAVIOR_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{BEHAVIOR_ID}` | この `meta:altId` または URL エンコード済み `$id` 検索する動作の数を指定します。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストでは、レコードの動作の詳細を取得するために、レコードの `meta:altId` リクエストパス内で使用します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/global/behaviors/_xdm.data.record \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/vnd.adobe.xed+json;version=1'
```

**応答**

正常な応答は、動作の詳細（バージョン、説明、動作を使用するクラスに提供される属性を含む）を返します。

```json
{
    "$id": "https://ns.adobe.com/xdm/data/record",
    "meta:altId": "_xdm.data.record",
    "meta:resourceType": "behaviors",
    "version": "1.16.4",
    "title": "Record Schema",
    "type": "object",
    "description": "Used to indicate the behavior of record data semantic when composed into data schemas.",
    "definitions": {
        "record": {
            "properties": {
                "_id": {
                    "title": "Identifier",
                    "type": "string",
                    "format": "uri-reference",
                    "description": "A unique identifier for the record.",
                    "meta:xdmType": "string",
                    "meta:xdmField": "@id"
                }
            }
        }
    },
    "allOf": [
        {
            "$ref": "#/definitions/record",
            "type": "object",
            "meta:xdmType": "object"
        },
        {
            "$ref": "https://ns.adobe.com/xdm/common/extensible#/definitions/@context",
            "type": "object",
            "meta:xdmType": "object"
        }
    ],
    "meta:extensible": true,
    "meta:abstract": true,
    "meta:xdmType": "object",
    "meta:status": "stable",
    "$schema": "http://json-schema.org/draft-06/schema#",
    "meta:registryMetadata": {
        "repo:createdDate": 1606266789446,
        "repo:lastModifiedDate": 1606266789446,
        "eTag": "2cc114a54949a9668fe2ad046ccece59192e1bfa28f14e5ac7c893acb7820ba2",
        "meta:globalLibVersion": "1.16.4"
    }
}
```

## 次の手順

このガイドでは、 API の `/behaviors` エンドポイントの使用について説明しました。[!DNL Schema Registry]API を使用して動作をクラスに割り当てる方法については、 [クラスエンドポイントガイド](./classes.md).
