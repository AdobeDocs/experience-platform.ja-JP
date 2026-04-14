---
title: Databricks
description: DatabricksをExperience Platformに接続するために必要な前提条件の手順について説明します。
badgeUltimate: label="Ultimate" type="Positive"
last-substantial-update: 2026-03-14T00:00:00Z
exl-id: 2f082898-aa0e-47a1-a4bf-077c21afdfee
source-git-commit: 23b8d5d49e217d587dfe3d68631e6056c61b2cb8
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 3%

---

# [!DNL Databricks]

>[!AVAILABILITY]
>
>[!DNL Databricks] ソースは、Real-Time CDP Ultimateを購入したユーザーがソースカタログで利用できます。

[!DNL Databricks]は、データ分析、マシンラーニング、AI向けに設計されたクラウドベースのプラットフォームです。 [!DNL Databricks]を使用して、大規模にデータソリューションを構築、デプロイ、管理するための包括的な環境を統合および提供できます。

[!DNL Databricks] ソースを使用してアカウントを接続し、[!DNL Databricks] データをAdobe Experience Platformに取り込みます。

## 前提条件

[!DNL Databricks] アカウントをExperience Platformに正常に接続するための前提条件の手順を完了します。

### コンテナ資格情報の取得

Experience Platform [!DNL Azure Blob Storage]の資格情報を取得して、[!DNL Databricks] アカウントが後でアクセスできるようにします。

資格情報を取得するには、`/credentials` APIの[!DNL Connectors] エンドポイントに対してGET リクエストを行います。

**API 形式**

```http
GET /data/foundation/connectors/landingzone/credentials?type=dlz_databricks_source
```

**リクエスト**

次のリクエストは、Experience Platform [!DNL Azure Blob Storage]の資格情報を取得します。

+++リクエストの例を表示

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/connectors/landingzone/credentials?type=dlz_databricks_source' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

+++

**応答**

応答が成功すると、後で`containerName`の構成で`SASToken`に使用するために資格情報（`storageAccountName`、[!DNL Apache Spark]、[!DNL Databricks]）が提供されます。

+++応答の例を表示

```json
{
    "containerName": "dlz-databricks-container",
    "SASToken": "sv=2020-10-02&si=dlz-b1f4060b-6bbd-4043-9bd9-a5f5be72de30&sr=c&sp=racwdlm&sig=zVQfmuElZJzOKkUk8z5lChrJ3YQUE2h6EShDZOsVeMc%3D",
    "storageAccountName": "sndbxdtlndga8m7ajbvgc64k",
    "SASUri": "https://sndbxdtlndga8m7ajbvgc64k.blob.core.windows.net/dlz-databricks-container?sv=2020-10-02&si=dlz-b1f4060b-6bbd-4043-9bd9-a5f5be72de30&sr=c&sp=racwdlm&sig=zVQfmuElZJzOKkUk8z5lChrJ3YQUE2h6EShDZOsVeMc%3D",
    "expiryDate": "2025-07-05"
}
```

| プロパティ | 説明 |
| --- | --- |
| `containerName` | [!DNL Azure Blob Storage] コンテナの名前。 この値は、後で[!DNL Apache Spark]の[!DNL Databricks]設定を完了する際に使用します。 |
| `SASToken` | [!DNL Azure Blob Storage]の共有アクセス署名トークン。 この文字列には、リクエストの承認に必要なすべての情報が含まれます。 |
| `storageAccountName` | ストレージアカウントの名前。 |
| `SASUri` | [!DNL Azure Blob Storage]の共有アクセス署名URI。 この文字列は、認証対象の[!DNL Azure Blob Storage]へのURIとそれに対応するSAS トークンの組み合わせです。 |
| `expiryDate` | SAS トークンの有効期限。 データを[!DNL Azure Blob Storage]にアップロードするアプリケーションでトークンを引き続き使用するには、有効期限の前にトークンを更新する必要があります。 指定された有効期限より前にトークンを手動で更新しない場合は、GET資格情報呼び出しが実行されたときに自動的に更新され、新しいトークンが提供されます。 |

+++

### 資格情報を更新

>[!NOTE]
>
>資格情報を更新すると、既存の資格情報が取り消されます。 したがって、ストレージ資格情報を更新するたびに、それに応じて[!DNL Spark]設定を更新する必要があります。 そうしないと、データフローは失敗します。

