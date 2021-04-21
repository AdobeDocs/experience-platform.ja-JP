---
keywords: Experience Platform；ホーム；人気の高いトピック
solution: Experience Platform
title: 同意APIエンドポイント
topic-legacy: developer guide
description: Privacy ServiceAPIを使用して、Experience Cloudアプリに対する顧客の同意要求を管理する方法について説明します。
exl-id: ec505749-c0a9-4050-be56-4c0657807ec7
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 2%

---

# 同意エンドポイント

特定の規制では、個人データを収集する前に、明示的な顧客の同意が必要です。 [!DNL Privacy Service] APIの`/consent`エンドポイントを使用すると、顧客の同意要求を処理し、それらをプライバシーワークフローに統合できます。

このガイドを使用する前に、以下のAPI呼び出しの例に示す必須の認証ヘッダーの情報については、[はじめに](./getting-started.md)の節を参照してください。

## 顧客の同意要求の処理

`/consent`エンドポイントにPOSTリクエストを行うことで、同意リクエストが処理されます。

**API 形式**

```http
POST /consent
```

**リクエスト**

次のリクエストは、`entities`配列で提供されるユーザーIDに対して新しい同意ジョブを作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/privacy/consent \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -d '{
        "optOutOfSale": true,
        "entities": [
          {
            "nameSpace": "email",
            "values": [
              "dsmith@acme.com",
              "ajones@acme.com"
            ]
          },
          {
            "nameSpace": "ECID",
            "values": [
              "443636576799758681021090721276"
            ]
          }
        ]
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `optOutOfSale` | trueに設定した場合、`entities`の下に提供されるユーザーが、個人データの販売または共有をオプトアウトすることを希望することを示します。 |
| `entities` | 同意要求が適用されるユーザーを示すオブジェクトの配列。 各オブジェクトには、`namespace`と`values`の配列が含まれ、各名前空間に対応付けられます。 |
| `nameSpace` | `entities`配列内の各オブジェクトには、Privacy ServiceAPIで認識される[標準ID名前空間](./appendix.md#standard-namespaces)の1つが含まれている必要があります。 |
| `values` | 指定された`nameSpace`に対応する、各ユーザーの値の配列。 |

>[!NOTE]
>
>[!DNL Privacy Service]に送信する顧客ID値を決定する方法について詳しくは、[IDデータ](../identity-data.md)の提供ガイドを参照してください。

**応答**

正常な応答は、ペイロードなしのHTTPステータス202 (Accepted)を返し、その要求が[!DNL Privacy Service]によって受け入れられ、処理中であることを示します。
