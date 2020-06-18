---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用してAzure Fileストレージコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: c843ebb72ee3f1e8d2233dd2be4021403417813b
workflow-type: tm+mt
source-wordcount: '626'
ht-degree: 1%

---


# Flow Service APIを使用してAzure Fileストレージコネクタを作成する

>[!NOTE]
>Azure Fileストレージコネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

フローサービスは、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集および一元化するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、Flow Service APIを使用して、Azure FileストレージをExperience Platformに接続する手順を順を追って説明します。

## はじめに

このガイドでは、次のAdobe Experience Platformのコンポーネントについて、十分に理解している必要があります。

* [ソース](../../../../home.md): Experience Platformを使用すると、Platformサービスを使用して、様々なソースからデータを取り込み、データの構造、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): Experience Platformは、1つのPlatformインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

Flow Service APIを使用してAzure Fileストレージに正常に接続するために必要な追加情報については、以下の節で説明します。

### 必要な資格情報の収集

Flow ServiceがAzure Fileストレージと接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | アクセスするAzure Fileストレージインスタンスのエンドポイントです。 |
| `userId` | Azure Fileストレージエンドポイントへの十分なアクセス権を持つユーザー。 |
| `password` | Azure Fileストレージインスタンスのパスワード |
| 接続指定ID | 接続を作成するために必要な一意の識別子。 Azure Fileストレージの接続仕様ID: `be5ec48c-5b78-49d5-b8fa-7c89ec4569b8` |

開始方法の詳細については、 [このAzure Fileストレージドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-use-files-windows)。

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

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するために使用できるAzure Fileストレージアカウントごとに1つの接続のみが必要です。

**API形式**

```http
POST /connections
```

**リクエスト**

次の要求は、ペイロードで提供されるプロパティで構成された新しいAzure Fileストレージ接続を作成します。


```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
        -d '{
        "name": "Azure File Storage connection",
        "description": "An Azure File Storage test connection",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                    "host": "{HOST}",
                    "userId": "{USER_ID}",
                    "password": "{PASSWORD}"
                }
        },
        "connectionSpec": {
            "id": "be5ec48c-5b78-49d5-b8fa-7c89ec4569b8",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.host` | アクセスするAzure Fileストレージインスタンスのエンドポイントです。 |
| `auth.params.userId` | Azure Fileストレージエンドポイントへの十分なアクセス権を持つユーザー。 |
| `auth.params.password` | Azure Fileストレージアクセスキー。 |
| `connectionSpec.id` | Azureファイルストレージ接続の指定ID: `be5ec48c-5b78-49d5-b8fa-7c89ec4569b8`. |

**応答**

正常な応答は、新たに作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "f9377f50-607a-4818-b77f-50607a181860",
    "etag": "\"2f0276fa-0000-0200-0000-5eab3abb0000\""
}
```

## 次の手順

このチュートリアルに従うと、Flow Service APIを使用してAzure Fileストレージ接続を作成し、接続の一意のID値を取得します。 このIDは、Flow Service APIを使用してサードパーティのクラウドストレージを [調査する方法を学習する際に、次のチュートリアルで使用できます](../../explore/cloud-storage.md)。
