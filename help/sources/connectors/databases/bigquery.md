---
title: Google BigQuery Source コネクタの概要
description: APIまたはユーザーインターフェイスを使用して、Google BigQueryをAdobe Experience Platformに接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 35c61382-a909-47f4-a937-15cb725ecbe3
source-git-commit: 2136ace3e3c1157ac7bbfe56071af3dc9bc66fd6
workflow-type: tm+mt
source-wordcount: '841'
ht-degree: 2%

---

# [!DNL Google BigQuery] ソース

>[!IMPORTANT]
>
>[!DNL Google BigQuery] ソースは、Real-Time Customer Data Platform Ultimateを購入したユーザーがソースカタログで利用できます。

AzureまたはAmazon Web Services（AWS）で[!DNL Google BigQuery] アカウントをAdobe Experience Platformに正常に接続するために必要な前提条件の手順については、このドキュメントを参照してください。

## 前提条件 {#prerequisites}

[!DNL Google BigQuery] アカウントをExperience Platformに接続する前に完了する必要がある前提条件の設定については、次の節を参照してください。

### IP アドレスの許可リスト

AzureまたはAmazon Web Services（AWS）のExperience Platformにソースを接続する前に、リージョン固有のIP アドレスを許可リストに追加する必要があります。 詳しくは、[AzureおよびAWS上のExperience Platformに接続するためのIP アドレスの許可リストに加える](../../ip-address-allow-list.md)に関するガイドを参照してください。

### AzureでのExperience Platformへの認証 {#azure}

[!DNL Google BigQuery] アカウントをAzure上のExperience Platformに接続するには、次の資格情報を指定する必要があります。

>[!BEGINTABS]

>[!TAB 基本認証]

OAuth 2.0と基本認証を組み合わせて認証するには、次の資格情報に適切な値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| `project` | プロジェクトは、[!DNL Google Cloud]を含む[!DNL Google BigQuery] リソースの基本レベルの整理エンティティです。 |
| `clientID` | クライアント IDは、[!DNL Google BigQuery] OAuth 2.0資格情報の半分です。 |
| `clientSecret` | クライアントの秘密鍵は、[!DNL Google BigQuery] OAuth 2.0資格情報の残りの半分です。 |
| `refreshToken` | 更新トークンを使用すると、APIの新しいアクセストークンを取得できます。 アクセストークンの有効期間は限られており、プロジェクトの期間中に失効する可能性があります。 更新トークンを使用すると、必要に応じてプロジェクトのその後のアクセストークンを認証してリクエストできます。 更新トークンに次の[!DNL Google]個のOAuth スコープが含まれていることを確認します。 <ul><li>`https://www.googleapis.com/auth/bigquery`</li><li>`https://www.googleapis.com/auth/cloud-platform`</li></ul> これらのスコープを使用すると、Experience PlatformはBigQuery ジョブを送信し、設定されたプロジェクトからデータを読み取ることができます。 |
| `largeResultsDataSetId` | （オプション）大規模な結果セットのサポートを有効にするために必要な、事前作成された[!DNL Google BigQuery] データセット ID。<ul><li>`largeResultsDataSetId`は、大規模な結果セットの一時テーブルを保存するために使用される、事前作成された[!DNL BigQuery] データセットを参照する必要があります。</li><li>値には、プロジェクトに適格な名前（`marketing_temp_results`を使用しない）ではなく、データセット ID （`my-project.marketing_temp_results`など）のみを含める必要があります。</li><li>`largeResultsDataSetId`で指定されたデータセットの場所（地域）は、クエリするテーブルの場所と一致する必要があります。</li><li>コネクタで使用されるアカウントには、このデータセットの一時的な結果を読み取り、書き込むための権限が必要です。 少なくとも、[!DNL BigQuery Data Editor]で指定されたデータセットに`largeResultsDataSetId`の役割を割り当てます。</li></ul> |

#### [!DNL Google] IDに必要なIAM役割

OAuth資格情報（クライアント ID、クライアント秘密鍵、およびrefreshToken）の生成に使用される[!DNL Google] IDには、ターゲット [!DNL Google Cloud] プロジェクトで次のIAM ロールが必要です。

- [!DNL BigQuery Job User]
- [!DNL BigQuery Data Viewer]
- [!DNL BigQuery Read Session User]

