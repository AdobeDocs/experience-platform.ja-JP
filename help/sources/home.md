---
keywords: Experience Platform;ホーム;人気のトピック;ソースコネクタ;ソースコネクタ;ソース;データソース;データソース;データソース接続
solution: Experience Platform
title: ソースコネクタの概要
description: Adobe Experience Platformを使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。
exl-id: efdbed4d-5697-43ef-a47a-a8bcf0f13237
source-git-commit: 2e4df13bae9f4afa24f761e650790704da44da90
workflow-type: tm+mt
source-wordcount: '1554'
ht-degree: 67%

---

# ソースコネクタの概要

Adobe Experience Platformを使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[!DNL Flow Service] を使用すると、様々な異なるソースから顧客データを収集し、Experience Platformで一元化できます。 このサービスにはユーザーインターフェイスおよび RESTful API が用意されており、様々なデータプロバイダーへのソース接続を簡単に設定できます。これらのソース接続を使用すると、サードパーティ製システムの認証、データ取り込み時間の設定、データ取り込みスループットの管理を行うことができます。

Experience Platform を使用すると、異なるソースから収集したデータを一元管理し、得たインサイトを利用して、より多くの作業を行うことができます。

<div id="recs-overview-body-1"></div>
<div id="recs-overview-body-2"></div>
<div id="recs-overview-body-3"></div>
<div id="recs-overview-body-4"></div>
<div id="recs-overview-body-5"></div>
<div id="recs-overview-body-6"></div>

## 高度なエンタープライズソース {#advanced-enterprise-sources}

次のソースは、[Adobe Real-Time Customer Data Platform Ultimate](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2c-edition-prime-and-ultimate-packages.html) のお客様のみが利用できます。

