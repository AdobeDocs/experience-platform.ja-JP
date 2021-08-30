---
keywords: Experience Platform；ホーム；人気のあるトピック；Azure;azure blob;blob;Blob
solution: Experience Platform
title: フローサービスAPIを使用したAzure Blob Base接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してAdobe Experience PlatformをAzure Blobに接続する方法を説明します。
exl-id: 4ab8033f-697a-49b6-8d9c-1aadfef04a04
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 8%

---

# [!DNL Flow Service] APIを使用して[!DNL Azure Blob]ベース接続を作成する

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して[!DNL Azure Blob]（以下「[!DNL Blob]」と呼びます）のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Platformサービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して[!DNL Blob]ソース接続を正しく作成するために必要な追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Blob]ストレージに接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | Experience Platformに対する[!DNL Blob]の認証に必要な認証情報を含む文字列。 [!DNL Blob]接続文字列パターンは次のとおりです。`DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`. 接続文字列の詳細については、[接続文字列](https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string)の設定に関するこの[!DNL Blob]ドキュメントを参照してください。 |
| `sasUri` | [!DNL Blob]アカウントに接続するための代替の認証タイプとして使用できる共有アクセス署名URIです。 [!DNL Blob] SAS URIパターンは次のとおりです。`https://{ACCOUNT_NAME}.blob.core.windows.net/?sv=<storage version>&st={START_TIME}&se={EXPIRE_TIME}&sr={RESOURCE}&sp={PERMISSIONS}>&sip=<{IP_RANGE}>&spr={PROTOCOL}&sig={SIGNATURE}>`詳しくは、[共有アクセス署名URI](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication)にあるこの[!DNL Blob]ドキュメントを参照してください。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Blob]の接続仕様IDは次のとおりです。`d771e9c1-4f26-40dc-8617-ce58c4b53702`. |

### Platform APIの使用

Platform APIを正常に呼び出す方法について詳しくは、[Platform APIの使用の手引き](../../../../../landing/api-guide.md)を参照してください。

## ベース接続を作成する

ベース接続は、ソースとプラットフォームの間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続IDなど）を保持します。 ベース接続IDを使用すると、ソース内からファイルを参照およびナビゲートし、取得する特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTIDを作成するには、リクエストパラメーターの一部として[!DNL Blob]認証資格情報を指定しながら、`/connections`エンドポイントに接続リクエストを実行します。

### 接続文字列ベースの認証を使用して[!DNL Blob]ベース接続を作成する

接続文字列ベースのPOSTを使用して[!DNL Blob]ベース接続を作成するには、[!DNL Blob] `connectionString`を提供しながら、[!DNL Flow Service] APIに接続リクエストを実行します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、接続文字列ベースの認証を使用して[!DNL Blob]のベース接続を作成します。

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
| `auth.params.connectionString` | BLOBストレージ内のデータにアクセスするために必要な接続文字列。 BLOB接続文字列パターンは次のとおりです。`DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`. |
| `connectionSpec.id` | BLOBストレージ接続の仕様IDは次のとおりです。`4c10e202-c428-4796-9208-5f1f5732b1cf` |

**応答**

正常な応答は、新しく作成されたベース接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700c57b-0000-0200-0000-5e3b3f440000\""
}
```

### 共有アクセス署名URIを使用して[!DNL Blob]ベース接続を作成する

共有アクセス署名(SAS) URIを使用すると、[!DNL Blob]アカウントに対する安全な委任承認を実行できます。 SASベースの認証では、権限、開始日、有効期限、および特定のリソースに対するプロビジョニングを設定できるので、SASを使用して、様々なアクセス度の認証資格情報を作成できます。

共有アクセスPOSTURIを使用して[!DNL Blob] BLOB接続を作成するには、[!DNL Blob] `sasUri`に値を指定しながら、[!DNL Flow Service] APIに対して接続リクエストを実行します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次の要求は、共有アクセス署名URIを使用して[!DNL Blob]のベース接続を作成します。

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
| `connectionSpec.id` | [!DNL Blob]ストレージ接続の仕様IDは次のとおりです。`4c10e202-c428-4796-9208-5f1f5732b1cf` |

**応答**

正常な応答は、新しく作成されたベース接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700c57b-0000-0200-0000-5e3b3f440000\""
}
```

## 次の手順

このチュートリアルでは、APIを使用して[!DNL Blob]接続を作成し、応答本文の一部として一意のIDを取得しました。 この接続IDを使用して[フローサービスAPI](../../explore/cloud-storage.md)または[フローサービスAPI](../../cloud-storage-parquet.md)を使用してParquetデータを取り込み、クラウドストレージを調べることができます。