これらの役割により、Experience Platformで[!DNL BigQuery]件のジョブを作成および実行し、設定されたテーブルからデータを読み取り、コネクタで必要に応じて読み取りセッションを使用できるようになります。 これらの役割は、ソースで使用する予定の[!DNL BigQuery] データセットを含む同じプロジェクトで付与されていることを確認してください。

[!DNL Google] APIのOAuth 2.0資格情報を生成する方法の詳細な手順については、次の[[!DNL Google] OAuth 2.0認証ガイド &#x200B;](https://developers.google.com/identity/protocols/oauth2)を参照してください。

>[!TAB  サービス認証]

サービス認証を使用して認証を行うには、次の資格情報に適切な値を指定します。

**注**: サービス認証で正常に認証するには、サービス アカウントに十分な権限（例：**[!DNL BigQuery Job User]**、**[!DNL BigQuery Data Viewer]**、**[!DNL BigQuery Read Session User]**、**[!DNL BigQuery Data Owner]**）が必要です。

| 資格情報 | 説明 |
| --- | --- |
| `projectId` | クエリの対象となる[!DNL Google BigQuery]のID。 |
| `keyFileContent` | サービスアカウントの認証に使用されるキーファイル。 この値は、[[!DNL Google Cloud service accounts]  ダッシュボード &#x200B;](https://console.cloud.google.com)から取得できます。 キーファイルの内容はJSON形式です。 Experience Platformに認証する場合は、これを[!DNL Base64]にエンコードする必要があります。 |
| `largeResultsDataSetId` | （オプション）大規模な結果セットのサポートを有効にするために必要な、事前作成された[!DNL Google BigQuery] データセット ID。<ul><li>`largeResultsDataSetId`は、大規模な結果セットの一時テーブルを保存するために使用される、事前作成された[!DNL BigQuery] データセットを参照する必要があります。</li><li>値には、プロジェクトに適格な名前（`marketing_temp_results`を使用しない）ではなく、データセット ID （`my-project.marketing_temp_results`など）のみを含める必要があります。</li><li>`largeResultsDataSetId`で指定されたデータセットの場所（地域）は、クエリするテーブルの場所と一致する必要があります。</li><li>コネクタで使用されるアカウントには、このデータセットの一時的な結果を読み取り、書き込むための権限が必要です。 少なくとも、[!DNL BigQuery Data Editor]で指定されたデータセットに`largeResultsDataSetId`の役割を割り当てます。</li></ul> |

[!DNL Google BigQuery]でのサービスアカウントの使用について詳しくは、[&#x200B; [!DNL Google BigQuery]での](https://cloud.google.com/bigquery/docs/use-service-accounts) サービスアカウントの使用に関するガイドを参照してください。

>[!ENDTABS]

### AWSでのExperience Platformへの認証 {#aws}

[!DNL Google BigQuery] アカウントをAWS上のExperience Platformに接続するには、次の資格情報を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| `projectId` | クエリの対象となる[!DNL Google BigQuery]のID。 |
| `keyFileContent` | サービスアカウントの認証に使用されるキーファイル。 この値は、[[!DNL Google Cloud service accounts]  ダッシュボード &#x200B;](https://console.cloud.google.com)から取得できます。 キーファイルの内容はJSON形式です。 Experience Platformに認証する場合は、これを[!DNL Base64]にエンコードする必要があります。 |
| `datasetId` | [!DNL Google BigQuery] データセット ID。 このIDは、データテーブルの場所を表します。 |

## [!DNL Google BigQuery]をExperience Platformに接続

以下のドキュメントでは、APIまたはユーザーインターフェイスを使用して[!DNL Google BigQuery]をExperience Platformに接続する方法について説明します。

### API の使用

- [Flow Service APIを使用したGoogle BigQuery ベース接続の作成](../../tutorials/api/create/databases/bigquery.md)
- [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
- [Flow Service APIを使用したデータベースソースのデータフローの作成](../../tutorials/api/collect/database-nosql.md)

### UI の使用

- [UIでのGoogle BigQuery ソース接続の作成](../../tutorials/ui/create/databases/bigquery.md)
- [UIでのデータベースソース接続のデータフローの作成](../../tutorials/ui/dataflow/databases.md)
