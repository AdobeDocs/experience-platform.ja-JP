---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用してAzure HDInsights ConnectorでApache Sparkを作成する
topic: overview
translation-type: tm+mt
source-git-commit: fc5cdaa661c47e14ed5412868f3a54fd7bd2b451
workflow-type: tm+mt
source-wordcount: '562'
ht-degree: 2%

---


# [!DNL Apache Spark] APIを使用したHDInsightsコネクター [!DNL Azure][!DNL Flow Service] 上での作成

>[!NOTE]
>オン [!DNL Apache Spark] の [!DNL Azure HDInsights] コネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、 [!DNL Flow Service] APIを使用して、に接続する手順 [!DNL Apache Spark] (以下「 [!DNL Azure HDInsights] 」と呼ばれる)を順を追って説明し[!DNL Spark][!DNL Experience Platform]ます。

## はじめに

このガイドでは、次のAdobe Experience Platformのコンポーネントについて、十分に理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、 [!DNL Spark][!DNL Flow Service] APIを使用してに正常に接続するために知っておく必要がある追加情報について説明します。

### 必要な資格情報の収集

と接続 [!DNL Flow Service] するには、次の接続プロパティの値を指定する必要があ [!DNL Spark]ります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | サー [!DNL Spark] バーのIPアドレスまたはホスト名。 |
| `username` | サー [!DNL Spark] バーへのアクセスに使用するユーザー名です。 |
| `password` | ユーザーに対応するパスワード。 |
| `connectionSpec.id` | 接続を作成するために必要な一意の識別子。 の接続指定ID [!DNL Spark] は次のとおりです。 `6a8d82bc-1caf-45d1-908d-cadabc9d63a6` |

使い始める方法の詳細については、 [このSparkドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/hdinsight/spark/apache-spark-overview)。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

### 必要なヘッダーの値の収集

APIを呼び出すには、まず [!DNL Platform] 認証チュートリアルを完了する必要があり [ます](../../../../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

に属するリソース [!DNL Experience Platform]を含む、のすべてのリソースは、特定の仮想サンドボックスに分離され [!DNL Flow Service]ます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* Content-Type: `application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込む複数のソースコネクタを作成する場合に使用できるので、 [!DNL Spark] アカウントごとに必要な接続は1つだけです。

**API形式**

```http
POST /connections
```

**リクエスト**

接続を作成するには、その [!DNL Spark] 一意の接続指定IDをPOSTリクエストの一部として指定する必要があります。 の接続指定ID [!DNL Spark] はです `6a8d82bc-1caf-45d1-908d-cadabc9d63a6`。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Spark test connection",
        "description": "A Spark test connection",
        "auth": {
            "specName": "HDInsights Basic Authentication",
        "params": {
            "host" :  "{HOST}",
            "username" : "{USERNAME}",
            "password" :"{PASSWORD}"
            }
        },
        "connectionSpec": {
            "id": "6a8d82bc-1caf-45d1-908d-cadabc9d63a6",
            "version": "1.0"
        }
    }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.host` | サー [!DNL Spark] バーのホスト。 |
| `auth.params.username` | 接続に関連付けられているユーザー名 [!DNL Spark] 。 |
| `auth.params.password` | 接続に関連付けられているパスワードで [!DNL Spark] す。 |
| `connectionSpec.id` | 接続 [!DNL Spark] 指定ID: `6a8d82bc-1caf-45d1-908d-cadabc9d63a6`. |

**応答**

正常な応答は、新たに作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "a45f2f58-e3a2-46ba-9f2f-58e3a2b6baf2",
    "etag": "\"900009d6-0000-0200-0000-5e8500010000\""
}
```

## 次の手順

このチュートリアルに従うことで、 [!DNL Spark] APIを使用して [!DNL Flow Service] 接続を作成し、接続の一意のID値を取得しました。 このIDは、Flow Service APIを使用してデータベースを [調査する方法を学習する際に、次のチュートリアルで使用できます](../../explore/database-nosql.md)。