---
keywords: Experience Platform；ホーム；人気の高いトピック；フェニックス；フェニックス
solution: Experience Platform
title: Flow Service APIを使用したPhoenixソース接続の作成
topic-legacy: overview
type: Tutorial
description: Flow Service APIを使用してPhoenixデータベースをAdobe Experience Platformに接続する方法を説明します。
exl-id: b69d9593-06fe-4fff-88a9-7860e4e45eb7
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '648'
ht-degree: 22%

---

# [!DNL Flow Service] APIを使用して[!DNL Phoenix]ソース接続を作成する

>[!NOTE]
>
>[!DNL Phoenix]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL Phoenix]データベースを[!DNL Experience Platform]に接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

[!DNL Flow Service] APIを使用して[!DNL Phoenix]に正しく接続するために知っておく必要のある追加情報については、以下の節で説明します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Phoenix]と接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | [!DNL Phoenix]サーバーのIPアドレスまたはホスト名。 |
| `username` | [!DNL Phoenix]サーバーへのアクセスに使用するユーザー名です。 |
| `password` | ユーザーに対応するパスワード。 |
| `port` | [!DNL Phoenix]サーバーがクライアント接続をリッスンするために使用するTCPポート。 [!DNL Azure] HDInsightsに接続する場合は、ポートを443に指定します。 |
| `httpPath` | [!DNL Phoenix]サーバーに対応する部分的なURLです。 [!DNL Azure] HDInsightsクラスターを使用する場合は、/hbasephoenix0を指定します。 |
| `enableSsl` | boolean値。 サーバーへの接続をSSLを使用して暗号化するかどうかを指定します。 |
| `connectionSpec.id` | 接続を作成するために必要な一意の識別子。 [!DNL Phoenix]の接続指定IDは次のとおりです。`102706fb-a5cd-42ee-afe0-bc42f017ff43` |

使い始めについての詳細は、[このフェニックスドキュメント](https://python-phoenixdb.readthedocs.io/en/latest/api.html)を参照してください。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。サンプル API 呼び出しのドキュメントで使用されている規則については、[!DNL Experience Platform] トラブルシューテングガイドの[サンプル API 呼び出しの読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

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

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するのに使用できるため、[!DNL Phoenix]アカウントごとに1つの接続のみが必要です。

**API 形式**

```http
POST /connections
```

**リクエスト**

[!DNL Phoenix]接続を作成するには、POST要求の一部として一意の接続指定IDを指定する必要があります。 [!DNL Phoenix]の接続指定IDは`102706fb-a5cd-42ee-afe0-bc42f017ff43`です。

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
| `auth.params.host` | [!DNL Phoenix]サーバーのホスト。 |
| `auth.params.username` | [!DNL Phoenix]接続に関連付けられているユーザー名。 |
| `auth.params.password` | [!DNL Phoenix]接続に関連付けられているパスワードです。 |
| `auth.params.port` | [!DNL Phoenix]接続用のTCPポート。 |
| `auth.params.httpPath` | [!DNL Phoenix]接続の部分的なhttpパスです。 |
| `auth.params.enableSsl` | サーバーへの接続がSSLを使用して暗号化されるかどうかを指定するboolean値です。 |
| `connectionSpec.id` | [!DNL Phoenix]接続指定ID:`102706fb-a5cd-42ee-afe0-bc42f017ff43`. |

**応答**

正常に応答すると、新たに作成された接続の詳細(一意の識別子(`id`)が返されます。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "0d982fff-c443-403e-982f-ffc443f03e37",
    "etag": "\"830082dc-0000-0200-0000-5e84ee560000\""
}
```

## 次の手順

このチュートリアルに従うと、[!DNL Flow Service] APIを使用して[!DNL Phoenix]接続を作成し、接続の一意のID値を取得したことになります。 このIDは、Flow Service API ](../../explore/database-nosql.md)を使用して[データベースを調査する方法を学習する際に、次のチュートリアルで使用できます。
