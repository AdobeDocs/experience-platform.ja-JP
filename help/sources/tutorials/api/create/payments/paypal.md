---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: フローサービスAPIを使用したPayPalコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: 34334a6ff5a3f0c16ad32b4d0438d4ee8513372f

---


# フローサービスAPIを使用したPayPalコネクタの作成

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、サポートされるすべてのソースを接続できるユーザーインターフェイスとRESTful APIを提供します。

このチュートリアルでは、フローサービスAPIを使用して、PayPalをエクスペリエンスプラットフォームに接続する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントに関する作業を理解している必要があります。

* [資料](../../../../home.md):エクスペリエンスプラットフォームを使用すると、様々なソースからデータを取り込みながら、プラットフォームサービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

次の節では、フローサービスAPIを使用してPayPalに正常に接続するために知っておく必要のある追加情報を示します。

### 必要な資格情報の収集

フローサービスがPayPalと接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| ホスト | PayPalインスタンスのURL。 (デフォルト：api.sandbox.paypal.com)を参照してください。 |
| クライアントID | PayPalアプリケーションに関連付けられているクライアントID。 |
| クライアントシークレット | PayPalアプリケーションに関連付けられているクライアントシークレット。 |
| 接続指定ID | 接続の作成に必要な一意の識別子。 PayPalの接続指定ID: `221c7626-58f6-4eec-8ee2-042b0226f03b` |

使い始めについて詳しくは、このPayPalのドキュメントを参照し [てくださ](https://developer.paypal.com/docs/api/overview/#get-credentials)い。

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

接続はソースを指定し、そのソースの資格情報を含みます。 PayPalアカウントごとに必要な接続は1つだけです。異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できます。

**API形式**

```http
POST /connections
```

**リクエスト**

PayPal接続を作成するには、POSTリクエストの一部として一意の接続指定IDを指定する必要があります。 PayPalの接続指定IDはです `221c7626-58f6-4eec-8ee2-042b0226f03b`。

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

成功した応答は、新しく作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "24151d58-ffa7-4960-951d-58ffa7396097",
    "etag": "\"65015e9d-0000-0200-0000-5e89162d0000\""
}
```

## 次の手順

このチュートリアルに従うと、フローサービスAPIを使用してPayPal接続を作成し、接続の一意のID値を取得したことになります。 このIDは、次のチュートリアルで、フローサービスAPIを使用して支払い申 [込みを調査する方法を学ぶ際に使用できます](../../explore/payments.md)。
