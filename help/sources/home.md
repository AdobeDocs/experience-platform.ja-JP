---
keywords: エクスペリエンス Platform、home、ポピュラーなトピック、ソースコネクター、ソースコネクター、ソース、データソース、データソース、データソース接続、データソース接続
solution: Experience Platform
title: ソースコネクタの概要
topic-legacy: overview
description: Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。
exl-id: efdbed4d-5697-43ef-a47a-a8bcf0f13237
source-git-commit: f8cecdaaab3d98c7f6542b51dc764a019b04b0b1
workflow-type: tm+mt
source-wordcount: '1008'
ht-degree: 56%

---

# ソースコネクタの概要

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[!DNL Flow Service] は、多種多様なプラットフォーム内の顧客データを収集して集中管理するために使用されます。 このサービスでは、ユーザーインターフェイスと RESTful API を使用して、様々なデータプロバイダーへのソース接続を容易に設定できるようになっています。 これらのソース接続を使用すると、サードパーティ製システムの認証、取得の実行時間の設定、データ取得スループットの管理をおこなうことができます。

Experience Platform を使用すると、異なるソースから収集したデータを一元管理し、得たインサイトを利用して、より多くの作業をおこなうことができます。

## ソースのタイプ

Experience Platform のソースは、次のカテゴリに分類されます。

### アドビアプリケーション

エクスペリエンスプラットフォームを使用すると、adobe アナリティクスや Adobe Audience Manager など、他の Adobe アプリケーションからデータを ingested することができます。 詳しくは、次の関連ドキュメントを参照してください。

- [Adobe Audience Manager コネクタの概要](connectors/adobe-applications/audience-manager.md)
- [UI での Adobe オーディエンス Manager ソース接続の作成](./tutorials/ui/create/adobe-applications/audience-manager.md)
- [Adobe アナリティクス分類データソース接続の概要](connectors/adobe-applications/classifications.md)
- [UI での Adobe アナリティクス分類データソース接続の作成](./tutorials/ui/create/adobe-applications/classifications.md)
- [Adobe アナリティクスレポートスイートデータソース接続の概要](connectors/adobe-applications/analytics.md)
- [UI での Adobe アナリティクスソース接続の作成](./tutorials/ui/create/adobe-applications/analytics.md)
- [UI での顧客属性ソース接続の作成](./tutorials/ui/create/adobe-applications/customer-attributes.md)
- [[!DNL Marketo Engage] コネクタの概要](connectors/adobe-applications/marketo/marketo.md)
- [ [!DNL Marketo Engage] UI でのソース接続の作成](./tutorials/ui/create/adobe-applications/marketo.md)

### Advertising Cloud

エクスペリエンスプラットフォームは、サードパーティの広告システムからの ingesting データのサポートを提供します。 特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

- [[!DNL Google AdWords]](connectors/advertising/ads.md) コネクタ

### クラウドストレージ

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードをおこなう必要なく、独自のデータを Platform に取り込むことができます。Ingested データは、XDM、XDM Parquet、または区切られた形式にフォーマットすることができます。 プロセスのすべての手順は、ユーザーインターフェイスを使用して Sources ワークフローに統合されます。詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Azure Data Lake Storage Gen2] コネクタ](connectors/cloud-storage/adls-gen2.md)
- [[!DNL Azure Blob] コネクタ](connectors/cloud-storage/blob.md)
- [[!DNL Amazon Kinesis] コネクタ](connectors/cloud-storage/kinesis.md)
- [[!DNL Amazon S3] コネクタ](connectors/cloud-storage/s3.md)
- [[!DNL Apache HDFS] コネクタ](connectors/cloud-storage/hdfs.md)
- [[!DNL Azure Event Hubs] コネクタ](connectors/cloud-storage/eventhub.md)
- [[!DNL Azure File Storage] コネクタ](connectors/cloud-storage/azure-file-storage.md)
- [[!DNL Data Landing Zone]](connectors/cloud-storage/data-landing-zone.md)
- [[!DNL FTP] コネクタ](connectors/cloud-storage/ftp.md)
- [[!DNL Google Cloud Storage] コネクタ](connectors/cloud-storage/google-cloud-storage.md)
- [[!DNL Google PubSub] コネクタ](connectors/cloud-storage/google-pubsub.md)
- [[!DNL Oracle Object Storage] コネクタ](connectors/cloud-storage/oracle-object-storage.md)
- [[!DNL SFTP] コネクタ](connectors/cloud-storage/sftp.md)

### 顧客関係管理（CRM）

CRM システムは顧客との関係を築くのに役立つデータを提供し、顧客との忠誠度を高め、顧客保持率を高めます。エクスペリエンスプラットフォームは、およびからの ingesting CRM データのサポートを提供し [!DNL Microsoft Dynamics 365] [!DNL Salesforce] ます。 詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Microsoft Dynamics] コネクタ](connectors/crm/ms-dynamics.md)
- [[!DNL Salesforce] コネクタ](connectors/crm/salesforce.md)
- [[!DNL Veeva CRM]](connectors/crm/veeva.md)
- [[!DNL Zoho CRM]](connectors/crm/zoho.md)

### お客様の成功

Experience Platform は、サードパーティの顧客成功アプリケーションからデータを取得する機能を備えています。詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Salesforce Service Cloud] コネクタ](connectors/customer-success/salesforce-service-cloud.md)
- [[!DNL ServiceNow] コネクタ](connectors/customer-success/servicenow.md)

