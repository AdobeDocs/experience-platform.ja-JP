---
keywords: Experience Platform；ホーム；人気のあるトピック；Azure;azure blob;blob;Blob
solution: Experience Platform
title: フローサービス API を使用した Azure Blob Base 接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを Azure BLOB に接続する方法を説明します。
exl-id: 4ab8033f-697a-49b6-8d9c-1aadfef04a04
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 8%

---

# [!DNL Flow Service] API を使用して [!DNL Azure Blob] ベース接続を作成する

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Azure Blob]（以下「[!DNL Blob]」と呼ばれます）の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Platform サービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用して [!DNL Blob] ソース接続を正しく作成するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] が [!DNL Blob] ストレージと接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | [!DNL Blob] の認証に必要な認証情報を含むExperience Platform。 [!DNL Blob] 接続文字列パターンは次のとおりです。`DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`. 接続文字列の詳細については、[ 接続文字列 ](https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string) の設定に関するこの [!DNL Blob] ドキュメントを参照してください。 |
| `sasUri` | [!DNL Blob] アカウントに接続するための代替の認証タイプとして使用できる共有アクセス署名 URI です。 [!DNL Blob] SAS URI パターンは次のとおりです。`https://{ACCOUNT_NAME}.blob.core.windows.net/?sv=<storage version>&st={START_TIME}&se={EXPIRE_TIME}&sr={RESOURCE}&sp={PERMISSIONS}>&sip=<{IP_RANGE}>&spr={PROTOCOL}&sig={SIGNATURE}>` 詳しくは、[ 共有アクセスの署名 URI](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication) にあるこの [!DNL Blob] ドキュメントを参照してください。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Blob] の接続仕様 ID は次のとおりです。`d771e9c1-4f26-40dc-8617-ce58c4b53702`. |

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の使用の手引き ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続を作成する

ベース接続は、ソースと Platform の間の情報を保持します。これには、ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID などが含まれます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTID を作成するには、要求パラメーターの一部として [!DNL Blob] 認証資格情報を指定しながら、`/connections` エンドポイントに接続要求を行います。

### 接続文字列ベースの認証を使用して [!DNL Blob] ベース接続を作成する

接続文字列ベースのPOSTを使用して [!DNL Blob] ベース接続を作成するには、[!DNL Blob] `connectionString` を提供しながら、[!DNL Flow Service] API に接続リクエストを実行します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、接続文字列ベースの認証を使用して [!DNL Blob] のベース接続を作成します。

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
| `auth.params.connectionString` | BLOB ストレージ内のデータにアクセスするために必要な接続文字列。 BLOB 接続文字列パターンは次のとおりです。`DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`. |
| `connectionSpec.id` | BLOB ストレージ接続仕様 ID は次のとおりです。`4c10e202-c428-4796-9208-5f1f5732b1cf` |

**応答**

正常な応答は、新しく作成されたベース接続の詳細 ( 一意の識別子 (`id`) を含む ) を返します。 この ID は、次の手順でソース接続を作成する際に必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700c57b-0000-0200-0000-5e3b3f440000\""
}
```

### 共有アクセス署名 URI を使用した [!DNL Blob] ベース接続の作成

共有アクセス署名 (SAS) URI を使用すると、[!DNL Blob] アカウントに対する安全な委任承認を実行できます。 SAS ベースの認証では、権限、開始日、有効期限、および特定のリソースに対するプロビジョニングを設定できるので、SAS を使用して、様々なアクセスレベルで認証資格情報を作成できます。

共有アクセスPOSTURI を使用して [!DNL Blob] BLOB 接続を作成するには、[!DNL Blob] `sasUri` に値を指定しながら、[!DNL Flow Service] API に対して署名要求を行います。

**API 形式**

```http
POST /connections
```

**リクエスト**

次の要求は、共有アクセス署名 URI を使用して [!DNL Blob] のベース接続を作成します。

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
| `auth.params.connectionString` | [!DNL Blob] ストレージ内のデータにアクセスするために必要な SAS URI。 [!DNL Blob] SAS URI パターンは次のとおりです。`https://{ACCOUNT_NAME}.blob.core.windows.net/?sv=<storage version>&st={START_TIME}&se={EXPIRE_TIME}&sr={RESOURCE}&sp={PERMISSIONS}>&sip=<{IP_RANGE}>&spr={PROTOCOL}&sig={SIGNATURE}>`. |
| `connectionSpec.id` | [!DNL Blob] ストレージ接続の仕様 ID は次のとおりです。`4c10e202-c428-4796-9208-5f1f5732b1cf` |

**応答**

正常な応答は、新しく作成されたベース接続の詳細 ( 一意の識別子 (`id`) を含む ) を返します。 この ID は、次の手順でソース接続を作成する際に必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700c57b-0000-0200-0000-5e3b3f440000\""
}
```

## 次の手順

このチュートリアルでは、API を使用して [!DNL Blob] 接続を作成し、応答本文の一部として一意の ID を取得しました。 この接続 ID を使用して [ フローサービス API](../../explore/cloud-storage.md) または [ フローサービス API](../../cloud-storage-parquet.md) を使用して Parquet データを取り込み、クラウドストレージを調べることができます。
