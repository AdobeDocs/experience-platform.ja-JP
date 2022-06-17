---
keywords: Experience Platform;ホーム;人気のトピック;ソースコネクタ;ソースコネクタ;ソース;データソース;データソース;データソース接続
solution: Experience Platform
title: ソースコネクタの概要
topic-legacy: overview
description: Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。
exl-id: efdbed4d-5697-43ef-a47a-a8bcf0f13237
source-git-commit: 16f61ae259f6da23cfb7aa96e685716cd623d1b2
workflow-type: ht
source-wordcount: '0'
ht-degree: 100%

---

# ソースコネクタの概要

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[!DNL Flow Service] を使用すると、様々な異なるソースから顧客データを収集し、Platform で一元化できます。このサービスにはユーザーインターフェイスおよび RESTful API が用意されており、様々なデータプロバイダーへのソース接続を簡単に設定できます。これらのソース接続を使用すると、サードパーティ製システムの認証、取得の実行時間の設定、データ取得スループットの管理を行うことができます。

Experience Platform を使用すると、異なるソースから収集したデータを一元管理し、得たインサイトを利用して、より多くの作業を行うことができます。

## ソースのタイプ

Experience Platform のソースは、次のカテゴリに分類されます。

### アドビアプリケーション {#adobe-applications}

Adobe Experience Platform を使用すると、Adobe Analytics や Adobe Audience Manager など、他のアドビアプリケーションからデータを取り込むことができます。詳しくは、次の関連ドキュメントを参照してください。

- [Adobe Audience Manager ソースの概要](connectors/adobe-applications/audience-manager.md)
- [UI での Adobe Audience Manager ソースコネクタの作成](./tutorials/ui/create/adobe-applications/audience-manager.md)
- [Adobe Analytics Classifications Data ソース接続の概要](connectors/adobe-applications/classifications.md)
- [UI での Adobe Analytics Classifications Data ソース接続の作成](./tutorials/ui/create/adobe-applications/classifications.md)
- [Adobe Analytics Report Suite Data ソース接続の概要](connectors/adobe-applications/analytics.md)
- [UI での Adobe Analytics ソースコネクタの作成](./tutorials/ui/create/adobe-applications/analytics.md)
- [Adobe Data Collection ソースの概要](connectors/adobe-applications/data-collection.md)
- [UI での Customer Attributes ソース接続の作成](./tutorials/ui/create/adobe-applications/customer-attributes.md)
- [[!DNL Marketo Engage] ソースの概要](connectors/adobe-applications/marketo/marketo.md)
- [UI での [!DNL Marketo Engage] ソース接続の作成](./tutorials/ui/create/adobe-applications/marketo.md)

### 広告 {#advertising}

Adobe Experience Platform には、サードパーティの広告システムからデータを取り込む機能が用意されています。特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Google AdWords]](connectors/advertising/ads.md)

<!-- ### Analytics {#analytics}

Experience Platform provides support for ingesting data from a third-party analytics platform. See the following related documents for more information:

- [[!DNL Mixpanel]](connectors/analytics/mixpanel.md) -->

### クラウドストレージ {#cloud-storage}

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードを行う必要なく、独自のデータを Platform に取り込むことができます。取り込んだデータは、XDM JSON、XDM Parquet 形式または区切り形式で書式設定できます。 プロセスのすべての手順は、ユーザーインターフェイスを使用した Sources ワークフローに統合されています。詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Azure Data Lake Storage Gen2]](connectors/cloud-storage/adls-gen2.md)
- [[!DNL Azure Blob]](connectors/cloud-storage/blob.md)
- [[!DNL Amazon Kinesis]](connectors/cloud-storage/kinesis.md)
- [[!DNL Amazon S3]](connectors/cloud-storage/s3.md)
- [[!DNL Apache HDFS]](connectors/cloud-storage/hdfs.md)
- [[!DNL Azure Event Hubs]](connectors/cloud-storage/eventhub.md)
- [[!DNL Azure File Storage]](connectors/cloud-storage/azure-file-storage.md)
- [[!DNL Data Landing Zone]](connectors/cloud-storage/data-landing-zone.md)
- [[!DNL FTP]](connectors/cloud-storage/ftp.md)
- [[!DNL Google Cloud Storage]](connectors/cloud-storage/google-cloud-storage.md)
- [[!DNL Google PubSub]](connectors/cloud-storage/google-pubsub.md)
- [[!DNL Oracle Object Storage]](connectors/cloud-storage/oracle-object-storage.md)
- [[!DNL SFTP]](connectors/cloud-storage/sftp.md)