- [[!DNL Amazon Kinesis]](connectors/cloud-storage/kinesis.md) [!BADGE &#x200B; ストリーミング &#x200B;]{type=Positive}
- [[!DNL Amazon Redshift]](connectors/databases/redshift.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Azure Databricks]](connectors/databases/databricks.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Azure Event Hubs]](connectors/cloud-storage/eventhub.md) [!BADGE &#x200B; ストリーミング &#x200B;]{type=Positive}
- [[!DNL Azure Synapse Analytics]](connectors/databases/synapse-analytics.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Google BigQuery]](connectors/databases/bigquery.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Google PubSub]](connectors/cloud-storage/google-pubsub.md) [!BADGE &#x200B; ストリーミング &#x200B;]{type=Positive}
- [[!DNL Snowflake]](connectors/databases/snowflake-streaming.md) [!BADGE &#x200B; ストリーミング &#x200B;]{type=Positive}
- [[!DNL Snowflake]](connectors/databases/snowflake.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}

## Adobeで構築されたソースとパートナーが構築したソース {#adobe-and-partner-built-sources}

Experience Platform ソースカタログ内のコネクタには、Adobeで構築および管理されるものと、[Sources SDK](/help/sources/sources-sdk/overview.md) を使用してパートナー企業が構築および管理するものがあります。 各パートナー構築コネクタのドキュメントページの上部にあるメモは、ソースがパートナーによって作成および管理される場合、を呼び出します。 例えば、[Amazon S3 コネクタ ](/help/sources/connectors/cloud-storage/s3.md) はAdobeによって作成され、&lbrace;RainFocus コネクタ [&#128279;](/help/sources/connectors/analytics/rainfocus.md) は RainFocus チームによって作成および管理されます 。

パートナーが作成および管理するコネクタの場合、コネクタに関する問題をパートナーチームが解決する必要が生じる場合があります（ドキュメントページのメモに記載されている連絡先方法）。アドビが作成および管理するコネクタに関する問題については、アドビ担当者またはカスタマーケア担当者にお問い合わせください。

## ソースカテゴリ

Experience Platform のソースは、次のカテゴリに分類されます。

### アドビアプリケーション {#adobe-applications}

Adobe Experience Platform を使用すると、Adobe Analytics や Adobe Audience Manager など、他のアドビアプリケーションからデータを取り込むことができます。詳しくは、次の関連ドキュメントを参照してください。

- [Adobe Audience Manager ソースの概要](connectors/adobe-applications/audience-manager.md)
   - [UI での Adobe Audience Manager ソース接続の作成](./tutorials/ui/create/adobe-applications/audience-manager.md)
- [Adobe Analytics Classifications データソースの概要](connectors/adobe-applications/classifications.md)
   - [UI での Adobe Analytics Classifications データソース接続の作成](./tutorials/ui/create/adobe-applications/classifications.md)
- [Adobe Analytics レポートスイートデータソースの概要](connectors/adobe-applications/analytics.md)
   - [UI での Adobe Analytics ソースコネクタの作成](./tutorials/ui/create/adobe-applications/analytics.md)
- [Adobe Campaign Managed Cloud Services ソースの概要](connectors/adobe-applications/campaign.md)
   - [UI での Adobe Campaign Managed Cloud Services ソース接続の作成](./tutorials/ui/create/adobe-applications/campaign.md)
- [Adobe Commerce ソースの概要](connectors/adobe-applications/commerce.md)
- [Adobe Data Collection ソースの概要](connectors/adobe-applications/data-collection.md)
   - [UI での Customer Attributes ソース接続の作成](./tutorials/ui/create/adobe-applications/customer-attributes.md)
- [[!DNL Marketo Engage] ソースの概要](connectors/adobe-applications/marketo/marketo.md)
   - [UI での  [!DNL Marketo Engage]  ソース接続の作成](./tutorials/ui/create/adobe-applications/marketo.md)
   - [カスタムア  [!DNL Marketo Engage]  ティビティデータ用のソース接続とデータフローの作成](./tutorials/ui/create/adobe-applications/marketo-custom-activities.md)

### 広告 {#advertising}

Adobe Experience Platform には、サードパーティの広告システムからデータを取り込む機能が用意されています。特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [Google広告 ](connectors/advertising/ads.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}

### Analytics {#analytics}

Experience Platform には、サードパーティの分析プラットフォームからデータを取り込む機能が用意されています。詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Mixpanel]](connectors/analytics/mixpanel.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Pendo]](connectors/analytics/pendo-webhook.md) [!BADGE &#x200B; ストリーミング &#x200B;]{type=Positive}
- [[!DNL RainFocus]](connectors/analytics/rainfocus.md) [!BADGE &#x200B; ストリーミング &#x200B;]{type=Positive}

### クラウドストレージ {#cloud-storage}

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードを行う必要なく、独自のデータをExperience Platformに取り込むことができます。 取り込んだデータは、XDM JSON、XDM Parquet 形式または区切り形式で書式設定できます。 プロセスのすべての手順は、ユーザーインターフェイスを使用した Sources ワークフローに統合されています。詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Azure Data Lake Storage Gen2]](connectors/cloud-storage/adls-gen2.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Azure Blob]](connectors/cloud-storage/blob.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Amazon S3]](connectors/cloud-storage/s3.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Apache HDFS]](connectors/cloud-storage/hdfs.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Azure File Storage]](connectors/cloud-storage/azure-file-storage.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Data Landing Zone]](connectors/cloud-storage/data-landing-zone.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL FTP]](connectors/cloud-storage/ftp.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Google Cloud Storage]](connectors/cloud-storage/google-cloud-storage.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Oracle Object Storage]](connectors/cloud-storage/oracle-object-storage.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL SFTP]](connectors/cloud-storage/sftp.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}

### 同意および環境設定 {#consent}

Experience Platform は、サードパーティの同意および環境設定管理プラットフォームからデータを取り込む機能を備えています。詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL OneTrust Integration]](connectors/consent-and-preferences/onetrust.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}

### 顧客関係管理（CRM） {#customer-relationship-management}

CRM システムは顧客との関係を築くのに役立つデータを提供し、顧客との忠誠度を高め、顧客保持率を高めます。Adobe Experience Platform には、[!DNL Microsoft Dynamics 365] および [!DNL Salesforce] から CRM データを取り込む機能が用意されています。詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Microsoft Dynamics]](connectors/crm/ms-dynamics.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Salesforce]](connectors/crm/salesforce.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL SugarCRM]](connectors/crm/sugarcrm.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Veeva CRM]](connectors/crm/veeva.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}

### カスタマーサクセス {#customer-success}

Experience Platform は、サードパーティの顧客成功アプリケーションからデータを取得する機能を備えています。詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Salesforce Service Cloud]](connectors/customer-success/salesforce-service-cloud.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL ServiceNow]](connectors/customer-success/servicenow.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Zendesk]](connectors/customer-success/zendesk.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}

### データベース {#database}

