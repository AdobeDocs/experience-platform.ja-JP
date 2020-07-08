---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: カスタム名前空間の作成
topic: API guide
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '75'
ht-degree: 5%

---


# カスタム名前空間の作成

この [!DNL Identity Namespace] APIを使用して、自分の組織だけが利用できるカスタムID名前空間を作成できます。

カスタム名前空間の作成に関する推奨事項については、「IDサービス [に関するFAQドキュメント](../troubleshooting-guide.md)」を参照してください。

>[!NOTE]
>
>名前空間はIDの限定子です。 したがって、一度名前空間を作成すると、削除できなくなります。

**API形式**

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

次のチュートリアルに進み、IDのネイティブIDを [リストします](./list-native-id.md)