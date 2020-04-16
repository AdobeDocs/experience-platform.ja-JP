---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: フローサービスAPIを使用したHubSpotコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: c08650ca1655e248f0a0f8a6b371c5fd005aab1c

---


# フローサービスAPIを使用したHubSpotコネクタの作成

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、サポートされるすべてのソースを接続できるユーザーインターフェイスとRESTful APIを提供します。

このチュートリアルでは、Flow Service APIを使用して、Experience PlatformをHubSpotに接続する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントに関する作業を理解している必要があります。

* [資料](../../../../home.md):エクスペリエンスプラットフォームを使用すると、様々なソースからデータを取り込みながら、プラットフォームサービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

次の節では、Flow Service APIを使用してHubSpotに正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

フローサービスがHubSpotと接続するには、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| クライアントID | HubSpotアプリケーションに関連付けられているクライアントID。 |
| クライアントシークレット | HubSpotアプリケーションに関連付けられているクライアントシークレット。 |
| アクセストークン | アクセストークンは、OAuth統合を最初に認証したときに取得されます。 |
| 更新トークン | OAuth統合を最初に認証する際に取得される更新トークン。 |
| 接続指定ID | 接続の作成に必要な一意の識別子。 HubSpotの接続指定ID: `cc6a4487-9e91-433e-a3a3-9cf6626c1806` |

使い始める前に、このHubSpotドキュメント [](https://developers.hubspot.com/docs/methods/oauth2/oauth2-overview)。

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

接続はソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するために使用できる接続は、HubSpotアカウントごとに1つだけ必要です。

**API形式**

```https
POST /connections
```

**リクエスト**

HubSpot接続を作成するには、POST要求の一部として一意の接続指定IDを指定する必要があります。 HubSpotの接続指定IDはです `cc6a4487-9e91-433e-a3a3-9cf6626c1806`。

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
| `auth.params.accessToken` | アクセストークンは、OAuth統合を最初に認証したときに取得されます。 |
| `auth.params.refreshToken` | OAuth統合を最初に認証する際に取得される更新トークン。 |

**応答**

成功した応答は、一意の識別子(`id`)を含む、API用に新しく作成された接続の詳細を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

このチュートリアルに従うと、フローサービスAPIを使用してHubSpot接続を作成し、接続の一意のID値を取得したことになります。 この接続IDは、次のチュートリアルで、Flow Service APIを使用してマーケティング自動化シ [ステムを調査する方法を学ぶ際に使用できます](../../explore/marketing-automation.md)。