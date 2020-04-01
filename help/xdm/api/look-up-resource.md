---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リソースの検索
topic: developer guide
translation-type: tm+mt
source-git-commit: 71c73a3899ccdd1c024a811b36c411915a3b14be

---


# リソースの検索

特定のリソースを検索するには、リソースの `$id` （URLエンコードされたURI）をリクエストパスに含むGETリクエストを作成します。

**API形式**

```http
GET /{CONTAINER_ID}/{RESOURCE_TYPE}/{RESOURCE_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | リソースが配置されるコンテナ（「グローバル」または「テナント」）。 |
| `{RESOURCE_TYPE}` | リソースライブラリから取得するスキーマのタイプ。 有効なタイプは、、、、お `datatypes`よびの `mixins`いず `schemas`れかです `classes`。 |
| `{RESOURCE_ID}` | URLエンコードされた `$id` URIまたはリ `meta:altId` ソースのURIです。 |

**リクエスト**

```SHELL
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/global/mixins/https%3A%2F%2Fns.adobe.com%2Fxdm%2Fcontext%2Fprofile-person-details \
  -H 'Accept: application/vnd.adobe.xed+json; version=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

リソース参照要求は、Acceptヘッ `version` ダーに含める必要があります。 検索には、次の「承認」ヘッダーを使用できます。

| 承認 | 説明 |
| ------- | ------------ |
| `application/vnd.adobe.xed+json; version={MAJOR_VERSION}` | 「生」と「お `$ref` よび」 `allOf`には、タイトルと説明が含まれます。 |
| `application/vnd.adobe.xed-full+json; version={MAJOR_VERSION}` | `$ref` 解決され `allOf` ました。タイトルと説明があります。 |
| `application/vnd.adobe.xed-notext+json; version={MAJOR_VERSION}` | 「および」を含 `$ref` む、タ `allOf`イトルや説明のない「生」。 |
| `application/vnd.adobe.xed-full-notext+json; version={MAJOR_VERSION}` | `$ref` タイトル `allOf` や説明がなく解決されました。 |
| `application/vnd.adobe.xed-full-desc+json; version={MAJOR_VERSION}` | `$ref` 解決され `allOf` 、記述子が含まれます。 |

>[!NOTE] バージョン `major` （1、2、3など）のみを指定した場合、レジストリは最新バージョン（.1、.2、.3など） `minor` を自動的に返します。

**応答**

成功した応答は、リソースの詳細を返します。 返されるフィールドは、リクエストで送信されるAcceptヘッダーによって異なります。 異なる受け入れヘッダーを試して、回答を比較し、使用事例に最適なヘッダーを判断します。

```JSON
{
    "$id": "https://ns.adobe.com/xdm/context/profile-person-details",
    "title": "Profile Person Details",
    "type": "object",
    "meta:extensible": true,
    "meta:abstract": true,
    "meta:intendedToExtend": [
        "https://ns.adobe.com/xdm/context/profile"
    ],
    "description": "Profile person details including naming, gender etc.",
    "definitions": {
        "profile-person-details": {
            "properties": {
                "person": {
                    "title": "Person",
                    "$ref": "https://ns.adobe.com/xdm/context/person",
                    "description": "An individual actor, contact, or owner.",
                    "meta:xdmField": "xdm:person"
                }
            }
        }
    },
    "allOf": [
        {
            "$ref": "https://ns.adobe.com/xdm/common/extensible#/definitions/@context"
        },
        {
            "$ref": "#/definitions/profile-person-details"
        }
    ],
    "meta:xdmId": "https://ns.adobe.com/xdm/context/profile-person-details",
    "meta:altId": "_xdm.context.profile-person-details",
    "meta:xdmType": "object",
    "meta:status": "experimental",
    "version": "1",
    "$schema": "http://json-schema.org/draft-06/schema#",
    "meta:resourceType": "mixins",
    "meta:registryMetadata": {
        "repo:createDate": 1551745787442,
        "repo:lastModifiedDate": 1551745787442
    }
}
```
