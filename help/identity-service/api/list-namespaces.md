---
keywords: Experience Platform；ホーム；人気のトピック；名前空間リスト；リスト名前空間
solution: Experience Platform
title: 使用可能な Id 名前空間のリスト
description: 使用可能なすべての名前空間を一覧表示します。
role: Developer
exl-id: b65e5f86-143d-4ca5-8b3f-2c0a24433bbf
source-git-commit: c16ce1020670065ecc5415bc3e9ca428adbbd50c
workflow-type: tm+mt
source-wordcount: '83'
ht-degree: 44%

---

# 使用可能な ID 名前空間のリスト

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答には、各オブジェクトが使用可能な名前空間を表すオブジェクトの配列が含まれます。「[!UICONTROL custom]」の値が「[!UICONTROL false]」の名前空間は標準の名前空間であり、「[!UICONTROL custom]」の値が「[!UICONTROL true]」の名前空間は組織が作成した名前空間です。

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
