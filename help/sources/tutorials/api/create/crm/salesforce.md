---
keywords: Experience Platform;home;popular topics;Salesforce;salesforce
solution: Experience Platform
title: フローサービスAPIを使用したSalesforceコネクタの作成
topic: overview
type: Tutorial
description: このチュートリアルでは、フローサービスAPIを使用して、CRMデータを収集するためのSalesforceアカウントにプラットフォームを接続する手順を説明します。
translation-type: tm+mt
source-git-commit: d332226541685108b58d88096146ed6048606774
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 18%

---


# APIを使用して [!DNL Salesforce][!DNL Flow Service] コネクタを作成する

フローサービスは、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元管理するために使用されます。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、 [!DNL Flow Service] APIを使用して、CRMデータを収集するための [!DNL Platform][!DNL Salesforce] アカウントに接続する手順を順を追って説明します。

でユーザインターフェイスを使用したい場合は、 [!DNL Experience Platform]SalesforceソースコネクタUIチュートリアル [](../../../ui/create/crm/salesforce.md) に、同様の操作を実行するための手順が順を追って記載されています。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

The following sections provide additional information that you will need to know in order to successfully connect [!DNL Platform] to a [!DNL Salesforce] account using the [!DNL Flow Service] API.

### 必要な資格情報の収集

に接続 [!DNL Flow Service] するに [!DNL Salesforce]は、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `environmentUrl` | The URL of the [!DNL Salesforce] source instance. |
| `username` | ユーザーアカウントのユー [!DNL Salesforce] ザー名。 |
| `password` | ユーザーアカウントのパス [!DNL Salesforce] ワードです。 |
| `securityToken` | ユーザーアカウントのセキュリティト [!DNL Salesforce] ークンです。 |

開始方法の詳細については、 [このSalesforceドキュメントを参照してください](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_authentication.htm)。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../../../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

All resources in [!DNL Experience Platform], including those belonging to the [!DNL Flow Service], are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* Content-Type: `application/json`

## 接続仕様の検索

アカウント [!DNL Platform] に接続する前に、の接続仕様が存在することを確認する必要があり [!DNL Salesforce][!DNL Salesforce]ます。 接続仕様が存在しない場合は、接続を確立できません。

使用可能な各ソースには、認証要件などのコネクタプロパティを記述するための固有の接続仕様のセットがあります。 GETリクエストを実行し、クエリパラメーターを使用 [!DNL Salesforce] して、の接続仕様を検索できます。

**API 形式**

クエリパラメーターを指定しないでGETリクエストを送信すると、使用可能なすべてのソースの接続仕様が返されます。 特別な情報を取得す `property=name=="salesforce"` るクエリを含めることができ [!DNL Salesforce]ます。

```http
GET /connectionSpecs
GET /connectionSpecs?property=name=="salesforce"
```

**リクエスト**

次のリクエストは、の接続仕様を取得し [!DNL Salesforce]ます。

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs?property=name==%22salesforce%22' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、固有な識別子( [!DNL Salesforce]`id`)を含む、の接続仕様を返します。 このIDは、次の手順でベース接続を作成する際に必要となります。

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

## API用の接続の作成

APIの接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するために使用できるので、APIは [!DNL Salesforce] アカウントごとに1つの接続のみ必要です。

**API 形式**

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
| `auth.params.username` | アカウントに関連付けられているユー [!DNL Salesforce] ザ名。 |
| `auth.params.password` | アカウントに関連付けられているパス [!DNL Salesforce] ワードです。 |
| `auth.params.securityToken` | アカウントに関連付けられているセキュリティト [!DNL Salesforce] ークン。 |
| `connectionSpec.id` | 前の手順で取得 `id` した [!DNL Salesforce] アカウントの接続仕様。 |

**応答** 

リクエストが成功した場合、ベース接続の一意の識別子（`id`）が返されます。このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700df7b-0000-0200-0000-5e3b424f0000\""
}
```

## 次の手順

このチュートリアルに従うと、APIを使用して [!DNL Salesforce] アカウントへの接続が作成され、一意のIDが応答本文の一部として取得されます。 Flow Service APIを使用してCRMシステムを [調査する方法について学習する際に、次のチュートリアルでこの接続IDを使用できます](../../explore/crm.md)。