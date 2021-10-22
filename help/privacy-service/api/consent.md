---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: 承認 API エンドポイント
topic-legacy: developer guide
description: プライバシーサービス API を使用して、経験のあるクラウドアプリケーションに対する顧客の同意要求を管理する方法について説明します。
exl-id: ec505749-c0a9-4050-be56-4c0657807ec7
source-git-commit: 82dea48c732b3ddea957511c22f90bbd032ed9b7
workflow-type: tm+mt
source-wordcount: '247'
ht-degree: 5%

---

# 承認エンドポイント

特定の規制については、個人データを収集する前に明示的なお客様の同意が必要です。 `/consent`API のエンドポイントを [!DNL Privacy Service] 使用すると、顧客の同意要求を処理し、プライバシーのワークフローに組み込むことができます。

このガイドを使用する前に、 [ 以下の「 ](./getting-started.md) API の例」で説明されている必要な認証ヘッダーに関する情報については、入門ガイドを参照してください。

## お客様の同意要求を処理

承認要求は、エンドポイントに対して POST 要求によって処理され `/consent` ます。

**API 形式**

```http
POST /consent
```

**リクエスト**

次の要求によって、配列で指定されたユーザー Id に対して新しい同意ジョブが作成され `entities` ます。

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
| `optOutOfSale` | True に設定されている場合 `entities` は、セールのオプトアウトまたは個人データの共有を行うために、ユーザーが指定したことを示します。 |
| `entities` | 承認要求が適用されるユーザーを表すオブジェクトの配列です。 各オブジェクトには、 `namespace` `values` その名前空間を使用して個々のユーザーと一致させるためのおよびの配列が含まれています。 |
| `nameSpace` | 配列内の各オブジェクトには、 `entities` [ ](./appendix.md#standard-namespaces) プライバシーサービス API によって認識される標準の id 名前空間のうちの1つが含まれている必要があります。 |
| `values` | 指定されたの値に対応する各ユーザーの値の配列 `nameSpace` 。 |

{style=&quot;table-layout:auto&quot;}

>[!NOTE]
>
>どの顧客 id 値を送信するかを決定する方法について詳しくは、 [!DNL Privacy Service] id データの指定に関するガイドを参照してください [ ](../identity-data.md) 。

**応答**

応答が成功した場合は、ペイロードなしの HTTP ステータス 202 (受諾) が返されます。これは、要求が受け入れられ、処理が進行中であることを示し [!DNL Privacy Service] ます。
