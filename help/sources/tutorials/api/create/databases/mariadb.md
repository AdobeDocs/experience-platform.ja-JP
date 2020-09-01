---
keywords: Experience Platform;home;popular topics;MariaDB;mariadb
solution: Experience Platform
title: Flow Service APIを使用してMariaDBコネクタを作成する
topic: overview
description: このチュートリアルでは、Flow Service APIを使用して、Experience Platform]をMariaDBに接続する手順を順を追って説明します。
translation-type: tm+mt
source-git-commit: 5959d4344ec1c16542de045899ce74beb39a7bc4
workflow-type: tm+mt
source-wordcount: '556'
ht-degree: 21%

---


# APIを使用して [!DNL MariaDB][!DNL Flow Service] コネクタを作成する

>[!NOTE]
>
>コネクタ [!DNL MariaDB] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、 [!DNL Flow Service] APIを使用して、に接続する手順を順を追って説明 [!DNL Experience Platform] し [!DNL MariaDB]ます。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

The following sections provide additional information that you will need to know in order to successfully connect to [!DNL MariaDB] using the [!DNL Flow Service] API.

### 必要な資格情報の収集

に接続 [!DNL Flow Service] するに [!DNL MariaDB]は、次の接続プロパティを指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | 認証に関連付けられている接続文字列 [!DNL MariaDB] です。 接続文字 [!DNL MariaDB] 列パターンは次のとおりです。 `Server={HOST};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}`. |
| `connectionSpec.id` | 接続の生成に使用するID。 の固定接続仕様ID [!DNL MariaDB] は、 `3000eb99-cd47-43f3-827c-43caf170f015`です。 |

接続文字列の取得について詳しくは、 [このMariaDBドキュメントを参照してください](https://mariadb.com/kb/en/about-mariadb-connector-odbc/)。

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

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込む複数のソースコネクタを作成する場合に使用できるので、 [!DNL MariaDB] アカウントごとに必要な接続は1つだけです。

**API 形式**

```http
POST /connections
```

**リクエスト**

接続を作成するには、その [!DNL MariaDB] 一意の接続仕様IDをPOST要求の一部として指定する必要があります。 の接続仕様ID [!DNL MariaDB] はで `3000eb99-cd47-43f3-827c-43caf170f015`す。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Test connection for maria-db",
        "description": "Test connection for maria-db",
        "auth": {
            "specName": "Connection String Based Authentication",
            "params": {
                "connectionString": "Server={HOST};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}"
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
| `auth.params.connectionString` | 認証に関連付けられている接続文字列 [!DNL MariaDB] です。 接続文字 [!DNL MariaDB] 列パターンは次のとおりです。 `Server={HOST};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}`. |
| `connectionSpec.id` | 接続 [!DNL MariaDB] 仕様IDは次のとおりです。 `3000eb99-cd47-43f3-827c-43caf170f015`. |

**応答** 

正常な応答は、新たに作成されたベース接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次の手順でデータベースを調査するために必要です。

```json
{
    "id": "be3a2d71-1fb6-4fea-ba2d-711fb61fea50",
    "etag": "\"02002624-0000-0200-0000-5e41f7040000\""
}
```

## 次の手順

このチュートリアルに従うことで、 [!DNL MariaDB] APIを使用して [!DNL Flow Service] 接続を作成し、接続の一意のID値を取得しました。 この接続IDは、Flow Service APIを使用してデータベースやNoSQLシステムを [探索する方法を学ぶ際に、次のチュートリアルで使用できます](../../explore/database-nosql.md)。
