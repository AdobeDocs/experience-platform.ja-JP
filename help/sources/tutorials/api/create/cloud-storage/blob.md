---
title: Flow Service API を使用した Azure Blob Storage のExperience Platformへの接続
description: Flow Service API を使用してAdobe Experience Platformを Azure Blob に接続する方法を説明します。
exl-id: 4ab8033f-697a-49b6-8d9c-1aadfef04a04
source-git-commit: 8e932a25026bef2b785cfddfb8b668b1dd47eb0d
workflow-type: tm+mt
source-wordcount: '657'
ht-degree: 9%

---

# API を使用した [!DNL Azure Blob Storage] のExperience Platformへの接続

このガイドでは、[!DNL Azure Blobg Storage]API[[!DNL Flow Service]  を使用して ](https://developer.adobe.com/experience-platform-apis/references/flow-service/) アカウントをAdobe Experience Platformに接続する方法について説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 ](../../../../../landing/api-guide.md) を参照してください。

### 必要な資格情報の収集

認証について詳しくは、[[!DNL Azure Blob Storage]  概要 ](../../../../connectors/cloud-storage/blob.md#authentication) を参照してください。

## [!DNL Azure Blob Storage] アカウントのExperience Platformへの接続 {#connect}

[!DNL Azure Blob Storage] アカウントをExperience Platformに接続する方法については、以下の手順を参照してください。

### ベース接続の作成

>[!NOTE]
>
>作成した後は、[!DNL Azure Blob Storage] ベース接続の認証タイプを変更できません。 認証タイプを変更するには、新しいベース接続を作成する必要があります。

ベース接続は、ソースをExperience Platformにリンクし、認証の詳細、接続ステータス、一意の ID を保存します。 この ID を使用して、ソースファイルを参照し、データのタイプや形式など、取り込む特定の項目を特定します。

次の認証タイプを使用して、[!DNL Azure Blob Storage] アカウントをExperience Platformに接続できます。

* **アカウントキー認証**: ストレージアカウントのアクセスキーを使用して認証し、[!DNL Azure Blob Storage] アカウントに接続します。
* **共有アクセス署名（SAS）**:SAS URI を使用して、[!DNL Azure Blob Storage] アカウントのリソースへのデリゲートされた時間制限アクセスを提供します。
* **サービスプリンシパルベースの認証**:Azure Active Directory （AAD）サービスプリンシパル（クライアント ID とシークレット）を使用して、Azure Blob Storage アカウントに対して安全に認証します。

**API 形式**

```https
POST /connections
```

ベース接続 ID を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、認証資格情報をリクエストパラメーターの一部として指定します。

>[!BEGINTABS]

>[!TAB  アカウントキー認証 ]

アカウントキー認証を使用するには、`connectionString`、`container`、`folderPath` の値を指定します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "Azure Blob Storage connection using connectingString",
    "description": "Azure Blob Storage connection using connectionString",
    "auth": {
      "specName": "ConnectionString",
      "params": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}",
        "container": "acme-blob-container",
        "folderPath": "/acme/customers/salesData"
      }
    },
    "connectionSpec": {
      "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
      "version": "1.0"
    }
  }'
```

| パラメーター | 説明 |
| --- | --- |
| `connectionString` | [!DNL Azure Blob Storage] アカウントの接続文字列。 接続文字列のパターンは `DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY};EndpointSuffix=core.windows.net` です。 |
| `container` | データファイルが格納されている [!DNL Azure Blob Storage] コンテナの名前。 |
| `folderPath` | ファイルが配置されている、指定されたコンテナ内のパス。 |
| `connectionSpec.id` | [!DNL Azure Blob Storage] ソースの接続仕様 ID。 この ID は、`4c10e202-c428-4796-9208-5f1f5732b1cf` として固定されます。 |

>[!TAB  共有アクセス署名 ]

共有アクセス署名を使用するには、`sasUri`、`container`、`folderPath` の値を指定します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "Azure Blob Storage source connection using SAS URI",
    "description": "Azure Blob Storage source connection using SAS URI",
    "auth": {
      "specName": "SAS URI Authentication",
      "params": {
        "sasUri": "https://{ACCOUNT_NAME}.blob.core.windows.net/?sv={STORAGE_VERSION}&st={START_TIME}&se={EXPIRE_TIME}&sr={RESOURCE}&sp={PERMISSIONS}>&sip=<{IP_RANGE}>&spr={PROTOCOL}&sig={SIGNATURE}>",
        "container": "acme-blob-container",
        "folderPath": "/acme/customers/salesData"
      }
    },
    "connectionSpec": {
      "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
      "version": "1.0"
    }
  }'
```

