---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用して、Azure HDInsightsコネクタ上にApache Sparkを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 2fd9f38673750af705021d1e8f160be9304039a0

---


# Flow Service APIを使用して、Azure HDInsightsコネクタ上にApache Sparkを作成する

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、サポートされるすべてのソースを接続できるユーザーインターフェイスとRESTful APIを提供します。

このチュートリアルでは、フローサービスAPIを使用して、Azure HDInsights上のApache Spark（以下「Spark」と呼ばれる）をExperience Platformに接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントに関する作業を理解している必要があります。

* [資料](../../../../home.md):エクスペリエンスプラットフォームを使用すると、様々なソースからデータを取り込みながら、プラットフォームサービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

次の節では、Flow Service APIを使用してSparkに正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

フローサービスがSparkと接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | SparkサーバーのIPアドレスまたはホスト名。 |
| `username` | Sparkサーバーへのアクセスに使用するユーザー名です。 |
| `password` | ユーザーに対応するパスワード。 |
| `connectionSpec.id` | 接続の作成に必要な一意の識別子。 Sparkの接続指定IDは次のとおりです。 `6a8d82bc-1caf-45d1-908d-cadabc9d63a6` |

使い始める方法の詳細については、このSparkの [ドキュメント](https://docs.microsoft.com/en-us/azure/hdinsight/spark/apache-spark-overview)。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を示します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../../../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

* 認証：無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

フローサービスに属するリソースを含む、エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* コンテンツタイプ： `application/json`

## 接続の作成

接続はソースを指定し、そのソースの資格情報を含みます。 Sparkアカウントごとに必要な接続は1つだけで、異なるデータを取り込むために複数のソースコネクタを作成するのに使用できます。

**API形式**

```http
POST /connections
```

**リクエスト**

Spark接続を作成するには、その一意の接続指定IDをPOST要求の一部として指定する必要があります。 Sparkの接続指定IDはです `6a8d82bc-1caf-45d1-908d-cadabc9d63a6`。

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
| `auth.params.host` | Sparkサーバーのホストです。 |
| `auth.params.username` | Spark接続に関連付けられているユーザー名。 |
| `auth.params.password` | Spark接続に関連付けられているパスワードです。 |
| `connectionSpec.id` | Spark接続指定ID: `6a8d82bc-1caf-45d1-908d-cadabc9d63a6`. |

**応答**

成功した応答は、新しく作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "a45f2f58-e3a2-46ba-9f2f-58e3a2b6baf2",
    "etag": "\"900009d6-0000-0200-0000-5e8500010000\""
}
```

## 次の手順

このチュートリアルに従うと、Flow Service APIを使用してSpark接続を作成し、接続の一意のID値を取得したことになります。 このIDは、次のチュートリアルで、Flow Service APIを使用してデータベースを [探索する方法を学ぶ際に使用できます](../../explore/database-nosql.md)。