---
keywords: Experience Platform；ホーム；人気の高いトピック；API;XDM;XDM;XDMシステム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；スキーマレジストリ；スキーマレジストリ；動作；動作；動作；動作；
solution: Experience Platform
title: 動作APIエンドポイント
description: スキーマレジストリAPIの/behaviorsエンドポイントを使用すると、グローバルコンテナで使用可能なすべての動作を取得できます。
topic: developer guide
translation-type: tm+mt
source-git-commit: 698639d6c2f7897f0eb4cce2a1f265a0f7bb57c9
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 5%

---


# 動作エンドポイント

エクスペリエンスデータモデル(XDM)では、スキーマが説明するデータの特性が動作によって定義されます。 各XDMクラスは、特定の動作を参照する必要があります。この動作は、そのクラスを使用するすべてのスキーマが継承します。 プラットフォームのほとんどすべての使用例で、次の2つの動作が利用できます。

* **[!UICONTROL レコード]**:件名の属性に関する情報を提供します。主体は、組織または個人にすることができます。
* **[!UICONTROL 時系列]**:レコードの件名によって直接または間接的にアクションが実行された時点のシステムのスナップショットを提供します。

>[!NOTE]
>
>プラットフォームでは、上記のいずれの動作も使用しないスキーマの使用が必要な使用例があります。 このような場合は、3つ目の「アドホック」動作を使用できます。 詳しくは、[アドホックスキーマの作成](../tutorials/ad-hoc.md)のチュートリアルを参照してください。
>
>データ動作がスキーマ構成に与える影響について詳しくは、[スキーマ構成の基本](../schema/composition.md)のガイドを参照してください。

[!DNL Schema Registry] APIの`/behaviors`エンドポイントを使用すると、`global`コンテナで使用可能な動作を表示できます。

## はじめに

このガイドで使用されるエンドポイントは、[[!DNL Schema Registry] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/behavior-registry.yaml)の一部です。 先に進む前に、[はじめに](./getting-started.md)を読んで、関連ドキュメントへのリンク、このドキュメントのサンプルAPI呼び出しを読むためのガイド、Experience PlatformAPIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報を確認してください。

## 動作のリスト{#list}を取得する

`/behaviors`エンドポイントにGETリクエストを行うと、使用可能なすべての動作のリストを取得できます。

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

## 動作{#lookup}を検索

`/behaviors`エンドポイントへのGETリクエストのパスにIDを指定すると、特定の動作を調べることができます。

**API 形式**

```http
GET /global/behaviors/{BEHAVIOR_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{BEHAVIOR_ID}` | 検索する動作の`meta:altId`またはURLエンコードされた`$id`です。 |

**リクエスト**

次のリクエストは、リクエストパスに`meta:altId`を指定することで、レコード動作の詳細を取得します。

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

正常に完了した応答は、バージョン、説明、およびその動作を使用するクラスに提供する属性など、動作の詳細を返します。

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

このガイドでは、[!DNL Schema Registry] APIでの`/behaviors`エンドポイントの使用について説明しました。 APIを使用してクラスに動作を割り当てる方法については、[クラスエンドポイントガイド](./classes.md)を参照してください。