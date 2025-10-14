---
title: 公開証明書エンドポイント
description: MTLS サービス API の/public-certificate エンドポイントを使用して公開証明書を取得する方法を説明します。
role: Developer
exl-id: 8369c783-e595-476f-9546-801cf4f10f71
source-git-commit: d74353e70e992150c031397009d0c8add3df5e7b
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 2%

---

# 公開証明書エンドポイント

>[!NOTE]
>
>Adobeでは、公開 mTLS 証明書の静的ダウンロードをサポートしなくなりました。 この API を使用して、統合用の有効な証明書を取得します。 サービスの中断を回避するために、自動取得が必要になりました。

このガイドでは、公開証明書エンドポイントを使用して、組織のAdobe アプリケーションの公開証明書を安全に取得する方法について説明します。 これには、サンプル API 呼び出しと、開発者がデータ交換を認証および検証するのに役立つ詳細な手順が含まれています。

## はじめに

続行する前に、[&#x200B; はじめる前に &#x200B;](./getting-started.md) を参照して、必要なヘッダーとサンプル API 呼び出しの解釈方法に関する重要な詳細を確認してください。

## API パス {#paths}

mTLS サービス API を使用する際に必要になる基本的な API パスを以下に示します。 これには、プラットフォームゲートウェイ URL、API のベースパス、公開証明書を取得するための完全パスの例が含まれます。

- プラットフォーム ゲートウェイ URL: `https://platform.adobe.io/`
- この API のベース パス：`/data/core/mtls`
- 完全パスの例：`https://platform.adobe.io/data/core/mtls/v1/certificate/public-certificate`

## 公開証明書の取得 {#list}

`/v1/certificate/public-certificate` エンドポイントに対してGET リクエストを実行し、組織のAdobe アプリケーションのいずれかについて公開証明書を取得します。

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

## 証明書のライフサイクルの自動化 {#certificate-lifecycle-automation}

Adobeは、公開 mTLS 証明書のライフサイクルを自動化して、継続性を確保し、サービスの中断を減らします。

- 証明書は有効期限の 60 日前に再発行されます。
- 証明書は有効期限の 30 日前に失効します。

>[!NOTE]
>
>これらのタイムラインは、証明書の有効期間を最大 47 日に短縮することを目的とした [CA/B フォーラムガイドライン &#x200B;](https://www.digicert.com/blog/tls-certificate-lifetimes-will-officially-reduce-to-47-days) に従って、時間の経過と共に短縮されます。

API を使用した自動取得をサポートするには、統合を更新する必要があります。 手動での証明書のダウンロードや静的コピーは、証明書の有効期限が切れたり失効したりする可能性があるので、信頼しないでください。

## 次の手順

API を使用して公開証明書を取得した後、統合を更新し、証明書の有効期限が切れる前に、このエンドポイントを定期的に呼び出します。 この呼び出しをインタラクティブにテストするには、[MTLS API リファレンスページ &#x200B;](https://developer.adobe.com/experience-platform-apis/references/mtls-service/) を参照してください。 証明書ベースの統合に関するより幅広いガイダンスについては、[Adobe Experience Platformでのデータ暗号化の概要 &#x200B;](../../landing/governance-privacy-security/encryption.md) または [&#x200B; データガバナンスの概要 &#x200B;](../home.md) を参照してください。
