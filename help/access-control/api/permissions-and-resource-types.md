---
keywords: Experience Platform；ホーム；人気のあるトピック；アクセス制御権限；アクセス制御リソースタイプ；アクセス制御api
solution: Experience Platform
title: 権限名とリソースタイプのリスト
topic: developer guide
description: Adobe Experience Platform のアクセス制御では、Adobe Admin Console を使用して、様々な Platform 機能のロールと権限を管理できます。/acl/referenceエンドポイントにGETリクエストを行うことで、すべての権限とリソースの種類の名前をリストできます。 これらの名前は、現在のユーザーに対する有効なポリシーを表示する API 呼び出しで使用できます。
translation-type: tm+mt
source-git-commit: 28b733a16b067f951a885c299d59e079f0074df8
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 69%

---


# 権限名とリソースタイプのリスト

`/acl/reference` エンドポイントに GET リクエストをおこなうことで、すべての権限とリソースタイプの名前をリストできます。これらの名前は、現在のユーザーに対する[有効なポリシーを表示する](./effective-policies.md) API 呼び出しで使用できます。

権限とは、Adobe Admin Console で管理され、0 個以上のリソースタイプポリシーにマッピングされるポリシーです。リソースタイプは、特定の種類の[!DNL Platform]リソース(データセットやスキーマなど)の読み取り、書き込み、削除の機能を有効にするポリシーです。

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
