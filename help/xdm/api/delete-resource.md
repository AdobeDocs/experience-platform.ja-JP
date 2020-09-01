---
keywords: Experience Platform;home;popular topics;api;API;XDM;XDM system;;experience data model;Experience data model;Experience Data Model;data model;Data Model;schema registry;Schema Registry;delete
solution: Experience Platform
title: リソースの削除
describe: It may occasionally be necessary to remove resource from the Schema Registry. Only resources that you create in the tenant container may be deleted. This is done by performing a DELETE request using the $id of the resource you wish to delete.
topic: developer guide
translation-type: tm+mt
source-git-commit: 74a4a3cc713cc068be30379e8ee11572f8bb0c63
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 63%

---


# リソースの削除

It may occasionally be necessary to remove (DELETE) a resource from the [!DNL Schema Registry]. 削除できるのは、テナントコンテナで作成したリソースのみです。これは、削除するリソースの `$id` を使用して DELETE リクエストを実行することでおこなわれます。

**API 形式**

```http
DELETE /tenant/{RESOURCE_TYPE}/{RESOURCE_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{RESOURCE_TYPE}` | The type of resource to be deleted from the [!DNL Schema Library]. 有効なタイプは、`datatypes`、`mixins`、`schemas` および `classes` です。 |
| `{RESOURCE_ID}` | リソースの URL エンコードされた `$id` URI または `meta:altId`。 |

**リクエスト**

DELETE リクエストには、Accept ヘッダーは不要です。

```SHELL
curl -X DELETE \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/mixins/https%3A%2F%2Fns.adobe.com%2F{TENANT_ID}%2Fmixins%2F4fbd5368aa67f0e74d5838f67694c867 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、HTTP ステータス 204（コンテンツなし）と空白の本文を返します。

リソースに対してルックアップ（GET）リクエストを試行すると、削除を確認できます。You will need to include an Accept header in the request, but should receive an HTTP status 404 (Not Found) because the resource has been removed from the [!DNL Schema Registry].