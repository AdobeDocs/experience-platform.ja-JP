---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform Source Connectorsの概要
topic: overview
translation-type: tm+mt
source-git-commit: b58b933fce9d1abe658a908ec07f390e4991c5c6

---


# ソースコネクタの概要

Adobe Experience Platformを使用すると、データを外部ソースから取り込むと同時に、Platform Servicesを使用して、入力データの構造、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

エクスペリエンスプラットフォームは、RESTful APIとインタラクティブUIを備えており、様々なデータプロバイダーへのソース接続を簡単に設定できます。 これらのソース接続を使用すると、サードパーティ製システムの認証、インジェストの実行時間の設定、データインジェストのスループットの管理を行うことができます。

エクスペリエンスプラットフォームを使用すると、異なるソースから収集したデータを一元管理し、得られた洞察を利用してより多くの作業を行うことができます。

## ソースのタイプ

Experience Platformのソースは、次のカテゴリにグループ化されています。

### アドビアプリケーション

Experience Platformを使用すると、Adobe Analytics、Adobe Platform Manager、Experience Platform Launchなどの他のアドビアプリケーションからデータを取り込むことができます。 詳しくは、次の関連ドキュメントを参照してください。

- [Adobeオーディエンスマネージャーコネクタの概要](connectors/adobe-applications/audience-manager.md)
- [UIでのAdobeオーディエンスマネージャーソースコネクタの作成](./tutorials/ui/create/adobe-applications/audience-manager.md)
- [Adobe Analytics data connectorsの概要](connectors/adobe-applications/analytics.md)
- [UIでのAdobe Analyticsソースコネクタの作成](./tutorials/ui/create/adobe-applications/analytics.md)
- [UIでの顧客属性ソースコネクタの作成](./tutorials/ui/create/adobe-applications/customer-attributes.md)

### 広告

エクスペリエンスプラットフォームは、サードパーティの広告システムからデータを取り込むためのサポートを提供します。 特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [Google広告コネクタ](connectors/advertising/ads.md)

### クラウドストレージ

クラウドストレージソースは、ダウンロード、形式設定、アップロードを行うことなく、独自のデータをプラットフォームに取り込むことができます。 取り込んだデータは、XDM JSON、XDMパーケー、または区切り文字として形式設定できます。 プロセスの各手順は、ユーザーインターフェイスを使用してSourcesワークフローに統合されます。 詳しくは、次の関連ドキュメントを参照してください。

- [Azure Data LakeストレージGen2コネクタ](connectors/cloud-storage/adls-gen2.md)
- [Azure BlobおよびAmazon S3コネクタ](connectors/cloud-storage/blob-s3.md)
- [FTPおよびSFTPコネクタ](connectors/cloud-storage/ftp-sftp.md)
- [Google Cloudストレージコネクタ](connectors/cloud-storage/google-cloud-storage.md)

### 顧客関係管理（CRM）

CRMシステムは、顧客との関係の構築に役立つデータを提供し、顧客の忠誠度を高め、顧客の定着を促進します。 エクスペリエンスプラットフォームは、Microsoft Dynamics 365およびSalesforceからCRMデータを取り込むためのサポートを提供します。 詳しくは、次の関連ドキュメントを参照してください。

- [Microsoft Dynamicsコネクタ](connectors/crm/ms-dynamics.md)
- [Salesforceコネクタ](connectors/crm/salesforce.md)

### 顧客の成功

エクスペリエンスプラットフォームは、サードパーティの顧客成功アプリケーションからデータを取り込むためのサポートを提供します。 詳しくは、次の関連ドキュメントを参照してください。

- [Salesforce Service Cloudコネクタ](connectors/customer-success/salesforce-service-cloud.md)
- [ServiceNowコネクタ](connectors/customer-success/servicenow.md)

### データベース

エクスペリエンスプラットフォームは、サードパーティのデータベースからデータを取り込むためのサポートを提供します。 特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [Amazon Redshiftコネクタ](connectors/databases/redshift.md)
- [Apache Cassandraコネクタ](connectors/databases/cassandra.md)
- [Azure HDInsightsコネクタのApache Hive](connectors/databases/hive.md)
- [Azure HDInsightsコネクタ上のApache Spark](connectors/databases/spark.md)
- [Azure Data Explorerコネクタ](connectors/databases/data-explorer.md)
- [Azure Synapse Analyticsコネクタ](connectors/databases/synapse-analytics.md)
- [Azureテーブルストレージコネクタ](connectors/databases/ats.md)
- [Google BigQuery Connector](connectors/databases/bigquery.md)
- [IBM DB2コネクタ](connectors/databases/ibm-db2.md)
- [MariaDBコネクタ](connectors/databases/mariadb.md)
- [Microsoft SQL Serverコネクタ](connectors/databases/sql-server.md)
- [MySQLコネクタ](connectors/databases/mysql.md)
- [Oracleコネクタ](connectors/databases/oracle.md)
- [フェニックスコネクタ](connectors/databases/phoenix.md)
- [PostgreSQLコネクタ](connectors/databases/postgres.md)

### マーケティングの自動化

エクスペリエンスプラットフォームは、サードパーティのマーケティング自動化システムからデータを取り込むためのサポートを提供します。 特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [HubSpotコネクタ](connectors/marketing-automation/hubspot.md)

### 支払い

エクスペリエンスプラットフォームは、サードパーティの支払いシステムからデータを取り込むためのサポートを提供します。 特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [PayPalコネクタ](connectors/payments/paypal.md)

### プロトコル

エクスペリエンスプラットフォームは、サードパーティのプロトコルシステムからデータを取り込むためのサポートを提供します。 特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [汎用ODataコネクタ](connectors/protocols/odata.md)

## データ取り込み時のソースのアクセス制御

データ取り込みのソースに対する権限は、Adobe Admin Consoleで管理できます。 権限には、特定の製品プロファイルの「 *権限* 」タブからアクセスできます。 権限を **編集パネルから** 、 *データ取り込み* メニューのエントリを使用して、ソースに関する権限にアクセスできます。 「 **表示ソース***」権限は「* カタログ *」タブの使用可能なソースおよび「認証されたソース」タブの読み取り専用アクセス権を付与し、「ソースの***** 管理」権限は「ソースの作成」、「編集」、「無効化」の各ソースに対する完全なアクセス権を付与します。

次の表に、権限の様々な組み合わせに基づくUIの動作を示します。

| 権限レベル | 説明 |
| ---- | ----|
| **表示ソース** ：オン | 「 *カタログ* 」タブの各ソースタイプのソースに対して、「 *参照*」、「 **** アカウント」、「DataFlow」の各タブと共に読み取り専用アクセス権を付与します。 |
| **ソースの管理** : | **表示ソースに含まれる機能に加えて、**&#x200B;ソースに含まれる機能に加えて、Connect Source *option Catalog内の* Connect Source *option Catalogへのアクセス権を付与し、Select Data Option* Browse *Not Browse*** Notにアクセス権を付与します。 **また、「ソースの管理** 」では、DataFlowsを有効または無効にしたり ** 、スケジュールを編集したりできます。 |
| **表示ソース** ：オフおよび **管理ソース** ：オフ | ソースへのアクセスをすべて取り消します。 |

管理コンソールから付与される使用可能な権限（これら4つのソースを含む）について詳しくは、 [アクセス制御の概要を参照してください](../access-control/home.md)。
