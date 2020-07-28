---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: カスタム名前空間の作成
topic: API guide
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '75'
ht-degree: 76%

---


# カスタム名前空間の作成

Using the [!DNL Identity Namespace] API, you can create a custom identity namespace that will be available only to your organization.

カスタム名前空間の作成に関する推奨事項については、[ID サービスに関する FAQ ドキュメント](../troubleshooting-guide.md)を参照してください。

>[!NOTE]
>
> 名前空間は ID の修飾子です。したがって、一度作成した名前空間は削除できません。

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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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