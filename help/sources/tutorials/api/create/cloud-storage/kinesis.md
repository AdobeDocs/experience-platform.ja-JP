---
keywords: Experience Platform;home;popular topics;Kinesis;kinesis;Amazon Kinesis;amazon kinesis
solution: Experience Platform
title: Flow Service APIを使用してAmazonKinesisコネクタを作成する
topic: overview
type: Tutorial
description: このチュートリアルでは、Flow Service APIを使用して、Experience PlatformをAmazonKinesisアカウントに接続する手順を順を追って説明します。
translation-type: tm+mt
source-git-commit: 967585ba078edd13f90c820f6b1a0490140ca0cf
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 21%

---


# Flow Service APIを使用して [!DNL Amazon Kinesis] コネクタを作成する

>[!NOTE]
>
>コネクタ [!DNL Amazon Kineses] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、 [!DNL Flow Service] APIを使用して、アカウントに接続する手順を順を追っ [!DNL Experience Platform] て説明し [!DNL Amazon Kinesis] ます。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

The following sections provide additional information that you will need to know in order to successfully connect to an [!DNL Amazon Kinesis] account using the [!DNL Flow Service] API.

### 必要な資格情報の収集

アカウント [!DNL Flow Service] に接続するには、次の接続プロパティの値を指定する必要があり [!DNL Amazon Kinesis] ます。

| Credential | 説明 |
| ---------- | ----------- |
| `accessKeyId` | アカウントのアクセスキーID [!DNL Kinesis] 。 |
| `secretKey` | アカウントの秘密アクセスキー [!DNL Kinesis] 。 |
| `region` | アカウントの地域 [!DNL Kinesis] です。 |
| `connectionSpec.id` | 接続 [!DNL Kinesis] 指定ID: `86043421-563b-46ec-8e6c-e23184711bf6` |

これらの値の詳細については、 [このKinesisドキュメントを参照してください](https://docs.aws.amazon.com/streams/latest/dev/getting-started.html)。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../../../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

All resources in [!DNL Experience Platform], including those belonging to the [!DNL Flow Service], are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込む複数のソースコネクタを作成する場合に使用できるので、 [!DNL Amazon Kinesis] アカウントごとに必要な接続は1つだけです。

**API 形式**

```http
POST /connections
```

**リクエスト**

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Amazon Kinesis connection",
        "description": "Connector for Amazon Kinesis",
        "auth": {
            "specName": "Basic Authentication for Kinesis",
            "params": {
                "accessKeyId": "accessKeyId",
                "secretKey": "secretKey"
            }
        },
        "connectionSpec": {
            "id": "86043421-563b-46ec-8e6c-e23184711bf6",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.accessKeyId` | アカウントのアクセスキーID [!DNL Kinesis] 。 |
| `auth.params.secretKey` | アカウントの秘密アクセスキー [!DNL Kinesis] 。 |
| `auth.params.region` | アカウントの地域 [!DNL Kinesis] です。 |
| `connectionSpec.id` | 接続 [!DNL Kinesis] 指定ID: `86043421-563b-46ec-8e6c-e23184711bf6` |

**応答** 

正常な応答は、新たに作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでクラウドストレージデータを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## 次の手順

このチュートリアルに従うと、APIを使用して [!DNL Amazon Kinesis] 接続を作成し、一意のIDを応答本文の一部として取得できます。 この接続IDを使用して、Flow Service APIを使用してストリーミングデータを [収集できます](../../collect/streaming.md)。