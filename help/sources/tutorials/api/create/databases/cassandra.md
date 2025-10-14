---
keywords: Experience Platform；ホーム；人気のトピック；Apache Cassandra;apache cassandra;Cassandra;cassandra
solution: Experience Platform
title: Flow Service API を使用した Apache Cassandra Source接続の作成
type: Tutorial
description: Flow Service API を使用して Apache Cassandra をAdobe Experience Platformに接続する方法を説明します。
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '611'
ht-degree: 47%

---


# [!DNL Flow Service] API を使用した [!DNL Apache Cassandra] ソース接続の作成

[!DNL Flow Service] を使用すると、様々な異なるソースから顧客データを収集し、Adobe Experience Platformで一元化できます。 このサービスは、ユーザーインターフェイスと RESTful API を提供し、サポートされているすべてのソースを接続できます。

このチュートリアルでは、[!DNL Flow Service] API を使用して、[!DNL Apache Cassandra] （以下「Cassandra」と呼ぶ）を [!DNL Experience Platform] に接続する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用して Cassandra に正しく接続するために必要な追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL Cassandra] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | [!DNL Cassandra] サーバーの IP アドレスまたはホスト名。 |
| `port` | クライアント接続をリッスンするために [!DNL Cassandra] サーバーが使用する TCP ポート。 デフォルトポートは `9042` です。 |
| `username` | 認証のために [!DNL Cassandra] サーバーに接続するために使用するユーザー名。 |
| `password` | 認証のために [!DNL Cassandra] サーバーに接続するためのパスワード。 |
| `connectionSpec.id` | 接続の作成に必要な一意の ID。 [!DNL Cassandra] の接続仕様 ID は `a8f4d393-1a6b-43f3-931f-91a16ed857f4` です。 |

基本について詳しくは、[&#x200B; この Cassandra ドキュメント &#x200B;](https://cassandra.apache.org/doc/latest/operating/security.html#authentication) を参照してください。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Experience Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* Authorization： Bearer `{ACCESS_TOKEN}`
* x-api-key： `{API_KEY}`
* x-gw-ims-org-id： `{ORG_ID}`

[!DNL Flow Service] に属するリソースを含む [!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。 [!DNL Experience Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name：`{SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* Content-Type: `application/json`

## 接続の作成

接続ではソースを指定し、そのソースの資格情報が含まれます。 複数のソースコネクタを作成して異なるデータを取り込むために使用できるので、[!DNL Cassandra] アカウントごとに必要なコネクタは 1 つだけです。

**API 形式**

```http
POST /connections
```

**リクエスト**

[!DNL Cassandra] 接続を作成するには、POST リクエストの一部として一意の接続仕様 ID を指定する必要があります。 [!DNL Cassandra] の接続仕様 ID は `a8f4d393-1a6b-43f3-931f-91a16ed857f4` です。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `auth.params.host` | [!DNL Cassandra] サーバーの IP アドレスまたはホスト名。 |
| `auth.params.port` | クライアント接続をリッスンするために [!DNL Cassandra] サーバーが使用する TCP ポート。 デフォルトポートは `9042` です。 |
| `auth.params.username` | 認証のために [!DNL Cassandra] サーバーに接続するために使用するユーザー名。 |
| `auth.params.password` | 認証のために [!DNL Cassandra] サーバーに接続するためのパスワード。 |
| `connectionSpec.id` | [!DNL Cassandra] 接続仕様 ID：`a8f4d393-1a6b-43f3-931f-91a16ed857f4`。 |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "ce69aa89-1baa-4054-a9aa-891baa605425",
    "etag": "\"5a026e19-0000-0200-0000-5eac76d00000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Cassandra] 接続を作成し、接続の一意の ID 値を取得しました。 次のチュートリアルでは、この ID を使用した [Flow Service API を使用したデータベースの調査 &#x200B;](../../explore/database-nosql.md) 方法を説明します。