### データベース

Experience Platform は、サードパーティのデータベースからデータを取得する機能を備えています。特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

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
- [[!DNL MariaDB] コネクタ](connectors/databases/mariadb.md)
- [[!DNL Microsoft SQL Server] コネクタ](connectors/databases/sql-server.md)
- [[!DNL MySQL] コネクタ](connectors/databases/mysql.md)
- [[!DNL Oracle] コネクタ](connectors/databases/oracle.md)
- [[!DNL Phoenix] コネクタ](connectors/databases/phoenix.md)
- [[!DNL PostgreSQL] コネクタ](connectors/databases/postgres.md)
- [[!DNL Snowflake] コネクタ](connectors/databases/snowflake.md)

### e コマース

エクスペリエンスプラットフォームは、サードパーティの電子商取引システムからのデータの ingesting をサポートします。 特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

- [[!DNL Shopify]](connectors/ecommerce/shopify.md)

### ローカルシステム

エクスペリエンスプラットフォームは、ローカルシステムからのデータの ingesting をサポートします。 特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

- [ローカルファイルのアップロード](connectors/local-system/local-file-upload.md)

### マーケティングの自動処理

Experience Platform は、サードパーティのマーケティング自動化システムからデータを取得する機能を備えています。特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

- [[!DNL HubSpot] コネクタ](connectors/marketing-automation/hubspot.md)
- [[!DNL MailChimp Campaign]](./tutorials/api/create/marketing-automation/mailchimp-campaign.md)
- [[!DNL MailChimp Members]](./tutorials/api/create/marketing-automation/mailchimp-members.md)
- [[!DNL Salesforce Marketing Cloud]](connectors/marketing-automation/salesforce-marketing-cloud.md)

### 行わ

エクスペリエンスプラットフォームは、サードパーティの支払システムからのデータの ingesting をサポートします。 特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

- [[!DNL PayPal] コネクタ](connectors/payments/paypal.md)

### ストリーミング

エクスペリエンスプラットフォームは、ストリーミングソースからのデータの ingesting をサポートします。 特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

- [[!DNL HTTP API]](connectors/streaming/http.md)

### プロトコル

エクスペリエンスプラットフォームは、サードパーティのプロトコルシステムからのデータの ingesting をサポートします。 特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

- [[!DNL Generic OData] コネクタ](connectors/protocols/odata.md)

## データ取得におけるソースのアクセス制御

データ取得元に対する権限は、Adobe Admin Console で管理できます。権限には、特定の製品プロファイルの「**[!UICONTROL 権限]**」タブからアクセスできます。**[!UICONTROL 権限を編集]**&#x200B;パネルから、**[!UICONTROL データ取得]**&#x200B;メニューのエントリを使用して、ソースに関連する権限にアクセスできます 。「**[!UICONTROL ソースの表示]**」権限は、「**[!UICONTROL カタログ]**」タブの使用可能なソースと「**[!UICONTROL 参照]**」タブの認証済みのソースへの読み取り専用アクセス権を付与する一方、「**[!UICONTROL ソースの管理]**」権限は、ソースの作成、編集および無効化に対するフルアクセス権を付与します。

次の表に、これらの権限の様々な組み合わせに基づく UI の動作の概要を示します。

| 権限レベル | 説明 |
| ---- | ----|
| **[!UICONTROL ソースの表示]**&#x200B;オン | 「カタログ」タブの各ソースタイプにおいて、「参照」、「勘定」および「流れ図」の各ソースには読み取り専用のアクセスを許可します。 |
| **[!UICONTROL ソースの管理]**&#x200B;オン | 「**[!UICONTROL ソースの表示]**」に含まれる機能に加えて、「**[!UICONTROL カタログ]**」の「**[!UICONTROL ソースの接続]**」オプションおよび「**[!UICONTROL 参照]**」の「**[!UICONTROL データを選択]**」オプションへのアクセス権を付与します。また、「**[!UICONTROL ソースの管理]**」では、**[!UICONTROL データフロー]**&#x200B;の有効/無効を切り替えたり、スケジュールを編集したりできます。 |
| 「**[!UICONTROL ソースの表示]**」オフおよび「**[!UICONTROL ソースの管理]**」オフ | ソースへのすべてのアクセスを取り消します。 |

Admin Console から付与される使用可能な権限（これら 4 つのソースを含む）の詳細については、「[アクセス制御の概要](../access-control/home.md)」を参照してください。

## 利用条件 {#terms-and-conditions}

ベータ (&quot;ベータ版&quot;) というラベルが付いたソースのいずれかを使用すると、ベータ版に「現状のまま」が提供されているということを、 ***何らいかなる種類の保証なしであるものとし*** ます。

アドビは、ベータ版を維持、修正、更新、変更、修正、またはサポートする必要がありません。このようなベータ版や付属資料の適切な機能やパフォーマンスについては、慎重に使用してください。 ベータ版は、アドビの機密情報として認識されています。

「フィードバック」 (「フィードバック」 (ベータ版に関する情報を含むが、ベータ版では問題または不具合のみが含まれていますが、このようなフィードバックはお勧めします)。これにより、すべての権利、権原、関心、そのようなフィードバックが含まれます。

オープンなフィードバックを送信するか、サポートチケットを作成して、提案を共有したり、バグを報告したりするには、機能強化をシークします。
