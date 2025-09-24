---
title: Databricks
description: Databricks をExperience Platformに接続するために必要な前提条件の手順について説明します。
badgeUltimate: label="Ultimate" type="Positive"
badgeBeta: label="ベータ版" type="Informative"
last-substantial-update: 2025-06-17T00:00:00Z
exl-id: 2f082898-aa0e-47a1-a4bf-077c21afdfee
source-git-commit: 96e395e3b3d977d7eb04c400f6fd290977bf1101
workflow-type: tm+mt
source-wordcount: '628'
ht-degree: 3%

---

# [!DNL Databricks]

>[!AVAILABILITY]
>
>* Real-Time CDP Ultimateを購入したユーザーは、ソースカタログで [!DNL Databricks] ソースを利用できます。
>
>* [!DNL Databricks] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、ソースの概要の [ 利用条件 ](../../home.md#terms-and-conditions) を参照してください。

[!DNL Databricks] は、データ分析、機械学習、AI 用に設計されたクラウドベースのプラットフォームです。 [!DNL Databricks] を使用すると、データソリューションを大規模に構築、デプロイ、管理するための総合的な環境を統合および提供できます。

[!DNL Databricks] ソースを使用してアカウントを接続し、[!DNL Databricks] データをAdobe Experience Platformに取り込みます。

## 前提条件

[!DNL Databricks] アカウントをExperience Platformに正常に接続するには、前提条件の手順を完了してください。

### コンテナ資格情報の取得

Experience Platform [!DNL Azure Blob Storage] の資格情報を取得し、[!DNL Databricks] アカウントが後でアクセスできるようにします。

資格情報を取得するには、`/credentials` API の [!DNL Connectors] エンドポイントに対してGET リクエストを実行します。

**API 形式**

```http
GET /data/foundation/connectors/landingzone/credentials?type=dlz_databricks_source
```

**リクエスト**

次のリクエストは、Experience Platform [!DNL Azure Blob Storage] の資格情報を取得します。

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

正常な応答では、後で `containerName` の設定で使用するための資格情報（`SASToken`、`storageAccountName`、[!DNL Apache Spark]） [!DNL Databricks] 提供されます。

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
| `containerName` | [!DNL Azure Blob Storage] コンテナの名前。 この値は、後で [!DNL Apache Spark] の [!DNL Databricks] 設定を完了する際に使用します。 |
| `SASToken` | [!DNL Azure Blob Storage] ーザーの共有アクセス署名トークン。 この文字列には、リクエストの認証に必要なすべての情報が含まれます。 |
| `storageAccountName` | ストレージアカウントの名前。 |
| `SASUri` | [!DNL Azure Blob Storage] ーザーの共有アクセス署名 URI。 この文字列は、認証対象の [!DNL Azure Blob Storage] への URI とそれに対応する SAS トークンの組み合わせです。 |
| `expiryDate` | SAS トークンの有効期限が切れる日付。 [!DNL Azure Blob Storage] へのデータのアップロードにアプリケーションで引き続き使用するには、有効期限の前にトークンを更新する必要があります。 指定された有効期限の前にトークンを手動で更新しない場合、GET資格情報の呼び出しが実行されると、自動的に更新され、新しいトークンが提供されます。 |

+++

### 資格情報を更新します

>[!NOTE]
>
>資格情報を更新すると、既存の資格情報は取り消されます。 したがって、ストレージ資格情報を更新する [!DNL Spark] びに、設定を適宜更新する必要があります。 そうしないと、データフローが失敗します。

資格情報を更新するには、POST リクエストを実行し、クエリパラメーターとして `action=refresh` を含めます。

**API 形式**

```http
POST /data/foundation/connectors/landingzone/credentials?type=dlz_databricks_source&action=refresh
```

**リクエスト**

次のリクエストは、[!DNL Azure Blob Storage] の資格情報を更新します。

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

### [!DNL Azure Blob Storage] へのアクセスの設定

>[!IMPORTANT]
>
>* クラスターが終了している場合、サービスはフローの実行中にクラスターを自動的に再起動します。 ただし、接続やデータフローを作成する際には、クラスターがアクティブであることを確認する必要があります。 さらに、データのプレビューや調査などのアクションを実行している場合は、停止したクラスターの自動再起動を促すことができないため、クラスターがアクティブになっている必要があります。
>
>* [!DNL Azure] コンテナには `adobe-managed-staging` という名前のフォルダーが含まれています。 データをシームレスに取り込むために、このフォルダーを変更 **しないでください**。


次に、[!DNL Databricks] クラスターがExperience Platform [!DNL Azure Blob Storage] アカウントにアクセスできることを確認する必要があります。 その際に、テーブルのデータを書き込むための暫定的な場所として [!DNL Azure Blob Storage] を使用 [!DNL delta lake] きます。

アクセスを提供するには、[!DNL Databricks] 設定の一部として [!DNL Apache Spark] クラスターに SAS トークンを設定する必要があります。

[!DNL Databricks] インターフェイスで **[!DNL Advanced options]** を選択し、[!DNL Spark config] 入力ボックスに次の内容を入力します。

```shell
fs.azure.sas.{CONTAINER_NAME}.{STORAGE-ACCOUNT}.blob.core.windows.net {SAS-TOKEN}
```

| プロパティ | 説明 |
| --- | --- |
| コンテナ名 | コンテナの名前。 この値を取得するには、[!DNL Azure Blob Storage] 資格情報を取得します。 |
| ストレージアカウント | ストレージアカウントの名前。 この値を取得するには、[!DNL Azure Blob Storage] 資格情報を取得します。 |
| SAS トークン | [!DNL Azure Blob Storage] ーザーの共有アクセス署名トークン。 この値を取得するには、[!DNL Azure Blob Storage] 資格情報を取得します。 |

![Azure の Databricks UI。](../../images/tutorials/create/databricks/databricks-ui.png)

指定しない場合、フロー実行でのコピーアクティビティは失敗し、次のエラーが返されます。

```shell
Unable to access container '{CONTAINER_NAME}' in account '{STORAGE_ACCOUNT}.blob.core.windows.net' using anonymous credentials. No credentials found in the configuration. Public access is not permitted on this storage account.
```

## [!DNL Databricks] をExperience Platformに接続

これで、前提条件の手順が完了したので、次に進み、[!DNL Databricks] アカウントをExperience Platformに接続できます。

* [API を使用した接続](../../tutorials/api/create/databases/databricks.md)
* [UI のソースワークスペースを使用した接続](../../tutorials/ui/create/databases/databricks.md)
