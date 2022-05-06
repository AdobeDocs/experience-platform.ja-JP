---
keywords: Experience Platform；ホーム；人気のあるトピック；Apache Cassandra;apache cassandra;Cassandra;cassandra
solution: Experience Platform
title: フローサービス API を使用した Apache Cassandra ソース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用して Apache Cassandra をAdobe Experience Platformに接続する方法を説明します。
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '620'
ht-degree: 41%

---


# の作成 [!DNL Apache Cassandra] を使用したソース接続 [!DNL Flow Service] API

[!DNL Flow Service] は、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用されます。 このサービスは、ユーザーインターフェイスと RESTful API を提供し、サポートされるすべてのソースから接続できます。

このチュートリアルでは、 [!DNL Flow Service] 接続手順を説明する API [!DNL Apache Cassandra] （以下「カッサンドラ」という。） [!DNL Experience Platform].

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

以下の節では、 [!DNL Flow Service] API

### 必要な認証情報の収集

[!DNL Flow Service] を [!DNL Cassandra] に接続するには、次の接続プロパティの値を指定する必要があります。

| 認証情報 | 説明 |
| ---------- | ----------- |
| `host` | の IP アドレスまたはホスト名 [!DNL Cassandra] サーバー。 |
| `port` | TCP ポート [!DNL Cassandra] サーバーは、を使用してクライアント接続をリッスンします。 デフォルトのポートは `9042`. |
| `username` | に接続するために使用するユーザー名 [!DNL Cassandra] 認証用のサーバ。 |
| `password` | に接続するパスワード [!DNL Cassandra] 認証用のサーバ。 |
| `connectionSpec.id` | 接続の作成に必要な一意の識別子。 の接続仕様 ID [!DNL Cassandra] が `a8f4d393-1a6b-43f3-931f-91a16ed857f4`. |

の導入について詳しくは、 [この Cassandra ドキュメント](https://cassandra.apache.org/doc/latest/operating/security.html#authentication).

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* Authorization： Bearer `{ACCESS_TOKEN}`
* x-api-key： `{API_KEY}`
* x-gw-ims-org-id： `{ORG_ID}`

のすべてのリソース [!DNL Experience Platform]( [!DNL Flow Service]は、特定の仮想サンドボックスに分離されています。 [!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name：`{SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* Content-Type: `application/json`

## 接続の作成

接続では、ソースを指定し、そのソースの資格情報を含めます。 1 つにつき 1 つのコネクタのみが必要です [!DNL Cassandra] アカウントを使用して複数のソースコネクタを作成し、異なるデータを取り込むことができます。

**API 形式**

```http
POST /connections
```

**リクエスト**

を作成するために、 [!DNL Cassandra] 接続の場合、一意の接続仕様 ID を接続リクエストの一部として指定する必要があります。POST の接続仕様 ID [!DNL Cassandra] が `a8f4d393-1a6b-43f3-931f-91a16ed857f4`.

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
| `auth.params.host` | の IP アドレスまたはホスト名 [!DNL Cassandra] サーバー。 |
| `auth.params.port` | TCP ポート [!DNL Cassandra] サーバーは、を使用してクライアント接続をリッスンします。 デフォルトのポートは `9042`. |
| `auth.params.username` | に接続するために使用するユーザー名 [!DNL Cassandra] 認証用のサーバ。 |
| `auth.params.password` | に接続するパスワード [!DNL Cassandra] 認証用のサーバ。 |
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

このチュートリアルに従って、 [!DNL Cassandra] を使用した接続 [!DNL Flow Service] API を介して取得され、接続の一意の ID 値を取得している。 この ID は、次のチュートリアルで、 [フローサービス API を使用したデータベースの調査](../../explore/database-nosql.md).
