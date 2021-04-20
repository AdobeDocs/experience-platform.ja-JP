---
keywords: Experience Platform；ホーム；人気のあるトピック；Kinesis;kinesis;AmazonKinesis;amazon kinesis
solution: Experience Platform
title: フローサービスAPIを使用したAmazonKinesisソース接続の作成
topic: overview
type: Tutorial
description: Flow Service APIを使用して、Adobe Experience PlatformをAmazonKinesisアカウントに接続する方法を説明します。
translation-type: tm+mt
source-git-commit: ed14fe464a4dc82f54902c8dc92fe00bc2a5381e
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 26%

---


# Flow Service APIを使用して[!DNL Amazon Kinesis]ソース接続を作成する

>[!NOTE]
>
>[!DNL Amazon Kineses]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL Experience Platform]を[!DNL Amazon Kinesis]アカウントに接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

[!DNL Flow Service] APIを使用して[!DNL Amazon Kinesis]アカウントに正しく接続するために知っておく必要がある追加情報については、以下の節で説明します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Amazon Kinesis]アカウントと接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `accessKeyId` | [!DNL Kinesis]アカウントのアクセスキーID。 |
| `secretKey` | [!DNL Kinesis]アカウントの秘密アクセスキー。 |
| `region` | [!DNL Kinesis]アカウントの地域です。 |
| `connectionSpec.id` | [!DNL Kinesis]接続指定ID:`86043421-563b-46ec-8e6c-e23184711bf6` |

これらの値の詳細については、[このKinesisドキュメント](https://docs.aws.amazon.com/streams/latest/dev/getting-started.html)を参照してください。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

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

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するのに使用できるため、[!DNL Amazon Kinesis]アカウントごとに1つの接続のみが必要です。

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
            "specName": "Aws Kinesis authentication credentials",
            "params": {
                "accessKeyId": "{ACCESS_KEY_ID}",
                "secretKey": "{SECRET_KEY}",
                "region: "{REGION}
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
| `auth.params.accessKeyId` | [!DNL Kinesis]アカウントのアクセスキーID。 |
| `auth.params.secretKey` | [!DNL Kinesis]アカウントの秘密アクセスキー。 |
| `auth.params.region` | [!DNL Kinesis]アカウントの地域です。 地域について詳しくは、[IPアドレス許可リスト](../../../../ip-address-allow-list.md)のドキュメントを参照してください |
| `connectionSpec.id` | [!DNL Kinesis]接続指定ID:`86043421-563b-46ec-8e6c-e23184711bf6` |

**応答** 

正常に応答すると、新たに作成された接続の詳細(一意の識別子(`id`)が返されます。 このIDは、次のチュートリアルでクラウドストレージデータを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## 次の手順

このチュートリアルに従うと、APIを使用して[!DNL Amazon Kinesis]接続を作成し、一意のIDを応答本文の一部として取得できます。 この接続IDを使用して[Flow Service API](../../collect/streaming.md)を使用してストリーミングデータを収集できます。