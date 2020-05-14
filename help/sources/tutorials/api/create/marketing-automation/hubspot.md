---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用してHubSpotコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 37a5f035023cee1fc2408846fb37d64b9a3fc4b6
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 1%

---


# Flow Service APIを使用してHubSpotコネクタを作成する

>[!NOTE]
>HubSpotコネクタはベータ版です。 機能とドキュメントは、変更されることがあります。

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元管理するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、Flow Service APIを使用して、Experience PlatformをHubSpotに接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [ソース](../../../../home.md): Experience Platformを使用すると、様々なソースからデータを取り込むと同時に、プラットフォームサービスを使用して、入力データの構造、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): Experience Platformは、1つのプラットフォームインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

Flow Service APIを使用してHubSpotに正常に接続するために必要な追加情報については、以下の節で説明します。

### 必要な資格情報の収集

フローサービスがHubSpotと接続するには、次の接続プロパティを指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| クライアントID | HubSpotアプリケーションに関連付けられているクライアントID。 |
| クライアントシークレット | HubSpotアプリケーションに関連付けられているクライアントシークレット。 |
| アクセストークン | OAuth統合を最初に認証する際に取得されるアクセストークン。 |
| 更新トークン | OAuth統合を最初に認証する際に取得される更新トークン。 |
| 接続指定ID | 接続を作成するために必要な一意の識別子。 HubSpotの接続仕様ID: `cc6a4487-9e91-433e-a3a3-9cf6626c1806` |

使い始める前に、このHubSpotドキュメントを参照して [ください](https://developers.hubspot.com/docs/methods/oauth2/oauth2-overview)。

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

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できるため、HubSpotアカウントごとに必要な接続は1つだけです。

**API形式**

```https
POST /connections
```

**リクエスト**

HubSpot接続を作成するには、POST要求の一部として一意の接続指定IDを指定する必要があります。 HubSpotの接続仕様IDはで `cc6a4487-9e91-433e-a3a3-9cf6626c1806`す。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "connection for hubspot",
        "description": "connection for hubspot",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                "clientId": "{CLIENT_ID}",
                "clientSecret": "{CLIENT_SECRET}",
                "accessToken": "{ACCESS_TOKEN}",
                "refreshToken": "{REFRESH_TOKEN}"
            }
        },
        "connectionSpec": {
            "id": "cc6a4487-9e91-433e-a3a3-9cf6626c1806",
            "version": "1.0"
        }
    }
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.clientId` | HubSpotアプリケーションに関連付けられているクライアントID。 |
| `auth.params.clientSecret` | HubSpotアプリケーションに関連付けられているクライアントシークレット。 |
| `auth.params.accessToken` | OAuth統合を最初に認証する際に取得されるアクセストークン。 |
| `auth.params.refreshToken` | OAuth統合を最初に認証する際に取得される更新トークン。 |

**応答**

正常な応答は、新たに作成されたAPI用の接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

このチュートリアルに従うことで、Flow Service APIを使用してHubSpot接続を作成し、接続の一意のID値を取得しました。 Flow Service APIを使用してマーケティング自動化システムを [調査する方法について学習する際に、次のチュートリアルでこの接続IDを使用できます](../../explore/marketing-automation.md)。