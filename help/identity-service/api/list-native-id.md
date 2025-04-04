---
keywords: Experience Platform；ホーム；人気のトピック；id xid;XID
solution: Experience Platform
title: ID のネイティブ ID の取得
description: ID データは、通常、取得される XDM データの ID 文字列値および ID 名前空間として提供され、API 呼び出しで使用する ID を指定する際に使用されます。ID が ID サービスで保持されると、ID が生成され、ネイティブ XID と呼ばれる ID に割り当てられます。集計された ID と名前空間に、このよりコンパクトな形式を使用した ID データのサポートを必要とするExperience Platform API。 XID は base64 エンコードされた文字列です。
role: Developer
exl-id: e734f5d8-e00b-43fa-b06c-97c73e1f7c71
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 58%

---

# ID のネイティブ ID の取得

ID データは、通常、取得される XDM データの ID 文字列値および ID 名前空間として提供され、API 呼び出しで使用する ID を指定する際に使用されます。ID が [!DNL Identity Service] に永続化されると、ID が生成され、ネイティブ XID と呼ばれるその ID に割り当てられます。 ID データを必要とする [!DNL Experience Platform] API は、集計された ID と名前空間に対して、このよりコンパクトな形式を使用します。 XID は base64 エンコードされた文字列です。

>[!NOTE]
>
>このフォーマットは、主にAdobeの内部用です。 単一の値としてのネイティブ XID は、よりスペースの効率が高く、ストレージおよびシリアル化の [!DNL Experience Platform] ソリューション内で内部的に使用されます。 しかし、人間が読み取れるわけではなく、不透明で、使用するために別の呼び出しが必要です。

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
