---
title: API での XDM フィールドの非推奨化
description: Schema Registry API でエクスペリエンスデータモデル（XDM）フィールドを非推奨（廃止予定）にする方法を説明します。
exl-id: e49517c4-608d-4e05-8466-75724ca984a8
source-git-commit: f9f783b75bff66d1bf3e9c6d1ed1c543bd248302
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 100%

---

# API での XDM フィールドの非推奨化

エクスペリエンスデータモデル（XDM）では、[Schema Registry API](https://developer.adobe.com/experience-platform-apis/references/schema-registry/) を使用して、スキーマまたはカスタムリソース内のフィールドを非推奨（廃止予定）にすることができます。フィールドを非推奨（廃止予定）にすることで、そのフィールドが[!UICONTROL プロファイル]ワークスペースや Customer Journey Analytics などのダウンストリーム UI で非表示になりますが、それ以外の点では、非破壊的な変更であり、既存のデータフローに悪影響を与えません。

このドキュメントでは、様々な XDM リソースのフィールドを非推奨（廃止予定）にする方法を説明します。 Experience Platform ユーザーインターフェイスのスキーマエディターを使用して XDM フィールドを非推奨（廃止予定）にする手順については、[UI での XDM フィールドの非推奨化](./field-deprecation-ui.md)に関するチュートリアルを参照してください。

## はじめに

このチュートリアルでは、Schema Registry API の呼び出しが必要です。これらの API を呼び出すために知っておくべき重要な情報については、[開発者ガイド](../api/getting-started.md)を参照してください。 そうした情報としては、`{TENANT_ID}`、「コンテナ」の概念、リクエストを行うのに必要なヘッダーなどがあります（ `Accept` ヘッダーとその取り得る値には特に注意を払います）。

## カスタムフィールドの非推奨化 {#custom}

カスタムクラス、フィールドグループまたはデータタイプのフィールドを非推奨（廃止予定）にするには、PUT または PATCH リクエストを使用してカスタムリソースを更新し、当該フィールドに属性 `meta:status: deprecated` を追加します。

>[!NOTE]
>
>XDM のカスタムリソースの更新に関する一般的な情報については、次のドキュメントを参照してください。
>
>* [クラスの更新](../api/classes.md#patch)
>* [フィールドグループの更新](../api/field-groups.md#patch)
>* [データタイプの更新](../api/data-types.md#patch)


以下の API 呼び出し例では、カスタムデータタイプのフィールドを非推奨（廃止予定）にしています。

**API 形式**

```http
PATCH /tenant/datatypes/{DATA_TYPE_ID}
```

**リクエスト**

次のリクエストでは、不動産物件を説明するデータタイプの `expansionArea` フィールドを非推奨（廃止予定）にしています。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/datatypes/_{TENANT_ID}.datatypes.8779fd45d6e4eb074300023a439862bbba359b60d451627a \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'content-type: application/json' \
  -d '[
        { 
          "op": "add",
          "path": "/properties/expansionArea/meta:status",
          "value": "deprecated"
        }
      ]'
```

**応答**

正常な応答は、カスタムリソースの更新の詳細を返します（非推奨フィールドには `meta:status` の値として `deprecated` が含まれています）。 次の応答例は、スペースの関係で一部省略されています。

```json
{
  "$id": "https://ns.adobe.com/{TENANT_ID}/datatypes/8779fd45d6e4eb074300023a439862bbba359b60d451627a",
  "meta:altId": "_{TENANT_ID}.datatypes.8779fd45d6e4eb074300023a439862bbba359b60d451627a",
  "meta:resourceType": "datatypes",
  "version": "1.2",
  "title": "Property Details",
  "type": "object",
  "description": "Details relating to a real-estate property operated by the company.",
  "definitions": {
    "property": {
      "properties": {
        "_{TENANT_ID}": {
        "type":"object",
        "properties": {
            "expansionArea": {
              "title": "Expansion Area",
              "description": "Square footage for renovated additions to the property.",
              "type": "integer",
              "meta:status": "deprecated",
            },
            "propertyName": {
              "title": "Property Name",
              "description": "Name of the property",
              "type": "string"
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
            },
            "squareFeet": {
              "title": "Expansion Area",
              "description": "Square footage for renovated additions to the property.",
              "type": "integer",
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
  "imsOrg": "{IMS_ORG}",
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

## スキーマ内の標準フィールドの非推奨化 {#standard}

標準クラス、フィールドグループおよびデータタイプのフィールドは、直接非推奨（廃止予定）にすることはできません。 代わりに、記述子を使用して、これらの標準リソースを使用する個々のスキーマでの使用を非推奨（廃止予定）にすることができます。

### フィールド非推奨化記述子の作成 {#create-descriptor}

非推奨（廃止予定）にするスキーマフィールドの記述子を作成するには、`/tenant/descriptors` エンドポイントに対して POST リクエストを実行します。

**API 形式**

```http
POST /tenant/descriptors
```

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "@type": "xdm:descriptorDeprecated",
        "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/c65ddf08cf2d4a2fe94bd06113bf4bc4c855e12a936410d5",
        "xdm:sourceVersion": 1,
        "xdm:sourceProperty": "/faxPhone"
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `@type` | 記述子のタイプ。フィールド非推奨化記述子の場合は、この値を `xdm:descriptorDeprecated` に設定する必要があります。 |
| `xdm:sourceSchema` | 記述子の適用先となるスキーマの URI `$id`。 |
| `xdm:sourceVersion` | 記述子の適用先となるスキーマのバージョン。 `1` に設定してください。 |
| `xdm:sourceProperty` | 記述子の適用先となるスキーマ内のプロパティへのパス。 記述子を複数のプロパティに適用する場合は、パスのリストを配列の形式（例：`["/firstName", "/lastName"]`）で指定します。 |

**応答**

```json
{
    "@id": "d882b1202bac0ac71f1e31fbcd9afbcc37f364270186b4b3",
    "@type": "xdm:descriptorDeprecated",
    "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/c65ddf08cf2d4a2fe94bd06113bf4bc4c855e12a936410d5",
    "xdm:sourceVersion": 1,
    "xdm:sourceProperty": "/faxPhone",
    "imsOrg": "{IMS_ORG}",
    "version": "1",
    "meta:containerId": "tenant",
    "meta:sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
    "meta:sandboxType": "production"
}
```

### 非推奨フィールドの確認 {#verify-deprecation}

記述子が適用されたら、適切な `Accept` ヘッダーを使用しながら当該のスキーマを検索することで、フィールドが非推奨（廃止予定）になったかどうかを確認できます。

>[!NOTE]
>
>スキーマを一覧表示する際に非推奨フィールドを表示することは、現在サポートされていません。

**API 形式**

```http
GET /tenant/schemas
```

**リクエスト**

API の応答に非推奨フィールドに関する情報を含めるには、`Accept` ヘッダーを `application/vnd.adobe.xed-deprecatefield+json; version=1` に設定する必要があります。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/_{TENANT_ID}.schemas.c65ddf08cf2d4a2fe94bd06113bf4bc4c855e12a936410d5 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/vnd.adobe.xed-deprecatefield+json; version=1'
```

**応答**

正常な応答はスキーマの詳細を返します（非推奨フィールドには `meta:status` の値として `deprecated` が含まれています）。次の応答例は、スペースの関係で一部省略されています。

```json
"faxPhone": {
    "title": "Fax phone",
    "description": "Fax phone number.",
    "type": "object",
    "meta:xdmType": "object",
    "properties": {},
    "meta:referencedFrom": "https://ns.adobe.com/xdm/context/phonenumber",
    "meta:xdmField": "xdm:faxPhone",
    "meta:status": "deprecated"
}
```

## 次の手順

このドキュメントでは、Schema Registry API を使用して XDM フィールドを非推奨（廃止予定）にする方法について説明しました。 カスタムリソースのフィールドの設定について詳しくは、[API での XDM フィールドの定義](./custom-fields-api.md)に関するガイドを参照してください。記述子の管理について詳しくは、[記述子エンドポイントガイド](../api/descriptors.md)を参照してください。
