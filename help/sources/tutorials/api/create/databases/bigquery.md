---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用してGoogle BigQueryコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 690ddbd92f0a2e4e06b988e761dabff399cd2367
workflow-type: tm+mt
source-wordcount: '696'
ht-degree: 17%

---


# APIを使用して [!DNL Google BigQuery][!DNL Flow Service] コネクタを作成する

>[!NOTE]
>
>コネクタ [!DNL Google BigQuery] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、 [!DNL Flow Service] APIを使用して、に接続する手順 [!DNL Experience Platform] (以下「BigQuery」と呼ばれ [!DNL Google BigQuery] ます)について説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

The following sections provide additional information that you will need to know in order to successfully connect to BigQuery using the [!DNL Flow Service] API.

### 必要な資格情報の収集

BigQueryと接続 [!DNL Flow Service] するには、次の接続プロパティを指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `project` | クエリ対象のデフォルト [!DNL BigQuery] プロジェクトのプロジェクトID。 |
| `clientID` | 更新トークンの生成に使用されるID値。 |
| `clientSecret` | 更新トークンの生成に使用されるシークレット値。 |
| `refreshToken` | へのアクセスを許可するために [!DNL Google] 使用された更新トークン [!DNL BigQuery]。 |

これらの値の詳細については、 [このBigQueryドキュメントを参照してください](https://cloud.google.com/storage/docs/json_api/v1/how-tos/authorizing)。

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

接続を作成するには、 [!DNL BigQuery] 接続仕様のセットがに存在する必要があり [!DNL BigQuery][!DNL Flow Service]ます。 に接続する最初の手順 [!DNL Platform] は、これらの仕様 [!DNL BigQuery] を取得することです。

**API 形式**

使用可能な各ソースには、認証要件などのコネクタプロパティを記述するための固有の接続仕様のセットがあります。 GETリクエストを実行し、クエリパラメーターを使用 [!DNL BigQuery] して、の接続仕様を検索できます。

クエリパラメーターを指定しないでGETリクエストを送信すると、使用可能なすべてのソースの接続仕様が返されます。 特別な情報を取得す `property=name=="google-big-query"` るクエリを含めることができ [!DNL BigQuery]ます。

```http
GET /connectionSpecs
GET /connectionSpecs?property=name=="google-big-query"
```

**リクエスト**

次のリクエストは、の接続仕様を取得し [!DNL BigQuery]ます。

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs?property=name=="google-big-query"' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、固有な識別子( [!DNL BigQuery]`id`)を含む、の接続仕様を返します。 このIDは、次の手順でベース接続を作成する際に必要となります。

```json
{
    "items": [
        {
            "id": "3c9b37f8-13a6-43d8-bad3-b863b941fedd",
            "name": "google-big-query",
            "providerId": "0ed90a81-07f4-4586-8190-b40eccef1c5a",
            "version": "1.0",
            "authSpec": [
                {
                    "name": "Basic Authentication",
                    "spec": {
                        "$schema": "http://json-schema.org/draft-07/schema#",
                        "type": "object",
                        "description": "defines auth params",
                        "properties": {
                            "project": {
                                "type": "string",
                                "description": "The project ID of the default BigQuery project to query against"
                            },
                            "clientId": {
                                "type": "string",
                                "description": "ID of the application used to generate the refresh token."
                            },
                            "clientSecret": {
                                "type": "string",
                                "description": "Secret of the application used to generate the refresh token.",
                                "format": "password"
                            },
                            "refreshToken": {
                                "type": "string",
                                "description": "The refresh token obtained from Google used to authorize access to BigQuery.",
                                "format": "password"
                            }
                        },
                        "required": [
                            "project",
                            "clientId",
                            "clientSecret",
                            "refreshToken"
                        ]
                    }
                }
            ]
        }
    ]
}
```

## ベース接続を作成する

ベース接続はソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するために使用できるので、 [!DNL BigQuery] アカウントごとに1つのベース接続が必要です。

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
        "name": "BigQuery base connection",
        "description": "Base connection for Google BigQuery",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                "project": "{PROJECT}",
                "clientId": "{CLIENT_ID}",
                "clientSecret": "{CLIENT_SECRET}",
                "refreshToken": "{REFRESH_TOKEN}"
            }
        },
        "connectionSpec": {
            "id": "3c9b37f8-13a6-43d8-bad3-b863b941fedd",
            "version": "1.0"
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.project` | クエリするデフォルトの [!DNL BigQuery] プロジェクトのプロジェクトID。 対して |
| `auth.params.clientId` | 更新トークンの生成に使用されるID値。 |
| `auth.params.clientSecret` | 更新トークンの生成に使用されるクライアント値。 |
| `auth.params.refreshToken` | へのアクセスを許可するために [!DNL Google] 使用された更新トークン [!DNL BigQuery]。 |
| `connectionSpec.id` | 前の手順で取得 `id` した [!DNL BigQuery] アカウントの接続仕様。 |

**応答** 

正常な応答は、新たに作成されたベース接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "26ced882-729b-470f-8ed8-82729b570f03",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## 次の手順

このチュートリアルに従うことで、 [!DNL BigQuery] APIを使用して [!DNL Flow Service] 基本接続を作成し、接続の一意のID値を取得しました。 フローサービスAPIを使用してデータベースやNoSQLシステムを [探索する方法を学ぶ際に、次のチュートリアルでこの基本接続IDを使用できます](../../explore/database-nosql.md)。