Experience Platform は、サードパーティのデータベースからデータを取得する機能を備えています。特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Apache Hive on Azure HDInsights]](connectors/databases/hive.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Apache Spark on Azure HDInsights]](connectors/databases/spark.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Azure Data Explorer]](connectors/databases/data-explorer.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Azure Table Storage]](connectors/databases/ats.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL GreenPlum]](connectors/databases/greenplum.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL HP Vertica]](connectors/databases/hp-vertica.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL IBM DB2]](connectors/databases/ibm-db2.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL MariaDB]](connectors/databases/mariadb.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Microsoft SQL Server]](connectors/databases/sql-server.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL MySQL]](connectors/databases/mysql.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Oracle]](connectors/databases/oracle.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL PostgreSQL]](connectors/databases/postgres.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Teradata Vantage]](connectors/databases/teradata-vantage.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}

### データおよび ID パートナー {#data-partner}

Experience Platformは、データおよび ID パートナーからのデータ取り込みをサポートしています。 特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Acxiom Data Ingestion]](connectors/data-partners/acxiom-data-ingestion.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Acxiom Prospecting Data Import]](connectors/data-partners/acxiom-prospecting-data-import.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Algolia User Profiles]](connectors/data-partners/algolia-user-profiles.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Bombora Intent]](connectors/data-partners/bombora.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Demandbase Intent]](connectors/data-partners/demandbase.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Merkury Enterprise Identity Resolution]](connectors/data-partners/merkury.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}

### e コマース {#ecommerce}

Adobe Experience Platform には、サードパーティの e コマースシステムからデータを取り込む機能が用意されています。特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL SAP Commerce]](connectors/ecommerce/sap-commerce.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Shopify]](connectors/ecommerce/shopify.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Shopify]](connectors/ecommerce/shopify-streaming.md) [!BADGE &#x200B; ストリーミング &#x200B;]{type=Positive}

### ローカルシステム {#local-system}

Adobe Experience Platform には、ローカルシステムからデータを取り込む機能が用意されています。 特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [ローカルファイルのアップロード](connectors/local-system/local-file-upload.md)

### マーケティングの自動処理 {#marketing-automation}

Experience Platform は、サードパーティのマーケティング自動化システムからデータを取得する機能を備えています。特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Braze]](connectors/marketing-automation/braze.md) [!BADGE &#x200B; ストリーミング &#x200B;]{type=Positive}
- [[!DNL Chatlio]](connectors/marketing-automation/chatlio-webhook.md) [!BADGE &#x200B; ストリーミング &#x200B;]{type=Positive}
- [[!DNL Customer.io]](connectors/marketing-automation/customerio-webhook.md) [!BADGE &#x200B; ストリーミング &#x200B;]{type=Positive}
- [[!DNL HubSpot]](connectors/marketing-automation/hubspot.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Mailchimp]](connectors/marketing-automation/mailchimp.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Oracle Eloqua]](connectors/marketing-automation/oracle-eloqua.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Oracle NetSuite]](connectors/marketing-automation/oracle-netsuite.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL PathFactory]](connectors/marketing-automation/pathfactory.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Salesforce Marketing Cloud]](connectors/marketing-automation/salesforce-marketing-cloud.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}

### 支払い {#payments}

Adobe Experience Platform には、サードパーティの支払いシステムからデータを取り込む機能が用意されています。特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Square]](connectors/payments/square.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Stripe]](connectors/payments/stripe.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}

### ストリーミング {#streaming}

