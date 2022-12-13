---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御権限;アクセス制御リソースタイプ;アクセス制御API
solution: Experience Platform
title: 参照 API エンドポイント
topic-legacy: developer guide
description: アクセス制御 API の参照エンドポイントを使用すると、使用可能な権限の名前とリソースの種類を表示できます。これを使用して、現在のユーザーに対する有効なアクセス制御ポリシーを表示できます。
exl-id: 18d84d54-9258-4451-9aa8-7c647b45a8da
source-git-commit: 38447348bc96b2f3f330ca363369eb423efea1c8
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 67%

---

# 参照エンドポイント

`/acl/reference` エンドポイントに GET リクエストをおこなうことで、すべての権限とリソースタイプの名前をリストできます。これらの名前は、 [有効なアクセス制御ポリシーの表示](./effective-policies.md) 現在のユーザーの

権限とは、Adobe Admin Console で管理され、0 個以上のリソースタイプポリシーにマッピングされるポリシーです。リソースタイプは、特定の種類の [!DNL Platform] リソース（データセットやスキーマなど）の読み取り、書き込み、削除の機能を有効にするポリシーです。

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
  -H 'x-gw-ims-org-id: {ORG_ID}'
```

**応答**

応答が成功すると、`permissions` オブジェクトと `resource-types` オブジェクトが返されます。前者にはアクセス権限の完全なリスト、後者にはリソースタイプの名前の完全なリストが含まれます。

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
