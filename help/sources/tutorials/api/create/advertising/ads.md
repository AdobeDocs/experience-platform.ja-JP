---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: フローサービスAPIを使用したGoogle広告コネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: 950fa88ed6c9235bff98658763b662113bb76caa

---


# フローサービスAPIを使用したGoogle広告コネクタの作成

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、サポートされるすべてのソースを接続できるユーザーインターフェイスとRESTful APIを提供します。

このチュートリアルでは、フローサービスAPIを使用して、Experience PlatformをGoogle広告に接続する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントに関する作業を理解している必要があります。

* [資料](../../../../home.md):エクスペリエンスプラットフォームを使用すると、様々なソースからデータを取り込みながら、プラットフォームサービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

次の節では、フローサービスAPIを使用して広告に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

フローサービスが広告と接続するには、次の接続プロパティの値を指定する必要があります。

| **資格情報** | **説明** |
| -------------- | --------------- |
| 顧客ID | 広告アカウントのクライアント顧客ID。 |
| 開発者トークン | マネージャーアカウントに関連付けられている開発者トークン。 |
| 更新トークン | 広告へのアクセスを承認するためにGoogleから取得した更新トークン。 |
| クライアントID | 更新トークンの取得に使用されるGoogleアプリケーションのクライアントID。 |
| クライアントシークレット | 更新トークンの取得に使用されるGoogleアプリケーションのクライアントシークレット。 |
| 接続指定ID | 接続の作成に必要な一意の識別子。 Google広告の接続指定IDは次のとおりです。 `d771e9c1-4f26-40dc-8617-ce58c4b53702` |

これらの値の詳細については、この [Google広告のドキュメント](https://developers.google.com/adwords/api/docs/guides/authentication)。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を示します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../../../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

* 認証：無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

フローサービスに属するリソースを含む、エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* コンテンツタイプ： `application/json`

## 接続の作成

接続はソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するのに使用できるので、Google Adのアカウントごとに必要な接続は1つだけです。

**API形式**

```https
POST /connections
```

**リクエスト**

Google Ads接続を作成するには、POSTリクエストの一部として一意の接続指定IDを指定する必要があります。 Google広告の接続指定IDはです `221c7626-58f6-4eec-8ee2-042b0226f03b`。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "google-ads connection",
        "description": "Connection for google-ads",
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
| `auth.params.clientCustomerID` | 広告アカウントのクライアント顧客ID。 |
| `auth.params.developerToken` | 広告アカウントの開発者トークン。 |
| `auth.params.refreshToken` | 広告アカウントの更新トークン。 |
| `auth.params.clientID` | 広告アカウントのクライアントID。 |
| `auth.params.clientSecret` | 広告アカウントのクライアントシークレット。 |
| `connectionSpec.id` | Google Ads接続仕様ID: `d771e9c1-4f26-40dc-8617-ce58c4b53702`. |

**応答**

成功した応答は、新しく作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2484f2df-c057-4ab5-84f2-dfc0577ab592",
    "etag": "\"10033e77-0000-0200-0000-5e96785b0000\""
}
```

## 次の手順

このチュートリアルに従うと、フローサービスAPIを使用してGoogle広告接続を作成し、接続の一意のID値を取得したことになります。 このIDは、次のチュートリアルで、Flow Service APIを使用して広告システムを調 [査する方法を学ぶ際に使用できます](../../explore/advertising.md)。
