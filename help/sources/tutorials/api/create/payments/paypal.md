---
keywords: Experience Platform；ホーム；人気のあるトピック；PayPalコネクタ；Paypal;Paypal
solution: Experience Platform
title: Flow Service APIを使用したPayPalソース接続の作成
topic-legacy: overview
type: Tutorial
description: Flow Service APIを使用してPayPalをAdobe Experience Platformに接続する方法を説明します。
exl-id: 5e6ca7b4-5e2f-4706-a339-ac159e2e0938
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '576'
ht-degree: 25%

---

# [!DNL Flow Service] APIを使用して[!DNL PayPal]ソース接続を作成する

>[!NOTE]
>
>[!DNL PayPal]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL PayPal]をExperience Platformに接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] 単一のプラットフォームインスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

[!DNL Flow Service] APIを使用して[!DNL PayPal]に正しく接続するために知っておく必要のある追加情報については、以下の節で説明します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL PayPal]と接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | [!DNL PayPal]インスタンスのURL。 (デフォルト：api.sandbox.paypal.com)。 |
| `clientId` | [!DNL PayPal]アプリケーションに関連付けられているクライアントID。 |
| `clientSecret` | [!DNL PayPal]アプリケーションに関連付けられているクライアントシークレット。 |
| `connectionSpec.id` | 接続を作成するために必要な一意の識別子。 [!DNL PayPal]の接続指定IDは次のとおりです。`221c7626-58f6-4eec-8ee2-042b0226f03b` |

使い始めについて詳しくは、[このPayPalドキュメント](https://developer.paypal.com/docs/api/overview/#get-credentials)を参照してください。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

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

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するのに使用できるため、[!DNL PayPal]アカウントごとに1つの接続のみが必要です。

**API 形式**

```http
POST /connections
```

**リクエスト**

[!DNL PayPal]接続を作成するには、POST要求の一部として一意の接続指定IDを指定する必要があります。 [!DNL PayPal]の接続指定IDは`221c7626-58f6-4eec-8ee2-042b0226f03b`です。

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
| `auth.params.host` | [!DNL PayPal]インスタンスのURL。 |
| `auth.params.clientId` | [!DNL PayPal]インスタンスに関連付けられているクライアントID。 |
| `auth.params.clientSecret` | [!DNL PayPal]インスタンスに関連付けられているクライアントシークレット。 |
| `connectionSpec.id` | [!DNL PayPal]接続指定ID:`221c7626-58f6-4eec-8ee2-042b0226f03b`. |

**応答**

正常に応答すると、新たに作成された接続が返されます。この接続には、一意の接続識別子(`id`)が含まれます。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "24151d58-ffa7-4960-951d-58ffa7396097",
    "etag": "\"65015e9d-0000-0200-0000-5e89162d0000\""
}
```

## 次の手順

このチュートリアルに従うと、[!DNL Flow Service] APIを使用して[!DNL PayPal]接続を作成し、接続の一意のID値を取得したことになります。 このIDは、Flow Service API ](../../explore/payments.md)を使用して[支払い申込みを調査する方法を学習する際に、次のチュートリアルで使用できます。
