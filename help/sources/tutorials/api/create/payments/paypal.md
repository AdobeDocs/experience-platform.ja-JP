---
keywords: Experience Platform;home;popular topics;PayPal connector;paypal;Paypal
solution: Experience Platform
title: Flow Service APIを使用してPayPalコネクタを作成する
topic: overview
type: Tutorial
description: このチュートリアルでは、Flow Service APIを使用して、PayPalをExperience Platformに接続する手順を順を追って説明します。
translation-type: tm+mt
source-git-commit: 97dfd3a9a66fe2ae82cec8954066bdf3b6346830
workflow-type: tm+mt
source-wordcount: '579'
ht-degree: 21%

---


# APIを使用して [!DNL PayPal][!DNL Flow Service] コネクタを作成する

>[!NOTE]
>
>コネクタ [!DNL PayPal] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、 [!DNL Flow Service] APIを使用してExperience Platformに接続する手順を順を追って説明 [!DNL PayPal] します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] 単一のプラットフォームインスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

The following sections provide additional information that you will need to know in order to successfully connect to [!DNL PayPal] using the [!DNL Flow Service] API.

### 必要な資格情報の収集

と接続 [!DNL Flow Service] するには、次の接続プロパティの値を指定する必要があ [!DNL PayPal]ります。

| Credential | 説明 |
| ---------- | ----------- |
| ホスト | The URL of the [!DNL PayPal] instance. (デフォルト：api.sandbox.paypal.com)。 |
| クライアント ID | アプリケーションに関連付けられているクライアントID [!DNL PayPal] 。 |
| クライアントシークレット | アプリケーションに関連付けられているクライアントシークレット [!DNL PayPal] です。 |
| 接続指定ID | 接続を作成するために必要な一意の識別子。 の接続指定ID [!DNL PayPal] は次のとおりです。 `221c7626-58f6-4eec-8ee2-042b0226f03b` |

使い始める前に詳しくは、 [このPayPalドキュメントを参照してください](https://developer.paypal.com/docs/api/overview/#get-credentials)。

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

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込む複数のソースコネクタを作成する場合に使用できるので、 [!DNL PayPal] アカウントごとに必要な接続は1つだけです。

**API 形式**

```http
POST /connections
```

**リクエスト**

接続を作成するには、その [!DNL PayPal] 一意の接続指定IDをPOST要求の一部として指定する必要があります。 の接続指定ID [!DNL PayPal] はです `221c7626-58f6-4eec-8ee2-042b0226f03b`。

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
| `auth.params.host` | The URL of the [!DNL PayPal] instance. |
| `auth.params.clientId` | インスタンスに関連付けられているクライアントID [!DNL PayPal] 。 |
| `auth.params.clientSecret` | インスタンスに関連付けられているクライアントシークレット [!DNL PayPal] 。 |
| `connectionSpec.id` | 接続 [!DNL PayPal] 指定ID: `221c7626-58f6-4eec-8ee2-042b0226f03b`. |

**応答** 

正常な応答は、新たに作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "24151d58-ffa7-4960-951d-58ffa7396097",
    "etag": "\"65015e9d-0000-0200-0000-5e89162d0000\""
}
```

## 次の手順

このチュートリアルに従うことで、 [!DNL PayPal] APIを使用して [!DNL Flow Service] 接続を作成し、接続の一意のID値を取得しました。 フローサービスAPIを使用した支払い申請の [調査方法を学ぶ際に、次のチュートリアルでこのIDを使用できます](../../explore/payments.md)。
