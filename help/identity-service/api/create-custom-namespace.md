---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: カスタム名前空間
topic: API guide
translation-type: tm+mt
source-git-commit: df85ea955b7a308e6be1e2149fcdfb4224facc53

---


# カスタム名前空間

ID名前空間APIを使用して、組織でのみ使用可能なカスタムID名前空間を作成できます。

カスタム名前空間の作成に関する推奨事項については、IDサ [ービスのFAQドキュメントを参照してくださ](../troubleshooting-guide.md)い。

>[!NOTE] 名前空間はIDの修飾子です。 したがって、一度作成した名前空間は削除できません。

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

次のチュートリアルに進み、IDのネイ [ティブIDをリストします。](./list-native-id.md)