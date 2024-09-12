---
title: 公開証明書エンドポイント
description: MTLS サービス API の/public-certificate エンドポイントを使用して公開証明書を取得する方法を説明します。
role: Developer
exl-id: 8369c783-e595-476f-9546-801cf4f10f71
source-git-commit: 754044621cdaf1445f809bceaa3e865261eb16f0
workflow-type: tm+mt
source-wordcount: '358'
ht-degree: 3%

---

# 公開証明書エンドポイント

このガイドでは、公開証明書エンドポイントを使用して、組織のAdobeアプリケーションの公開証明書を安全に取得する方法について説明します。 これには、サンプル API 呼び出しと、開発者がデータ交換を認証および検証するのに役立つ詳細な手順が含まれています。

## はじめに

続行する前に、[ はじめる前に ](./getting-started.md) を参照して、必要なヘッダーやサンプル API 呼び出しの読み取り方法など、API の呼び出しを正常に実行するために必要な重要な情報を確認してください。

## API パス {#paths}

mTLS サービス API を使用する際に必要になる基本的な API パスを以下に示します。 これには、プラットフォームゲートウェイ URL、API のベースパス、公開証明書を取得するための完全パスの例が含まれます。

- プラットフォーム ゲートウェイ URL: `https://platform.adobe.io/`
- この API のベース パス：`/data/core/mtls`
- 完全パスの例：`https://platform.adobe.io/data/core/mtls/v1/certificate/public-certificate`

## 公開証明書の取得 {#list}

`/v1/certificate/public-certificate` エンドポイントにGETリクエストを行うことで、組織のAdobeアプリケーションの任意の公開証明書を取得できます。

**API 形式**

```http
GET /v1/certificate/public-certificate
```

次のオプションのクエリパラメーターを、公開証明書を取得する際に使用できます。

| クエリーパラメーター | 説明 | 例 |
| --------------- | ----------- | ------- |
| `page` | リクエストの結果を開始するページを指定します。 | `page=5` |
| `limit` | 1 ページにつき取得する公開証明書の最大数です。 | `limit=20` |

{style="table-layout:auto"}

**リクエスト**

組織に関連付けられている公開証明書を返すリクエストのサンプルは、以下の折りたたみ可能なセクションに表示されます。

+++サンプルリクエスト

```shell
curl -X GET https://platform.adobe.io/data/core/mtls/v1/certificate/public-certificate
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' 
```

+++

**応答**

応答が成功すると、HTTP ステータス 200 が返され、組織の公開証明書の一覧が表示されます。

+++成功した応答のサンプル

```json
{
   "results":[
      {
         "certCommonName":"Adobe Experience Platform",
         "publicCertificate":"-----BEGIN CERTIFICATE-----\nMIIDQTCCAimgAwIBAgITBmyfACAfma......KJY5u89CjAwj\n-----END CERTIFICATE-----",
         "expiryDate":"2024-07-17T21:27:57.434Z"
      }
   ],
   "total":0,
   "count":0,
   "_links":{
      "next":{
         "href":"string",
         "templated":true
      },
      "prev":{
         "href":"string",
         "templated":true
      },
      "page":{
         "href":"string",
         "templated":true
      }
   }
}
```

| プロパティ | 説明 |
| --- | --- |
| `certCommonName` | 証明書の共通名（CN）。通常、証明書の発行先のサーバーまたはエンティティの名前または ID を表します。 |
| `publicCertificate` | 実際の公開証明書（文字列形式）。通信の認証と暗号化に使用されます。 |
| `expiryDate` | 公開証明書の有効期限が切れる日時。ISO 8601 （UTC）形式です。 |

{style="table-layout:auto"}

+++

## 次の手順

このガイドでは、Adobe Experience Platform API を使用した公開証明書の取得方法を確認しました。 規制や組織のポリシーへのコンプライアンスを確保するための顧客データの管理について詳しくは、[ データガバナンスの概要 ](../home.md) を参照してください。

<!-- To test this API call, navigate to the [MTLS API reference page]() to interact with the Experience Platform API endpoints. -->

<!-- Add link after developer page is live -->
