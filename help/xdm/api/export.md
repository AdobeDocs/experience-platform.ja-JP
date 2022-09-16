---
title: 書き出し API エンドポイント
description: スキーマレジストリ API の/export エンドポイントを使用すると、サンドボックス間で XDM リソースを共有できます。
exl-id: 1dcbfa59-af98-4db5-b6f4-f848e5bf5e81
source-git-commit: 32d4a364ba740194d4fd7a0f4df7bd69f25f62b8
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 14%

---

# 書き出しエンドポイント

内のすべてのリソース [!DNL Schema Library] は、Adobe Experience Platform内の特定のサンドボックスに含まれます。 サンドボックスと組織の間で Experience Data Model(XDM) リソースを共有する必要が生じる場合があります。 この `/rpc/export` エンドポイント [!DNL Schema Registry] API を使用すると、 [!DNL Schema Library]を使用し、そのペイロードを使用して、 [`/rpc/import` endpoint](./import.md).

## はじめに

この `/rpc/export` エンドポイントが [[!DNL Schema Registry] API](https://www.adobe.io/experience-platform-apis/references/schema-registry/). 先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

この `/rpc/export` endpoint は、 [!DNL Schema Registry]. の他のエンドポイントとは異なり、 [!DNL Schema Registry] API、RPC エンドポイントは、 `Accept` または `Content-Type`、およびを使用しない `CONTAINER_ID`. 代わりに、 `/rpc` 名前空間と呼ばれます。

## リソースの書き出しペイロードの生成 {#export}

内の既存のスキーマ、フィールドグループまたはデータタイプの場合 [!DNL Schema Library]に値を指定しない場合、 `/export` エンドポイント：パスにリソースの ID を指定します。

**API 形式**

```http
GET /rpc/export/{RESOURCE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{RESOURCE_ID}` | この `meta:altId` または URL エンコード済み `$id` 書き出す XDM リソースの XDM リソース。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、 `Restaurant` フィールドグループを使用します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/rpc/export/_{TENANT_ID}.mixins.922a56b58c6b4e4aeb49e577ec82752106ffe8971b23b4d9 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/vnd.adobe.xdm-link+json'
```

**応答**

正常な応答は、オブジェクトの配列を返します。この配列は、ターゲット XDM リソースとその依存リソースをすべて表します。 この例では、配列の最初のオブジェクトはテナントが作成します `Property` データタイプ `Restaurant` フィールドグループが使用され、2 番目のオブジェクトが `Restaurant` フィールドグループ自体を使用します。 このペイロードは、 [リソースをインポート](#import) を別のサンドボックスまたは IMS 組織に追加する必要があります。

リソースのテナント ID のすべてのインスタンスが、 `<XDM_TENANTID_PLACEHOLDER>`. これにより、スキーマレジストリで、後続の読み込み呼び出しでの送信先に応じて、正しいテナント ID をリソースに自動的に適用できます。

```json
[
    {
        "$id": "https://ns.adobe.com/<XDM_TENANTID_PLACEHOLDER>/datatypes/fc07162ee7ca8d18e074a3bb50c3938c76160bf6040e8495",
        "meta:altId": "_<XDM_TENANTID_PLACEHOLDER>.datatypes.fc07162ee7ca8d18e074a3bb50c3938c76160bf6040e8495",
        "meta:resourceType": "datatypes",
        "version": "1.0",
        "title": "Property",
        "type": "object",
        "description": "",
        "definitions": {
            "customFields": {
                "properties": {
                    "propertyId": {
                        "title": "Property ID",
                        "description": "ID for a company-owned property.",
                        "type": "string",
                        "isRequired": false,
                        "meta:ui": {
                            "ref": [
                                "schema://5fbc29ec292534000055dd55",
                                "#/definitions/customFields"
                            ],
                            "path": "{}._<XDM_TENANTID_PLACEHOLDER>{}.property{}.propertyId",
                            "editable": true,
                            "generateDate": 1606168175975
                        },
                        "meta:xdmType": "string"
                    },
                    "jurisdiction": {
                        "title": "Jurisdiction",
                        "description": "",
                        "type": "string",
                        "isRequired": false,
                        "enum": [
                            "NA",
                            "UK",
                            "EU"
                        ],
                        "meta:enum": {
                            "NA": "North America",
                            "UK": "United Kingdom",
                            "EU": "European Union"
                        },
                        "meta:ui": {
                            "ref": [
                                "schema://5fbc29ec292534000055dd55",
                                "#/definitions/customFields"
                            ],
                            "path": "{}._<XDM_TENANTID_PLACEHOLDER>{}.property{}.jurisdiction",
                            "editable": true,
                            "generateDate": 1606168175975
                        },
                        "meta:xdmType": "string"
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
        "meta:extensible": true,
        "meta:abstract": true,
        "meta:xdmType": "object",
        "meta:sandboxId": "ff0f6870-c46d-11e9-8ca3-036939a64204",
        "meta:sandboxType": "production"
    },
    {
        "$id": "https://ns.adobe.com/<XDM_TENANTID_PLACEHOLDER>/mixins/922a56b58c6b4e4aeb49e577ec82752106ffe8971b23b4d9",
        "meta:altId": "_<XDM_TENANTID_PLACEHOLDER>.mixins.922a56b58c6b4e4aeb49e577ec82752106ffe8971b23b4d9",
        "meta:resourceType": "mixins",
        "version": "1.0",
        "title": "Restaurant",
        "type": "object",
        "description": "",
        "definitions": {
            "customFields": {
                "type": "object",
                "properties": {
                    "_<XDM_TENANTID_PLACEHOLDER>": {
                        "type": "object",
                        "properties": {
                            "capacity": {
                                "title": "Capacity",
                                "description": "Restaurant capacity",
                                "type": "string",
                                "isRequired": false,
                                "meta:xdmType": "string"
                            },
                            "kitchen": {
                                "title": "Kitchen Style",
                                "description": "Style of kitchen",
                                "type": "string",
                                "isRequired": false,
                                "meta:xdmType": "string"
                            },
                            "rating": {
                                "title": "Rating",
                                "description": "",
                                "type": "integer",
                                "isRequired": false,
                                "meta:xdmType": "int"
                            },
                            "property": {
                                "title": "Property",
                                "description": "",
                                "$ref": "https://ns.adobe.com/<XDM_TENANTID_PLACEHOLDER>/datatypes/fc07162ee7ca8d18e074a3bb50c3938c76160bf6040e8495",
                                "type": "object",
                                "meta:xdmType": "object"
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
        "meta:extensible": true,
        "meta:abstract": true,
        "meta:intendedToExtend": [],
        "meta:xdmType": "object",
        "meta:sandboxId": "ff0f6870-c46d-11e9-8ca3-036939a64204",
        "meta:sandboxType": "production"
    }
]
```

## リソースをインポート {#import}

CSV ファイルから書き出しペイロードを生成したら、そのペイロードを `/rpc/import` endpoint ：スキーマを生成します。

詳しくは、 [インポートエンドポイントガイド](./import.md) を参照してください。