資格情報を更新するには、POST リクエストを行い、`action=refresh`をクエリパラメーターとして含めます。

**API 形式**

```http
POST /data/foundation/connectors/landingzone/credentials?type=dlz_databricks_source&action=refresh
```

**リクエスト**

次のリクエストは、[!DNL Azure Blob Storage]の資格情報を更新します。

+++リクエストの例を表示

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/connectors/landingzone/credentials?type=dlz_databricks_source&action=refresh' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

+++

**応答**

応答が成功すると、新しい資格情報が返されます。

+++応答の例を表示

```json
{
    "containerName": "dlz-databricks-container",
    "SASToken": "sv=2020-10-02&si=dlz-6e17e5d6-de18-4efc-88c7-45f37d242617&sr=c&sp=racwdlm&sig=wvA4K3fcEmqAA%2FPvcMhB%2FA8y8RLwVJ7zhdWbxvT1uFM%3D",
    "storageAccountName": "sndbxdtlndga8m7ajbvgc64k",
    "SASUri": "https://sndbxdtlndga8m7ajbvgc64k.blob.core.windows.net/dlz-databricks-container?sv=2020-10-02&si=dlz-6e17e5d6-de18-4efc-88c7-45f37d242617&sr=c&sp=racwdlm&sig=wvA4K3fcEmqAA%2FPvcMhB%2FA8y8RLwVJ7zhdWbxvT1uFM%3D",
    "expiryDate": "2025-07-20"
}
```

+++

### [!DNL Azure Blob Storage]へのアクセスを設定

>[!IMPORTANT]
>
>* クラスターが終了した場合、フローの実行中にサービスが自動的に再起動します。 ただし、接続またはデータフローを作成する際は、クラスターがアクティブであることを確認する必要があります。 さらに、データのプレビューや探索などのアクションを実行する場合は、クラスターがアクティブである必要があります。これらのアクションでは、終了したクラスターの自動再起動を促すことができません。
>
>* [!DNL Azure] コンテナには、`adobe-managed-staging`という名前のフォルダーが含まれています。 データをシームレスに取り込むには、**このフォルダーを変更しないでください**。


次に、[!DNL Databricks] クラスターがExperience Platform [!DNL Azure Blob Storage] アカウントにアクセスできることを確認する必要があります。 この場合、[!DNL Azure Blob Storage]を[!DNL delta lake] テーブル データを書き込むための暫定的な場所として使用できます。

アクセスを提供するには、[!DNL Databricks]設定の一部として、[!DNL Apache Spark] クラスターにSAS トークンを設定する必要があります。

[!DNL Databricks] インターフェイスで「**[!DNL Advanced options]**」を選択し、[!DNL Spark config]入力ボックスに次の情報を入力します。

```shell
fs.azure.sas.{CONTAINER_NAME}.{STORAGE-ACCOUNT}.blob.core.windows.net {SAS-TOKEN}
```

| プロパティ | 説明 |
| --- | --- |
| コンテナ名 | コンテナの名前。 この値は、[!DNL Azure Blob Storage]資格情報を取得することで取得できます。 |
| ストレージアカウント | ストレージアカウントの名前。 この値は、[!DNL Azure Blob Storage]資格情報を取得することで取得できます。 |
| SAS トークン | [!DNL Azure Blob Storage]の共有アクセス署名トークン。 この値は、[!DNL Azure Blob Storage]資格情報を取得することで取得できます。 |

![AzureのDatabricks UI。](../../images/tutorials/create/databricks/databricks-ui.png)

指定しない場合、フロー実行のコピーアクティビティは失敗し、次のエラーが返されます。

```shell
Unable to access container '{CONTAINER_NAME}' in account '{STORAGE_ACCOUNT}.blob.core.windows.net' using anonymous credentials. No credentials found in the configuration. Public access is not permitted on this storage account.
```

## [!DNL Databricks]をExperience Platformに接続

前提条件の手順が完了したので、次に進み、[!DNL Databricks] アカウントをExperience Platformに接続します。

* [API経由の接続](../../tutorials/api/create/databases/databricks.md)
* [UIのソースワークスペースを介して接続する](../../tutorials/ui/create/databases/databricks.md)
