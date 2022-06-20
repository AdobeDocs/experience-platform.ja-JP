---
title: XDM フィールドの廃止
description: スキーマレジストリ API で Experience Data Model(XDM) フィールドを廃止する方法について説明します。
source-git-commit: dc400dce8a77f27347e767230faf7301afc7c1fb
workflow-type: tm+mt
source-wordcount: '519'
ht-degree: 11%

---

# XDM フィールドの廃止

エクスペリエンスデータモデル (XDM) では、 [スキーマレジストリ API](https://developer.adobe.com/experience-platform-apis/references/schema-registry/). このドキュメントでは、様々な XDM リソースのフィールドを非推奨にする方法について説明します。

## はじめに

このチュートリアルでは、スキーマレジストリ API を呼び出す必要があります。 次を確認してください： [開発者ガイド](../api/getting-started.md) を参照してください。 これには、`{TENANT_ID}`、「コンテナ」の概念、リクエストを行うのに必要なヘッダー（ ヘッダーと使用可能な値には特に注意を払う）が含まれます。`Accept`

## カスタムフィールドを廃止 {#custom}

カスタムクラス、フィールドグループ、またはデータ型のフィールドを廃止するには、PUTまたはPATCHリクエストを使用してカスタムリソースを更新し、属性を追加します `meta:status: deprecated` 問題のフィールドに

>[!NOTE]
>
>XDM のカスタムリソースの更新に関する一般的な情報については、次のドキュメントを参照してください。
>
>* [クラスの更新](../api/classes.md#patch)
>* [フィールドグループの更新](../api/field-groups.md#patch)
>* [データタイプの更新](../api/data-types.md#patch)


以下の API 呼び出しの例では、カスタムデータ型のフィールドが区別されています。

**API 形式**

```http
PATCH /tenant/datatypes/{DATA_TYPE_ID}
```

**リクエスト**

次のリクエストは、 `expansionArea` 不動産プロパティを説明するデータ型のフィールド。

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

正常な応答は、カスタムリソースの更新の詳細を返します。非推奨のフィールドには `meta:status` 値 `deprecated`. 次の応答の例は、スペースを節約するために省略されています。

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

## スキーマ内の標準フィールドを廃止 {#standard}

標準クラス、フィールドグループ、データ型のフィールドは、直接非推奨にすることはできません。 代わりに、記述子を使用して、これらの標準リソースを使用する個々のスキーマでの使用を非推奨にできます。

### フィールド廃止記述子の作成 {#create-descriptor}

廃止するスキーマフィールドの記述子を作成するには、に対してPOSTリクエストを実行します。 `/tenant/descriptors` endpoint.

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
| `@type` | 記述子のタイプ。 フィールド廃止記述子の場合、この値をに設定する必要があります。 `xdm:descriptorDeprecated`. |
| `xdm:sourceSchema` | URI `$id` 記述子を適用するスキーマの。 |
| `xdm:sourceVersion` | 記述子を適用するスキーマのバージョン。 をに設定する必要があります。 `1`. |
| `xdm:sourceProperty` | 記述子を適用するスキーマ内のプロパティへのパス。 記述子を複数のプロパティに適用する場合は、パスのリストを配列 ( 例えば、 `["/firstName", "/lastName"]`) をクリックします。 |

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

### 非推奨のフィールドを確認します。 {#verify-deprecation}

記述子が適用されたら、適切なを使用しながら問題のスキーマを検索することで、フィールドが非推奨になったかどうかを確認できます `Accept` ヘッダー。

>[!NOTE]
>
>現在、スキーマをリストする際の非推奨のフィールドの表示はサポートされていません。

**API 形式**

```http
GET /tenant/schemas
```

**リクエスト**

API 応答に非推奨のフィールドに関する情報を含めるには、 `Accept` ヘッダー `application/vnd.adobe.xed-deprecatefield+json; version=1`.

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

正常な応答は、スキーマの詳細を返します。非推奨のフィールドには、 `meta:status` 値 `deprecated`. 次の応答の例は、スペースを節約するために省略されています。

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

このドキュメントでは、スキーマレジストリ API を使用して XDM フィールドを廃止する方法について説明しました。 カスタムリソースのフィールドの設定について詳しくは、 [API での XDM フィールドの定義](./custom-fields-api.md). 記述子の管理について詳しくは、 [記述子エンドポイントガイド](../api/descriptors.md).
