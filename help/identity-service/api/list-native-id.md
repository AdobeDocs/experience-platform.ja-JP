---
keywords: Experience Platform；ホーム；人気の高いトピック；ID xid;XID
solution: Experience Platform
title: ID のネイティブ ID の取得
description: ID データは、通常、取得される XDM データの ID 文字列値および ID 名前空間として提供され、API 呼び出しで使用する ID を指定する際に使用されます。ID が ID サービスで保持されると、ID が生成され、ネイティブ XID と呼ばれる ID に割り当てられます。集約された ID と名前空間にこのよりコンパクトなフォームを使用して ID データのサポートを必要とする Platform API。XID は base64 エンコードされた文字列です。
role: Developer
exl-id: e734f5d8-e00b-43fa-b06c-97c73e1f7c71
source-git-commit: c16ce1020670065ecc5415bc3e9ca428adbbd50c
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 65%

---

# ID のネイティブ ID の取得

ID データは、通常、取得される XDM データの ID 文字列値および ID 名前空間として提供され、API 呼び出しで使用する ID を指定する際に使用されます。ID が [!DNL Identity Service]の場合、ID が生成され、ネイティブ XID と呼ばれる ID に割り当てられます。 [!DNL Platform] 集計 ID と名前空間にこのよりコンパクトなフォームを使用して ID データのサポートを必要とする API。 XID は base64 エンコードされた文字列です。

>[!NOTE]
>
>この形式は主に内部Adobeでの使用です。 単数値としてのネイティブ XID は、スペース効率が高く、内部的に使用される値です。 [!DNL Platform] ストレージとシリアル化のソリューションです。 しかし、人間が読み取れるわけではなく、不透明で、使用するために別の呼び出しが必要です。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

```json
{
    "xid":"BVrqzwVuzbXrLfmnaG3rXrLf3KJg"
}
```
