---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用してApache HDFSコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 855f543a1cef394d121502f03471a60b97eae256
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 2%

---


# Flow Service APIを使用してApache HDFSコネクタを作成する

>[!NOTE]
>Apache HDFSコネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

フローサービスは、さまざまな異なるソースから顧客データを収集および一元化してAdobe Experience Platformに導くために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、Flow Service APIを使用して、Apache Hadoop分散ファイルシステム（以下「HDFS」と呼ばれる）をExperience Platformに接続する手順を順を追って説明します。

## はじめに

このガイドでは、次のAdobe Experience Platformのコンポーネントについて、十分に理解している必要があります。

* [ソース](../../../../home.md): Experience Platformを使用すると、Platformサービスを使用して、様々なソースからデータを取り込み、データの構造、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): Experience Platformは、1つのPlatformインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

Flow Service APIを使用してHDFSに正常に接続するために必要な追加情報については、以下の節で説明します。

### 必要な資格情報の収集

| Credential | 説明 |
| ---------- | ----------- |
| `url` | URLは、HDFSへの接続に必要な認証パラメーターを匿名で定義します。 この値の取得方法の詳細については、 [このHDFSドキュメントを参照してください](https://hadoop.apache.org/docs/r1.2.1/HttpAuthentication.html)。 |
| `connectionSpec.id` | 接続を作成するために必要な識別子。 HDFSの接続仕様IDは修正され `54e221aa-d342-4707-bcff-7a4bceef0001`ました。 |

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例 [の読み方に関する節](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照してください。

### 必要なヘッダーの値の収集

PlatformAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../../../../tutorials/authentication.md)。 次に示すように、Experience PlatformAPIのすべての呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

フローサービスに属するリソースを含む、Experience Platform内のすべてのリソースは、特定の仮想サンドボックスに分離されます。 PlatformAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* Content-Type: `application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できるため、HDFSアカウントごとに必要な接続は1つだけです。

**API形式**

```http
POST /connections
```

**リクエスト**

次の要求は、ペイロードで提供されるプロパティによって設定された、新しいHDFS接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "HDFS test connection",
        "description": "A test connection for an HDFS source",
        "auth": {
            "specName": "Anonymous Authentication",
            "params": {
                "url": "{URL}"
                }
        },
        "connectionSpec": {
            "id": "54e221aa-d342-4707-bcff-7a4bceef0001",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.url` | HDFSへの匿名接続に必要な認証パラメーターを定義するURLです |
| `connectionSpec.id` | HDFS接続仕様ID: `54e221aa-d342-4707-bcff-7a4bceef0001`. |

**応答**

正常な応答は、新たに作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "6a6a880a-2b15-4051-aa88-0a2b1570516d",
    "etag": "\"1801bb7d-0000-0200-0000-5ed6ad580000\""
}
```

## 次の手順

このチュートリアルに従うと、Flow Service APIを使用してHDFS接続を作成し、接続の一意のID値を取得したことになります。 このIDは、Flow Service APIを使用してサードパーティのクラウドストレージを [調査する方法を学習する際に、次のチュートリアルで使用できます](../../explore/cloud-storage.md)。
