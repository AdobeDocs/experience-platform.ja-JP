---
keywords: Experience Platform;home;popular topics;source connectors;source connector;sources;data sources;data source;data source connection
solution: Experience Platform
title: Adobe Experience Platform ソースコネクタの概要
topic: overview
description: Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。
translation-type: tm+mt
source-git-commit: 5e5ac80e0c79b3cc0354b469edc036523e29b45d
workflow-type: tm+mt
source-wordcount: '887'
ht-degree: 57%

---


# ソースコネクタの概要

Adobe Experience Platform allows data to be ingested from external sources while providing you with the ability to structure, label, and enhance incoming data using [!DNL Platform] services. アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーへのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を提供します。これらのソース接続を使用すると、サードパーティ製システムの認証、取得の実行時間の設定、データ取得スループットの管理をおこなうことができます。

With [!DNL Experience Platform], you can centralize data you collect from disparate sources and use the insights gained from it to do more.

## ソースのタイプ

Sources in [!DNL Experience Platform] are grouped into the following categories:

### アドビアプリケーション

[!DNL Experience Platform] adobe analytics、Adobe Audience Manager、その他のAdobeアプリケーションからデータを取り込むことがで [!DNL Experience Platform Launch]きます。 詳しくは、次の関連ドキュメントを参照してください。

- [Adobe Audience Manager コネクタの概要](connectors/adobe-applications/audience-manager.md)
- [UI での Adobe Audience Manager ソースコネクタの作成](./tutorials/ui/create/adobe-applications/audience-manager.md)
- [Adobe Analytics分類データコネクタの概要](connectors/adobe-applications/classifications.md)
- [UIでのAdobe Analytics分類データソースコネクタの作成](./tutorials/ui/create/adobe-applications/classifications.md)
- [Adobe Analytics Data コネクタの概要](connectors/adobe-applications/analytics.md)
- [UI での Adobe Analytics ソースコネクタの作成](./tutorials/ui/create/adobe-applications/analytics.md)
- [UIでの顧客属性ソースコネクタの作成](./tutorials/ui/create/adobe-applications/customer-attributes.md)

### 広告

[!DNL Experience Platform] は、サードパーティの広告システムからデータを取り込むためのサポートを提供します。 特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

- [[!DNL Google AdWords]](connectors/advertising/ads.md) コネクタ

### クラウドストレージ

Cloud storage sources can bring your own data into [!DNL Platform] without the need to download, format, or upload. 取り込んだデータは、XDM JSON、XDMパーケー、または区切り文字として形式設定できます。 プロセスのすべての手順は、ユーザーインターフェイスを使用して Sources ワークフローに統合されます。詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Azure Data Lake Storage Gen2] コネクタ](connectors/cloud-storage/adls-gen2.md)
- [[!DNL Azure Blob] コネクタ](connectors/cloud-storage/blob.md)
- [[!DNL Amazon Kinesis] コネクタ](connectors/cloud-storage/kinesis.md)
- [[!DNL Amazon S3] コネクタ](connectors/cloud-storage/s3.md)
- [[!DNL Apache HDFS] コネクタ](connectors/cloud-storage/hdfs.md)
- [[!DNL Azure Event Hubs] コネクタ](connectors/cloud-storage/eventhub.md)
- [[!DNL Azure File Storage] コネクタ](connectors/cloud-storage/azure-file-storage.md)
- [[!DNL FTP] コネクタ](connectors/cloud-storage/ftp.md)
- [[!DNL Google Cloud Storage] コネクタ](connectors/cloud-storage/google-cloud-storage.md)
- [[!DNL SFTP] コネクタ](connectors/cloud-storage/sftp.md)

### 顧客関係管理（CRM）

CRM システムは顧客との関係を築くのに役立つデータを提供し、顧客との忠誠度を高め、顧客保持率を高めます。[!DNL Experience Platform] とからCRMデータを取り込むためのサポート [!DNL Microsoft Dynamics 365] を提供 [!DNL Salesforce]します。 詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Microsoft Dynamics] コネクタ](connectors/crm/ms-dynamics.md)
- [[!DNL Salesforce] コネクタ](connectors/crm/salesforce.md)

### 顧客の成功

[!DNL Experience Platform] は、サードパーティの顧客成功アプリケーションからデータを取得する機能を備えています。詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Salesforce Service Cloud] コネクタ](connectors/customer-success/salesforce-service-cloud.md)
- [[!DNL ServiceNow] コネクタ](connectors/customer-success/servicenow.md)

### データベース

[!DNL Experience Platform] は、サードパーティのデータベースからデータを取得する機能を備えています。特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

