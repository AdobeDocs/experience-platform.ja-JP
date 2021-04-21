---
keywords: Experience Platform；ホーム；人気のあるトピック；Azure;azure blob;blob;Blob
solution: Experience Platform
title: Flow Service APIを使用してAzure Blobソース接続を作成する
topic-legacy: overview
type: Tutorial
description: Flow Service APIを使用してAdobe Experience PlatformをAzure Blobに接続する方法を説明します。
exl-id: 4ab8033f-697a-49b6-8d9c-1aadfef04a04
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '761'
ht-degree: 26%

---

# [!DNL Flow Service] APIを使用して[!DNL Azure Blob]ソース接続を作成する

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml)を使用して、[!DNL Azure Blob]（以下「BLOB」と呼ばれる）をAdobe Experience Platformに接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込むことができ、Platform Servicesを使用して、データの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して[!DNL Blob]ソース接続を正しく作成するために知っておく必要がある追加情報を紹介します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Blob]ストレージと接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | Experience Platformに対する[!DNL Blob]の認証に必要な認証情報を含む文字列です。 [!DNL Blob]接続文字列パターンは次のとおりです。`DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`. 接続文字列の詳細については、[接続文字列の設定](https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string)に関するこの[!DNL Blob]ドキュメントを参照してください。 |
| `sasUri` | [!DNL Blob]アカウントに接続するための代替認証タイプとして使用できる共有アクセス署名URIです。 [!DNL Blob] SAS URIパターンは次のとおりです。`https://{ACCOUNT_NAME}.blob.core.windows.net/?sv=<storage version>&st={START_TIME}&se={EXPIRE_TIME}&sr={RESOURCE}&sp={PERMISSIONS}>&sip=<{IP_RANGE}>&spr={PROTOCOL}&sig={SIGNATURE}>`詳しくは、[共有アクセス署名URI](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication)のこの[!DNL Blob]ドキュメントを参照してください。 |
| `connectionSpec.id` | 接続を作成するために必要な一意の識別子。 [!DNL Blob]の接続指定IDは次のとおりです。`4c10e202-c428-4796-9208-5f1f5732b1cf` |

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーの値の収集

Platform API への呼び出しを実行する前に、[認証に関するチュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Flow Service]に属するリソースを含む、Experience Platform内のすべてのリソースは、特定の仮想サンドボックスに分離されます。 Platform API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込む複数のデータフローの作成に使用できるため、[!DNL Blob]アカウントごとに必要な接続は1つだけです。

### 接続文字列ベースの認証を使用した[!DNL Blob]接続の作成

接続文字列ベースの認証を使用して[!DNL Blob]POSTを作成するには、[!DNL Blob] `connectionString`を提供しながら[!DNL Flow Service] APIに接続リクエストを行います。

**API 形式**

```http
POST /connections
```

**リクエスト**

[!DNL Blob]接続を作成するには、POST要求の一部として一意の接続指定IDを指定する必要があります。 [!DNL Blob]の接続指定IDは`4c10e202-c428-4796-9208-5f1f5732b1cf`です。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Azure Blob connection using connectionString",
        "description": "Azure Blob connection using connectionString",
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

### 共有アクセス署名URIを使用した[!DNL Blob]接続の作成

共有アクセス署名(SAS)URIを使用すると、[!DNL Blob]アカウントに対する安全な委任承認を許可できます。 SASベースの認証では、権限、開始、有効期限の設定、特定のリソースに対する規定を行えるため、SASを使用して、様々なアクセス度の認証資格情報を作成できます。

共有アクセス署名URIを使用して[!DNL Blob]POSTを作成するには、[!DNL Blob] `sasUri`に値を提供しながら、[!DNL Flow Service] APIに接続リクエストを行います。

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
        "name": "Azure Blob source connection using SAS URI",
        "description": "Azure Blob source connection using SAS URI",
        "auth": {
            "specName": "SasURIAuthentication",
            "params": {
                "sasUri": "https://{ACCOUNT_NAME}.blob.core.windows.net/?sv={STORAGE_VERSION}&st={START_TIME}&se={EXPIRE_TIME}&sr={RESOURCE}&sp={PERMISSIONS}>&sip=<{IP_RANGE}>&spr={PROTOCOL}&sig={SIGNATURE}>"
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
| `auth.params.connectionString` | [!DNL Blob]ストレージ内のデータにアクセスするために必要なSAS URI。 [!DNL Blob] SAS URIパターンは次のとおりです。`https://{ACCOUNT_NAME}.blob.core.windows.net/?sv=<storage version>&st={START_TIME}&se={EXPIRE_TIME}&sr={RESOURCE}&sp={PERMISSIONS}>&sip=<{IP_RANGE}>&spr={PROTOCOL}&sig={SIGNATURE}>`. |
| `connectionSpec.id` | [!DNL Blob]ストレージ接続の指定ID:`4c10e202-c428-4796-9208-5f1f5732b1cf` |

**応答**

正常に応答すると、新たに作成された接続の詳細(一意の識別子(`id`)が返されます。 このIDは、次のチュートリアルでストレージを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700c57b-0000-0200-0000-5e3b3f440000\""
}
```

## 次の手順

このチュートリアルに従うと、APIを使用して[!DNL Blob]接続を作成し、一意のIDを応答本文の一部として取得できます。 この接続IDを使用して、Flow Service API](../../explore/cloud-storage.md)または[Flow Service API](../../cloud-storage-parquet.md)を使用してParketストレージを[調査できます。
