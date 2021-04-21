---
keywords: Experience Platform；ホーム；人気のあるトピック；Apache Cassandra;apache cassandra;Cassandra;cassandra
solution: Experience Platform
title: Flow Service APIを使用したApache Cassandraソース接続の作成
topic-legacy: overview
type: Tutorial
description: Flow Service APIを使用してApache CassandraをAdobe Experience Platformに接続する方法を説明します。
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '620'
ht-degree: 24%

---


# [!DNL Flow Service] APIを使用して[!DNL Apache Cassandra]ソース接続を作成する

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL Apache Cassandra] （以下「Cassandra」と呼びます）を[!DNL Experience Platform]に接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

[!DNL Flow Service] APIを使用してCassandraに正しく接続するために必要な追加情報については、以下の節で説明します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Cassandra]と接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | [!DNL Cassandra]サーバーのIPアドレスまたはホスト名。 |
| `port` | [!DNL Cassandra]サーバーがクライアント接続をリッスンするために使用するTCPポート。 デフォルトのポートは`9042`です。 |
| `username` | 認証用に[!DNL Cassandra]サーバーに接続するために使用するユーザー名です。 |
| `password` | 認証用に[!DNL Cassandra]サーバーに接続するためのパスワードです。 |
| `connectionSpec.id` | 接続を作成するために必要な一意の識別子。 [!DNL Cassandra]の接続指定IDは`a8f4d393-1a6b-43f3-931f-91a16ed857f4`です。 |

使い始めについての詳細は、[このCassandraドキュメント](https://cassandra.apache.org/doc/latest/operating/security.html#authentication)を参照してください。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。サンプル API 呼び出しのドキュメントで使用されている規則については、[!DNL Experience Platform] トラブルシューテングガイドの[サンプル API 呼び出しの読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソース（[!DNL Flow Service]に属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* Content-Type: `application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できるので、[!DNL Cassandra]アカウントごとに1つのコネクタが必要です。

**API 形式**

```http
POST /connections
```

**リクエスト**

[!DNL Cassandra]接続を作成するには、POST要求の一部として一意の接続指定IDを指定する必要があります。 [!DNL Cassandra]の接続指定IDは`a8f4d393-1a6b-43f3-931f-91a16ed857f4`です。

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
| `auth.params.host` | [!DNL Cassandra]サーバーのIPアドレスまたはホスト名。 |
| `auth.params.port` | [!DNL Cassandra]サーバーがクライアント接続をリッスンするために使用するTCPポート。 デフォルトのポートは`9042`です。 |
| `auth.params.username` | 認証用に[!DNL Cassandra]サーバーに接続するために使用するユーザー名です。 |
| `auth.params.password` | 認証用に[!DNL Cassandra]サーバーに接続するためのパスワードです。 |
| `connectionSpec.id` | [!DNL Cassandra]接続仕様ID:`a8f4d393-1a6b-43f3-931f-91a16ed857f4`. |

**応答**

正常に応答すると、新たに作成された接続の詳細(一意の識別子(`id`)が返されます。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "ce69aa89-1baa-4054-a9aa-891baa605425",
    "etag": "\"5a026e19-0000-0200-0000-5eac76d00000\""
}
```

## 次の手順

このチュートリアルに従うと、[!DNL Flow Service] APIを使用して[!DNL Cassandra]接続を作成し、接続の一意のID値を取得したことになります。 このIDは、Flow Service API ](../../explore/database-nosql.md)を使用して[データベースを調査する方法を学習する際に、次のチュートリアルで使用できます。
