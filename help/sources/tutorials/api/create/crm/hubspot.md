---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用してHubSpotコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 5839e4695589455bd32b6e3e33a7c377343f920d
workflow-type: tm+mt
source-wordcount: '640'
ht-degree: 1%

---


# APIを使用して [!DNL HubSpot][!DNL Flow Service] コネクタを作成する

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、 [!DNL Flow Service] APIを使用して、に接続する手順を順を追って説明 [!DNL Experience Platform] し [!DNL HubSpot]ます。

## はじめに

このガイドでは、次のAdobe Experience Platformのコンポーネントについて、十分に理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、 [!DNL HubSpot][!DNL Flow Service] APIを使用してに正常に接続するために知っておく必要がある追加情報について説明します。

### 必要な資格情報の収集

と接続 [!DNL Flow Service] するには、次の接続プロパティを指定する必要があ [!DNL HubSpot]ります。

| Credential | 説明 |
| ---------- | ----------- |
| `clientId` | アプリケーションに関連付けられているクライアントID [!DNL HubSpot] 。 |
| `clientSecret` | アプリケーションに関連付けられているクライアントシークレット [!DNL HubSpot] です。 |
| `accessToken` | OAuth統合を最初に認証する際に取得されるアクセストークン。 |
| `refreshToken` | OAuth統合を最初に認証する際に取得される更新トークン。 |

使い始める前に、このHubSpotドキュメントを参照して [ください](https://developers.hubspot.com/docs/methods/oauth2/oauth2-overview)。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

### 必要なヘッダーの値の収集

APIを呼び出すには、まず [!DNL Platform] 認証チュートリアルを完了する必要があり [ます](../../../../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

に属するリソース [!DNL Experience Platform]を含む、のすべてのリソースは、特定の仮想サンドボックスに分離され [!DNL Flow Service]ます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* Content-Type: `application/json`

## 接続仕様の検索

接続を作成するには、 [!DNL HubSpot] 接続仕様のセットがに存在する必要があり [!DNL HubSpot][!DNL Flow Service]ます。 に接続する最初の手順 [!DNL Platform] は、これらの仕様 [!DNL HubSpot] を取得することです。

**API形式**

使用可能な各ソースには、認証要件などのコネクタプロパティを記述するための固有の接続仕様のセットがあります。 GET要求をエンドポイントに送信すると、使用可能なすべてのソースの接続指定が返され `/connectionSpecs` ます。 特別な情報を取得するクエリ `property=name=="hubspot"` を含めることもでき [!DNL HubSpot]ます。

```http
GET /connectionSpecs
GET /connectionSpecs?property=name=="hubspot"
```

**リクエスト**

次のリクエストは、の接続仕様を取得し [!DNL HubSpot]ます。

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs?property=name=="hubspot"' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常に応答すると、固有な識別子( [!DNL HubSpot]`id`)を含む、の接続仕様が返されます。 このIDは、次の手順でAPIの接続を作成する際に必要です。

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

APIの接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するために使用できるので、APIは [!DNL HubSpot] アカウントごとに1つの接続のみ必要です。

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
| `auth.params.clientId` | アプリケーションに関連付けられているクライアントID [!DNL HubSpot] 。 |
| `auth.params.clientSecret` | アプリケーションに関連付けられているクライアントシークレット [!DNL HubSpot] です。 |
| `auth.params.accessToken` | OAuth統合を最初に認証する際に取得されるアクセストークン。 |
| `auth.params.refreshToken` | OAuth統合を最初に認証する際に取得される更新トークン。 |

**応答**

正常な応答は、新たに作成されたAPI用の接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2eb9c78b-e8b8-4400-b9c7-8be8b86400b2",
    "etag": "\"05026cf5-0000-0200-0000-5e4c42920000\""
}
```

このチュートリアルに従うことで、 [!DNL HubSpot] APIを使用して [!DNL Flow Service] 接続を作成し、接続の一意のID値を取得しました。 Flow Service APIを使用してCRMシステムを [調査する方法について学習する際に、次のチュートリアルでこの接続IDを使用できます](../../explore/crm.md)。