---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: フローサービスAPIを使用したSalesforceコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: cc999ce1ab426f412c0cc2b69173a336a14024f3
workflow-type: tm+mt
source-wordcount: '734'
ht-degree: 1%

---


# フローサービスAPIを使用したSalesforceコネクタの作成

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元管理するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、フローサービスAPIを使用して、CRMデータを収集するためのSalesforceアカウントにプラットフォームを接続する手順を説明します。

エクスペリエンスプラットフォームでユーザーインターフェイスを使用したい場合は、 [DynamicsまたはSalesforceソースコネクタのUIチュートリアル](../../../ui/create/crm/dynamics-salesforce.md) に、同様の操作を実行するための手順が順を追って説明されています。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [ソース](../../../../home.md): Experience Platformを使用すると、様々なソースからデータを取り込むと同時に、プラットフォームサービスを使用して、入力データの構造、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): Experience Platformは、1つのプラットフォームインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

Flow Service APIを使用してPlatformをSalesforceアカウントに正しく接続するために知っておく必要がある追加情報については、以下の節に説明します。

### 必要な資格情報の収集

フローサービスがSalesforceに接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `environmentUrl` | SalesforceソースインスタンスのURL。 |
| `username` | Salesforceユーザーアカウントのユーザー名。 |
| `password` | Salesforceユーザーアカウントのパスワード。 |
| `securityToken` | Salesforceユーザーアカウントのセキュリティトークン。 |

開始方法の詳細については、 [このSalesforceドキュメントを参照してください](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_authentication.htm)。

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

## 接続仕様の検索

プラットフォームをSalesforceアカウントに接続する前に、Salesforce用の接続仕様が存在することを確認する必要があります。 接続仕様が存在しない場合は、接続を確立できません。

使用可能な各ソースには、認証要件などのコネクタプロパティを記述するための固有の接続仕様のセットがあります。 GETリクエストを実行し、クエリパラメータを使用して、Salesforceの接続仕様を検索できます。

**API形式**

クエリパラメータを指定せずにGET要求を送信すると、使用可能なすべてのソースの接続仕様が返されます。 このクエリを含めて、Salesforce専用 `property=name=="salesforce"` の情報を取得できます。

```http
GET /connectionSpecs
GET /connectionSpecs?property=name=="salesforce"
```

**リクエスト**

次のリクエストは、Salesforceの接続仕様を取得します。

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs?property=name==%22salesforce%22' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常に応答すると、固有の識別子(`id`)を含むSalesforceの接続仕様が返されます。 このIDは、次の手順でベース接続を作成する際に必要となります。

```json
{
    "items": [
        {
            "id": "cfc0fee1-7dc0-40ef-b73e-d8b134c436f5",
            "name": "salesforce",
            "providerId": "0ed90a81-07f4-4586-8190-b40eccef1c5a",
            "version": "1.0",
            "authSpec": [
                {
                    "name": "Basic Authentication",
                    "type": "Basic_Authentication",
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
            ]
        }
    ]
}
```

## ベース接続を作成する

ベース接続はソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するのに使用できるので、Salesforceアカウントごとに1つのベース接続が必要です。

次のPOSTリクエストを実行して、ベース接続を作成します。

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
        "name": "Salesforce Base Connection",
        "description": "Base connection for Salesforce account",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                "username": "{USERNAME}",
                "password": "{PASSWORD}",
                "securityToken": "{SECURITY_TOKEN}"
            }
        },
        "connectionSpec": {
            "id": "cfc0fee1-7dc0-40ef-b73e-d8b134c436f5",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.username` | Salesforceアカウントに関連付けられているユーザ名。 |
| `auth.params.password` | Salesforceアカウントに関連付けられているパスワード。 |
| `auth.params.securityToken` | Salesforceアカウントに関連付けられているセキュリティトークン。 |
| `connectionSpec.id` | 前の手順で取得 `id` したSalesforceアカウントの接続仕様。 |

**応答**

成功した応答には、ベース接続の固有な識別子(`id`)が含まれます。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700df7b-0000-0200-0000-5e3b424f0000\""
}
```

## 次の手順

このチュートリアルに従うと、APIを使用してSalesforceアカウントの基本接続を作成し、一意のIDを応答本文の一部として取得できます。 Flow Service APIを使用してCRMシステムを [調査する方法について学習する際に、次のチュートリアルでこの基本接続IDを使用できます](../../explore/crm.md)。