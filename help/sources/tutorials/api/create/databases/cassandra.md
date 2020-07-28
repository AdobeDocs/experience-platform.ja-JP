---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用してApache Cassandraコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: fc5cdaa661c47e14ed5412868f3a54fd7bd2b451
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 20%

---


# APIを使用した [!DNL Apache Cassandra][!DNL Flow Service] コネクタの作成

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、 [!DNL Flow Service] APIを使用して、に接続する手順(以下「Cassandra」と呼ばれ [!DNL Apache Cassandra] る)を順を追って説明 [!DNL Experience Platform]します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

The following sections provide additional information that you will need to know in order to successfully connect to Cassandra using the [!DNL Flow Service] API.

### 必要な資格情報の収集

と接続 [!DNL Flow Service] するには、次の接続プロパティの値を指定する必要があ [!DNL Cassandra]ります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | サー [!DNL Cassandra] バーのIPアドレスまたはホスト名。 |
| `port` | サー [!DNL Cassandra] バーがクライアント接続をリッスンするために使用するTCPポートです。 The default port is `9042`. |
| `username` | 認証のためにサー [!DNL Cassandra] バーに接続するために使用するユーザー名です。 |
| `password` | 認証用に [!DNL Cassandra] サーバーに接続するためのパスワードです。 |
| `connectionSpec.id` | 接続を作成するために必要な一意の識別子。 の接続指定ID [!DNL Cassandra] はです `a8f4d393-1a6b-43f3-931f-91a16ed857f4`。 |

使い始める前に、このCassandraドキュメントを参照し [てください](https://cassandra.apache.org/doc/latest/operating/security.html#authentication)。

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

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込む複数のソースコネクタを作成する場合に使用できるので、 [!DNL Cassandra] アカウントごとに1つのコネクタが必要です。

**API 形式**

```http
POST /connections
```

**リクエスト**

接続を作成するには、その [!DNL Cassandra] 一意の接続指定IDをPOST要求の一部として指定する必要があります。 の接続指定ID [!DNL Cassandra] はです `a8f4d393-1a6b-43f3-931f-91a16ed857f4`。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Cassandra test connection",
        "description": "A test connection for Cassandra",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                    "host": "{HOST},
                    "port": "{PORT}",
                    "username": "{USERNAME}",
                    "password": "{PASSWORD}"
                }
        },
        "connectionSpec": {
            "id": "a8f4d393-1a6b-43f3-931f-91a16ed857f4",
            "version": "1.0"
        }
    }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.host` | サー [!DNL Cassandra] バーのIPアドレスまたはホスト名。 |
| `auth.params.port` | サー [!DNL Cassandra] バーがクライアント接続をリッスンするために使用するTCPポートです。 The default port is `9042`. |
| `auth.params.username` | 認証のためにサー [!DNL Cassandra] バーに接続するために使用するユーザー名です。 |
| `auth.params.password` | 認証用に [!DNL Cassandra] サーバーに接続するためのパスワードです。 |
| `connectionSpec.id` | 接続 [!DNL Cassandra] 仕様ID: `a8f4d393-1a6b-43f3-931f-91a16ed857f4`. |

**応答** 

正常な応答は、新たに作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "ce69aa89-1baa-4054-a9aa-891baa605425",
    "etag": "\"5a026e19-0000-0200-0000-5eac76d00000\""
}
```

## 次の手順

このチュートリアルに従うことで、 [!DNL Cassandra] APIを使用して [!DNL Flow Service] 接続を作成し、接続の一意のID値を取得しました。 このIDは、Flow Service APIを使用してデータベースを [調査する方法を学習する際に、次のチュートリアルで使用できます](../../explore/database-nosql.md)。
