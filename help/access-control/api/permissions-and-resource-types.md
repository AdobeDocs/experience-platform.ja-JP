---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 権限名とリソースタイプのリスト
topic: developer guide
translation-type: tm+mt
source-git-commit: 73a492ba887ddfe651e0a29aac376d82a7a1dcc4
workflow-type: tm+mt
source-wordcount: '123'
ht-degree: 100%

---


# 権限名とリソースタイプのリスト

`/acl/reference` エンドポイントに GET リクエストをおこなうことで、すべての権限とリソースタイプの名前をリストできます。これらの名前は、現在のユーザーに対する[有効なポリシーを表示する](./effective-policies.md) API 呼び出しで使用できます。

**権限**&#x200B;とは、Adobe Admin Console で管理され、0 個以上のリソースタイプポリシーにマッピングされるポリシーです。**リソースタイプ**&#x200B;は、特定の種類の リソース（データセットやスキーマなど）の読み取り、書き込み、削除の機能を有効にするポリシーです。[!DNL Platform]

**API 形式**

```http
GET /acl/reference
```

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/access-control/acl/reference \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
```

**応答**

正常な応答は、`permissions` オブジェクトと `resource-types` オブジェクトを返します。前者にはアクセス権限の完全なリスト、後者にはリソースタイプの名前の完全なリストが含まれます。

```json
{
  "permissions": {
    "export-audience-for-segment": {
      "segments": [
        "read"
      ]
    },
    "manage-datasets": {
      "connection": [
        "read",
        "write",
        "delete"
      ],
      "datasets": [
        "read",
        "write",
        "delete"
      ]
    }
    {"..."}
  },
  "resource-types": {
    "classes": [
      "read",
      "write",
      "delete"
    ],
    "connection": [
      "read",
      "write",
      "delete"
    ],
    "data-types": [
      "read",
      "write",
      "delete"
    ],
    "...": [
      "..."
    ]
  }
}
```
