---
keywords: Experience Platform；ホーム；人気のあるトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；データモデル；スキーマレジストリ；スキーマレジストリ；動作；動作；動作；動作；
solution: Experience Platform
title: ビヘイビアー API エンドポイント
description: スキーマレジストリ API の/behaviors エンドポイントを使用すると、グローバルコンテナ内で使用可能なすべての動作を取得できます。
topic-legacy: developer guide
exl-id: 3b45431f-1d55-4279-8b62-9b27863885ec
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 11%

---

# 動作エンドポイント

エクスペリエンスデータモデル (XDM) では、ビヘイビアーは、スキーマが記述するデータの特性を定義します。 各 XDM クラスは、特定の動作を参照する必要があります。この動作は、そのクラスを使用するすべてのスキーマが継承します。 Platform のほとんどすべての使用例で、次の 2 つの動作を使用できます。

* **[!UICONTROL レコード]**:主体の属性に関する情報を提供します。主体は、組織または個人にすることができます。
* **[!UICONTROL 時系列]**:レコードの主体によって直接または間接的にアクションが実行された時点のシステムのスナップショットを提供します。

>[!NOTE]
>
>Platform の一部の使用例では、上記のいずれの動作も使用しないスキーマを使用する必要があります。 この場合、3 つ目の「アドホック」動作を使用できます。 詳しくは、[ アドホックスキーマ ](../tutorials/ad-hoc.md) の作成に関するチュートリアルを参照してください。
>
>データビヘイビアーがスキーマ構成に与える影響の詳細については、[ スキーマ構成の基本 ](../schema/composition.md) に関するガイドを参照してください。

[!DNL Schema Registry] API の `/behaviors` エンドポイントを使用すると、`global` コンテナで使用可能な動作を表示できます。

## はじめに

このガイドで使用するエンドポイントは、[[!DNL Schema Registry]  API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/behavior-registry.yaml) の一部です。続行する前に、関連ドキュメントへのリンク、このドキュメントの API 呼び出し例の読み方、およびExperience PlatformAPI を正しく呼び出すために必要なヘッダーに関する重要な情報については、[ はじめに ](./getting-started.md) を参照してください。

## 動作のリストの取得 {#list}

`/behaviors` エンドポイントにGETリクエストを送信することで、使用可能なすべての動作のリストを取得できます。

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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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

`/behaviors` エンドポイントへのGETリクエストのパスに ID を指定することで、特定の動作を検索できます。

**API 形式**

```http
GET /global/behaviors/{BEHAVIOR_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{BEHAVIOR_ID}` | 検索する動作の `meta:altId` または URL エンコードされた `$id`。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストでは、リクエストパスに `meta:altId` を指定して、レコードの動作の詳細を取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/global/behaviors/_xdm.data.record \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/vnd.adobe.xed+json;version=1'
```

**応答**

正常な応答は、動作の詳細（バージョン、説明、動作を使用するクラスに提供される属性など）を返します。

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

このガイドでは、 API の `/behaviors` エンドポイントの使用について説明しました。[!DNL Schema Registry]API を使用して動作をクラスに割り当てる方法については、[ クラスエンドポイントガイド ](./classes.md) を参照してください。
