---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用してMySQLコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 0a2247a9267d4da481b3f3a5dfddf45d49016e61
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 2%

---


# Flow Service APIを使用してMySQLコネクタを作成する

>[!NOTE]
>MySQL Connectorはベータ版です。 機能とドキュメントは、変更されることがあります。

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元管理するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、Flow Service APIを使用して、エクスペリエンスプラットフォームをMySQLに接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [ソース](../../../../home.md): Experience Platformを使用すると、様々なソースからデータを取り込むと同時に、プラットフォームサービスを使用して、入力データの構造、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): Experience Platformは、1つのプラットフォームインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

Flow Service APIを使用してMySQLに正常に接続するために必要な追加情報については、以下の節で説明します。

### 必要な資格情報の収集

フローサービスがMySQLストレージと接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | アカウントに関連付けられているMySQL接続文字列。 MySQLの接続文字列パターンは次のとおりです。 `Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}`. |
| `connectionSpec.id` | 接続の生成に使用するID。 MySQLの固定接続仕様IDは `26d738e0-8963-47ea-aadf-c60de735468a`です。 |

接続文字列の取得について詳しくは、 [このMySQLドキュメントを参照してください](https://dev.mysql.com/doc/connector-net/en/connector-net-connections-string.html)。

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

接続は、ソースを指定し、そのソースの資格情報を含みます。 MySQLアカウントごとに必要な接続は1つだけです。異なるデータを取り込むために複数のソースコネクタを作成するのに使用できます。

**API形式**

```http
POST /connections
```

**リクエスト**

MySQL接続を作成するには、一意の接続仕様IDをPOST要求の一部として指定する必要があります。 MySQLの接続仕様IDはで `26d738e0-8963-47ea-aadf-c60de735468a`す。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "MySQL Test Connection",
        "description": "MySQL Test Connection",
        "auth": {
            "specName": "Connection String Based Authentication",
            "params": {
                "connectionString": "Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}"
            }
        },
        "connectionSpec": {
            "id": "26d738e0-8963-47ea-aadf-c60de735468a",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.connectionString` | アカウントに関連付けられているMySQL接続文字列。 MySQLの接続文字列パターンは次のとおりです。 `Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}`. |
| `connectionSpec.id` | MySQLの固定接続仕様ID: `26d738e0-8963-47ea-aadf-c60de735468a`. |

**応答**

正常な応答は、新たに作成されたベース接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータベースを調べるために必要です。

```json
{
    "id": "1a444165-3439-4c16-8441-653439dc166a",
    "etag": "\"5b04c219-0000-0200-0000-5e179c8f0000\""
}
```

## 次の手順

このチュートリアルに従うと、Flow Service APIを使用してMySQL接続を作成し、接続の一意のID値を取得したことになります。 この接続IDは、Flow Service APIを使用してデータベースやNoSQLシステムを [探索する方法を学ぶ際に、次のチュートリアルで使用できます](../../explore/database-nosql.md)。