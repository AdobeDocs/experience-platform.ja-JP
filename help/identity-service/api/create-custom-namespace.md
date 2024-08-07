---
keywords: Experience Platform；ホーム；人気のトピック；名前空間；名前空間；名前空間；名前空間；ID 名前空間；ID;ID
solution: Experience Platform
title: ID サービス API でのカスタム名前空間の作成
description: ID 名前空間 API を使用して、組織でのみ使用可能なカスタム ID 名前空間を作成できます。
role: Developer
exl-id: 6015a225-4508-49cc-9dda-fb9f73a8746c
source-git-commit: c16ce1020670065ecc5415bc3e9ca428adbbd50c
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 52%

---

# ID サービス API でのカスタム名前空間の作成

[!DNL Identity Namespace] API を使用して、組織のみが使用できるカスタム ID 名前空間を作成できます。

カスタム名前空間の作成に関するレコメンデーションについては、[ID サービスに関する FAQ ドキュメント](../troubleshooting-guide.md)を参照してください。

>[!NOTE]
>
>名前空間は ID の修飾子です。 したがって、一度作成した名前空間は削除できません。

**API 形式**

```http
POST /idnamespace/identities
```

**リクエスト**

```shell
curl -X POST \
  https://platform-va7.adobe.io/data/core/idnamespace/identities \
  -H 'Accept-Encoding: gzip, deflate' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -d '{
        "name": "Loyalty Member",
        "code": "Loyalty",
        "description": "Loyalty Program Member ID",
        "idType": "Cross_device"
      }'
```

**応答**

```json
{
    "updateTime": 1576286879075,
    "code": "Loyalty",
    "status": "ACTIVE",
    "description": "Loyalty Program Member ID",
    "id": 10093197,
    "createTime": 1576286879075,
    "idType": "Cross_device",
    "name": "Loyalty Member",
    "custom": true
}
```

## 次の手順

次のチュートリアルに進み、[ID のネイティブ ID をリストします](./list-native-id.md)。
