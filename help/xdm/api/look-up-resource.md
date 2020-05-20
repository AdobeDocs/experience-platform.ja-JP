---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リソースの検索
topic: developer guide
translation-type: tm+mt
source-git-commit: 71c73a3899ccdd1c024a811b36c411915a3b14be
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 2%

---


# リソースの検索

リクエストパスにリソースの `$id` （URLエンコードされたURI）を含むGETリクエストを作成することで、特定のリソースを検索できます。

**API形式**

```http
GET /{CONTAINER_ID}/{RESOURCE_TYPE}/{RESOURCE_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | リソースが配置されているコンテナ（「グローバル」または「テナント」）。 |
| `{RESOURCE_TYPE}` | スキーマライブラリから取得するリソースのタイプ。 有効なタイプは、 `datatypes`、、、お `mixins`よび `schemas``classes`です。 |
| `{RESOURCE_ID}` | URLエンコードされた `$id` URIまたはリソース `meta:altId` のURIです。 |

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

リソース参照要求を受け入れるには、Acceptヘッダーに含める必要があ `version` ります。 検索には、次のAcceptヘッダーを使用できます。

| 同意 | 説明 |
| ------- | ------------ |
| `application/vnd.adobe.xed+json; version={MAJOR_VERSION}` | 「および」を含む「生」 `$ref` には、タイトルと説明が含まれ `allOf`ます。 |
| `application/vnd.adobe.xed-full+json; version={MAJOR_VERSION}` | `$ref` にはタイトルと説明が `allOf` あります。 |
| `application/vnd.adobe.xed-notext+json; version={MAJOR_VERSION}` | 「お `$ref` よび」付きの生データ `allOf`で、タイトルや説明は付きません。 |
| `application/vnd.adobe.xed-full-notext+json; version={MAJOR_VERSION}` | `$ref` タイトルも説明も `allOf` なく解決しました。 |
| `application/vnd.adobe.xed-full-desc+json; version={MAJOR_VERSION}` | `$ref` 解 `allOf` 決され、記述子が含まれます。 |

>[!NOTE] バージョン（1、2、3など）のみを指定した場合、レジストリは最新 `major``minor` バージョン（.1、.2、.3など）を自動的に返します。

**応答**

成功した応答は、リソースの詳細を返します。 返されるフィールドは、要求で送信されるAcceptヘッダーによって異なります。 様々なAcceptヘッダーを使用してテストし、応答を比較し、使用事例に最適なヘッダーを判断します。

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
