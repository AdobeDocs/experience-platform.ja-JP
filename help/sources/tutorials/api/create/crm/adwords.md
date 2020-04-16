---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: フローサービスAPIを使用してGoogle AdWordsコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: a456dce58c32348c9e680cd672b71dd7bbdc1a04

---


# フローサービスAPIを使用してGoogle AdWordsコネクタを作成する

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、サポートされるすべてのソースを接続できるユーザーインターフェイスとRESTful APIを提供します。

このチュートリアルでは、Flow Service APIを使用して、Experience PlatformをGoogle AdWords（以下「AdWords」と呼ばれる）に接続する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントに関する作業を理解している必要があります。

* [資料](../../../../home.md):エクスペリエンスプラットフォームを使用すると、様々なソースからデータを取り込みながら、プラットフォームサービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

次の節では、フローサービスAPIを使用してAdWordsに正常に接続するために知っておく必要のある追加情報を示します。

### 必要な資格情報の収集

フローサービスがAdWordsと接続するには、次の接続プロパティの値を指定する必要があります。

| **資格情報** | **説明** |
| -------------- | --------------- |
| `clientCustomerID` | Google AdWordsターゲットに関連付けられた顧客ID | account. |
| `developerToken` | AdWords API開発者を識別するために使用する文字列。 |
| `refreshToken` | AdWordsへのアクセスを承認するために使用される、Googleから取得された更新トークン。 |
| `clientID` | 更新トークンの生成に使用するアプリケーションのID。 |
| `clientSecret` | 更新トークンの生成に使用するアプリケーションのクライアントシークレット。 |

これらの値の詳細については、この [Google AdWordsドキュメント](https://developers.google.com/adwords/api/docs/guides/authentication)。

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

AdWords接続を作成するには、一連のAdWords接続指定がフローサービス内に存在する必要があります。 PlatformをAdWordsに接続する最初の手順は、これらの仕様を取得することです。

**API形式**

使用可能な各ソースには、認証要件などのコネクタプロパティを記述するための固有の接続仕様のセットがあります。 GETリクエストを実行し、接続パラメーターを使用して、AdWordsの接続指定を検索することがクエリできます。

GETリクエストをクエリパラメータなしで送信すると、使用可能なすべてのソースの接続指定が返されます。 この情報を含めて、AdWords専用のクエリ `property=name=="google-adwords"` を取得することができます。

```http
GET /connectionSpecs
GET /connectionSpecs?property=name=="google-adwords"
```

**リクエスト**

次のリクエストは、AdWordsの接続指定を取得します。

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs?{QUERY_PARAMS}' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、一意の識別子(`id`)を含むAdWordsの接続指定を返します。 このIDは、次の手順でベース接続を作成する際に必要です。

```json
{
    "items": [
        {
            "id": "d771e9c1-4f26-40dc-8617-ce58c4b53702",
            "name": "google-adwords",
            "providerId": "0ed90a81-07f4-4586-8190-b40eccef1c5a",
            "version": "1.0",
            "authSpec": [
                {
                    "name": "Basic Authentication for google-adwords",
                    "type": "Basic_Authentication",
                    "spec": {
                        "$schema": "http://json-schema.org/draft-07/schema#",
                        "type": "object",
                        "description": "defines auth params",
                        "properties": {
                            "clientCustomerID": {
                                "type": "string",
                                "description": "The Client customer ID of the AdWords account"
                            },
                            "developerToken": {
                                "type": "string",
                                "description": "The developer token associated with the manager account",
                                "format": "password"
                            },
                            "refreshToken": {
                                "type": "string",
                                "description": "The refresh token obtained from Google for authorizing access to AdWords",
                                "format": "password"
                            },
                            "clientId": {
                                "type": "string",
                                "description": "The client ID of the Google application used to acquire the refresh token"
                            },
                            "clientSecret": {
                                "type": "string",
                                "description": "The client secret of the google application used to acquire the refresh token",
                                "format": "password"
                            }
                        },
                        "required": [
                            "clientCustomerID",
                            "developerToken",
                            "refreshToken",
                            "clientId",
                            "clientSecret"
                        ]
                    }
                }
            ],
        }
    ]
}
```

## ベース接続の作成

ベース接続はソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するのに使用できるAdWordsアカウントごとに1つのベース接続が必要です。

**API形式**

```http
POST /connections
```

**リクエスト**

```shell
curl -X POST \
    'http://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "google-adwords base connection",
        "description": "Base connection for google-adwords",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                "clientCustomerID": "{CLIENT_CUSTOMER_ID}",
                "developerToken": "{DEVELOPER_TOKEN}",
                "clientId": "{CLIENT_ID}",
                "clientSecret": "{CLIENT_SECRET}",
                "refreshToken": "{REFRESH_TOKEN}"
            }
        },
        "connectionSpec": {
            "id": "d771e9c1-4f26-40dc-8617-ce58c4b53702",
            "version": "1.0"
        }
    }
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.clientCustomerID` | AdWordsアカウントのクライアント顧客ID。 |
| `auth.params.developerToken` | AdWordsアカウントの開発者トークン。 |
| `auth.params.refreshToken` | AdWordsアカウントの更新トークン。 |
| `auth.params.clientID` | AdWordsアカウントのクライアントID。 |
| `auth.params.clientSecret` | AdWordsアカウントのクライアントシークレット。 |
| `connectionSpec.id` | 前の手順で取 `id` 得したAdWordsアカウントの接続指定。 |

**応答**

成功した応答は、新たに作成されたベース接続`id`の一意の識別子()を返します。 このIDは、次のチュートリアルでAdWordsデータを調べるために必要です。

```json
{
    "id": "e6ce1eab-416b-4a20-8e1e-ab416b1a206f",
    "etag": "\"8e0052a2-0000-0200-0000-5e25fb330000\""
}
```

## 次の手順

このチュートリアルに従うと、フローサービスAPIを使用してAdWordsベースの接続を作成し、接続の一意のID値を取得したことになります。 この基本接続IDは、次のチュートリアルでフローサービスAPIを使用してCRMシ [ステムを調査する方法を学ぶ際に使用できます](../../explore/crm.md)。
