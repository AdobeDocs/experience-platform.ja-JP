---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リソースの検索
topic: developer guide
translation-type: tm+mt
source-git-commit: d04bf35e49488ab7d5e07de91eb77d0d9921b6fa
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 95%

---


# リソースの検索

特定のリソースを検索するには、リソースの `$id`（URL エンコードされた URI）をリクエストパスに含む GET リクエストを作成します。

**API 形式**

```http
GET /{CONTAINER_ID}/{RESOURCE_TYPE}/{RESOURCE_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | リソースが配置されるコンテナ（「グローバル」または「テナント」）。 |
| `{RESOURCE_TYPE}` | The type of resource to retrieve from the [!DNL Schema Library]. 有効なタイプは、`datatypes`、`mixins`、`schemas` および `classes` です。 |
| `{RESOURCE_ID}` | リソースの URL エンコードされた `$id` URI または `meta:altId`。 |

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

リソース検索リクエストでは、`version` を Accept ヘッダーに含める必要があります。検索には、次の Accept ヘッダーを使用できます。

| Accept | 説明 |
| ------- | ------------ |
| `application/vnd.adobe.xed+json; version={MAJOR_VERSION}` | `$ref` および `allOf` で生、タイトルと説明を含む |
| `application/vnd.adobe.xed-full+json; version={MAJOR_VERSION}` | `$ref` および `allOf` を解決、タイトルと説明を含む |
| `application/vnd.adobe.xed-notext+json; version={MAJOR_VERSION}` | `$ref` および `allOf` で生、タイトルや説明なし |
| `application/vnd.adobe.xed-full-notext+json; version={MAJOR_VERSION}` | `$ref` および `allOf` で解決、タイトルや説明なし |
| `application/vnd.adobe.xed-full-desc+json; version={MAJOR_VERSION}` | `$ref` および `allOf` で解決、説明を含む |

>[!NOTE]
>
>`major` バージョン（1、2、3 など）のみを指定した場合、レジストリは最新 `minor` バージョン（1、2、3 など）を自動的に返します。

**応答**

正常な応答は、リソースの詳細を返します。返されるフィールドは、リクエストで送信される Accept ヘッダーによって異なります。異なる Accept ヘッダーを試して、応答を比較し、使用事例に最適なヘッダーを判断します。

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
