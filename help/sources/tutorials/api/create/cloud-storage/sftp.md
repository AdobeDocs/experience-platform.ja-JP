---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用したSFTPコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: 855f543a1cef394d121502f03471a60b97eae256
workflow-type: tm+mt
source-wordcount: '575'
ht-degree: 2%

---


# Flow Service APIを使用したSFTPコネクタの作成

>[!NOTE]
>SFTPコネクタはベータ版です。 機能とドキュメントは、変更されることがあります。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

フローサービスは、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集および一元化するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、Flow Service APIを使用して、Experience PlatformをSFTP(Secure File Transfer Protocol)サーバーに接続する手順を順を追って説明します。

Experience Platformでユーザインターフェイスを使用したい場合は、 [UIチュートリアル](../../../ui/create/cloud-storage/ftp-sftp.md) に、同様の操作を実行するための操作手順が順を追って示されています。

## はじめに

このガイドでは、次のAdobe Experience Platformのコンポーネントについて、十分に理解している必要があります。

* [ソース](../../../../home.md): Experience Platformを使用すると、Platformサービスを使用して、様々なソースからデータを取り込み、データの構造、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): Experience Platformは、1つのPlatformインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

Flow Service APIを使用してSFTPサーバーに正常に接続するために必要な追加情報については、以下の節で説明します。

### 必要な資格情報の収集

フローサービスがSFTPに接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | SFTPサーバーに関連付けられている名前またはIPアドレス。 |
| `username` | SFTPサーバーへのアクセス権を持つユーザー名。 |
| `password` | SFTPサーバーのパスワードです。 |

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

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できるので、SFTPアカウントごとに必要な接続は1つだけです。

**API形式**

```http
POST /connections
```

**リクエスト**

```shell
curl -X POST \
    'http://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d  "auth": {
        "specName": "Basic Authentication for sftp",
        "params": {
            "host": "{HOST_NAME}",
            "userName": "{USER_NAME}",
            "password": "{PASSWORD}"
        }
    },
    "connectionSpec": {
        "id": "b7bf2577-4520-42c9-bae9-cad01560f7bc",
        "version": "1.0"
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.host` | SFTPサーバーのホスト名です。 |
| `auth.params.username` | SFTPサーバーに関連付けられているユーザー名です。 |
| `auth.params.password` | SFTPサーバーに関連付けられているパスワードです。 |
| `connectionSpec.id` | STFPサーバー接続仕様ID: `b7bf2577-4520-42c9-bae9-cad01560f7bc` |

**応答**

正常な応答は、新たに作成された接続の固有な識別子(`id`)を返します。 このIDは、次のチュートリアルでSFTPサーバーを調べるために必要です。

```json
{
    "id": "bf367b0d-3d9b-4060-b67b-0d3d9bd06094",
    "etag": "\"1700cc7b-0000-0200-0000-5e3b3fba0000\""
}
```

## 次の手順

このチュートリアルに従うことで、Flow Service APIを使用してSFTP接続を作成し、接続の一意のID値を取得したことになります。 この接続IDを使用して、Flow Service APIを使用してクラウドストレージを [調べたり、Flow Service APIを使用してパーケーデータを](../../explore/cloud-storage.md) 取り込んだりできます [](../../cloud-storage-parquet.md)。
