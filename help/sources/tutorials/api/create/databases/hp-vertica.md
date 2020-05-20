---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用してHP Verticaコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: a015d2612bc5a72004e15dc5706c7718617a0af4
workflow-type: tm+mt
source-wordcount: '622'
ht-degree: 1%

---


# Flow Service APIを使用してHP Verticaコネクタを作成する

>[!NOTE]
>HP Verticaコネクタはベータ版です。 機能とドキュメントは、変更されることがあります。

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元管理するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、Flow Service APIを使用して、HP VerticaをExperience Platformに接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

- [ソース](https://docs.adobe.com/content/help/en/experience-platform/source-connectors/home.html): Experience Platformを使用すると、様々なソースからデータを取り込むと同時に、プラットフォームサービスを使用して、受信データの構成、マッピング、拡張を行うことができます。
- [サンドボックス](https://docs.adobe.com/content/help/en/experience-platform/sandbox/home.html): Experience Platformは、1つのプラットフォームインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

Flow Service APIを使用してHP Verticaに正常に接続するために知っておく必要がある追加情報については、以下の節で説明します。

### 必要な資格情報の収集

フローサービスがHP Verticaと接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | HP Verticaインスタンスへの接続に使用する接続文字列。 HP Verticaの接続文字列パターンは、 `Server=<server>;Port=<port>;Database=<database>;UID=<user name>;PWD=<password>` |
| `connectionSpec.id` | 接続を作成するために必要な識別子。 HP Verticaの固定接続仕様IDは次のとおりです。 `a8b6a1a4-5735-42b4-952c-85dce0ac38b5` |

接続文字列の取得の詳細については、 [このHP Verticaドキュメントを参照してください](https://www.vertica.com/docs/9.2.x/HTML/Content/Authoring/ConnectingToVertica/ClientJDBC/CreatingAndConfiguringAConnection.htm)。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](https://docs.adobe.com/content/help/en/experience-platform/landing/troubleshooting.html#reading-example-api-calls) 。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](https://docs.adobe.com/content/help/en/experience-platform/tutorials/authentication.html)。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

ソースコネクターを含むエクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されています。 プラットフォームAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

- Content-Type: `application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 HP Verticaアカウントごとに必要な接続は1つだけです。異なるデータを取り込むために複数のソースコネクタを作成するのに使用できます。

**API形式**

```http
POST /connections
```

**リクエスト**

HP Vertica接続を作成するには、一意の接続仕様IDをPOST要求の一部として指定する必要があります。 HP Verticaの接続仕様IDはです `a8b6a1a4-5735-42b4-952c-85dce0ac38b5`。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Connection for HP Vertica",
        "description": "Connection for HP Vertica",
        "auth": {
            "specName": "Connection String Based Authentication",
            "params": {
                "connectionString": "Server=<server>;Port=<port>;Database=<database>;UID=<user name>;PWD=<password>"
            }
        },
        "connectionSpec": {
            "id": "a8b6a1a4-5735-42b4-952c-85dce0ac38b5",
            "version": "1.0"
        }
    }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.connectionString` | HP Verticaアカウントに関連付けられている接続文字列。 |
| `connectionSpec.id` | HP Vertica接続仕様ID: `a8b6a1a4-5735-42b4-952c-85dce0ac38b5`. |

**応答**

正常な応答は、新たに作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "6bc13a3b-3546-455f-813a-3b3546a55fb1",
    "etag": "\"3500866c-0000-0200-0000-5e83afa30000\""
}
```

## 次の手順

このチュートリアルに従って、Flow Service APIを使用してHP Vertica接続を作成し、接続の一意のID値を取得しました。 このIDは、Flow Service APIを使用してデータベースを [調査する方法を学習する際に、次のチュートリアルで使用できます](../../explore/database-nosql.md)。
