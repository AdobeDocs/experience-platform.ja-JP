---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformソースコネクタの概要
topic: overview
translation-type: tm+mt
source-git-commit: a9ce046d6ee8622e23f31edbbf777b045109c13b
workflow-type: tm+mt
source-wordcount: '906'
ht-degree: 1%

---


# ソースコネクタの概要

Adobe Experience Platformを使用すると、Platformサービスを使用して、外部ソースからデータを取り込み、データの構造化、ラベル付け、および入力データの拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience PlatformにはRESTful APIとインタラクティブUIが用意されており、これにより様々なデータプロバイダーへのソース接続を簡単に設定できます。 これらのソース接続を使用すると、サードパーティ製システムの認証、インジェストの実行時間の設定、データインジェストのスループットの管理を行うことができます。

Experience Platformにより、異なるソースから収集したデータを一元管理し、得られた洞察を利用してより多くの作業を行うことができます。

## ソースのタイプ

Experience Platform内のソースは、次のカテゴリにグループ化されます。

### アドビアプリケーション

Experience Platformを使用すると、アドビのAnalytics、Adobe Audience Manager、Experience Platform Launchなど、他のアドビアプリケーションからデータを取り込むことができます。 詳しくは、次の関連ドキュメントを参照してください。

- [Adobe Audience Managerコネクタの概要](connectors/adobe-applications/audience-manager.md)
- [UIでのAdobe Audience Managerソースコネクタの作成](./tutorials/ui/create/adobe-applications/audience-manager.md)
- [Adobe Data Connectorの概要](connectors/adobe-applications/analytics.md)
- [UIでのAdobeAnalyticsソースコネクタの作成](./tutorials/ui/create/adobe-applications/analytics.md)
- [UIでの顧客属性ソースコネクタの作成](./tutorials/ui/create/adobe-applications/customer-attributes.md)

### 広告

Experience Platformは、サードパーティの広告システムからデータを取り込むためのサポートを提供します。 特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [Google AdWordsコネクタ](connectors/advertising/ads.md)

### クラウドストレージ

Cloudストレージソースを使用すると、ダウンロード、フォーマット、アップロードを必要とせずに、独自のデータをPlatformに取り込むことができます。 取り込んだデータは、XDM JSON、XDMパーケー、または区切り文字として形式設定できます。 プロセスの各手順は、ユーザーインターフェイスを使用してSourcesワークフローに統合されます。 詳しくは、次の関連ドキュメントを参照してください。

- [Azure Data LakeストレージGen2コネクタ](connectors/cloud-storage/adls-gen2.md)
- [Azure BlobおよびAmazon S3コネクタ](connectors/cloud-storage/blob-s3.md)
- [Amazon Kinesisコネクタ](connectors/cloud-storage/kinesis.md)
- [Apache HDFSコネクタ](connectors/cloud-storage/hdfs.md)
- [Azureイベントハブコネクタ](connectors/cloud-storage/eventhub.md)
- [Azure Fileストレージコネクタ](connectors/cloud-storage/azure-file-storage.md)
- [FTPおよびSFTPコネクタ](connectors/cloud-storage/ftp-sftp.md)
- [Google Cloudストレージコネクタ](connectors/cloud-storage/google-cloud-storage.md)

### 顧客関係管理（CRM）

CRMシステムは、顧客との関係の構築に役立つデータを提供し、顧客の忠誠度を高め、顧客の定着を促進します。 Experience Platformは、Microsoft Dynamics 365およびSalesforceからCRMデータを取り込むためのサポートを提供します。 詳しくは、次の関連ドキュメントを参照してください。

- [Microsoft Dynamicsコネクタ](connectors/crm/ms-dynamics.md)
- [Salesforceコネクタ](connectors/crm/salesforce.md)

### 顧客の成功

Experience Platformは、サードパーティの顧客サクセスアプリケーションからデータを取り込むためのサポートを提供します。 詳しくは、次の関連ドキュメントを参照してください。

- [Salesforce Service Cloudコネクタ](connectors/customer-success/salesforce-service-cloud.md)
- [ServiceNowコネクタ](connectors/customer-success/servicenow.md)

### データベース

