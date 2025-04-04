---
title: Flow Service API を使用した Azure Blob ベース接続の作成
description: Flow Service API を使用してAdobe Experience Platformを Azure Blob に接続する方法を説明します。
exl-id: 4ab8033f-697a-49b6-8d9c-1aadfef04a04
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '772'
ht-degree: 28%

---

# [!DNL Flow Service] API を使用した [!DNL Azure Blob] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Azure Blob] （以下「[!DNL Blob]」と呼びます）のベース接続を作成する手順について説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用して [!DNL Blob] ソース接続を正常に作成するために必要な追加情報を示しています。

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL Blob] ストレージに接続するには、次の接続プロパティの値を指定する必要があります。

>[!BEGINTABS]

>[!TAB  接続文字列認証 ]

| 資格情報 | 説明 |
| --- | --- |
| `connectionString` | Experience Platformに対する [!DNL Blob] の認証に必要な認証情報を含む文字列。 [!DNL Blob] の接続文字列パターンは `DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}` です。 接続文字列について詳しくは、この [!DNL Blob] ドキュメント [ 接続文字列の設定 ](https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string) を参照してください。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Blob] の接続仕様 ID は `d771e9c1-4f26-40dc-8617-ce58c4b53702` です。 |

>[!TAB SAS URI 認証 ]

| 資格情報 | 説明 |
| --- | --- |
| `sasUri` | [!DNL Blob] アカウントを接続するための代替認証タイプとして使用できる共有アクセス署名 URI です。 [!DNL Blob] の SAS URI パターンを以下に示します。`https://{ACCOUNT_NAME}.blob.core.windows.net/?sv=<storage version>&st={START_TIME}&se={EXPIRE_TIME}&sr={RESOURCE}&sp={PERMISSIONS}>&sip=<{IP_RANGE}>&spr={PROTOCOL}&sig={SIGNATURE}>` 詳しくは、この [!DNL Blob] ドキュメントの [ 共有アクセス署名 URI](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication) を参照してください。 |
| `container` | アクセスを指定するコンテナの名前。 [!DNL Blob] ソースで新しいアカウントを作成する際に、コンテナ名を指定して、選択したサブフォルダーへのユーザーアクセスを指定できます。 |
| `folderPath` | アクセス権を付与するフォルダーへのパス。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Blob] の接続仕様 ID は `d771e9c1-4f26-40dc-8617-ce58c4b53702` です。 |

>[!ENDTABS]

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続の作成

>[!TIP]
>
>作成した後は、[!DNL Blob] ベース接続の認証タイプを変更できません。 認証タイプを変更するには、新しいベース接続を作成する必要があります。

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

[!DNL Blob] ソースは、接続文字列と共有アクセス署名（SAS）認証の両方をサポートしています。 共有アクセス署名（SAS） URI を使用すると、[!DNL Blob] アカウントに対する安全なデリゲート認証が可能になります。 SAS ベースの認証では、権限、開始日、有効期限および特定のリソースへのプロビジョニングを設定できるので、SAS を使用して、様々なレベルのアクセスで認証資格情報を作成できます。

この手順では、コンテナの名前とサブフォルダーへのパスを定義することで、アカウントがアクセスできるサブフォルダーを指定することもできます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Blob] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```http
POST /connections
```

**リクエスト**

>[!BEGINTABS]

>[!TAB  接続文字列 ]

次のリクエストは、接続文字列ベースの認証を使用して、[!DNL Blob] のベース接続を作成します。

+++リクエスト

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

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.connectionString` | Blob ストレージのデータにアクセスするために必要な接続文字列。 Blob 接続文字列のパターンは `DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}` です。 |
| `connectionSpec.id` | BLOB ストレージ接続仕様 ID は `4c10e202-c428-4796-9208-5f1f5732b1cf` です。 |

+++

+++応答

リクエストが成功した場合は、一意の ID（`id`）を含め、新しく作成されたベース接続の詳細が返されます。この ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700c57b-0000-0200-0000-5e3b3f440000\""
}
```

+++

>[!TAB SAS URI 認証 ]

共有アクセス署名 URI を使用して [!DNL Blob] 接続を作成するには、[!DNL Flow Service] API に POST リクエストを実行し、その際に [!DNL Blob] `sasUri` の値を指定します。

+++リクエスト

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

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.connectionString` | [!DNL Blob] ストレージ内のデータにアクセスするために必要な SAS URI。 [!DNL Blob] の SAS URI パターンは `https://{ACCOUNT_NAME}.blob.core.windows.net/?sv=<storage version>&st={START_TIME}&se={EXPIRE_TIME}&sr={RESOURCE}&sp={PERMISSIONS}>&sip=<{IP_RANGE}>&spr={PROTOCOL}&sig={SIGNATURE}>` です。 |
| `connectionSpec.id` | [!DNL Blob] ストレージ接続仕様 ID は `4c10e202-c428-4796-9208-5f1f5732b1cf` です。 |

+++

+++応答

リクエストが成功した場合は、一意の ID（`id`）を含め、新しく作成されたベース接続の詳細が返されます。この ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"1700c57b-0000-0200-0000-5e3b3f440000\""
}
```

+++

>[!ENDTABS]

## 次の手順

このチュートリアルでは、API を使用して [!DNL Blob] 接続を作成し、一意の ID を応答本文の一部として取得しました。 この接続 ID を使用して [Flow Service API を使用したクラウドストレージの調査 ](../../explore/cloud-storage.md) を行うことができます。
