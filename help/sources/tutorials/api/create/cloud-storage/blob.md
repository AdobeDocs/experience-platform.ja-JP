---
keywords: Experience Platform；ホーム；人気の高いトピック；Azure;azure blob;blob;Blob
solution: Experience Platform
title: フローサービス API を使用した Azure Blob ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを Azure Blob に接続する方法を説明します。
exl-id: 4ab8033f-697a-49b6-8d9c-1aadfef04a04
source-git-commit: 0d891acd4e33eb7080da44e204672dc3601cf166
workflow-type: tm+mt
source-wordcount: '692'
ht-degree: 40%

---

# の作成 [!DNL Azure Blob] を使用したベース接続 [!DNL Flow Service] API

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、のベース接続を作成する手順を説明します。 [!DNL Azure Blob] （以下「」という。）[!DNL Blob]」) [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：Experience Platform を使用すると、様々なソースからデータを取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

以下の節では、 [!DNL Blob] を使用したソース接続 [!DNL Flow Service] API

### 必要な認証情報の収集

次のために [!DNL Flow Service] を [!DNL Blob] ストレージの場合は、次の接続プロパティの値を指定する必要があります。

| 認証情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | 認証に必要な認証情報を含む文字列 [!DNL Blob] Experience Platformに この [!DNL Blob] 接続文字列のパターン： `DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`. 接続文字列について詳しくは、 [!DNL Blob] 文書 [接続文字列の設定](https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string). |
| `sasUri` | 代替の認証タイプとして使用して、 [!DNL Blob] アカウント この [!DNL Blob] SAS URI パターンは次のとおりです。 `https://{ACCOUNT_NAME}.blob.core.windows.net/?sv=<storage version>&st={START_TIME}&se={EXPIRE_TIME}&sr={RESOURCE}&sp={PERMISSIONS}>&sip=<{IP_RANGE}>&spr={PROTOCOL}&sig={SIGNATURE}>` 詳しくは、 [!DNL Blob] 文書 [共有アクセス署名 URI](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication). |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Blob] の接続仕様 ID は `d771e9c1-4f26-40dc-8617-ce58c4b53702` です。 |

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Blob] 認証資格情報をリクエストパラメーターの一部として使用します。

### の作成 [!DNL Blob] 接続文字列ベースの認証を使用したベース接続

を作成するには、以下を実行します。 [!DNL Blob] 接続文字列ベースの認証を使用したベース接続、 [!DNL Flow Service] を提供する際の API [!DNL Blob] `connectionString`.

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、 [!DNL Blob] 接続文字列ベースの認証を使用する場合：

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `auth.params.connectionString` | BLOB ストレージ内のデータにアクセスするために必要な接続文字列です。 BLOB 接続文字列のパターンは次のとおりです。 `DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`. |
| `connectionSpec.id` | BLOB ストレージ接続の仕様 ID は次のとおりです。 `4c10e202-c428-4796-9208-5f1f5732b1cf` |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含め、新しく作成されたベース接続の詳細が返されます。この ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700c57b-0000-0200-0000-5e3b3f440000\""
}
```

### の作成 [!DNL Blob] 共有アクセス署名 URI を使用したベース接続

共有アクセス署名 (SAS)URI を使用すると、 [!DNL Blob] アカウント SAS ベースの認証では、権限設定、開始日、有効期限日、および特定のリソースに対するプロビジョニングを設定できるので、SAS を使用して、様々なアクセス度の認証資格情報を作成できます。

を作成するには、以下を実行します。 [!DNL Blob] 共有アクセス署名 URI を使用した blob 接続、へのPOSTリクエスト [!DNL Flow Service] API を使用して [!DNL Blob] `sasUri`.

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、 [!DNL Blob] 共有アクセス署名 URI を使用：

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Azure Blob source connection using SAS URI",
        "description": "Azure Blob source connection using SAS URI",
        "auth": {
            "specName": "SAS URI Authentication",
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
| `auth.params.connectionString` | 内のデータにアクセスするために必要な SAS URI [!DNL Blob] ストレージ。 この [!DNL Blob] SAS URI パターンは次のとおりです。 `https://{ACCOUNT_NAME}.blob.core.windows.net/?sv=<storage version>&st={START_TIME}&se={EXPIRE_TIME}&sr={RESOURCE}&sp={PERMISSIONS}>&sip=<{IP_RANGE}>&spr={PROTOCOL}&sig={SIGNATURE}>`. |
| `connectionSpec.id` | この [!DNL Blob] ストレージ接続の仕様 ID: `4c10e202-c428-4796-9208-5f1f5732b1cf` |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含め、新しく作成されたベース接続の詳細が返されます。この ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700c57b-0000-0200-0000-5e3b3f440000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL Blob] 応答本文の一部として、API と一意の ID を使用した接続を取得しました。 この接続 ID を [フローサービス API を使用したクラウドストレージの調査](../../explore/cloud-storage.md).
