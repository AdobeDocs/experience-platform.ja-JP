---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 同意
topic: developer guide
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 1%

---


# 同意

個人データを収集するには、一定の規制で顧客の明示的な同意が必要でした。 Privacy ServiceAPIの `/consent` エンドポイントを使用すると、顧客の同意要求を処理して、プライバシーワークフローに統合できます。

このガイドを使用する前に、以下のAPI呼び出し例に示す必須の認証ヘッダーの詳細については、 [「はじめに](./getting-started.md) 」の節を参照してください。

## 顧客の同意要求の処理

同意要求は、エンドポイントに対してPOST要求を行うことで処理され `/consent` ます。

**API形式**

```http
POST /consent
```

**リクエスト**

次の要求は、 `entities` 配列に指定されたユーザーIDに対して新しい同意ジョブを作成します。

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
| `optOutOfSale` | trueに設定されている場合、ユーザーは個人データの販売または共有をオプトアウトし `entities` たいと思っていることを示します。 |
| `entities` | 同意要求が適用されるユーザーを示すオブジェクトの配列。 各オブジェクトには、個々のユーザー `namespace` とその名前空間 `values` を一致させるための、との配列が含まれます。 |
| `nameSpace` | 配列内の各オブジェクトには、Privacy ServiceAPIで `entities` 認識される [標準ID名前空間の1つが含まれている必要があります](./appendix.md#standard-namespaces) 。 |
| `values` | 各ユーザーの値の配列。指定された値に対応し `nameSpace`ます。 |

>[!NOTE]
>
>Privacy Serviceに送信する顧客ID値を決定する方法について詳しくは、IDデータの [提供に関するガイドを参照してください](../identity-data.md)。

**応答**

正常な応答は、ペイロードのないHTTPステータス202(Accepted)を返し、その要求がPrivacy Serviceによって受け入れられ、処理中であることを示します。