| パラメーター | 説明 |
| --- | --- |
| `sasUri` | アカウントを接続するための代替認証タイプとして使用できる共有アクセス署名 URI。 SAS URI パターンは `https://{ACCOUNT_NAME}.blob.core.windows.net/?sv={STORAGE_VERSION}&st={START_TIME}&se={EXPIRE_TIME}&sr={RESOURCE}&sp={PERMISSIONS}>&sip=<{IP_RANGE}>&spr={PROTOCOL}&sig={SIGNATURE}` です。 |
| `container` | データファイルが格納されている [!DNL Azure Blob Storage] コンテナの名前。 |
| `folderPath` | ファイルが配置されている、指定されたコンテナ内のパス。 |
| `connectionSpec.id` | [!DNL Azure Blob Storage] ソースの接続仕様 ID。 この ID は、`4c10e202-c428-4796-9208-5f1f5732b1cf` として固定されます。 |

>[!TAB  サービスプリンシパルベースの認証 ]

サービスプリンシパルベースの認証を使用して接続するには、`serviceEndpoint`、`servicePrincipalId`、`servicePrincipalKey`、`accountKind`、`tenant`、`container` および `folderPath` の値を指定します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "Azure Blob Storage source connection using service principal based authentication",
    "description": "Azure Blob Storage source connection using service principal based authentication",
    "auth": {
      "specName": "Service Principal Based Authentication",
      "params": {
        "serviceEndpoint": "{SERVICE_ENDPOINT}",
        "servicePrincipalId": "{SERVICE_PRINCIPAL_ID}",
        "servicePrincipalKey": "{SERVICE_PRINCIPAL_KEY}",
        "accountKind": "{ACCOUNT_KIND}",
        "tenant": "{TENANT}",
        "container": "acme-blob-container",
        "folderPath": "/acme/customers/salesData"
      }
    },
    "connectionSpec": {
      "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
      "version": "1.0"
    }
  }'
```

| パラメーター | 説明 |
| --- | --- |
| `serviceEndpoint` | [!DNL Azure Blob Storage] アカウントのエンドポイント URL。 通常は、`https://{ACCOUNT_NAME}.blob.core.windows.net` の形式です。 |
| `servicePrincipalId` | 認証に使用する Azure Active Directory （AAD） サービス プリンシパルのクライアント/アプリケーション ID。 |
| `servicePrincipalKey` | Azure サービスプリンシパルに関連付けられたクライアントシークレットまたはパスワード。 |
| `accountKind` | [!DNL Azure Blob Storage] アカウントのタイプ。 共通の値には、`Storage` （汎用 V1）、`StorageV2` （汎用 V2）、`BlobStorage`、`BlockBlobStorage` などがあります。 |
| `tenant` | サービスプリンシパルが登録されている Azure Active Directory （AAD）テナント ID。 |
| `container` | データファイルが格納されている [!DNL Azure Blob Storage] コンテナの名前。 |
| `folderPath` | ファイルが配置されている、指定されたコンテナ内のパス。 |
| `connectionSpec.id` | [!DNL Azure Blob Storage] ソースの接続仕様 ID。 この ID は、`4c10e202-c428-4796-9208-5f1f5732b1cf` として固定されます。 |

>[!ENDTABS]

リクエストが成功した場合は、一意の ID（`id`）を含め、新しく作成されたベース接続の詳細が返されます。この ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700c57b-0000-0200-0000-5e3b3f440000\""
}
```



## 次の手順

このチュートリアルでは、API を使用して [!DNL Blob] 接続を作成し、一意の ID を応答本文の一部として取得しました。 この接続 ID を使用して [Flow Service API を使用したクラウドストレージの調査 ](../../explore/cloud-storage.md) を行うことができます。
