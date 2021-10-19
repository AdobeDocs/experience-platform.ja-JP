---
keywords: エクスペリエンス Platform、home、人気のある話題。Azure、azure ブロブ、ブロッブ、Blob
solution: Experience Platform
title: フローサービス API を使用した Azure Blob Base 接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用して Adobe エクスペリエンスプラットフォームを Azure Blob に接続する方法について説明します。
exl-id: 4ab8033f-697a-49b6-8d9c-1aadfef04a04
source-git-commit: 13bd1254dfe89004465174a7532b4f6aaef54c09
workflow-type: tm+mt
source-wordcount: '692'
ht-degree: 9%

---

# [!DNL Azure Blob]API を使用したベース接続の作成 [!DNL Flow Service]

ベース接続は、ソースと Adobe エクスペリエンスプラットフォームとの間の認証された接続を表します。

このチュートリアルでは、 [!DNL Azure Blob] API を使用して (hereinafter と呼ばれる) ベース接続を作成する手順について説明し [!DNL Blob] [[!DNL Flow Service]  ](https://www.adobe.io/experience-platform-apis/references/flow-service/) ます。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース: エクスペリエンスプラットフォームを使用すると、 ](../../../../home.md) 多種多様なソースからデータを ingested することができます。また、プラットフォームサービスを使用して、着信データを構造化、ラベル付け、拡張する機能が提供されます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の各セクションでは、 [!DNL Blob] API を使用してソース接続を正常に作成するために必要な追加情報を示し [!DNL Flow Service] ます。

### 必要な資格情報の収集

[!DNL Flow Service]ストレージに接続するには [!DNL Blob] 、次の接続プロパティの値を指定する必要があります。

| Chap | 説明 |
| ---------- | ----------- |
| `connectionString` | プラットフォームを利用した認証に必要な認証情報を含むストリング [!DNL Blob] 。 [!DNL Blob]接続ストリングパターンは、次のとおりです `DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}` 。接続ストリングについて詳しくは、このドキュメントを参照してください [!DNL Blob] [ ](https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string) 。 |
| `sasUri` | アカウントに接続するための代替認証タイプとして使用できる shared access signature URI [!DNL Blob] です。 [!DNL Blob]SAS URI パターンは以下のとおりです。 `https://{ACCOUNT_NAME}.blob.core.windows.net/?sv=<storage version>&st={START_TIME}&se={EXPIRE_TIME}&sr={RESOURCE}&sp={PERMISSIONS}>&sip=<{IP_RANGE}>&spr={PROTOCOL}&sig={SIGNATURE}>` 詳しくは、 [!DNL Blob] [ shared access signature URIs について、このドキュメントを参照してください ](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication) 。 |
| `connectionSpec.id` | コネクション仕様は、ベースおよびソース接続の作成に関連付けられた認証仕様を含む、ソースのコネクタプロパティを返します。 の接続仕様 ID [!DNL Blob] は、次のとおりです `d771e9c1-4f26-40dc-8617-ce58c4b53702` 。 |

### プラットフォーム Api の使用

プラットフォーム Api の呼び出しを適切に行う方法については、Platform Api の概要を参照してください [ ](../../../../../landing/api-guide.md) 。

## ベース接続の作成

ベース接続を行うと、ソースとプラットフォームの間の情報が保持されます。ソースの認証の資格情報、接続の現在の状態、および一意の基本接続 ID が含まれています。 ベース接続 ID を使用して、ソース内でファイルを検索してナビゲートし、データの種類とフォーマットに関する情報も含めて、取り込む特定のアイテムを指定することができます。

ベース接続 ID を作成するには、そのエンドポイントに POST 要求を行います。この場合は、 `/connections` [!DNL Blob] 要求パラメーターの一部として認証資格情報を指定します。

### [!DNL Blob]接続ストリングベースの認証を使用したベース接続の作成

[!DNL Blob]接続ストリングベースの認証を使用してベース接続を作成するには、API への POST 要求を行い [!DNL Flow Service] [!DNL Blob] `connectionString` ます。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、 [!DNL Blob] 接続ストリングベースの認証を使用するための基本的な接続を作成します。

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
| `auth.params.connectionString` | ブロブストレージのデータにアクセスするには、接続ストリングを指定する必要があります。 Blob 接続ストリングパターンは、次のとおりです `DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}` 。 |
| `connectionSpec.id` | ブロブストレージの接続スペシフィケーション ID は以下のとおりです。 `4c10e202-c428-4796-9208-5f1f5732b1cf` |

**応答**

応答が成功した場合は、一意の識別子 () を含む、新しく作成されたベース接続の詳細が返され `id` ます。 この ID は、次の手順に従ってソース接続を作成する必要があります。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700c57b-0000-0200-0000-5e3b3f440000\""
}
```

### [!DNL Blob]Shared access SIGNATURE URI を使用したベース接続の作成

Shared access signature (SAS) URI を使用すると、アカウントに対して安全に委任された認証を行うことができ [!DNL Blob] ます。 Sas を使用すると、様々なアクセス許可を持つ認証資格情報を作成することができます。 SAS ベースの認証を使用すると、アクセス権、開始日と終了日を設定できます。また、特定のリソースに対する繰り入れも設定できます。

[!DNL Blob]Shared access SIGNATURE URI を使用して blob 接続を作成するには、API への POST 要求を行い、 [!DNL Flow Service] の値を指定し [!DNL Blob] `sasUri` ます。

**API 形式**

```http
POST /connections
```

**リクエスト**

次の要求によっ [!DNL Blob] て、shared access SIGNATURE URI を使用するための基本的な接続が作成されます。

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
| `auth.params.connectionString` | 格納されたデータにアクセスするには、SAS URI が必要に [!DNL Blob] なります。 [!DNL Blob]SAS URI パターンは、次のとおりです `https://{ACCOUNT_NAME}.blob.core.windows.net/?sv=<storage version>&st={START_TIME}&se={EXPIRE_TIME}&sr={RESOURCE}&sp={PERMISSIONS}>&sip=<{IP_RANGE}>&spr={PROTOCOL}&sig={SIGNATURE}>` 。 |
| `connectionSpec.id` | [!DNL Blob]ストレージ接続のスペシフィケーション ID は以下のとおりです。`4c10e202-c428-4796-9208-5f1f5732b1cf` |

**応答**

応答が成功した場合は、一意の識別子 () を含む、新しく作成されたベース接続の詳細が返され `id` ます。 この ID は、次の手順に従ってソース接続を作成する必要があります。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700c57b-0000-0200-0000-5e3b3f440000\""
}
```

## 次の手順

このチュートリアルでは、Api を使用して接続を作成し、 [!DNL Blob] 応答本体の一部として一意の ID を取得しました。 この接続 ID を使用し [ て、フローサービス API を使用してクラウドストレージを探すことができ ](../../explore/cloud-storage.md) ます。
