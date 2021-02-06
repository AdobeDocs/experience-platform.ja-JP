---
keywords: Experience Platform；ホーム；人気のあるトピック；Google Cloudストレージ;Googleクラウドストレージ;google;Google
solution: Experience Platform
title: Flow Service APIを使用したGoogle Cloudストレージソース接続の作成
topic: overview
type: Tutorial
description: Flow Service APIを使用して、Adobe Experience PlatformをGoogle Cloudストレージアカウントに接続する方法を説明します。
translation-type: tm+mt
source-git-commit: a489ab248793a063295578943ad600d8eacab6a2
workflow-type: tm+mt
source-wordcount: '554'
ht-degree: 20%

---


# [!DNL Flow Service] APIを使用して[!DNL Google Cloud Storage]ソース接続を作成する

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL Experience Platform]を[!DNL Google Cloud Storage]アカウントに接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

[!DNL Flow Service] APIを使用してGoogle Cloudストレージアカウントに正しく接続するために必要な追加情報については、以下の節で説明します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Google Cloud Storage]アカウントと接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `accessKeyId` | [!DNL Google Cloud Storage]アカウントのアクセスキーID。 |
| `secretAccessKey` | [!DNL Google Cloud Storage]アカウントの秘密アクセスキー。 |

使い始める前に、[このGoogle Cloudドキュメント](https://cloud.google.com/docs/authentication)を参照してください。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

[!DNL Platform] APIを呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。 次に示すように、すべての[!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソース（[!DNL Flow Service]に属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するのに使用できるため、[!DNL Google Cloud Storage]アカウントごとに1つの接続のみが必要です。

**API 形式**

```http
POST /connections
```

**リクエスト**

[!DNL Google Cloud Storage]接続を作成するには、POST要求の一部として一意の接続指定IDを指定する必要があります。 [!DNL Google Cloud Storage]の接続指定IDは`32e8f412-cdf7-464c-9885-78184cb113fd`です。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Google Cloud Storage connection",
        "description": "Connector for Google Cloud Storage",
        "auth": {
            "specName": "Basic Authentication for google-cloud",
            "params": {
                "accessKeyId": "accessKeyId",
                "secretAccessKey": "secretAccessKey"
            }
        },
        "connectionSpec": {
            "id": "32e8f412-cdf7-464c-9885-78184cb113fd",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.accessKeyId` | [!DNL Google Cloud Storage]アカウントに関連付けられているアクセスキーID。 |
| `auth.params.secretAccessKey` | [!DNL Google Cloud Storage]アカウントに関連付けられている秘密アクセスキー。 |
| `connectionSpec.id` | [!DNL Google Cloud Storage]接続指定ID:`32e8f412-cdf7-464c-9885-78184cb113fd` |

**応答** 

正常に応答すると、新たに作成された接続の詳細(一意の識別子(`id`)が返されます。 このIDは、次のチュートリアルでクラウドストレージデータを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## 次の手順

このチュートリアルに従うと、APIを使用して[!DNL Google Cloud Storage]接続を作成し、一意のIDを応答本文の一部として取得できます。 この接続IDを使用して、Flow Service API](../../explore/cloud-storage.md)または[Flow Service API](../../cloud-storage-parquet.md)を使用してParketストレージを[調査できます。