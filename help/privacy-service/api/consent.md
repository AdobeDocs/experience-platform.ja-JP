---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: 同意 API エンドポイント
topic-legacy: developer guide
description: Privacy ServiceAPI を使用して、Experience Cloudアプリケーションに対する顧客の同意リクエストを管理する方法を説明します。
exl-id: ec505749-c0a9-4050-be56-4c0657807ec7
source-git-commit: e226990fc84926587308077b32b128bfe334e812
workflow-type: tm+mt
source-wordcount: '247'
ht-degree: 5%

---

# 同意エンドポイント

特定の規制では、個人データを収集する前に、明示的な顧客の同意が必要です。 [!DNL Privacy Service] API の `/consent` エンドポイントを使用すると、顧客の同意リクエストを処理して、プライバシーワークフローに統合できます。

このガイドを使用する前に、以下の API 呼び出し例で示す必要な認証ヘッダーの詳細について、[ はじめに ](./getting-started.md) の節を参照してください。

## 顧客の同意要求の処理

同意要求は、`/consent` エンドポイントに対してPOST要求をおこなうことで処理されます。

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
| `optOutOfSale` | true に設定した場合、`entities` の下に提供されたユーザーが、自分の個人データの販売または共有をオプトアウトすることを示します。 |
| `entities` | 同意リクエストが適用されるユーザーを示すオブジェクトの配列。 各オブジェクトには、`namespace` と `values` の配列が含まれ、その名前空間と個々のユーザーを照合します。 |
| `nameSpace` | `entities` 配列の各オブジェクトには、Privacy ServiceAPI で認識される [ 標準 ID 名前空間 ](./appendix.md#standard-namespaces) の 1 つを含める必要があります。 |
| `values` | 各ユーザーの値の配列。指定された `nameSpace` に対応します。 |

{style=&quot;table-layout:auto&quot;}

>[!NOTE]
>
>[!DNL Privacy Service] に送信する顧客 ID 値を決定する方法の詳細については、[ID データの提供 ](../identity-data.md) に関するガイドを参照してください。

**応答**

正常な応答は、ペイロードなしで HTTP ステータス 202（許可済み）を返し、要求が [!DNL Privacy Service] によって受け入れられ、処理中であることを示します。
