---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リストの権限名とリソースタイプ
topic: developer guide
translation-type: tm+mt
source-git-commit: 7b354a96d70332cf7a7e9eff322cd3d6ee0fc96a

---


# リストの権限名とリソースタイプ

エンドポイントにGETリクエストを行うことで、すべての権限とリソースタイプの名前をリストで `/acl/reference` きます。 これらの名前は、現在のユーザーに対する有効なポリシーを [表示する](./effective-policies.md) API呼び出しで使用できます。

権限 **とは** 、Adobe Admin Consoleで管理され、0個以上のリソースタイプポリシーに対応付けられたポリシーです。 リソー **スタイプ** は、特定の種類のプラットフォームリソース(データセットやスキーマなど)の読み取り、書き込み、削除の機能を有効にするポリシーです。

**API形式**

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

成功した応答は、オブジェ `permissions` クトとオブジ `resource-types` ェクトを返します。各オブジェクトには、アクセス権限またはリソースタイプの名前の完全なリストが含まれます。

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
