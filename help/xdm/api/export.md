---
title: 書き出し API エンドポイント
description: Schema Registry API の/export エンドポイントを使用すると、サンドボックス間で XDM リソースを共有できます。
exl-id: 1dcbfa59-af98-4db5-b6f4-f848e5bf5e81
source-git-commit: fcd44aef026c1049ccdfe5896e6199d32b4d1114
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 12%

---

# 書き出しエンドポイント

[!DNL Schema Library] 内のすべてのリソースは、Adobe Experience Platform内の特定のサンドボックスに含まれています。 場合によっては、サンドボックスと組織の間で Experience Data Model （XDM）リソースを共有する必要があります。 [!DNL Schema Registry] API の `/rpc/export` エンドポイントを使用すると、[!DNL Schema Library] 内の任意のスキーマ、スキーマフィールドグループ、データタイプの書き出しペイロードを生成し、そのペイロードを使用して、[`/rpc/import` エンドポイントを介してターゲットサンドボックスと組織にそのリソース（およびすべての依存リソース）を読み込むことができま ](./import.md)。

## はじめに

`/rpc/export` エンドポイントは、[[!DNL Schema Registry] API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) の一部です。 先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

`/rpc/export` エンドポイントは、[!DNL Schema Registry] でサポートされているリモート プロシージャ コール （RPC）の一部です。 [!DNL Schema Registry] API の他のエンドポイントとは異なり、RPC エンドポイントには `Accept` や `Content-Type` などの追加のヘッダーは必要なく、`CONTAINER_ID` も使用しません。 代わりに、以下の API 呼び出しで示すように、`/rpc` 名前空間を使用する必要があります。

## リソースの書き出しペイロードの生成 {#export}

パス内の既存のスキーマ、フィールドグループまたはデータタイプの場合、[!DNL Schema Library] エンドポイントに対してGETリクエストを行い、パスにリソースの ID を指定することで、書き出しペイロードを `/export` 成できます。

**API 形式**

```http
GET /rpc/export/{RESOURCE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{RESOURCE_ID}` | 書き出す XDM リソースの `meta:altId` または URL エンコードされた `$id`。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、`Restaurant` フィールドグループの書き出しペイロードを取得します。

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

応答が成功すると、オブジェクトの配列が返されます。この配列は、ターゲット XDM リソースとその依存リソースをすべて表します。 この例では、配列内の最初のオブジェクトが、`Restaurant` フィールドグループが採用するテナントで作成された `Property` データタイプであり、2 番目のオブジェクトは `Restaurant` フィールドグループ自体です。 その後、このペイロードを使用して、別のサンドボックスまたは組織に [ リソースを読み込み ](#import) ことができます。

リソースのテナント ID のインスタンスはすべて `<XDM_TENANTID_PLACEHOLDER>` に置き換えられます。 これにより、スキーマレジストリは、後続の読み込み呼び出しでの送信先に応じて、正しいテナント ID をリソースに自動的に適用できます。

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

## リソースの読み込み {#import}

CSV ファイルから書き出しペイロードを生成したら、そのペイロードを `/rpc/import` エンドポイントに送信して、スキーマを生成できます。

エクスポートペイロードからスキーマを生成する方法について詳しくは、[ インポートエンドポイントガイド ](./import.md) を参照してください。
