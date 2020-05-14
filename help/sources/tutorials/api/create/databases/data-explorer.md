---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用してAzure Data Explorerコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 37a5f035023cee1fc2408846fb37d64b9a3fc4b6
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 1%

---


# Flow Service APIを使用してAzure Data Explorerコネクタを作成する

>[!NOTE]
>Azure Data Explorerコネクタはベータ版です。 機能とドキュメントは、変更されることがあります。

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元管理するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、Flow Service APIを使用して、Azure Data Explorer（以下「Data Explorer」と呼ばれる）をエクスペリエンスプラットフォームに接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [ソース](../../../../home.md): Experience Platformを使用すると、様々なソースからデータを取り込むと同時に、プラットフォームサービスを使用して、入力データの構造、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): Experience Platformは、1つのプラットフォームインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

次の節では、Flow Service APIを使用してData Explorerに正常に接続するために知っておく必要がある追加情報について説明します。

### 必要な資格情報の収集

フローサービスがData Explorerと接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `endpoint` | Data Explorerサーバーのエンドポイント。 |
| `database` | Data Explorerデータベースの名前。 |
| `tenant` | Data Explorerデータベースへの接続に使用する一意のテナントID。 |
| `servicePrincipalId` | Data Explorerデータベースへの接続に使用する一意のサービスプリンシパルID。 |
| `servicePrincipalKey` | Data Explorerデータベースへの接続に使用する一意のサービスプリンシパルキーです。 |
| `connectionSpec.id` | 接続を作成するために必要な一意の識別子。 Data Explorerの接続指定IDはで `0479cc14-7651-4354-b233-7480606c2ac3`す。 |

使い始める前に、 [このData Explorerドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/management/access-control/how-to-authenticate-with-aad)。

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

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できるので、Data Explorerアカウントごとに1つのコネクタが必要です。

**API形式**

```http
POST /connections
```

**リクエスト**

Data Explorerの接続を作成するには、一意の接続指定IDをPOSTリクエストの一部として指定する必要があります。 Data Explorerの接続指定IDはで `0479cc14-7651-4354-b233-7480606c2ac3`す。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Azure Data Explorer connection",
        "description": "A connection for Azure Data Explorer",
        "auth": {
            "specName": "Service Principal Based Authentication",
            "params": {
                    "endpoint": "{ENDPOINT}",
                    "database": "{DATABASE}",
                    "tenant": "{TENANT}",
                    "servicePrincipalId": "{SERVICE_PRINCIPAL_ID}",
                    "servicePrincipalKey": "{SERVICE_PRINCIPAL_KEY}"
                }
        },
        "connectionSpec": {
            "id": "0479cc14-7651-4354-b233-7480606c2ac3",
            "version": "1.0"
        }
    }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.endpoint` | Data Explorerサーバーのエンドポイント。 |
| `auth.params.database` | Data Explorerデータベースの名前。 |
| `auth.params.tenant` | Data Explorerデータベースへの接続に使用する一意のテナントID。 |
| `auth.params.servicePrincipalId` | Data Explorerデータベースへの接続に使用する一意のサービスプリンシパルID。 |
| `auth.params.servicePrincipalKey` | Data Explorerデータベースへの接続に使用する一意のサービスプリンシパルキーです。 |
| `connectionSpec.id` | Data Explorerの接続仕様ID: `0479cc14-7651-4354-b233-7480606c2ac3`. |

**応答**

正常な応答は、新たに作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "f088e4f2-2464-480c-88e4-f22464b80c90",
    "etag": "\"43011faa-0000-0200-0000-5ea740cd0000\""
}
```

## 次の手順

このチュートリアルに従うことで、Flow Service APIを使用してData Explorer接続を作成し、接続の一意のID値を取得したことになります。 このIDは、Flow Service APIを使用してデータベースを [調査する方法を学習する際に、次のチュートリアルで使用できます](../../explore/database-nosql.md)。
