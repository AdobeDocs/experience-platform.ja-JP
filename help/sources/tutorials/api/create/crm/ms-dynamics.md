---
keywords: Experience Platform;home;popular topics;Microsoft Dynamics;microsoft dynamics;dynamics;Dynamics
solution: Experience Platform
title: Flow Service APIを使用してMicrosoft Dynamics Connectorを作成する
topic: overview
type: Tutorial
description: このチュートリアルでは、Flow Service APIを使用して、CRMデータを収集するためのMicrosoft Dynamics（以下「Dynamics」と呼ばれる）アカウントにプラットフォームを接続する手順を説明します。
translation-type: tm+mt
source-git-commit: 97dfd3a9a66fe2ae82cec8954066bdf3b6346830
workflow-type: tm+mt
source-wordcount: '709'
ht-degree: 18%

---


# APIを使用して [!DNL Microsoft Dynamics][!DNL Flow Service] コネクタを作成する

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、 [!DNL Flow Service] APIを使用して、CRMデータを収集するため [!DNL Platform] の [!DNL Microsoft Dynamics] （以下「Dynamics」と呼ばれる）アカウントに接続する手順を順を追って説明します。

でユーザインターフェイスを使用する場合は、 [!DNL Experience Platform]DynamicsソースコネクタのUIチュートリアル [](../../../ui/create/crm/dynamics.md) で、同様の操作を実行するための手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md):E[!DNL xperience Platform] は、1つのインスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスのアプリケーションを開発および発展させる仮想 [!DNL Platform] サンドボックスを提供します。

The following sections provide additional information that you will need to know in order to successfully connect [!DNL Platform] to a Dynamics account using the [!DNL Flow Service] API.

### 必要な資格情報の収集

に接続 [!DNL Flow Service] するに [!DNL Dynamics]は、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `serviceUri` | インスタンスのサービスURL [!DNL Dynamics] 。 |
| `username` | ユーザーアカウントのユー [!DNL Dynamics] ザー名です。 |
| `password` | アカウントのパス [!DNL Dynamics] ワード。 |

開始方法の詳細については、 [このDynamicsドキュメントを参照してください](https://docs.microsoft.com/en-us/powerapps/developer/common-data-service/authenticate-oauth)。

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

アカウント [!DNL Platform] に接続する前に、の接続仕様が存在することを確認する必要があり [!DNL Dynamics][!DNL Dynamics]ます。 接続仕様が存在しない場合は、接続を確立できません。

使用可能な各ソースには、認証要件などのコネクタプロパティを記述するための固有の接続仕様のセットがあります。 GETリクエストを実行し、クエリパラメーターを使用 [!DNL Dynamics] して、の接続仕様を検索できます。

**API 形式**

クエリパラメーターを指定しないでGETリクエストを送信すると、使用可能なすべてのソースの接続仕様が返されます。 特別な情報を取得す `property=name=="dynamics-online"` るクエリを含めることができ [!DNL Dynamics]ます。

```http
GET /connectionSpecs
GET /connectionSpecs?property=name=="dynamics-online"
```

**リクエスト**

次のリクエストは、の接続仕様を取得し [!DNL Dynamics]ます。

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs?property=name=="dynamics-online"' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、固有な識別子( [!DNL Dynamics]`id`)を含む、の接続仕様を返します。 このIDは、次の手順でベース接続を作成する際に必要となります。

```json
{
    "items": [
        {
            "id": "38ad80fe-8b06-4938-94f4-d4ee80266b07",
            "name": "dynamics-online",
            "providerId": "0ed90a81-07f4-4586-8190-b40eccef1c5a",
            "version": "1.0",
            "authSpec": [
                {
                    "name": "Basic Authentication for Dynamics-Online",
                    "settings": {
                        "authenticationType": "Office365",
                        "deploymentType": "Online"
                    },
                    "spec": {
                        "$schema": "http://json-schema.org/draft-07/schema#",
                        "type": "object",
                        "description": "defines auth params required for connecting to dynamics-online",
                        "properties": {
                            "serviceUri": {
                                "type": "string",
                                "description": "The service URL of your Dynamics instance"
                            },
                            "username": {
                                "type": "string",
                                "description": "User name for the user account"
                            },
                            "password": {
                                "type": "string",
                                "description": "Password for the user account",
                                "format": "password"
                            }
                        },
                        "required": [
                            "username",
                            "password",
                            "serviceUri"
                        ]
                    }
                }
            ]
        }
    ]
}
```

## ベース接続を作成する

ベース接続はソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するために使用できるので、 [!DNL Dynamics] アカウントごとに1つのベース接続が必要です。

次のPOST要求を実行して、ベース接続を作成します。

**API 形式**

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
        "name": "Dynamics Base Connection",
        "description": "Base connection for Dynamics account",
        "auth": {
            "specName": "Basic Authentication for Dynamics-Online",
            "params": {
                "serviceUri": "{SERVICE_URI}",
                "username": "{USERNAME}",
                "password": "{PASSWORD}"
            }
        },
        "connectionSpec": {
            "id": "38ad80fe-8b06-4938-94f4-d4ee80266b07",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.serviceUri` | インスタンスに関連付けられているサービスURI [!DNL Dynamics] です。 |
| `auth.params.username` | アカウントに関連付けられているユー [!DNL Dynamics] ザ名。 |
| `auth.params.password` | アカウントに関連付けられているパス [!DNL Dynamics] ワードです。 |
| `connectionSpec.id` | 前の手順で取得 `id` した [!DNL Dynamics] アカウントの接続仕様。 |

**応答** 

リクエストが成功した場合、ベース接続の一意の識別子（`id`）が返されます。このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"9e0052a2-0000-0200-0000-5e35tb330000\""
}
```

## 次の手順

このチュートリアルに従うと、APIを使用して [!DNL Dynamics] アカウントの基本接続を作成し、一意のIDを応答本文の一部として取得できます。 Flow Service APIを使用してCRMシステムを [調査する方法について学習する際に、次のチュートリアルでこの基本接続IDを使用できます](../../explore/crm.md)。