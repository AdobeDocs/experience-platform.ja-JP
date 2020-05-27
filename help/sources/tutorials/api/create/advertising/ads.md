---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用してGoogle AdWordsコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 00f785577999d2ec3147a3cc2b8edd1028be2471
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 1%

---


# Flow Service APIを使用してGoogle AdWordsコネクタを作成する

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元管理するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、Flow Service APIを使用して、Experience PlatformをGoogle AdWordsに接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [ソース](../../../../home.md): Experience Platformを使用すると、様々なソースからデータを取り込むと同時に、プラットフォームサービスを使用して、入力データの構造、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): Experience Platformは、1つのプラットフォームインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

Flow Service APIを使用してAdに正常に接続するために知っておく必要がある追加情報については、以下の節に説明します。

### 必要な資格情報の収集

フローサービスがAdWordsと接続するには、次の接続プロパティの値を指定する必要があります。

| **Credential** | **説明** |
| -------------- | --------------- |
| 顧客ID | AdWordsアカウントのクライアント顧客ID。 |
| 開発者トークン | マネージャーアカウントに関連付けられている開発者トークン。 |
| 更新トークン | AdWordsへのアクセスを承認するためにGoogleから取得した更新トークン。 |
| クライアントID | 更新トークンの取得に使用されるGoogleアプリケーションのクライアントID。 |
| クライアントシークレット | 更新トークンの取得に使用されるGoogleアプリケーションのクライアントシークレット。 |
| 接続指定ID | 接続を作成するために必要な一意の識別子。 Google AdWordsの接続仕様IDは次のとおりです。 `d771e9c1-4f26-40dc-8617-ce58c4b53702` |

これらの値の詳細については、この [Google AdWordsドキュメントを参照してください](https://developers.google.com/adwords/api/docs/guides/authentication)。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request) 。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../../../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

Experience Platformのすべてのリソース（Flow Serviceに属するリソースを含む）は、特定の仮想サンドボックスに分離されています。 プラットフォームAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* Content-Type: `application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できるので、Google AdWordsアカウントごとに必要な接続は1つだけです。

**API形式**

```https
POST /connections
```

**リクエスト**

Google AdWords接続を作成するには、その一意の接続指定IDをPOSTリクエストの一部として指定する必要があります。 Google AdWordsの接続仕様IDはで `221c7626-58f6-4eec-8ee2-042b0226f03b`す。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "google-AdWords connection",
        "description": "Connection for google-AdWords",
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
| `auth.params.clientCustomerID` | AdWordsアカウントのクライアント顧客ID。 |
| `auth.params.developerToken` | AdWordsアカウントの開発者トークン。 |
| `auth.params.refreshToken` | AdWordsアカウントの更新トークン。 |
| `auth.params.clientID` | AdWordsアカウントのクライアントID。 |
| `auth.params.clientSecret` | AdWordsアカウントのクライアントシークレット。 |
| `connectionSpec.id` | Google AdWords接続仕様ID: `d771e9c1-4f26-40dc-8617-ce58c4b53702`. |

**応答**

正常な応答は、新たに作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2484f2df-c057-4ab5-84f2-dfc0577ab592",
    "etag": "\"10033e77-0000-0200-0000-5e96785b0000\""
}
```

## 次の手順

このチュートリアルに従うと、Flow Service APIを使用してGoogle AdWords接続を作成し、接続の一意のID値を取得したことになります。 このIDは、Flow Service APIを使用して広告システムを [調査する方法を学ぶ際に、次のチュートリアルで使用できます](../../explore/advertising.md)。
