---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リストで使用できる名前空間
topic: API guide
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '68'
ht-degree: 5%

---


# リストで使用できる名前空間

**API形式**

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

応答は、オブジェクトの配列を含み、各オブジェクトは使用可能な名前空間を表します。 「[!UICONTROL custom]」値が「[!UICONTROL false」の名前空間は標準名前空間ですが、「]custom[!UICONTROL 」値が「]true」の名前空間は組織が作成したです。

>[!NOTE]
>
>この応答は領域のために切り捨てられました。

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

カスタム名前空間を [作成するには、次のチュートリアルに進みます](./create-custom-namespace.md)