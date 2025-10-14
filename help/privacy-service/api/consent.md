---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: 同意 API エンドポイント
description: Privacy ServiceAPI を使用して、Experience Cloudアプリケーションの顧客同意リクエストを管理する方法について説明します。
role: Developer
exl-id: ec505749-c0a9-4050-be56-4c0657807ec7
source-git-commit: c16ce1020670065ecc5415bc3e9ca428adbbd50c
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 4%

---

# 同意エンドポイント

特定の規制では、個人データを収集する前に、顧客の明示的な同意が必要です。 [!DNL Privacy Service] API の `/consent` エンドポイントを使用すると、顧客同意リクエストを処理し、それらをプライバシーワークフローに統合できます。

このガイドを使用する前に、以下の API 呼び出しの例に示されている必要な認証ヘッダーについて詳しくは、[&#x200B; はじめに &#x200B;](./getting-started.md) ガイドを参照してください。

## 顧客の同意リクエストの処理

同意リクエストは、`/consent` エンドポイントに対してPOSTリクエストを行うことで処理されます。

**API 形式**

```http
POST /consent
```

**リクエスト**

次のリクエストは、`entities` 配列で指定されたユーザー ID に対して新しい同意ジョブを作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/privacy/consent \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `optOutOfSale` | true に設定した場合、に基づいて提供されたユーザーが個人データの販売 `entities` たは共有をオプトアウトすることを希望することを示します。 |
| `entities` | 同意リクエストが適用されるユーザーを示すオブジェクトの配列。 各オブジェクトには、`namespace` と、個々のユーザーをその名前空間に一致させる `values` の配列が含まれます。 |
| `nameSpace` | `entities` 配列の各オブジェクトには、Privacy ServiceAPI で認識される [&#x200B; 標準 ID 名前空間 &#x200B;](./appendix.md#standard-namespaces) の 1 つが含まれている必要があります。 |
| `values` | 指定された `nameSpace` に対応する、各ユーザーの値の配列。 |

{style="table-layout:auto"}

>[!NOTE]
>
>[!DNL Privacy Service] に送信する顧客 ID 値を決定する方法について詳しくは、[ID データの提供 &#x200B;](../identity-data.md) に関するガイドを参照してください。

**応答**

応答が成功すると、ペイロードのない HTTP ステータス 202 （許可済み）が返され、リクエストが [!DNL Privacy Service] によって受け入れられ、処理中であることを示します。