Experience Platformは、サードパーティのデータベースからデータを取り込むためのサポートを提供します。 特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [Amazon Redshiftコネクタ](connectors/databases/redshift.md)
- [Azure HDInsightsコネクタのApache Hive](connectors/databases/hive.md)
- [Azure HDInsightsコネクタ上のApache Spark](connectors/databases/spark.md)
- [Azure Data Explorerコネクタ](connectors/databases/data-explorer.md)
- [Azure SynapseAnalyticsコネクタ](connectors/databases/synapse-analytics.md)
- [Azureテーブルストレージコネクタ](connectors/databases/ats.md)
- [Couchbaseコネクタ](connectors/databases/couchbase.md)
- [Google BigQuery Connector](connectors/databases/bigquery.md)
- [GreenPlumコネクタ](connectors/databases/greenplum.md)
- [HP Verticaコネクタ](connectors/databases/hp-vertica.md)
- [IBM DB2コネクタ](connectors/databases/ibm-db2.md)
- [MariaDBコネクタ](connectors/databases/mariadb.md)
- [Microsoft SQL Serverコネクタ](connectors/databases/sql-server.md)
- [MySQLコネクタ](connectors/databases/mysql.md)
- [Oracleコネクタ](connectors/databases/oracle.md)
- [フェニックスコネクタ](connectors/databases/phoenix.md)
- [PostgreSQLコネクタ](connectors/databases/postgres.md)

### マーケティングの自動化

Experience Platformは、サードパーティのマーケティング自動化システムからデータを取り込むためのサポートを提供します。 特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [HubSpotコネクタ](connectors/marketing-automation/hubspot.md)

### 支払い

Experience Platformは、サードパーティの支払いシステムからデータを取り込むためのサポートを提供します。 特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [PayPalコネクタ](connectors/payments/paypal.md)

### プロトコル

Experience Platformは、サードパーティのプロトコルシステムからデータを取り込むためのサポートを提供します。 特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [汎用ODataコネクタ](connectors/protocols/odata.md)

## データ取り込み時のソースのアクセス制御

データ取り込みのソースに対する権限は、アドビのAdmin Console内で管理できます。 権限には、特定の製品プロファイルの「 *権限* 」タブからアクセスできます。 権限を **編集パネルから** 、 *データ取り込み* メニューのエントリを使用して、ソースに関する権限にアクセスできます。 「 **表示ソース***」権限は「* カタログ *」タブの使用可能なソースおよび「認証されたソース」タブの読み取り専用アクセス権を付与し、「ソースの***** 管理」権限は「ソースの作成」、「編集」、「無効化」の各ソースに対する完全なアクセス権を付与します。

次の表に、権限の様々な組み合わせに基づくUIの動作を示します。

| 権限レベル | 説明 |
| ---- | ----|
| **表示ソース** ：オン | 「 *カタログ* 」タブの各ソースタイプのソースに対して、「 *参照*」、「 **** アカウント」、「DataFlow」の各タブと共に読み取り専用アクセス権を付与します。 |
| **ソースの管理** : | **表示ソースに含まれる機能に加えて、**&#x200B;ソースに含まれる機能に加えて、Connect Source *option Catalog内の* Connect Source *option Catalogへのアクセス権を付与し、Select Data Option* Browse *Not Browse*** Notにアクセス権を付与します。 **また、「ソースの管理** 」では、DataFlowsを有効または無効にしたり ** 、スケジュールを編集したりできます。 |
| **表示ソース** ：オフおよび **管理ソース** ：オフ | ソースへのアクセスをすべて取り消します。 |

Admin Consoleを通じて付与される権限（これら4つのソースを含む）の詳細については、 [アクセス制御の概要を参照してください](../access-control/home.md)。

## Terms and conditions {#terms-and-conditions}

ベータ版（「ベータ版」）と表示されたソースのいずれかを使用することにより、お客様は、いかなる種類の保証もなく、ベータ版が「現状のまま」提供され ***ることを本書で認めます***。

アドビは、ベータ版を保守、修正、更新、変更、変更、またはその他のサポートする義務はありません。 ベータ版および/または付属のマテリアルの正しい機能やパフォーマンスに一切依存しないように注意し、注意が必要です。 ベータ版はアドビの機密情報と見なされます。

お客様からアドビに提供される「フィードバック」（ベータ版の使用中に発生した問題や欠陥、提案、改善点、推奨事項を含む、ただしこれに限定しない、ベータ版に関する情報）は、すべての権利、役職、およびそのフィードバックに対する関心を含むアドビに割り当てられます。

Open Feedbackを送信するか、サポートチケットを作成して、ご提案を共有したり、バグを報告したり、機能強化を求めたりします。
