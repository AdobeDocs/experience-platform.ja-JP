---
keywords: Experience Platform；ホーム；人気のトピック；google 広告；Google広告；google 広告；広告
title: フローサービス API を使用したGoogle Ads ベース接続の作成
description: フローサービス API を使用してAdobe Experience PlatformをGoogle広告に接続する方法を説明します。
exl-id: 4658e392-1bd9-4e74-aa05-96109f9b62a0
source-git-commit: 56419f41188c9bfdbeda7dde680f269b980a37f0
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 23%

---

# を使用してGoogle Ads ベース接続を作成する [!DNL Flow Service] API

>[!NOTE]
>
>Google Ads ソースはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Experience Platformサービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md):Experience Platformは、単一のExperience Platformインスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、 [!DNL Flow Service] API

### 必要な認証情報の収集

次のために [!DNL Flow Service] Google Ads と接続するには、次の接続プロパティの値を指定する必要があります。

| 認証情報 | 説明 |
| ---------- | ----------- |
| `clientCustomerId` | クライアント顧客 ID は、Google Ads API で管理するGoogle Ads クライアントアカウントに対応するアカウント番号です。 この ID は、 `123-456-7890`. |
| `developerToken` | 開発者トークンを使用すると、Google Ads API にアクセスできます。 同じ開発者トークンを使用して、すべてのGoogle Ads アカウントに対してリクエストを実行できます。 次の方法で開発者トークンを取得します： [マネージャーアカウントへのログイン](https://ads.google.com/home/tools/manager-accounts/) 次に、 [!DNL API Center] ページ。 |
| `refreshToken` | 更新トークンは、 [!DNL OAuth2] 認証。 このトークンを使用すると、期限切れになったアクセストークンを再生成できます。 |
| `clientId` | クライアント ID は、 [!DNL OAuth2] 認証。 クライアント ID とクライアント秘密鍵を組み合わせることで、Googleへのアプリケーションを識別し、アカウントに代わってアプリケーションを操作できます。 |
| `clientSecret` | クライアントシークレットは、 [!DNL OAuth2] 認証。 クライアント ID とクライアント秘密鍵を組み合わせることで、Googleへのアプリケーションを識別し、アカウントに代わってアプリケーションを操作できます。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 Google Ads の接続仕様 ID は次のとおりです。 `d771e9c1-4f26-40dc-8617-ce58c4b53702`. |

API の概要に関するドキュメント ( [Google Ads の概要の詳細](https://developers.google.com/google-ads/api/docs/first-call/overview).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、 `/connections` エンドポイントを使用して、Google Ads 認証資格情報をリクエストパラメーターの一部として提供する際に使用されます。

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
| `auth.params.clientCustomerID` | Google Ads アカウントのクライアント顧客 ID。 |
| `auth.params.developerToken` | Google Ads アカウントのデベロッパートークン。 |
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

このチュートリアルに従って、Google Ads のベース接続を [!DNL Flow Service] API このベース接続 ID は、次のチュートリアルで使用できます。

* [を使用してデータテーブルの構造と内容を調べる [!DNL Flow Service] API](../../explore/tabular.md)
* [データフローを作成し、 [!DNL Flow Service] API](../../collect/advertising.md)
