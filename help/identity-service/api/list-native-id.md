---
keywords: Experience Platform；ホーム；人気のあるトピック；ID xid;XID
solution: Experience Platform
title: ID のネイティブ ID の取得
topic-legacy: API guide
description: ID データは、通常、取得される XDM データの ID 文字列値および ID 名前空間として提供され、API 呼び出しで使用する ID を指定する際に使用されます。ID が ID サービスで保持されると、ID が生成され、ネイティブ XID と呼ばれる ID に割り当てられます。集約された ID と名前空間にこのよりコンパクトなフォームを使用して ID データのサポートを必要とする Platform API。XID は base64 エンコードされた文字列です。
exl-id: e734f5d8-e00b-43fa-b06c-97c73e1f7c71
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 76%

---

# ID のネイティブ ID の取得

ID データは、通常、取得される XDM データの ID 文字列値および ID 名前空間として提供され、API 呼び出しで使用する ID を指定する際に使用されます。ID が [!DNL Identity Service] に保持されると、ID が生成され、ネイティブ XID と呼ばれる ID に割り当てられます。 [!DNL Platform]集約された ID と名前空間にこのよりコンパクトなフォームを使用して ID データのサポートを必要とする API。XID は base64 エンコードされた文字列です。

>[!NOTE]
>
> この形式は主にアドビ内部での使用を目的としています。単数値としてのネイティブ XID は、スペース効率が高く、[!DNL Platform] ソリューション内でストレージとシリアル化のために内部的に使用されます。 しかし、人間が読み取れるわけではなく、不透明で、使用するために別の呼び出しが必要です。

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
