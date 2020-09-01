---
keywords: Experience Platform;home;popular topics;identity xid;XID
solution: Experience Platform
title: ID のネイティブ ID の取得
topic: API guide
description: ID データは、通常、取得される XDM データの ID 文字列値および ID 名前空間として提供され、API 呼び出しで使用する ID を指定する際に使用されます。ID が ID サービスで保持されると、ID が生成され、ネイティブ XID と呼ばれる ID に割り当てられます。集約された ID と名前空間にこのよりコンパクトなフォームを使用して ID データのサポートを必要とする Platform API。XID は base64 エンコードされた文字列です。
translation-type: tm+mt
source-git-commit: 0af537e965605e6c3e02963889acd85b9d780654
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 81%

---


# ID の XID の取得

ID データは、通常、取得される XDM データの ID 文字列値および ID 名前空間として提供され、API 呼び出しで使用する ID を指定する際に使用されます。When identities are persisted in [!DNL Identity Service], an ID is generated and assigned to that identity, called the native XID. [!DNL Platform]集約された ID と名前空間にこのよりコンパクトなフォームを使用して ID データのサポートを必要とする API。XID は base64 エンコードされた文字列です。

>[!NOTE]
>
> この形式は主にアドビ内部での使用を目的としています。Native XID as a singular value is more space efficient and is what is used internally within [!DNL Platform] solutions for storage and serialization. しかし、人間が読み取れるわけではなく、不透明で、使用するために別の呼び出しが必要です。

この節で説明するサービスを使用して、指定した ID 値と名前空間の XID を取得します。

**API 形式**

```http
GET https://platform-{REGION}.adobe.io/data/core/identity/identity?namespace={NAMESPACE}&id={ID_VALUE}
```

**リクエスト**

```shell
curl -X GET \
  'https://platform-va7.adobe.io/data/core/identity/identity?namespace=email&id=test@adobetest.com' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

```json
{
    "xid":"BVrqzwVuzbXrLfmnaG3rXrLf3KJg"
}
```
