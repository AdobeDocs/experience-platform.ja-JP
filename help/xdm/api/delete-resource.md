---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リソースの削除
topic: developer guide
translation-type: tm+mt
source-git-commit: d9ab2b1226b051be43f8fc0dd222bc075caed6f0

---


# リソースの削除

リソースレジストリからリソースを削除(DELETE)する必要が生じる場合があります。 削除できるのは、テナントリソースで作成したコンテナのみです。 これは、削除するリソースのを使用してDELETE `$id` リクエストを実行することで行われます。

**API形式**

```http
DELETE /tenant/{RESOURCE_TYPE}/{RESOURCE_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{RESOURCE_TYPE}` | リソースライブラリから削除するスキーマの種類。 有効なタイプは、、、、お `datatypes`よびの `mixins`いず `schemas`れかです `classes`。 |
| `{RESOURCE_ID}` | URLエンコードされた `$id` URIまたはリ `meta:altId` ソースのURIです。 |

**リクエスト**

DELETE要求には、Acceptヘッダーは不要です。

```SHELL
curl -X DELETE \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/mixins/https%3A%2F%2Fns.adobe.com%2F{TENANT_ID}%2Fmixins%2F4fbd5368aa67f0e74d5838f67694c867 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、HTTPステータス204（コンテンツなし）と空白の本文を返します。

リソースに対してルックアップ(GET)リクエストを試行すると、削除を確認できます。 リソースがスキーマレジストリから削除されているので、Acceptヘッダーを要求に含める必要がありますが、HTTPステータス404 (Not Found)を受け取る必要があります。