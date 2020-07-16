---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リソースの削除
topic: developer guide
translation-type: tm+mt
source-git-commit: d04bf35e49488ab7d5e07de91eb77d0d9921b6fa
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 7%

---


# リソースの削除

リソースをから削除(DELETE)する必要がある場合があり [!DNL Schema Registry]ます。 テナントコンテナで作成したリソースのみを削除できます。 これは、削除するリソースを使用してDELETEリクエスト `$id` を実行することで行われます。

**API形式**

```http
DELETE /tenant/{RESOURCE_TYPE}/{RESOURCE_ID} 
```

| パラメーター | 説明 |
| --- | --- |
| `{RESOURCE_TYPE}` | から削除するリソースの種類で [!DNL Schema Library]す。 有効なタイプは、 `datatypes`、、、お `mixins`よび `schemas``classes`です。 |
| `{RESOURCE_ID}` | URLエンコードされた `$id` URIまたはリソース `meta:altId` のURIです。 |

**リクエスト**

DELETE要求にはAcceptヘッダーは必要ありません。

```SHELL
curl -X DELETE \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/mixins/https%3A%2F%2Fns.adobe.com%2F{TENANT_ID}%2Fmixins%2F4fbd5368aa67f0e74d5838f67694c867 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、HTTPステータス204（コンテンツなし）と空白の本文が返されます。

リソースに対してルックアップ(GET)要求を試行すると、削除を確認できます。 要求にはAcceptヘッダーを含める必要がありますが、リソースがリソースから削除されたので、HTTPステータス404 （見つかりません）を受け取る必要があり [!DNL Schema Registry]ます。