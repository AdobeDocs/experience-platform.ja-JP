---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用してMariaDBコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 37a5f035023cee1fc2408846fb37d64b9a3fc4b6
workflow-type: tm+mt
source-wordcount: '664'
ht-degree: 1%

---


# Flow Service APIを使用してMariaDBコネクタを作成する

>[!NOTE]
>MariaDBコネクタはベータ版です。 機能とドキュメントは、変更されることがあります。

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元管理するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、Flow Service APIを使用して、Experience PlatformをMaria DBに接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [ソース](../../../../home.md): Experience Platformを使用すると、様々なソースからデータを取り込むと同時に、プラットフォームサービスを使用して、入力データの構造、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): Experience Platformは、1つのプラットフォームインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

Flow Service APIを使用してMaria DBに正常に接続するために知っておく必要がある追加情報については、以下の節に説明します。

### 必要な資格情報の収集

フローサービスがMaria DBと接続するには、次の接続プロパティを指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | Maria DB認証に関連付けられている接続文字列。 |

Maria DBの使い始めについて詳しくは、 [このドキュメント](https://mariadb.com/kb/en/about-mariadb-connector-odbc/) を参照してください。

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

Maria DBに接続するには、一連のMaria DB接続仕様がフローサービス内に存在する必要があります。 PlatformをMaria DBに接続する最初の手順は、これらの仕様を取得することです。

**API形式**

使用可能な各ソースには、認証要件などのコネクタプロパティを記述するための固有の接続仕様のセットがあります。 GET要求をエンドポイントに送信すると、使用可能なすべてのソースの接続指定が返され `/connectionSpecs` ます。 このクエリを含めて、Maria DB専用 `property=name=="maria-db"` の情報を取得できます。

```http
GET /connectionSpecs
GET /connectionSpecs?property=name=="maria-db"
```

**リクエスト**

次のリクエストは、Maria DBの接続仕様を取得します。

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs?property=name=="maria-db"' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、一意の識別子(`id`)を含むMaria DBの接続仕様を返します。 このIDは、次の手順でベース接続を作成する際に必要となります。

```json
{
    "items": [
        {
            "id": "3000eb99-cd47-43f3-827c-43caf170f015",
            "name": "maria-db",
            "providerId": "0ed90a81-07f4-4586-8190-b40eccef1c5a",
            "version": "1.0",
            "authSpec": [
                {
                    "name": "Connection String Based Authentication",
                    "type": "connectionStringAuth",
                    "spec": {
                        "$schema": "http://json-schema.org/draft-07/schema#",
                        "type": "object",
                        "description": "defines auth params required for connecting to Maria DB",
                        "properties": {
                            "connectionString": {
                                "type": "string",
                                "description": "connection string to connect to any Maria DB instance.",
                                "format": "password",
                                "pattern": "^(Server=)(.*)(;Port=)(.*)(;Database=)(.*)(;UID=)(.*)(;PWD=)(.*)",
                                "examples": [
                                    "Server=<host>;Port=<port>;Database=<database>;UID=<user name>;PWD=<password>"
                                ]
                            }
                        },
                        "required": [
                            "connectionString"
                        ]
                    }
                }
            ],
        }
    ]
}
```

## ベース接続を作成する

ベース接続はソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するのに使用できるので、Maria DBアカウントごとに必要なベース接続は1つだけです。

**API形式**

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
        "name": "base connection for maria-db",
        "description": "base connection for maria-db",
        "auth": {
            "specName": "Connection String Based Authentication",
            "params": {
                "connectionString": "{CONNECTION_STRING}"
            }
        },
        "connectionSpec": {
            "id": "3000eb99-cd47-43f3-827c-43caf170f015",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.connectionString` | Maria DB認証に関連付けられている接続文字列。 |
| `connectionSpec.id` | 前の手順で収集した接続仕様(`id`)。 |

**応答**

正常な応答は、新たに作成されたベース接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次の手順でクラウドストレージを調査するために必要です。

```json
{
    "id": "be3a2d71-1fb6-4fea-ba2d-711fb61fea50",
    "etag": "\"02002624-0000-0200-0000-5e41f7040000\""
}
```

## 次の手順

このチュートリアルに従って、Flow Service APIを使用してMaria DBベースの接続を作成し、接続の一意のID値を取得しました。 フローサービスAPIを使用してデータベースやNoSQLシステムを [探索する方法を学ぶ際に、次のチュートリアルでこの基本接続IDを使用できます](../../explore/database-nosql.md)。
