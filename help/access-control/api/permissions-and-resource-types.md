---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御権限;アクセス制御リソースタイプ;アクセス制御API
solution: Experience Platform
title: 参照 API エンドポイント
description: Access Control API の参照エンドポイントを使用すると、使用可能な権限とリソースタイプの名前を表示し、現在のユーザーに対する有効なアクセス制御ポリシーを表示するために使用できます。
role: Developer
exl-id: 18d84d54-9258-4451-9aa8-7c647b45a8da
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 59%

---

# 参照エンドポイント

>[!NOTE]
>
>ユーザートークンが渡される場合、トークンのユーザーには、リクエストされた組織の「組織管理者」の役割が必要です。

`/acl/reference` エンドポイントに GET リクエストをおこなうことで、すべての権限とリソースタイプの名前をリストできます。これらの名前は、現在のユーザーに対する [ 有効なアクセス制御ポリシーを表示 ](./effective-policies.md) する API 呼び出しで使用できます。

権限とは、Adobe Admin Console で管理され、0 個以上のリソースタイプポリシーにマッピングされるポリシーです。リソースタイプは、特定の種類の [!DNL Experience Platform] リソース（データセットやスキーマなど）の読み取り、書き込み、削除の機能を有効にするポリシーです。

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
