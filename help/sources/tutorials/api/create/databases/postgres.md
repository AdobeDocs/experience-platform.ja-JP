---
keywords: Experience Platform；ホーム；人気のあるトピック；PostgreSQL;postgresql;PSQL;psql
solution: Experience Platform
title: Flow Service APIを使用したPostgreSQLソース接続の作成
topic: 概要
type: チュートリアル
description: Flow Service APIを使用して、Adobe Experience PlatformをPostgreSQLに接続する方法を学びます。
translation-type: tm+mt
source-git-commit: 8851e11e956b393e56714d4d48870b7f68947c18
workflow-type: tm+mt
source-wordcount: '562'
ht-degree: 27%

---


# [!DNL Flow Service] APIを使用して[!DNL PostgreSQL]ソース接続を作成する

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、[!DNL Flow Service] APIを使用して、[!DNL Experience Platform]を[!DNL PostgreSQL]に接続する手順（以下「PSQL」と呼びます）を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

[!DNL Flow Service] APIを使用してPSQLに正しく接続するために必要な追加情報については、以下の節で説明します。

### 必要な資格情報の収集

[!DNL Flow Service]がPSQLと接続するには、次の接続プロパティを指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | PSQLアカウントに関連付けられている接続文字列です。 PSQL接続文字列パターンは次のとおりです。`Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD}`. |
| `connectionSpec.id` | 接続の生成に使用するID。 PSQLの固定接続仕様IDは`74a1c565-4e59-48d7-9d67-7c03b8a13137`です。 |

接続文字列の取得について詳しくは、[このPSQLドキュメント](https://www.postgresql.org/docs/9.2/app-psql.html)を参照してください。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソース（[!DNL Flow Service]に属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できるため、PSQLアカウントごとに必要な接続は1つだけです。

**API 形式**

```http
POST /connections
```

**リクエスト**

PSQL接続を作成するには、一意の接続仕様IDをPOST要求の一部として指定する必要があります。 PSQLの接続仕様IDは`74a1c565-4e59-48d7-9d67-7c03b8a13137`です。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Test connection for PostgreSQL",
        "description": "Test connection for PostgreSQL",
        "auth": {
            "specName": "Connection String Based Authentication",
            "params": {
                "connectionString": "Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD}"
            }
        },
        "connectionSpec": {
            "id": "74a1c565-4e59-48d7-9d67-7c03b8a13137",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| ------------- | --------------- |
| `auth.params.connectionString` | PSQLアカウントに関連付けられている接続文字列です。 PSQL接続文字列パターンは次のとおりです。`Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD}`. |
| `connectionSpec.id` | PSQLの接続仕様ID:`74a1c565-4e59-48d7-9d67-7c03b8a13137`. |

**応答** 

正常な応答は、新たに作成されたベース接続の固有な識別子(`id`)を返します。 このIDは、次のチュートリアルでPSQLデータベースを調べるために必要です。

```json
{
    "id": "056dd1b4-da33-42f9-add1-b4da3392f94e",
    "etag": "\"1700e582-0000-0200-0000-5e3c85180000\""
}
```

## 次の手順

このチュートリアルに従うと、[!DNL Flow Service] APIを使用してPSQL接続を作成し、接続の一意のID値を取得したことになります。 この接続IDは、Flow Service API](../../explore/database-nosql.md)を使用して[データベースやNoSQLシステムを探索する方法を学ぶ際に、次のチュートリアルで使用できます。
