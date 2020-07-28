---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 名前空間のリスト
topic: API guide
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '68'
ht-degree: 63%

---


# 名前空間のリスト

**API 形式**

```http
GET /idnamespace/identities
```

**リクエスト**

```shell
curl -X GET \
  'https://platform-va7.adobe.io/data/core/idnamespace/identities' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答には、各オブジェクトが使用可能な名前空間を表すオブジェクトの配列が含まれます。Namespaces with a &quot;[!UICONTROL custom]&quot; value of &quot;[!UICONTROL false]&quot; are standard namespaces, while those with a &quot;[!UICONTROL custom]&quot; value of &quot;[!UICONTROL true]&quot; are namespaces that your organization has created.

>[!NOTE]
>
>この応答はスペースを節約するために切り捨てられています。

```json
[
  {
        "updateTime": 1441122419000,
        "code": "CORE",
        "status": "ACTIVE",
        "description": "CORE Namespace",
        "id": 0,
        "createTime": 1441122419000,
        "idType": "COOKIE",
        "name": "CORE",
        "custom": false
    },
    {
        "updateTime": 1495153678000,
        "code": "ECID",
        "status": "ACTIVE",
        "description": "ECID Namespace",
        "id": 4,
        "createTime": 1495153678000,
        "idType": "COOKIE",
        "name": "ECID",
        "custom": false
    },
    {
        "updateTime": 1522783145000,
        "code": "AdCloud",
        "status": "ACTIVE",
        "description": "Adobe AdCloud - ID Syncing Partner",
        "id": 411,
        "createTime": 1522783145000,
        "idType": "COOKIE",
        "name": "AdCloud",
        "custom": false
    }
]
```

## 次の手順

次のチュートリアルに進み、[カスタム名前空間を作成](./create-custom-namespace.md)します。