---
keywords: Experience Platform;home;popular topics;bigquery;Google;google;Google BigQuery
solution: Experience Platform
title: Flow Service APIを使用してGoogle BigQueryコネクタを作成する
topic: overview
type: Tutorial
description: このチュートリアルでは、Flow Service APIを使用して、Experience PlatformをGoogle BigQuery（以下「BigQuery」と呼ばれる）に接続する手順を順を追って説明します。
translation-type: tm+mt
source-git-commit: 74fbf388cf645c89f9f6d00a5ae2e59ba94041b9
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 18%

---


# [!DNL Flow Service] APIを使用して[!DNL Google BigQuery]コネクタを作成する

>[!NOTE]
>
>[!DNL Google BigQuery]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、[!DNL Flow Service] APIを使用して、[!DNL Experience Platform]を[!DNL Google BigQuery]に接続する手順（以下「BigQuery」と呼びます）を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

[!DNL Flow Service] APIを使用してBigQueryに正しく接続するために知っておく必要がある追加情報については、以下の節で説明します。

### 必要な資格情報の収集

[!DNL Flow Service]がBigQueryをプラットフォームに接続するには、次のOAuth 2.0認証値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `project` | クエリするデフォルトの[!DNL BigQuery]プロジェクトのプロジェクトID。 |
| `clientID` | 更新トークンの生成に使用されるID値。 |
| `clientSecret` | 更新トークンの生成に使用されるシークレット値。 |
| `refreshToken` | [!DNL BigQuery]へのアクセスを許可するために使用される[!DNL Google]から取得される更新トークン。 |

これらの値の詳細については、[このBigQueryドキュメント](https://cloud.google.com/storage/docs/json_api/v1/how-tos/authorizing)を参照してください。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

[!DNL Platform] APIを呼び出すには、まず[認証チュートリアル](../../../../../tutorials/authentication.md)を完了する必要があります。 次に示すように、すべての[!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソース（[!DNL Flow Service]に属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込む複数のデータフローの作成に使用できるため、[!DNL BigQuery]アカウントごとに必要な接続は1つだけです。

**API 形式**

```http
POST /connections
```

**リクエスト**

[!DNL BigQuery]接続を作成するには、POST要求の一部として一意の接続指定IDを指定する必要があります。 [!DNL BigQuery]の接続指定IDは`3c9b37f8-13a6-43d8-bad3-b863b941fedd`です。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Google BigQuery connection",
        "description": "Google BigQuery connection",
        "auth": {
            "specName": "Basic Authentication",
            "type": "OAuth2.0",
            "params": {
                    "project": "{PROJECT}",
                    "clientId": "{CLIENT_ID},
                    "clientSecret": "{CLIENT_SECRET}",
                    "refreshToken": "{REFRESH_TOKEN}"
                }
        },
        "connectionSpec": {
            "id": "3c9b37f8-13a6-43d8-bad3-b863b941fedd",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.project` | クエリするデフォルトの[!DNL BigQuery]プロジェクトのプロジェクトID。 対して |
| `auth.params.clientId` | 更新トークンの生成に使用されるID値。 |
| `auth.params.clientSecret` | 更新トークンの生成に使用されるクライアント値。 |
| `auth.params.refreshToken` | [!DNL BigQuery]へのアクセスを許可するために使用される[!DNL Google]から取得される更新トークン。 |
| `connectionSpec.id` | 前の手順で取得した[!DNL BigQuery]アカウントの接続仕様`id`。 |

**応答** 

正常に応答すると、新たに作成された接続の詳細(一意の識別子(`id`)が返されます。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "26ced882-729b-470f-8ed8-82729b570f03",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## 次の手順

このチュートリアルに従うと、[!DNL Flow Service] APIを使用して[!DNL BigQuery]接続を作成し、接続の一意のID値を取得したことになります。 この接続IDは、Flow Service API](../../explore/database-nosql.md)を使用して[データベースやNoSQLシステムを探索する方法を学ぶ際に、次のチュートリアルで使用できます。
