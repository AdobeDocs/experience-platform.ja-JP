---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用してPhoenixコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: c00edf487404e1d08bc86cb6692325f536964af6

---


# Flow Service APIを使用してPhoenixコネクタを作成する

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、サポートされるすべてのソースを接続できるユーザーインターフェイスとRESTful APIを提供します。

このチュートリアルでは、Flow Service APIを使用して、PhoenixデータベースをExperience Platformに接続する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントに関する作業を理解している必要があります。

* [資料](../../../../home.md):エクスペリエンスプラットフォームを使用すると、様々なソースからデータを取り込みながら、プラットフォームサービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

次の節では、Flow Service APIを使用してPhoenixに正常に接続するために知っておく必要のある追加情報を示します。

### 必要な資格情報の収集

フローサービスがPhoenixと接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | PhoenixサーバーのIPアドレスまたはホスト名。 |
| `username` | Phoenixサーバーにアクセスするために使用するユーザー名。 |
| `password` | ユーザーに対応するパスワード。 |
| `port` | Phoenixサーバーがクライアント接続のリッスンに使用するTCPポートです。 Azure HDInsightsに接続する場合は、ポートを443に指定します。 |
| `httpPath` | Phoenixサーバーに対応する部分的なURL。 Azure HDInsightsクラスターを使用する場合は、/hbasephoenix0を指定します。 |
| `enableSsl` | boolean値。 サーバーへの接続をSSLを使用して暗号化するかどうかを指定します。 |
| `connectionSpec.id` | 接続の作成に必要な一意の識別子。 Phoenixの接続仕様IDは次のとおりです。 `102706fb-a5cd-42ee-afe0-bc42f017ff43` |

使い始めの詳細は、このフェニックス [ドキュメント](https://python-phoenixdb.readthedocs.io/en/latest/api.html)。

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

接続はソースを指定し、そのソースの資格情報を含みます。 Phoenixアカウントごとに1つの接続のみが必要です。異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できます。

**API形式**

```http
POST /connections
```

**リクエスト**

Phoenix接続を作成するには、POST要求の一部として一意の接続指定IDを指定する必要があります。 フェニックスの接続仕様IDはです `102706fb-a5cd-42ee-afe0-bc42f017ff43`。

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
| `auth.params.host` | フェニックス・サーバのホスト。 |
| `auth.params.username` | Phoenix接続に関連付けられているユーザー名。 |
| `auth.params.password` | Phoenix接続に関連付けられているパスワードです。 |
| `auth.params.port` | Phoenix接続用のTCPポート。 |
| `auth.params.httpPath` | Phoenix接続の部分的なhttpパス。 |
| `auth.params.enableSsl` | サーバーへの接続がSSLを使用して暗号化されるかどうかを指定するboolean値です。 |
| `connectionSpec.id` | フェニックス接続の仕様ID: `102706fb-a5cd-42ee-afe0-bc42f017ff43`. |

**応答**

成功した応答は、新しく作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "0d982fff-c443-403e-982f-ffc443f03e37",
    "etag": "\"830082dc-0000-0200-0000-5e84ee560000\""
}
```

## 次の手順

このチュートリアルに従うと、Flow Service APIを使用してPhoenix接続を作成し、接続の一意のID値を取得したことになります。 このIDは、次のチュートリアルで、Flow Service APIを使用してデータベースを [探索する方法を学ぶ際に使用できます](../../explore/database-nosql.md)。