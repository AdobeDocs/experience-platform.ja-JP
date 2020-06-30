---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformソースコネクタの概要
topic: overview
translation-type: tm+mt
source-git-commit: 6ffdcc2143914e2ab41843a52dc92344ad51bcfb
workflow-type: tm+mt
source-wordcount: '800'
ht-degree: 1%

---


# ソースコネクタの概要

Adobe Experience Platform allows data to be ingested from external sources while providing you with the ability to structure, label, and enhance incoming data using [!DNL Platform] services. アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] には、様々なデータプロバイダーへのソース接続を簡単に設定できる、RESTful APIとインタラクティブUIが用意されています。 これらのソース接続を使用すると、サードパーティ製システムの認証、インジェストの実行時間の設定、データインジェストのスループットの管理を行うことができます。

こ [!DNL Experience Platform]れにより、異なるソースから収集したデータを一元管理し、得られた洞察を利用してより多くの作業を行うことができます。

## ソースのタイプ

のソース [!DNL Experience Platform] は、次のカテゴリにグループ化されます。

### アドビアプリケーション

[!DNL Experience Platform] アドビのAnalytics、Adobe Audience Manager、その他のアドビアプリケーションなど、他のアドビアプリケーションからデータを取り込むことができ [!DNL Experience Platform Launch]ます。 詳しくは、次の関連ドキュメントを参照してください。

- [Adobe Audience Managerコネクタの概要](connectors/adobe-applications/audience-manager.md)
- [UIでのAdobe Audience Managerソースコネクタの作成](./tutorials/ui/create/adobe-applications/audience-manager.md)
- [Adobe Data Connectorの概要](connectors/adobe-applications/analytics.md)
- [UIでのAdobeAnalyticsソースコネクタの作成](./tutorials/ui/create/adobe-applications/analytics.md)
- [UIでの顧客属性ソースコネクタの作成](./tutorials/ui/create/adobe-applications/customer-attributes.md)

### 広告

[!DNL Experience Platform] は、サードパーティの広告システムからデータを取り込むためのサポートを提供します。 特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [!DNL Google AdWords](connectors/advertising/ads.md) コネクタ

### クラウドストレージ

Cloudストレージソースからは、ダウンロード、フォーマット、アップロードを行う [!DNL Platform] ことなく、独自のデータをに取り込むことができます。 取り込んだデータは、XDM JSON、XDMパーケー、または区切り文字として形式設定できます。 プロセスの各手順は、ユーザーインターフェイスを使用してSourcesワークフローに統合されます。 詳しくは、次の関連ドキュメントを参照してください。

- [!DNL Azure Data Lake Storage Gen2](connectors/cloud-storage/adls-gen2.md) コネクタ
- [!DNL Azure Blob and Amazon S3](connectors/cloud-storage/blob-s3.md) コネクタ
- [!DNL Amazon Kinesis](connectors/cloud-storage/kinesis.md) コネクタ
- [!DNL Apache HDFS](connectors/cloud-storage/hdfs.md) コネクタ
- [!DNL Azure Event Hubs](connectors/cloud-storage/eventhub.md) コネクタ
- [!DNL Azure File Storage](connectors/cloud-storage/azure-file-storage.md) コネクタ
- [!DNL FTP and SFTP](connectors/cloud-storage/ftp-sftp.md) コネクタ
- [!DNL Google Cloud Storage](connectors/cloud-storage/google-cloud-storage.md) コネクタ

### 顧客関係管理（CRM）

CRMシステムは、顧客との関係の構築に役立つデータを提供し、顧客の忠誠度を高め、顧客の定着を促進します。 [!DNL Experience Platform] とからCRMデータを取り込むためのサポート [!DNL Microsoft Dynamics 365] を提供 [!DNL Salesforce]します。 詳しくは、次の関連ドキュメントを参照してください。

- [!DNL Microsoft Dynamics](connectors/crm/ms-dynamics.md) コネクタ
- [!DNL Salesforce](connectors/crm/salesforce.md) コネクタ

### 顧客の成功

[!DNL Experience Platform] は、サードパーティの顧客成功アプリケーションからデータを取り込むためのサポートを提供します。 詳しくは、次の関連ドキュメントを参照してください。

- [!DNL Salesforce Service Cloud](connectors/customer-success/salesforce-service-cloud.md) コネクタ
- [!DNL ServiceNow](connectors/customer-success/servicenow.md) コネクタ

### データベース