Adobe Experience Platform には、ストリーミングソースからデータを取り込む機能が用意されています。特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL HTTP API]](connectors/streaming/http.md) [!BADGE &#x200B; ストリーミング &#x200B;]{type=Positive}

### プロトコル {#protocols}

Adobe Experience Platform には、サードパーティのプロトコルシステムからデータを取り込む機能が用意されています。特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Generic OData]](connectors/protocols/odata.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}
- [[!DNL Generic REST API]](connectors/protocols/generic-rest.md) [!BADGE &#x200B; バッチ &#x200B;]{type=Informative}

## データ取得におけるソースのアクセス制御

データ取得元に対する権限は、Adobe Admin Console で管理できます。権限には、特定の製品プロファイルの「**[!UICONTROL 権限]**」タブからアクセスできます。**[!UICONTROL 権限を編集]**&#x200B;パネルから、**[!UICONTROL データ取得]**&#x200B;メニューのエントリを使用して、ソースに関連する権限にアクセスできます。「**[!UICONTROL ソースの表示]**」権限は、「**[!UICONTROL カタログ]**」タブの使用可能なソースと「**[!UICONTROL 参照]**」タブの認証済みのソースへの読み取り専用アクセス権を付与する一方、「**[!UICONTROL ソースの管理]**」権限は、ソースの作成、編集および無効化に対するフルアクセス権を付与します。

次の表に、これらの権限の様々な組み合わせに基づく UI の動作の概要を示します。

| 権限レベル | 説明 |
| ---- | ----|
| **[!UICONTROL ソースの表示]**&#x200B;オン | 「Catalog」タブ、「Browse」タブ、「Accounts」タブ、「Dataflow」タブの各ソースタイプのソースに読み取り専用アクセス権を付与します。 |
| **[!UICONTROL ソースの管理]**&#x200B;オン | 「**[!UICONTROL ソースの表示]**」に含まれる機能に加えて、「**[!UICONTROL カタログ]**」の「**[!UICONTROL ソースの接続]**」オプションおよび「**[!UICONTROL 参照]**」の「**[!UICONTROL データを選択]**」オプションへのアクセス権を付与します。また、「**[!UICONTROL ソースの管理]**」では、**[!UICONTROL データフロー]**&#x200B;の有効/無効を切り替えたり、スケジュールを編集したりできます。 |
| 「**[!UICONTROL ソースの表示]**」オフおよび「**[!UICONTROL ソースの管理]**」オフ | ソースへのすべてのアクセスを取り消します。 |

Adobe Permissions を通じて付与される使用可能な権限の詳細については、[アクセス制御の概要](../access-control/home.md)を参照してください。

### 属性ベースのアクセス制御

Adobe Experience Platform での属性ベースのアクセス制御では、管理者が属性に基づいて特定のオブジェクトや機能へのアクセスを制御できます。

属性ベースのアクセス制御を使用すると、権限を持つフィールドにマッピング設定を適用できます。さらに、データセット内のすべてのフィールドにアクセスできない場合は、データセットにデータを取り込むことはできません。

#### ソースの属性ベースのアクセス制御のサポート

>[!TIP]
>
>属性ベースのアクセス制御は次のように動作します。**roles** を作成して、Experience Platform インスタンスとやり取りするユーザーのタイプを分類します。 **ラベル** は、**役割** に適用され、その役割のアクセスを指定します。 **ラベル** は、スキーマフィールドやセグメントなどのリソースにも適用されます。 ユーザーが特定のスキーマフィールドおよびセグメントにアクセスできるようにするには、それらを *クエリされたリソースに割り当てられたのと同じラベルの役割* に追加する必要があります。 詳しくは、[ 属性ベースのアクセス制御エンドツーエンドガイド ](../access-control/abac/end-to-end-guide.md) を参照してください。

- スキーマフィールドにラベルを適用して、組織内の特定のスキーマフィールドへのアクセスを定義します。 特定のスキーマフィールドへのアクセスが確立されると、ユーザーは、アクセス権のあるフィールドのマッピングのみを作成できるようになります。
- 適切な役割を持たないユーザーは、アクセスできないスキーマフィールドを含むマッピングを使用したデータフローを作成または更新できません。 さらに、権限のないユーザーは、アクセスできないスキーマフィールドを含む既存のデータフローを更新、削除、有効化または無効化できません。
- さらに、データフローは、マッピング、ターゲットデータセット、ターゲット接続でまったく同じスキーマ ID とバージョンを持つ必要があります。

属性ベースのアクセス制御の詳細については、[属性ベースのアクセス制御の概要](../access-control/abac/overview.md)を参照してください。

## 利用条件 {#terms-and-conditions}

「ベータ版」としてラベル付けされたソースを使用することにより、お客様は、ベータ版が&#x200B;***「現状のまま」でいかなる保証もなく***&#x200B;提供されていることを承諾します。

アドビは、ベータ版を維持、訂正、更新、変更、修正、またはその他の方法でサポートする義務を負いません。 お客様は、情報を使用し、そのようなBetaおよび/または付属の資料の正しい機能やパフォーマンスに何ら依存しないことをお勧めします。 ベータ版はアドビの機密情報と見なされます。

お客様がアドビに提供するあらゆる「フィードバック」（ベータ版の使用中に発生した問題や欠陥、提案、改善、レコメンデーションを含むがこれに限定されないベータ版に関する情報）は、このようなフィードバックに含まれる、およびフィードバックに対するすべての権利、所有権、利益を含め、アドビに帰属します。

公開フィードバックを送信するかサポートチケットを作成して、お客様の提案を共有したりバグを報告したりして、機能強化にご協力ください。
