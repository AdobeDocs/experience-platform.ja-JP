---
title: Flow Service API を使用したGoogle Ads ベース接続の作成
description: Flow Service API を使用してAdobe Experience PlatformをGoogle Ads に接続する方法について説明します。
exl-id: 4658e392-1bd9-4e74-aa05-96109f9b62a0
source-git-commit: ce3dabe4ab08a41e581b97b74b3abad352e3267c
workflow-type: tm+mt
source-wordcount: '741'
ht-degree: 27%

---

# を使用したGoogle Ads ベース接続の作成 [!DNL Flow Service] API

>[!WARNING]
>
>この [!DNL Google Ads] ソースは一時的に使用できません。 Adobeはこのソースに関する問題を解決するために取り組んでいます。

>[!NOTE]
>
>Google Ads ソースはベータ版です。 を参照してください。 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベル付きソースの使用の詳細については、を参照してください。

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、を使用してGoogle Ads のベース接続を作成する手順について説明します [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platformサービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md):Experience Platformには、1 つのExperience Platformインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、を使用してGoogle Ads に正しく接続するために必要な追加情報を示します [!DNL Flow Service] API です。

### 必要な資格情報の収集

の目的で [!DNL Flow Service] Google Ads に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `clientCustomerId` | クライアントカスタマー ID は、Google Ads API で管理するGoogle Ads クライアントアカウントに対応するアカウント番号です。 この ID は、のテンプレートに従います `123-456-7890`. |
| `loginCustomerId` | ログインカスタマー ID は、Google Ads Manager のアカウントに対応するアカウント番号で、特定の運用顧客からレポートデータを取得するために使用されます。 ログイン顧客 ID について詳しくは、を参照してください。 [Google Ads API ドキュメント](https://developers.google.com/search-ads/reporting/concepts/login-customer-id). |
| `developerToken` | 開発者トークンを使用すると、Google Ads API にアクセスできます。 同じ開発者トークンを使用して、すべてのGoogle Ads アカウントに対してリクエストを行うことができます。 による開発者トークンの取得 [manager アカウントへのログイン](https://ads.google.com/home/tools/manager-accounts/) その後、に移動します。 [!DNL API Center] ページ。 |
| `refreshToken` | 更新トークンはの一部です [!DNL OAuth2] 認証。 このトークンを使用すると、有効期限が切れた後にアクセストークンを再生成できます。 |
| `clientId` | クライアント ID は、クライアント秘密鍵と並行して、の一部として使用されます [!DNL OAuth2] 認証。 クライアント ID とクライアント秘密鍵を組み合わせることで、Googleに対するアプリケーションを識別し、お客様のアカウントに代わってアプリケーションを動作させることができます。 |
| `clientSecret` | クライアント秘密鍵は、クライアント ID と並行して次の目的で使用されます [!DNL OAuth2] 認証。 クライアント ID とクライアント秘密鍵を組み合わせることで、Googleに対するアプリケーションを識別し、お客様のアカウントに代わってアプリケーションを動作させることができます。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。Google Ads の接続仕様 ID は次のとおりです。 `d771e9c1-4f26-40dc-8617-ce58c4b53702`. |

の API の概要ドキュメントを参照してください [Google Ads の使用の手引きの詳細情報](https://developers.google.com/google-ads/api/docs/first-call/overview).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、次に対してPOSTリクエストを実行します。 `/connections` エンドポイント：リクエストパラメーターの一部としてGoogle Ads 認証資格情報を指定します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、Google Ads のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json'
  -d '{
      "name": "Google Ads base connection",
      "description": "Google Ads base connection",
      "auth": {
          "specName": "Basic Authentication",
          "params": {
              "clientCustomerID": "{CLIENT_CUSTOMER_ID}",
              "loginCustomerID": "{LOGIN_CUSTOMER_ID}",
              "developerToken": "{DEVELOPER_TOKEN}",
              "authenticationType": "{AUTHENTICATION_TYPE}"
              "clientId": "{CLIENT_ID}",
              "clientSecret": "{CLIENT_SECRET}",
              "refreshToken": "{REFRESH_TOKEN}"
          }
      },
      "connectionSpec": {
          "id": "d771e9c1-4f26-40dc-8617-ce58c4b53702",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.clientCustomerID` | Google Ads アカウントのクライアントカスタマー ID。 |
| `auth.params.loginCustomerID` | Google Ads Manager アカウントに対応するログインカスタマー ID。 |
| `auth.params.developerToken` | Google Ads アカウントの開発者トークン。 |
| `auth.params.refreshToken` | Google Ads アカウントの更新トークン。 |
| `auth.params.clientID` | Google Ads アカウントのクライアント ID。 |
| `auth.params.clientSecret` | Google Ads アカウントのクライアント秘密鍵。 |
| `connectionSpec.id` | Google Ads 接続仕様 ID: `d771e9c1-4f26-40dc-8617-ce58c4b53702`. |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含め、新しく作成されたベース接続の詳細が返されます。この ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "2484f2df-c057-4ab5-84f2-dfc0577ab592",
    "etag": "\"10033e77-0000-0200-0000-5e96785b0000\""
}
```

## 次の手順

このチュートリアルでは、を使用してGoogle Ads ベース接続を作成しました [!DNL Flow Service] API です。 このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [を使用した、広告データを Platform に取り込むデータフローの作成 [!DNL Flow Service] API](../../collect/advertising.md)