[!DNL Experience Platform] は、サードパーティのデータベースからデータを取り込むためのサポートを提供します。 特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [!DNL Amazon Redshift](connectors/databases/redshift.md) コネクタ
- [!DNL Apache Hive on Azure HDInsights](connectors/databases/hive.md) コネクタ
- [!DNL Apache Spark on Azure HDInsights](connectors/databases/spark.md) コネクタ
- [!DNL Azure Data Explorer](connectors/databases/data-explorer.md) コネクタ
- [!DNL Azure Synapse Analytics](connectors/databases/synapse-analytics.md) コネクタ
- [!DNL Azure Table Storage](connectors/databases/ats.md) コネクタ
- [!DNL Couchbase](connectors/databases/couchbase.md) コネクタ
- [!DNL Google BigQuery](connectors/databases/bigquery.md) コネクタ
- [!DNL GreenPlum](connectors/databases/greenplum.md) コネクタ
- [!DNL HP Vertica](connectors/databases/hp-vertica.md) コネクタ
- [!DNL IBM DB2](connectors/databases/ibm-db2.md) コネクタ
- [!DNL MariaDB](connectors/databases/mariadb.md) コネクタ
- [!DNL Microsoft SQL Server](connectors/databases/sql-server.md) コネクタ
- [!DNL MySQL](connectors/databases/mysql.md) コネクタ
- [!DNL Oracle](connectors/databases/oracle.md) コネクタ
- [!DNL Phoenix](connectors/databases/phoenix.md) コネクタ
- [!DNL PostgreSQL](connectors/databases/postgres.md) コネクタ

### マーケティングの自動化

[!DNL Experience Platform] は、サードパーティのマーケティング自動化システムからデータを取り込むためのサポートを提供します。 特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [!DNL HubSpot](connectors/marketing-automation/hubspot.md) コネクタ

### 支払い

[!DNL Experience Platform] は、サードパーティの支払いシステムからデータを取り込むためのサポートを提供します。 特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [!DNL PayPal](connectors/payments/paypal.md) コネクタ

### プロトコル

[!DNL Experience Platform] は、サードパーティのプロトコルシステムからデータを取り込むためのサポートを提供します。 特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [!DNL Generic OData](connectors/protocols/odata.md) コネクタ

## データ取り込み時のソースのアクセス制御

データ取り込みのソースに対する権限は、アドビのAdmin Console内で管理できます。 権限には、特定の製品プロファイルの「 *[!UICONTROL 権限]* 」タブからアクセスできます。 権限を **[!UICONTROL 編集パネルから]** 、 *[!UICONTROL データ取り込み]* メニューのエントリを使用して、ソースに関する権限にアクセスできます。 「 **[!UICONTROL 表示ソース]***[!UICONTROL 」権限は「]* カタログ *[!UICONTROL 」タブの使用可能なソースおよび「認証されたソース」タブの読み取り専用アクセス権を付与し、「ソースの]***** 管理」権限は「ソースの作成」、「編集」、「無効化」の各ソースに対する完全なアクセス権を付与します。

次の表に、権限の様々な組み合わせに基づくUIの動作を示します。

| 権限レベル | 説明 |
| ---- | ----|
| **[!UICONTROL 表示ソース]** ：オン | 「 *カタログ* 」タブの各ソースタイプのソースに対して、「 *参照*」、「 **** アカウント」、「DataFlowAccounts」の各タブと共に読み取り専用アクセス権を付与します。 |
| **[!UICONTROL ソースの管理]** : | **[!UICONTROL 表示ソースに含まれる機能に加えて、]**&#x200B;ソースに含まれる機能に加えて、Connect Source *[!UICONTROL option Catalog内の]* Connect Source *[!UICONTROL option Catalogへのアクセス権を付与し、Select Data Option]* Browse *[!UICONTROL Not Browse]*** Notにアクセス権を付与します。 **[!UICONTROL また、「ソースの管理]** 」では、DataFlowsを有効または無効にしたり ** 、スケジュールを編集したりできます。 |
| **[!UICONTROL 表示ソース]** ：オフおよび **[!UICONTROL 管理ソース]** ：オフ | ソースへのアクセスをすべて取り消します。 |

Admin Consoleを通じて付与される権限（これら4つのソースを含む）の詳細については、 [アクセス制御の概要を参照してください](../access-control/home.md)。

## Terms and conditions {#terms-and-conditions}

ベータ版（「ベータ版」）と表示されたソースのいずれかを使用することにより、お客様は、いかなる種類の保証もなく、ベータ版が「現状のまま」提供され ***ることを本書で認めます***。

アドビは、ベータ版を保守、修正、更新、変更、変更、またはその他のサポートする義務はありません。 ベータ版および/または付属のマテリアルの正しい機能やパフォーマンスに一切依存しないように注意し、注意が必要です。 ベータ版はアドビの機密情報と見なされます。

お客様からアドビに提供される「フィードバック」（ベータ版の使用中に発生した問題や欠陥、提案、改善点、推奨事項を含む、ただしこれに限定しない、ベータ版に関する情報）は、すべての権利、役職、およびそのフィードバックに対する関心を含むアドビに割り当てられます。

Open Feedbackを送信するか、サポートチケットを作成して、ご提案を共有したり、バグを報告したり、機能強化を求めたりします。
