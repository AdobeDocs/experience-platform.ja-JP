---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: フローサービスAPIを使用したHubSpotコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: 7aa6f85308bacb275bd6f3234d03530a621c1c02

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
| `clientId` | HubSpotアプリケーションに関連付けられているクライアントID。 |
| `clientSecret` | HubSpotアプリケーションに関連付けられているクライアントシークレット。 |
| `accessToken` | アクセストークンは、OAuth統合を最初に認証したときに取得されます。 |
| `refreshToken` | OAuth統合を最初に認証する際に取得される更新トークン。 |

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

## 接続仕様の検索

HubSpot接続を作成するには、一連のHubSpot接続仕様がフローサービス内に存在する必要があります。 プラットフォームをHubSpotに接続する最初の手順は、これらの仕様を取得することです。

**API形式**

使用可能な各ソースには、認証要件などのコネクタプロパティを記述するための固有の接続仕様のセットがあります。 GET要求をエンドポイントに送信すると、 `/connectionSpecs` 使用可能なすべてのソースの接続指定が返されます。 また、この情報を含めて、HubSpot専用のクエリ `property=name=="hubspot"` を取得することもできます。

```http
GET /connectionSpecs
GET /connectionSpecs?property=name=="hubspot"
```

**リクエスト**

次のリクエストは、HubSpotの接続仕様を取得します。

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs?property=name=="hubspot"' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、一意の識別子(`id`)を含むHubSpotの接続仕様を返します。 このIDは、次の手順でAPIの接続を作成する必要があります。

```json
{
    "items": [
        {
            "id": "cc6a4487-9e91-433e-a3a3-9cf6626c1806",
            "name": "hubspot",
            "providerId": "0ed90a81-07f4-4586-8190-b40eccef1c5a",
            "version": "1.0",
            "authSpec": [
                {
                    "name": "Basic Authentication",
                    "spec": {
                        "$schema": "http://json-schema.org/draft-07/schema#",
                        "type": "object",
                        "description": "defines auth params required for connecting to HubSpot",
                        "properties": {
                            "clientId": {
                                "type": "string",
                                "description": "The client ID associated with your HubSpot application."
                            },
                            "clientSecret": {
                                "type": "string",
                                "description": "The client secret associated with your HubSpot application.",
                                "format": "password"
                            },
                            "accessToken": {
                                "type": "string",
                                "description": "The access token obtained when initially authenticating your OAuth integration.",
                                "format": "password"
                            },
                            "refreshToken": {
                                "type": "string",
                                "description": "The refresh token obtained when initially authenticating your OAuth integration.",
                                "format": "password"
                            }
                        },
                        "required": [
                            "clientId",
                            "clientSecret",
                            "accessToken",
                            "refreshToken"
                        ]
                    }
                }
            ],
        }
    ]
}
```

## API用の接続の作成

APIの接続は、ソースを指定し、そのソースの資格情報を含みます。 APIは、異なるデータを取り込むために複数のソースコネクタを作成するのに使用できるので、HubSpotアカウントごとに1つの接続のみ必要です。

**API形式**

```http
POST /connections
```

**リクエスト**

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
    "id": "2eb9c78b-e8b8-4400-b9c7-8be8b86400b2",
    "etag": "\"05026cf5-0000-0200-0000-5e4c42920000\""
}
```

このチュートリアルに従うと、フローサービスAPIを使用してHubSpot接続を作成し、接続の一意のID値を取得したことになります。 この接続IDは、次のチュートリアルで、フローサービスAPIを使用してCRMシ [ステムを調査する方法を学ぶ際に使用できます](../../explore/crm.md)。