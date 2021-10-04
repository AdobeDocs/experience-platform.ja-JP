---
keywords: Experience Platform；ホーム；人気のあるトピック；Apache Cassandra;apache cassandra;Cassandra;cassandra
solution: Experience Platform
title: フローサービス API を使用した Apache Cassandra ソース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用して Apache Cassandra をAdobe Experience Platformに接続する方法を説明します。
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '620'
ht-degree: 31%

---


# [!DNL Flow Service] API を使用して [!DNL Apache Cassandra] ソース接続を作成する

[!DNL Flow Service] は、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、ユーザーインターフェイスと RESTful API を提供し、サポートされているすべてのソースから接続できます。

このチュートリアルでは、[!DNL Flow Service] API を使用して、[!DNL Apache Cassandra]（以下「Cassandra」と呼ばれます）を [!DNL Experience Platform] に接続する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して、受信データの構造化、ラベル付け、強化をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用して Cassandra に正しく接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] が [!DNL Cassandra] と接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | [!DNL Cassandra] サーバの IP アドレスまたはホスト名。 |
| `port` | [!DNL Cassandra] サーバーがクライアント接続をリッスンするために使用する TCP ポート。 デフォルトのポートは `9042` です。 |
| `username` | [!DNL Cassandra] サーバーに接続して認証を行う際に使用するユーザー名。 |
| `password` | [!DNL Cassandra] サーバーに接続して認証を行うためのパスワード。 |
| `connectionSpec.id` | 接続の作成に必要な一意の識別子。 [!DNL Cassandra] の接続仕様 ID は `a8f4d393-1a6b-43f3-931f-91a16ed857f4` です。 |

使い始める方法については、[ この Cassandra のドキュメント ](https://cassandra.apache.org/doc/latest/operating/security.html#authentication) を参照してください。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* Authorization： Bearer `{ACCESS_TOKEN}`
* x-api-key： `{API_KEY}`
* x-gw-ims-org-id： `{IMS_ORG}`

[!DNL Flow Service] に属するリソースを含む [!DNL Experience Platform] 内のすべてのリソースは、特定の仮想サンドボックスに分離されます。 [!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name： `{SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* Content-Type: `application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できるので、[!DNL Cassandra] アカウントごとに必要なコネクタは 1 つだけです。

**API 形式**

```http
POST /connections
```

**リクエスト**

[!DNL Cassandra] 接続を作成するには、接続リクエストの一部として一意の接続仕様 ID を指定する必要があります。 [!DNL Cassandra] の接続仕様 ID は `a8f4d393-1a6b-43f3-931f-91a16ed857f4` です。

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
| `auth.params.host` | [!DNL Cassandra] サーバの IP アドレスまたはホスト名。 |
| `auth.params.port` | [!DNL Cassandra] サーバーがクライアント接続をリッスンするために使用する TCP ポート。 デフォルトのポートは `9042` です。 |
| `auth.params.username` | [!DNL Cassandra] サーバーに接続して認証を行う際に使用するユーザー名。 |
| `auth.params.password` | [!DNL Cassandra] サーバーに接続して認証を行うためのパスワード。 |
| `connectionSpec.id` | [!DNL Cassandra] 接続仕様 ID:`a8f4d393-1a6b-43f3-931f-91a16ed857f4`. |

**応答**

正常な応答は、新しく作成された接続の詳細 ( 一意の識別子 (`id`) を含む ) を返します。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "ce69aa89-1baa-4054-a9aa-891baa605425",
    "etag": "\"5a026e19-0000-0200-0000-5eac76d00000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Cassandra] 接続を作成し、接続の一意の ID 値を取得しました。 この ID は、次のチュートリアルでフローサービス API](../../explore/database-nosql.md) を使用してデータベースを調べる方法を学ぶ際に使用できます。[
