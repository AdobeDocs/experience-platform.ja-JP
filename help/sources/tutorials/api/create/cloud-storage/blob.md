---
keywords: Experience Platform;home;popular topics;Azure;azure blob;blob;Blob
solution: Experience Platform
title: Flow Service APIを使用してAzure BLOBコネクタを作成する
topic: overview
type: Tutorial
description: このチュートリアルでは、Flow Service APIを使用して、Experience PlatformをAzure Blob （以下「Blob」）ストレージに接続する手順を順を追って説明します。
translation-type: tm+mt
source-git-commit: fc6449d260ea7b96956689ce6c95c5e8b9002d89
workflow-type: tm+mt
source-wordcount: '605'
ht-degree: 19%

---


# [!DNL Flow Service] APIを使用して[!DNL Azure Blob]コネクタを作成する

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL Experience Platform]を[!DNL Azure Blob]（以下「Blob」）ストレージに接続する手順を順を追って説明します。

[!DNL Experience Platform]でユーザーインターフェイスを使用する場合は、[Azure BlobソースコネクタUIチュートリアル](../../../ui/create/cloud-storage/blob.md)に、同様の操作を実行するための手順を順を追って示します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

[!DNL Flow Service] APIを使用してBLOBストレージに正しく接続するために必要な追加情報については、以下の節で説明します。

### 必要な資格情報の収集

[!DNL Flow Service]がBLOBストレージと接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | BLOBストレージのデータにアクセスするために必要な接続文字列です。 BLOB接続文字列パターンは次のとおりです。`DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`. |
| `connectionSpec.id` | 接続を作成するために必要な一意の識別子。 BLOBの接続指定ID:`4c10e202-c428-4796-9208-5f1f5732b1cf` |

接続文字列の取得の詳細については、[このAzure BLOBドキュメント](https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string)を参照してください。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

[!DNL Platform] APIを呼び出すには、まず[認証チュートリアル](../../../../../tutorials/authentication.md)を完了する必要があります。 次に示すように、すべての[!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソース（[!DNL Flow Service]に属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json``

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できるので、BLOBアカウントごとに必要な接続は1つだけです。

**API 形式**

```http
POST /connections
```

**リクエスト**

BLOB接続を作成するには、POST要求の一部として一意の接続指定IDを指定する必要があります。 Blobの接続指定IDは`4c10e202-c428-4796-9208-5f1f5732b1cf`です。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Blob Connection",
        "description": "Cnnection for an Azure Blob account",
        "auth": {
            "specName": "ConnectionString",
            "params": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}"
            }
        },
        "connectionSpec": {
            "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.connectionString` | BLOBストレージのデータにアクセスするために必要な接続文字列です。 BLOB接続文字列パターンは次のとおりです。`DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`. |
| `connectionSpec.id` | BLOBストレージ接続の指定ID:`4c10e202-c428-4796-9208-5f1f5732b1cf` |

**応答** 

正常に応答すると、新たに作成された接続の詳細(一意の識別子(`id`)が返されます。 このIDは、次のチュートリアルでストレージを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700c57b-0000-0200-0000-5e3b3f440000\""
}
```

## 次の手順

このチュートリアルに従うと、APIを使用してBlob接続を作成し、応答本文の一部として一意のIDを取得できます。 この接続IDを使用して、Flow Service API](../../explore/cloud-storage.md)または[Flow Service API](../../cloud-storage-parquet.md)を使用したインジェストパーケットデータを使用して、[クラウドストレージを探索できます。