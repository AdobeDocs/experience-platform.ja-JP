---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 権限とリソースタイプのリスト名
topic: developer guide
translation-type: tm+mt
source-git-commit: 7b354a96d70332cf7a7e9eff322cd3d6ee0fc96a
workflow-type: tm+mt
source-wordcount: '124'
ht-degree: 1%

---


# 権限とリソースタイプのリスト名

エンドポイントにGETリクエストを行うことで、すべての権限とリソースの種類の名前をリストでき `/acl/reference` ます。 これらの名前は、現在のユーザーに対して有効なポリシーを [表示するためのAPI呼び出しで使用でき](./effective-policies.md) ます。

「 **権限** 」とは、Adobe管理コンソールで管理され、0個以上のリソースタイプポリシーに対応付けられるポリシーです。 リ **ソースタイプ** とは、特定の種類のプラットフォームリソース(データセットやスキーマなど)の読み取り、書き込み、削除の機能を有効にするポリシーです。

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

成功した応答は、それぞれアクセス権限またはリソースタイプの名前の完全なリストを含む `permissions` オブジェクトと `resource-types` オブジェクトを返します。

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
