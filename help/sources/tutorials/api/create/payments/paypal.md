---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用してPayPalコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 7328226b8349ffcdddadbd27b74fc54328b78dc5
workflow-type: tm+mt
source-wordcount: '604'
ht-degree: 2%

---


# Flow Service APIを使用してPayPalコネクタを作成する

>[!NOTE]
>PayPalコネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

フローサービスは、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集および一元化するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、Flow Service APIを使用して、PayPalをExperience Platformに接続する手順を順を追って説明します。

## はじめに

このガイドでは、次のAdobe Experience Platformのコンポーネントについて、十分に理解している必要があります。

* [ソース](../../../../home.md): Experience Platformを使用すると、Platformサービスを使用して、様々なソースからデータを取り込み、データの構造、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): Experience Platformは、1つのPlatformインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

Flow Service APIを使用してPayPalに正しく接続するために必要な追加情報については、以下の節で説明します。

### 必要な資格情報の収集

フローサービスがPayPalと接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| ホスト | PayPalインスタンスのURL。 (デフォルト： api.sandbox.paypal.com)。 |
| クライアントID | PayPalアプリケーションに関連付けられているクライアントID。 |
| クライアントシークレット | PayPalアプリケーションに関連付けられているクライアントシークレット。 |
| 接続指定ID | 接続を作成するために必要な一意の識別子。 PayPalの接続仕様IDは次のとおりです。 `221c7626-58f6-4eec-8ee2-042b0226f03b` |

使い始める前に詳しくは、 [このPayPalドキュメントを参照してください](https://developer.paypal.com/docs/api/overview/#get-credentials)。

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

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できるので、PayPalアカウントごとに必要な接続は1つだけです。

**API形式**

```http
POST /connections
```

**リクエスト**

PayPal接続を作成するには、POSTリクエストの一部として一意の接続指定IDを指定する必要があります。 PayPalの接続指定IDはで `221c7626-58f6-4eec-8ee2-042b0226f03b`す。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Paypal connection",
        "description": "Paypal connection",
        "auth": {
        "specName": "Client-Id-Secret Based Authentication",
        "params": {
            "host": "{HOST}",
            "clientId": "{CLIENT_ID}",
            "clientSecret": "{CLIENT_SECRET}"
            }
        },
        "connectionSpec": {
            "id": "221c7626-58f6-4eec-8ee2-042b0226f03b",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.host` | PayPalインスタンスのURL。 |
| `auth.params.clientId` | PayPalインスタンスに関連付けられているクライアントID。 |
| `auth.params.clientSecret` | PayPalインスタンスに関連付けられているクライアントシークレット。 |
| `connectionSpec.id` | PayPal接続指定ID: `221c7626-58f6-4eec-8ee2-042b0226f03b`. |

**応答**

正常な応答は、新たに作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "24151d58-ffa7-4960-951d-58ffa7396097",
    "etag": "\"65015e9d-0000-0200-0000-5e89162d0000\""
}
```

## 次の手順

このチュートリアルに従うと、Flow Service APIを使用してPayPal接続を作成し、接続の一意のID値を取得したことになります。 フローサービスAPIを使用した支払い申請の [調査方法を学ぶ際に、次のチュートリアルでこのIDを使用できます](../../explore/payments.md)。
