---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: IDのネイティブIDの取得
topic: API guide
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 1%

---


# IDのXIDの取得

IDデータは、通常、取り込まれるXDMデータのID文字列値およびID名前空間として提供され、API呼び出しで使用するIDを指定する際に使用されます。 IDがで保持され [!DNL Identity Service]る場合、IDが生成され、ネイティブXIDと呼ばれるIDに割り当てられます。 [!DNL Platform] 集計されたIDと名前空間に対して、よりコンパクトなフォームを使用するIDデータのサポートが必要なAPI。 XIDは、base64エンコードされた文字列です。

>[!NOTE]
>
>この形式は主にアドビ内部での使用を目的としています。 単数値としてのネイティブXIDは、スペース効率が高く、ストレージとシリアル化のための [!DNL Platform] ソリューション内で内部的に使用されるものです。 ただし、読み取り専用ではなく不透明で、使用するには別の呼び出しが必要です。

この節で説明するサービスを使用して、特定のID値と名前空間のXIDを取得します。

**API形式**

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
