---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: フローサービスAPIを使用したSalesforceサービスクラウドコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: cada7c7eff7597015caa7333559bef16a59eab65
workflow-type: tm+mt
source-wordcount: '703'
ht-degree: 1%

---


# フローサービスAPIを使用したSalesforceサービスクラウドコネクタの作成

>[!NOTE]
>Salesforce Service Cloudコネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

フローサービスは、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集および一元化するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、Flow Service APIを使用して、Experience PlatformをSalesforce Service Cloud（以下「SSC」と呼ぶ）に接続する手順を順を追って説明します。

## はじめに

このガイドでは、次のAdobe Experience Platformのコンポーネントについて、十分に理解している必要があります。

* [ソース](../../../../home.md): Experience Platformを使用すると、Platformサービスを使用して、様々なソースからデータを取り込み、データの構造、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): Experience Platformは、1つのPlatformインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

Flow Service APIを使用してSSCに正常に接続するために知っておく必要がある追加情報については、以下の節で説明します。

### 必要な資格情報の収集

フローサービスがSSCに接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `username` | ユーザーアカウントのユーザー名です。 |
| `password` | ユーザーアカウントのパスワードです。 |
| `securityToken` | ユーザーアカウントのセキュリティトークン。 |

使い始める前に詳しくは、 [このSalesforce Service Cloudドキュメントを参照してください](https://developer.salesforce.com/docs/atlas.en-us.api_iot.meta/api_iot/qs_auth_access_token.htm)。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例 [の読み方に関する節](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照してください。

### 必要なヘッダーの値の収集

PlatformAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../../../../tutorials/authentication.md)。 次に示すように、Experience PlatformAPIのすべての呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

フローサービスに属するリソースを含む、Experience Platform内のすべてのリソースは、特定の仮想サンドボックスに分離されます。 PlatformAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* Content-Type: `application/json`

## 接続仕様の検索

SSC接続を作成するには、一連のSSC接続仕様がフローサービス内に存在する必要があります。 PlatformをSSCに接続する最初の手順は、これらの仕様を取得することです。

**API形式**

使用可能な各ソースには、認証要件などのコネクタプロパティを記述するための固有の接続仕様のセットがあります。 GET要求をエンドポイントに送信すると、使用可能なすべてのソースの接続指定が返され `/connectionSpecs` ます。 また、SSC専用の情報を取得す `property=name=="salesforce-service-cloud"` るクエリを含めることもできます。

```http
GET /connectionSpecs
GET /connectionSpecs?property=name=="salesforce-service-cloud"
```

**リクエスト**

次のリクエストは、SSCの接続仕様を取得します。

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs?property=name=="mysql"' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常に応答すると、固有な識別子(`id`)を含むSSCの接続仕様が返されます。 このIDは、次のベース接続を作成する手順で必要になります。

```json
{
    "items": [
        {
            "id": "cb66ab34-8619-49cb-96d1-39b37ede86ea",
            "name": "salesforce-service-cloud",
            "providerId": "0ed90a81-07f4-4586-8190-b40eccef1c5a",
            "version": "1.0",
            "authSpec": [
                {
                    "name": "Basic Authentication",
                    "type": "BasicAuthentication",
                    "spec": {
                        "$schema": "http://json-schema.org/draft-07/schema#",
                        "type": "object",
                        "description": "defines auth params",
                        "properties": {
                            "environmentUrl": {
                                "type": "string",
                                "description": "URL of the source instance"
                            },
                            "username": {
                                "type": "string",
                                "description": "User name for the user account"
                            },
                            "password": {
                                "type": "string",
                                "description": "Password for the user account",
                                "format": "password"
                            },
                            "securityToken": {
                                "type": "string",
                                "description": "Security token for the user account",
                                "format": "password"
                            }
                        },
                        "required": [
                            "username",
                            "password",
                            "securityToken"
                        ]
                    }
                }
            ],
        }
    ]
}
```

## ベース接続を作成する

ベース接続はソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できるので、SSCアカウントごとに1つのベース接続が必要です。

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
        "name": "Base connection for salesforce service cloud",
        "description": "Base connection for salesforce service cloud",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                "username": "{USERNAME}",
                "password": "{PASSWORD}",
                "securityToken": "{SECURITY_TOKEN}"
            }
        },
        "connectionSpec": {
            "id": "b66ab34-8619-49cb-96d1-39b37ede86ea",
            "version": "1.0"
        }
    }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.username` | SSCアカウントに関連付けられているユーザ名。 |
| `auth.params.password` | SSCアカウントに関連付けられているパスワード。 |
| `auth.params.securityToken` | SSCアカウントに関連付けられているセキュリティトークン。 |
| `connectionSpec.id` | 前の手順で取得したSSCアカウント `id` の接続仕様。 |

**応答**

正常な応答は、新たに作成されたベース接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "4267c2ab-2104-474f-a7c2-ab2104d74f86",
    "etag": "\"0200f1c5-0000-0200-0000-5e4352bf0000\""
}
```

## 次の手順

このチュートリアルに従うことで、Flow Service APIを使用してSSCベースの接続を作成し、接続の一意のID値を取得しました。 Flow Service APIを使用して顧客の成功システムを [調査する方法について学習する際に、次のチュートリアルでこの基本接続IDを使用できます](../../explore/customer-success.md)。
