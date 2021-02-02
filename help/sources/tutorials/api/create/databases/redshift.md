---
keywords: Experience Platform；ホーム；人気のあるトピック；redshift;Redshift;AmazonRedshift;Amazon Redshift
solution: Experience Platform
title: フローサービスAPIを使用して、AmazonRedshiftコネクタを作成する
topic: overview
type: Tutorial
description: このチュートリアルでは、Flow Service APIを使用して、Experience PlatformをAmazonRedshiftに接続する手順（以下「Redshift」と呼びます）を順を追って説明します。
translation-type: tm+mt
source-git-commit: ece2ae1eea8426813a95c18096c1b428acfd1a71
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 20%

---


# [!DNL Flow Service] APIを使用して[!DNL Amazon Redshift]コネクタを作成する

>[!NOTE]
>
>[!DNL Amazon Redshift]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、[!DNL Flow Service] APIを使用して、[!DNL Experience Platform]を[!DNL Amazon Redshift]に接続する手順（以下「[!DNL Redshift]」と呼びます）を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

[!DNL Flow Service] APIを使用して[!DNL Redshift]に正しく接続するために知っておく必要のある追加情報については、以下の節で説明します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Redshift]と接続するには、次の接続プロパティを指定する必要があります。

| **Credential** | **説明** |
| -------------- | --------------- |
| `server` | [!DNL Redshift]アカウントに関連付けられているサーバーです。 |
| `username` | [!DNL Redshift]アカウントに関連付けられているユーザー名。 |
| `password` | [!DNL Redshift]アカウントに関連付けられているパスワードです。 |
| `database` | アクセスしている[!DNL Redshift]データベース。 |

開始方法の詳細については、[このRedshiftドキュメント](https://docs.aws.amazon.com/redshift/latest/gsg/getting-started.html)を参照してください。

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

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するのに使用できるため、[!DNL Redshift]アカウントごとに1つの接続のみが必要です。

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
        "name": "amazon-redshift base connection",
        "description": "base connection for amazon-redshift,
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                "server": "{SERVER}",
                "database": "{DATABASE}",
                "password": "{PASSWORD}",
                "username": "{USERNAME}"
            }
        },
        "connectionSpec": {
            "id": "3416976c-a9ca-4bba-901a-1f08f66978ff",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| ------------- | --------------- |
| `auth.params.server` | [!DNL Redshift]サーバー。 |
| `auth.params.database` | [!DNL Redshift]アカウントに関連付けられているデータベースです。 |
| `auth.params.password` | [!DNL Redshift]アカウントに関連付けられているパスワードです。 |
| `auth.params.username` | [!DNL Redshift]アカウントに関連付けられているユーザー名。 |
| `connectionSpec.id` | 前の手順で取得した[!DNL Redshift]アカウントの接続仕様`id`。 |

**応答** 

正常に応答すると、新たに作成された接続が、一意の識別子(`id`)を含めて返されます。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "373e88fc-43da-4e3c-be88-fc43da3e3c0f",
    "etag": "\"1700ce7b-0000-0200-0000-5e3b405e0000\""
}
```

## 次の手順

このチュートリアルに従うと、[!DNL Flow Service] APIを使用して[!DNL Redshift]接続を作成し、接続の一意のID値を取得したことになります。 この接続IDは、Flow Service API](../../explore/database-nosql.md)を使用して[データベースやNoSQLシステムを探索する方法を学ぶ際に、次のチュートリアルで使用できます。
