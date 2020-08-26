---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用してPhoenixコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 690ddbd92f0a2e4e06b988e761dabff399cd2367
workflow-type: tm+mt
source-wordcount: '625'
ht-degree: 19%

---


# APIを使用して [!DNL Phoenix][!DNL Flow Service] コネクタを作成する

>[!NOTE]
>
>コネクタ [!DNL Phoenix] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、 [!DNL Flow Service] APIを使用して、データベースの接続手順を順を追って説明し [!DNL Phoenix] ま [!DNL Experience Platform]す。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

The following sections provide additional information that you will need to know in order to successfully connect to [!DNL Phoenix] using the [!DNL Flow Service] API.

### 必要な資格情報の収集

と接続 [!DNL Flow Service] するには、次の接続プロパティの値を指定する必要があ [!DNL Phoenix]ります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | サー [!DNL Phoenix] バーのIPアドレスまたはホスト名。 |
| `username` | サー [!DNL Phoenix] バーへのアクセスに使用するユーザー名です。 |
| `password` | ユーザーに対応するパスワード。 |
| `port` | サー [!DNL Phoenix] バーがクライアント接続をリッスンするために使用するTCPポートです。 HDInsightsに接続する場合は、ポートを443に指定し [!DNL Azure] ます。 |
| `httpPath` | サー [!DNL Phoenix] バーに対応する部分的なURL。 HDInsightsクラスターを使用する場合は、/hbasephoenix0を指定し [!DNL Azure] ます。 |
| `enableSsl` | boolean値。 サーバーへの接続をSSLを使用して暗号化するかどうかを指定します。 |
| `connectionSpec.id` | 接続を作成するために必要な一意の識別子。 の接続指定ID [!DNL Phoenix] は次のとおりです。 `102706fb-a5cd-42ee-afe0-bc42f017ff43` |

使い始める前に詳しくは、 [このフェニックスドキュメントを参照してください](https://python-phoenixdb.readthedocs.io/en/latest/api.html)。

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

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込む複数のソースコネクタを作成する場合に使用できるので、 [!DNL Phoenix] アカウントごとに必要な接続は1つだけです。

**API 形式**

```http
POST /connections
```

**リクエスト**

接続を作成するには、その [!DNL Phoenix] 一意の接続指定IDをPOST要求の一部として指定する必要があります。 の接続指定ID [!DNL Phoenix] はです `102706fb-a5cd-42ee-afe0-bc42f017ff43`。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Phoenix test connection",
        "description": "Phoenix test connection",
        "auth": {
            "specName": "Basic Authentication",
        "params": {
            "host" :  "{HOST}",
            "username" : "{USERNAME}",
            "password" :"{PASSWORD}",
            "port" : {PORT},
            "httpPath" : "{PATH}",
            "enableSsl" : {SSL}
            }
        },
        "connectionSpec": {
            "id": "102706fb-a5cd-42ee-afe0-bc42f017ff43",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.host` | The host of the [!DNL Phoenix] server. |
| `auth.params.username` | 接続に関連付けられているユーザー名 [!DNL Phoenix] 。 |
| `auth.params.password` | 接続に関連付けられているパスワードで [!DNL Phoenix] す。 |
| `auth.params.port` | 接続用のTCPポート [!DNL Phoenix] 。 |
| `auth.params.httpPath` | 接続の部分的なhttpパス [!DNL Phoenix] です。 |
| `auth.params.enableSsl` | サーバーへの接続がSSLを使用して暗号化されるかどうかを指定するboolean値です。 |
| `connectionSpec.id` | 接続 [!DNL Phoenix] 指定ID: `102706fb-a5cd-42ee-afe0-bc42f017ff43`. |

**応答** 

正常な応答は、新たに作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "0d982fff-c443-403e-982f-ffc443f03e37",
    "etag": "\"830082dc-0000-0200-0000-5e84ee560000\""
}
```

## 次の手順

このチュートリアルに従うことで、 [!DNL Phoenix] APIを使用して [!DNL Flow Service] 接続を作成し、接続の一意のID値を取得しました。 このIDは、Flow Service APIを使用してデータベースを [調査する方法を学習する際に、次のチュートリアルで使用できます](../../explore/database-nosql.md)。