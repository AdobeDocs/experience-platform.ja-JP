---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: 同意 API エンドポイント
topic-legacy: developer guide
description: Privacy ServiceAPI を使用して、Experience Cloudアプリケーションの顧客の同意リクエストを管理する方法について説明します。
exl-id: ec505749-c0a9-4050-be56-4c0657807ec7
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '247'
ht-degree: 5%

---

# 同意エンドポイント

特定の規制では、個人データを収集する前に、明示的なお客様の同意が必要です。 この `/consent` エンドポイント [!DNL Privacy Service] API を使用すると、顧客の同意リクエストを処理し、プライバシーワークフローに統合できます。

このガイドを使用する前に、 [はじめに](./getting-started.md) 以下の API 呼び出し例で示す、必要な認証ヘッダーに関する情報を参照してください。

## 顧客の同意要求の処理

同意リクエストは、 `/consent` endpoint.

**API 形式**

```http
POST /consent
```

**リクエスト**

次のリクエストでは、 `entities` 配列。

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
| `optOutOfSale` | true に設定した場合、は、ユーザーが `entities` 個人データの販売または共有のオプトアウトを希望しています。 |
| `entities` | 同意リクエストが適用されるユーザーを示すオブジェクトの配列。 各オブジェクトには、 `namespace` と、 `values` を追加して、個々のユーザーをその名前空間に一致させます。 |
| `nameSpace` | 各オブジェクト ( `entities` 配列には、次のいずれかを含める必要があります： [標準 id 名前空間](./appendix.md#standard-namespaces) はPrivacy ServiceAPI で認識される。 |
| `values` | 各ユーザーの値の配列で、指定した `nameSpace`. |

{style=&quot;table-layout:auto&quot;}

>[!NOTE]
>
>送信先の顧客 ID 値を決定する方法の詳細 [!DNL Privacy Service]( [id データの提供](../identity-data.md).

**応答**

正常な応答は、ペイロードなしで HTTP ステータス 202（許可済み）を返し、リクエストがによって受け入れられたことを示します。 [!DNL Privacy Service] 処理中です。
