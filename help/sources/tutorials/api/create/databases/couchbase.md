---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用してCouchbaseコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: e5789a10c49b2933a0727692dedf2601a214dbc2
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 2%

---


# Flow Service APIを使用してCouchbaseコネクタを作成する

>[!NOTE]
>Couchbaseコネクタはベータ版です。 機能とドキュメントは、変更されることがあります。

フローサービスは、さまざまな異なるソースから顧客データを収集および一元化して、Adobe Experience Platformに組み込むために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、Flow Service APIを使用してCouchbaseをエクスペリエンスプラットフォームに接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [ソース](../../../../home.md): Experience Platformを使用すると、様々なソースからデータを取り込むと同時に、プラットフォームサービスを使用して、入力データの構造、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): Experience Platformは、1つのプラットフォームインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

Flow Service APIを使用してCouchbaseに正常に接続するために知っておく必要がある追加情報については、以下の節に説明します。

### 必要な資格情報の収集

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | Couchbaseインスタンスへの接続に使用する接続文字列です。 Couchbaseの接続文字列パターンはで `Server={SERVER}; Port={PORT};AuthMech=1;CredString=[{\"user\": \"{USER}\", \"pass\":\"{PASS}\"}];`す。 接続文字列の取得の詳細については、 [このCouchbaseドキュメントを参照してください](https://docs.Couchbase.com/c-sdk/2.10/client-settings.html#configuring-overview)。 |
| `connectionSpec.id` | 接続を作成するために必要な識別子。 Couchbaseの接続仕様IDは修正され `1fe283f6-9bec-11ea-bb37-0242ac130002`ました。 |

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request) 。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../../../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

Experience Platformのすべてのリソース（フローサービスに属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* Content-Type: `application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できるので、Couchbaseアカウントごとに1つのコネクタが必要です。

**API形式**

```http
POST /connections
```

**リクエスト**

次の要求は、ペイロードで提供されるプロパティで設定された、新しいCouchbase接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Couchbase test connection",
        "description": "A test connection for a Couchbase source",
        "auth": {
            "specName": "Connection String Based Authentication",
            "params": {
                    "connectionString": "Server={SERVER}; Port={PORT};AuthMech=1;CredString=[{\"user\": \"{USER}\", \"pass\":\"{PASS}\"}];"
                }
        },
        "connectionSpec": {
            "id": "1fe283f6-9bec-11ea-bb37-0242ac130002",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.connectionString` | Couchbaseアカウントへの接続に使用する接続文字列です。 接続文字列パターンは次のとおりです。 `Server={SERVER}; Port={PORT};AuthMech=1;CredString=[{\"user\": \"{USER}\", \"pass\":\"{PASS}\"}];`. |
| `connectionSpec.id` | Couchbase接続仕様ID: `1fe283f6-9bec-11ea-bb37-0242ac130002`. |

**応答**

正常な応答は、新たに作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "54997109-07b5-40b7-9971-0907b5a0b75a",
    "etag": "\"280168f5-0000-0200-0000-5ed72b230000\""
}
```

## 次の手順

このチュートリアルに従うことで、Flow Service APIを使用してCouchbase接続を作成し、接続の一意のID値を取得したことになります。 このIDは、Flow Service APIを使用してデータベースを [調査する方法を学習する際に、次のチュートリアルで使用できます](../../explore/database-nosql.md)。