- [[!DNL Amazon Redshift] コネクタ](connectors/databases/redshift.md)
- [[!DNL Apache Hive on Azure HDInsights] コネクタ](connectors/databases/hive.md)
- [[!DNL Apache Spark on Azure HDInsights] コネクタ](connectors/databases/spark.md)
- [[!DNL Azure Data Explorer] コネクタ](connectors/databases/data-explorer.md)
- [[!DNL Azure Synapse Analytics] コネクタ](connectors/databases/synapse-analytics.md)
- [[!DNL Azure Table Storage] コネクタ](connectors/databases/ats.md)
- [[!DNL Couchbase] コネクタ](connectors/databases/couchbase.md)
- [[!DNL Google BigQuery] コネクタ](connectors/databases/bigquery.md)
- [[!DNL GreenPlum] コネクタ](connectors/databases/greenplum.md)
- [[!DNL HP Vertica] コネクタ](connectors/databases/hp-vertica.md)
- [[!DNL IBM DB2] コネクタ](connectors/databases/ibm-db2.md)
- [[!DNL Microsoft SQL Server] コネクタ](connectors/databases/sql-server.md)
- [[!DNL MySQL] コネクタ](connectors/databases/mysql.md)
- [[!DNL Oracle] コネクタ](connectors/databases/oracle.md)
- [[!DNL Phoenix] コネクタ](connectors/databases/phoenix.md)
- [[!DNL PostgreSQL] コネクタ](connectors/databases/postgres.md)

### eCommerce

[!DNL Experience Platform] は、サードパーティeCommerceシステムからデータを取り込むためのサポートを提供します。 特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

- [[!DNL Shopify]](connectors/ecommerce/shopify.md)

### マーケティングの自動処理

[!DNL Experience Platform] は、サードパーティのマーケティング自動化システムからデータを取得する機能を備えています。特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

- [[!DNL HubSpot] コネクタ](connectors/marketing-automation/hubspot.md)

### 支払い

[!DNL Experience Platform] は、サードパーティの支払いシステムからデータを取り込むためのサポートを提供します。 特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

- [[!DNL PayPal] コネクタ](connectors/payments/paypal.md)

### プロトコル

[!DNL Experience Platform] は、サードパーティのプロトコルシステムからデータを取り込むためのサポートを提供します。 特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

- [[!DNL Generic OData] コネクタ](connectors/protocols/odata.md)

## データ取得におけるソースのアクセス制御

データ取得元に対する権限は、Adobe Admin Console で管理できます。権限には、特定の製品プロファイルの「**[!UICONTROL 権限]**」タブからアクセスできます。**[!UICONTROL 権限を編集]**&#x200B;パネルから、**[!UICONTROL データ取得]**&#x200B;メニューのエントリを使用して、ソースに関連する権限にアクセスできます 。「**[!UICONTROL ソースの表示]**」権限は、「**[!UICONTROL カタログ]**」タブの使用可能なソースと「**[!UICONTROL 参照]**」タブの認証済みのソースへの読み取り専用アクセス権を付与する一方、「**[!UICONTROL ソースの管理]**」権限は、ソースの作成、編集および無効化に対するフルアクセス権を付与します。

次の表に、これらの権限の様々な組み合わせに基づく UI の動作の概要を示します。

| 権限レベル | 説明 |
| ---- | ----|
| **[!UICONTROL ソースの表示]**&#x200B;オン | 「カタログ」タブの各ソースタイプと、「参照」タブ、「アカウント」タブ、「データフロー」タブで、ソースへの読み取り専用アクセス権を付与します。 |
| **[!UICONTROL ソースの管理]**&#x200B;オン | 「**[!UICONTROL ソースの表示]**」に含まれる機能に加えて、「**[!UICONTROL カタログ]**」の「**[!UICONTROL ソースの接続]**」オプションおよび「**[!UICONTROL 参照]**」の「**[!UICONTROL データを選択]**」オプションへのアクセス権を付与します。また、「**[!UICONTROL ソースの管理]**」では、**[!UICONTROL データフロー]**&#x200B;の有効/無効を切り替えたり、スケジュールを編集したりできます。 |
| 「**[!UICONTROL ソースの表示]**」オフおよび「**[!UICONTROL ソースの管理]**」オフ | ソースへのすべてのアクセスを取り消します。 |

Admin Console から付与される使用可能な権限（これら 4 つのソースを含む）の詳細については、「[アクセス制御の概要](../access-control/home.md)」を参照してください。

## Terms and conditions {#terms-and-conditions}

ベータ版（「ベータ版」）と表示されたソースのいずれかを使用することにより、お客様は、いかなる種類の保証もなく、ベータ版が「現状のまま」提供され ***ることを本書で認めます***。

Adobeは、ベータ版を保守、修正、更新、変更、またはその他の方法でサポートする義務はありません。 ベータ版および/または付属のマテリアルの正しい機能やパフォーマンスに一切依存しないように注意し、注意が必要です。 ベータ版はAdobeの機密情報と見なされます。

お客様がAdobeに提供する「フィードバック」（ベータ版の使用中に発生した問題や欠陥、提案、改善点、推奨事項を含む、ただしこれに限定しない、ベータ版に関する情報）は、本書により、そのフィードバックに対するすべての権利、役職、関心を含むAdobeに割り当てられます。

Open Feedbackを送信するか、サポートチケットを作成して、ご提案を共有したり、バグを報告したり、機能強化を求めたりします。