### 同意および環境設定 {#consent}

Experience Platform は、サードパーティの同意および環境設定管理プラットフォームからデータを取り込む機能を備えています。詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL OneTrust Integration]](connectors/consent-and-preferences/onetrust.md)

### 顧客関係管理（CRM） {#customer-relationship-management}

CRM システムは顧客との関係を築くのに役立つデータを提供し、顧客との忠誠度を高め、顧客保持率を高めます。Adobe Experience Platform には、[!DNL Microsoft Dynamics 365] および [!DNL Salesforce] から CRM データを取り込む機能が用意されています。詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Microsoft Dynamics]](connectors/crm/ms-dynamics.md)
- [[!DNL Salesforce]](connectors/crm/salesforce.md)
- [[!DNL Veeva CRM]](connectors/crm/veeva.md)
- [[!DNL Zoho CRM]](connectors/crm/zoho.md)

### カスタマーサクセス {#customer-success}

Experience Platform は、サードパーティの顧客成功アプリケーションからデータを取得する機能を備えています。詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Salesforce Service Cloud]](connectors/customer-success/salesforce-service-cloud.md)
- [[!DNL ServiceNow]](connectors/customer-success/servicenow.md)
- [[!DNL Zendesk]](connectors/customer-success/zendesk.md)

### データベース {#database}

Experience Platform は、サードパーティのデータベースからデータを取得する機能を備えています。特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Amazon Redshift]](connectors/databases/redshift.md)
- [[!DNL Apache Hive on Azure HDInsights]](connectors/databases/hive.md)
- [[!DNL Apache Spark on Azure HDInsights]](connectors/databases/spark.md)
- [[!DNL Azure Data Explorer]](connectors/databases/data-explorer.md)
- [[!DNL Azure Synapse Analytics]](connectors/databases/synapse-analytics.md)
- [[!DNL Azure Table Storage]](connectors/databases/ats.md)
- [[!DNL Couchbase]](connectors/databases/couchbase.md)
- [[!DNL Google BigQuery]](connectors/databases/bigquery.md)
- [[!DNL GreenPlum]](connectors/databases/greenplum.md)
- [[!DNL HP Vertica]](connectors/databases/hp-vertica.md)
- [[!DNL IBM DB2]](connectors/databases/ibm-db2.md)
- [[!DNL MariaDB]](connectors/databases/mariadb.md)
- [[!DNL Microsoft SQL Server]](connectors/databases/sql-server.md)
- [[!DNL MySQL]](connectors/databases/mysql.md)
- [[!DNL Oracle]](connectors/databases/oracle.md)
- [[!DNL Phoenix]](connectors/databases/phoenix.md)
- [[!DNL PostgreSQL]](connectors/databases/postgres.md)
- [[!DNL Snowflake]](connectors/databases/snowflake.md)

### e コマース {#ecommerce}

Adobe Experience Platform には、サードパーティの e コマースシステムからデータを取り込む機能が用意されています。特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Shopify]](connectors/ecommerce/shopify.md)

### ローカルシステム {#local-system}

Adobe Experience Platform には、ローカルシステムからデータを取り込む機能が用意されています。 特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [ローカルファイルのアップロード](connectors/local-system/local-file-upload.md)

### マーケティングの自動処理 {#marketing-automation}

Experience Platform は、サードパーティのマーケティング自動化システムからデータを取得する機能を備えています。特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL HubSpot]](connectors/marketing-automation/hubspot.md)
- [[!DNL Mailchimp]](connectors/marketing-automation/mailchimp.md)
- [[!DNL Oracle Eloqua]](connectors/marketing-automation/oracle-eloqua.md)
- [[!DNL Salesforce Marketing Cloud]](connectors/marketing-automation/salesforce-marketing-cloud.md)

