---
keywords: Experience Platform；ホーム；人気のトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；スキーマレジストリ；動作；動作；動作；
solution: Experience Platform
title: 動作 API エンドポイント
description: スキーマレジストリ API の/behaviors エンドポイントを使用すると、グローバルコンテナで使用可能なすべての動作を取得できます。
exl-id: 3b45431f-1d55-4279-8b62-9b27863885ec
source-git-commit: 863889984e5e77770638eb984e129e720b3d4458
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 23%

---

# 動作エンドポイント

エクスペリエンスデータモデル（XDM）では、動作によって、スキーマが記述するデータの特性が定義されます。 各 XDM クラスは、特定の動作を参照する必要があります。この動作は、そのクラスを使用するすべてのスキーマに継承されます。 Platform のほぼすべてのユースケースに対して、次の 2 つの動作を使用できます。

* **[!UICONTROL レコード]**：主体の属性に関する情報を提供します。主体は、組織または個人にすることができます。
* **[!UICONTROL 時系列]**：レコードの主体によって直接または間接的にアクションが実行された時点のシステムのスナップショットを提供します。

>[!NOTE]
>
>Platform には、上記のどちらかの動作を使用しないスキーマの使用を必要とするユースケースがあります。 このような場合は、3 つ目の「アドホック」動作を使用できます。 詳しくは、[ アドホックスキーマの作成 ](../tutorials/ad-hoc.md) に関するチュートリアルを参照してください。
>
>データの動作がスキーマ構成に与える影響について詳しくは、[ スキーマ構成の基本 ](../schema/composition.md) に関するガイドを参照してください。

[!DNL Schema Registry] API の `/behaviors` エンドポイントを使用すると、`global` コンテナで使用可能な動作を表示できます。

## はじめに

このガイドで使用するエンドポイントは、[[!DNL Schema Registry] API](https://developer.adobe.com/experience-platform-apis/references/schema-registry/) の一部です。 先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## 動作のリストの取得 {#list}

`/behaviors` エンドポイントに対してGETリクエストをおこなうと、使用可能なすべてのビヘイビアーのリストを取得できます。

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

## 行動の検索 {#lookup}

`/behaviors` エンドポイントに対するGETリクエストのパスで ID を指定することで、特定の動作を検索できます。

**API 形式**

```http
GET /global/behaviors/{BEHAVIOR_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{BEHAVIOR_ID}` | 検索する動作の `meta:altId` または URL エンコードされた `$id`。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、リクエストパスに `meta:altId` を指定して、レコードの動作の詳細を取得します。

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

応答が成功すると、バージョン、説明、動作が使用するクラスに提供する属性など、動作の詳細が返されます。

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

このガイドでは、[!DNL Schema Registry] API の `/behaviors` エンドポイントの使用について説明しました。 API を使用してクラスにビヘイビアーを割り当てる方法については、[ クラスエンドポイントガイド ](./classes.md) を参照してください。