### 支払い {#payments}

Adobe Experience Platform には、サードパーティの支払いシステムからデータを取り込む機能が用意されています。特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL PayPal]](connectors/payments/paypal.md)
- [[!DNL Square]](connectors/payments/square.md)

### ストリーミング {#streaming}

Adobe Experience Platform には、ストリーミングソースからデータを取り込む機能が用意されています。特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL HTTP API]](connectors/streaming/http.md)

### プロトコル {#protocols}

Adobe Experience Platform には、サードパーティのプロトコルシステムからデータを取り込む機能が用意されています。特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Generic OData]](connectors/protocols/odata.md)
- [[!DNL Generic REST API]](connectors/protocols/generic-rest.md)

## データ取得におけるソースのアクセス制御

データ取得元に対する権限は、Adobe Admin Console で管理できます。権限には、特定の製品プロファイルの「**[!UICONTROL 権限]**」タブからアクセスできます。**[!UICONTROL 権限を編集]**&#x200B;パネルから、**[!UICONTROL データ取得]**&#x200B;メニューのエントリを使用して、ソースに関連する権限にアクセスできます。「**[!UICONTROL ソースの表示]**」権限は、「**[!UICONTROL カタログ]**」タブの使用可能なソースと「**[!UICONTROL 参照]**」タブの認証済みのソースへの読み取り専用アクセス権を付与する一方、「**[!UICONTROL ソースの管理]**」権限は、ソースの作成、編集および無効化に対するフルアクセス権を付与します。

次の表に、これらの権限の様々な組み合わせに基づく UI の動作の概要を示します。

| 権限レベル | 説明 |
| ---- | ----|
| **[!UICONTROL ソースの表示]**&#x200B;オン | 「Catalog」タブ、「Browse」タブ、「Accounts」タブ、「Dataflow」タブの各ソースタイプのソースに読み取り専用アクセス権を付与します。 |
| **[!UICONTROL ソースの管理]**&#x200B;オン | 「**[!UICONTROL ソースの表示]**」に含まれる機能に加えて、「**[!UICONTROL カタログ]**」の「**[!UICONTROL ソースの接続]**」オプションおよび「**[!UICONTROL 参照]**」の「**[!UICONTROL データを選択]**」オプションへのアクセス権を付与します。また、「**[!UICONTROL ソースの管理]**」では、**[!UICONTROL データフロー]**&#x200B;の有効/無効を切り替えたり、スケジュールを編集したりできます。 |
| 「**[!UICONTROL ソースの表示]**」オフおよび「**[!UICONTROL ソースの管理]**」オフ | ソースへのすべてのアクセスを取り消します。 |

Admin Console から付与される使用可能な権限（これら 4 つのソースを含む）について詳しくは、「[アクセス制御の概要](../access-control/home.md)」を参照してください。

## 利用条件 {#terms-and-conditions}

「ベータ版」としてラベル付けされたソースを使用することにより、お客様は、ベータ版が&#x200B;***「現状のまま」でいかなる保証もなく***&#x200B;提供されていることを承諾します。

アドビは、ベータ版を維持、訂正、更新、変更、修正、またはその他の方法でサポートする義務を負いません。このようなベータ版および付属の資料またはそのいずれかが、正しく機能しパフォーマンスを提供することに関して、お客様は注意を払い、いかなる形でも依存しないことをお勧めします。ベータ版はアドビの機密情報と見なされます。

お客様がアドビに提供するあらゆる「フィードバック」（ベータ版の使用中に発生した問題や欠陥、提案、改善、推奨事項を含むがこれに限定されないベータ版に関する情報）は、このようなフィードバックに含まれる、およびフィードバックに対するすべての権利、所有権、利益を含め、アドビに帰属します。

公開フィードバックを送信するかサポートチケットを作成して、お客様の提案を共有したりバグを報告したりして、機能強化にご協力ください。
