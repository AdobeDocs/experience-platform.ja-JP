---
keywords: Experience Platform;ホーム;人気のトピック;ソースコネクタ;ソースコネクタ;ソース;データソース;データソース;データソース接続
solution: Experience Platform
title: ソースコネクタの概要
description: Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。
exl-id: efdbed4d-5697-43ef-a47a-a8bcf0f13237
source-git-commit: bd5611b23740f16e41048f3bc65f62312593a075
workflow-type: tm+mt
source-wordcount: '1696'
ht-degree: 54%

---

# ソースコネクタの概要

Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[!DNL Flow Service] を使用すると、様々な異なるソースから顧客データを収集し、Experience Platformで一元化できます。 このサービスにはユーザーインターフェイスおよび RESTful API が用意されており、様々なデータプロバイダーへのソース接続を簡単に設定できます。これらのソース接続を使用すると、サードパーティ製システムの認証、データ取り込み時間の設定、データ取り込みスループットの管理を行うことができます。

Experience Platform を使用すると、異なるソースから収集したデータを一元管理し、得たインサイトを利用して、より多くの作業を行うことができます。

<div id="recs-overview-body-1"></div>
<div id="recs-overview-body-2"></div>
<div id="recs-overview-body-3"></div>
<div id="recs-overview-body-4"></div>
<div id="recs-overview-body-5"></div>
<div id="recs-overview-body-6"></div>

>[!BEGINSHADEBOX]

## Adobeで構築されたソースとパートナーが構築したソース {#adobe-and-partner-built-sources}

Experience Platform ソースカタログ内のコネクタには、Adobeで構築および管理されるものと、[Sources SDK](/help/sources/sources-sdk/overview.md) を使用してパートナー企業が構築および管理するものがあります。 各パートナー構築コネクタのドキュメントページの上部にあるメモは、ソースがパートナーによって作成および管理される場合、を呼び出します。 例えば、[Amazon S3 コネクタ &#x200B;](/help/sources/connectors/cloud-storage/s3.md) はAdobeによって作成され、&lbrace;RainFocus コネクタ [&#x200B; は RainFocus チームによって作成および管理されます &#x200B;](/help/sources/connectors/analytics/rainfocus.md)。

パートナーが作成および管理するコネクタの場合、コネクタに関する問題をパートナーチームが解決する必要が生じる場合があります（ドキュメントページのメモに記載されている連絡先方法）。アドビが作成および管理するコネクタに関する問題については、アドビ担当者またはカスタマーケア担当者にお問い合わせください。

>[!ENDSHADEBOX]

## ソースカタログ

ソースカタログで使用可能なすべてのソースのリストについては、次の節を参照してください。

### アドビアプリケーション {#adobe-applications}

Adobe Experience Platform を使用すると、Adobe Analytics や Adobe Audience Manager など、他のアドビアプリケーションからデータを取り込むことができます。詳しくは、次の関連ドキュメントを参照してください。

- [Adobe Audience Manager](connectors/adobe-applications/audience-manager.md)
   - [UI での Adobe Audience Manager ソース接続の作成](./tutorials/ui/create/adobe-applications/audience-manager.md)
- [Adobe Analytics Classifications データ](connectors/adobe-applications/classifications.md)
   - [UI での Adobe Analytics Classifications データソース接続の作成](./tutorials/ui/create/adobe-applications/classifications.md)
- [Adobe Analytics レポートスイートデータ](connectors/adobe-applications/analytics.md)
   - [UI での Adobe Analytics ソースコネクタの作成](./tutorials/ui/create/adobe-applications/analytics.md)
- [Adobe Campaign Managed Cloud Services](connectors/adobe-applications/campaign.md)
   - [UI での Adobe Campaign Managed Cloud Services ソース接続の作成](./tutorials/ui/create/adobe-applications/campaign.md)
- [Adobe Commerce](connectors/adobe-applications/commerce.md)
- [Adobe Data Collection](connectors/adobe-applications/data-collection.md)
   - [UI での Customer Attributes ソース接続の作成](./tutorials/ui/create/adobe-applications/customer-attributes.md)
- [[!DNL Marketo Engage]](connectors/adobe-applications/marketo/marketo.md)
   - [UI での  [!DNL Marketo Engage]  ソース接続の作成](./tutorials/ui/create/adobe-applications/marketo.md)
   - [カスタムア  [!DNL Marketo Engage]  ティビティデータ用のソース接続とデータフローの作成](./tutorials/ui/create/adobe-applications/marketo-custom-activities.md)

### 高度なエンタープライズソース {#advanced-enterprise-sources}

次のソースは、[Adobe Real-Time Customer Data Platform Ultimate](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2c-edition-prime-and-ultimate-packages.html) のお客様のみが利用できます。

| ソース | カテゴリ | 取り込みタイプ | Cloud |
| --- | --- | --- | --- |
| [[!DNL Amazon Kinesis]](connectors/cloud-storage/kinesis.md) | クラウドストレージ | ストリーミング | AWS, Azure |
| [[!DNL Amazon Redshift]](connectors/databases/redshift.md) | データベース | バッチ | AWS, Azure |
| [[!DNL Azure Databricks]](connectors/databases/databricks.md) | データベース | バッチ | Azure |
| [[!DNL Azure Event Hubs]](connectors/cloud-storage/eventhub.md) | クラウドストレージ | ストリーミング | AWS, Azure |
| [[!DNL Azure Synapse Analytics]](connectors/databases/synapse-analytics.md) | データベース | バッチ | Azure |
| [[!DNL Google BigQuery]](connectors/databases/bigquery.md) | データベース | バッチ | AWS, Azure |
| [[!DNL Google PubSub]](connectors/cloud-storage/google-pubsub.md) | クラウドストレージ | ストリーミング | Azure |
| [[!DNL Snowflake]](connectors/databases/snowflake-streaming.md) | データベース | ストリーミング | AWS, Azure |
| [[!DNL Snowflake]](connectors/databases/snowflake.md) | データベース | バッチ | AWS, Azure |

{style="table-layout:auto"}

### 広告 {#advertising}

次のソースを使用して、広告データをExperience Platformに取り込むことができます。

| ソース | 取り込みタイプ | Cloud |
| --- | --- | --- |
| [Google 広告](connectors/advertising/ads.md) | バッチ | Azure |

{style="table-layout:auto"}

### Analytics {#analytics}

次のソースを使用して、分析データをExperience Platformに取り込むことができます。

| ソース | 取り込みタイプ | Cloud |
| --- | --- | --- |
| [[!DNL Mixpanel]](connectors/analytics/mixpanel.md) | バッチ | Azure |
| [[!DNL Pendo]](connectors/analytics/pendo-webhook.md) | ストリーミング | Azure |
| [[!DNL RainFocus]](connectors/analytics/rainfocus.md) | ストリーミング | Azure |

{style="table-layout:auto"}

### クラウドストレージ {#cloud-storage}

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードを行う必要なく、独自のデータをExperience Platformに取り込むことができます。 取り込んだデータは、XDM JSON、XDM Parquet 形式または区切り形式で書式設定できます。 プロセスのすべての手順は、ユーザーインターフェイスを使用した Sources ワークフローに統合されています。詳しくは、次の関連ドキュメントを参照してください。

次のソースを使用して、クラウドストレージデータをExperience Platformに取り込むことができます。

| ソース | 取り込みタイプ | Cloud |
| --- | --- | --- |
| [[!DNL Azure Data Lake Storage Gen2]](connectors/cloud-storage/adls-gen2.md) | バッチ | Azure |
| [[!DNL Azure Blob Storage]](connectors/cloud-storage/blob.md) | バッチ | Azure |
| [[!DNL Amazon S3]](connectors/cloud-storage/s3.md) | バッチ | AWS, Azure |
| [[!DNL Apache HDFS]](connectors/cloud-storage/hdfs.md) | バッチ | Azure |
| [[!DNL Azure File Storage]](connectors/cloud-storage/azure-file-storage.md) | バッチ | Azure |
| [[!DNL Data Landing Zone]](connectors/cloud-storage/data-landing-zone.md) | バッチ | AWS, Azure |
| [[!DNL FTP]](connectors/cloud-storage/ftp.md) | バッチ | Azure |
| [[!DNL Google Cloud Storage]](connectors/cloud-storage/google-cloud-storage.md) | バッチ | Azure |
| [[!DNL Oracle Object Storage]](connectors/cloud-storage/oracle-object-storage.md) | バッチ | Azure |
| [[!DNL SFTP]](connectors/cloud-storage/sftp.md) | バッチ | Azure |

{style="table-layout:auto"}

### 同意および環境設定 {#consent}

次のソースを使用して、同意および環境設定データをExperience Platformに取り込むことができます。

| ソース | 取り込みタイプ | Cloud |
| --- | --- | --- |
| [[!DNL Didomi]](../sources/connectors/consent-and-preferences/didomi.md) | ストリーミング | Azure |
| [[!DNL OneTrust Integration]](connectors/consent-and-preferences/onetrust.md) | バッチ | Azure |

{style="table-layout:auto"}

### 顧客関係管理（CRM） {#customer-relationship-management}

CRM システムは顧客との関係を築くのに役立つデータを提供し、顧客とのロイヤルティを高め、顧客保持率を高めます。Adobe Experience Platform には、[!DNL Microsoft Dynamics 365] および [!DNL Salesforce] から CRM データを取り込む機能が用意されています。詳しくは、次の関連ドキュメントを参照してください。

次のソースを使用して、CRM データをExperience Platformに取り込むことができます。

| ソース | 取り込みタイプ | Cloud |
| --- | --- | --- |
| [[!DNL Microsoft Dynamics]](connectors/crm/ms-dynamics.md) | バッチ | Azure |
| [[!DNL Salesforce]](connectors/crm/salesforce.md) | バッチ | AWS, Azure |
| [[!DNL SugarCRM]](connectors/crm/sugarcrm.md) | バッチ | Azure |
| [[!DNL Veeva CRM]](connectors/crm/veeva.md) | バッチ | Azure |

{style="table-layout:auto"}

### カスタマーサクセス {#customer-success}

次のソースを使用して、カスタマーサクセスデータをExperience Platformに取り込むことができます。

| ソース | 取り込みタイプ | Cloud |
| --- | --- | --- |
| [[!DNL Salesforce Service Cloud]](connectors/customer-success/salesforce-service-cloud.md) | バッチ | Azure |
| [[!DNL ServiceNow]](connectors/customer-success/servicenow.md) | バッチ | Azure |
| [[!DNL Zendesk]](connectors/customer-success/zendesk.md) | バッチ | Azure |

{style="table-layout:auto"}

### データベース {#database}

Experience Platform は、サードパーティのデータベースからデータを取得する機能を備えています。特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

次のソースを使用して、データベースからExperience Platformにデータを取り込むことができます。

| ソース | 取り込みタイプ | Cloud |
| --- | --- | --- |
| [[!DNL Apache Hive on Azure HDInsights]](connectors/databases/hive.md) | バッチ | Azure |
| [[!DNL Apache Spark on Azure HDInsights]](connectors/databases/spark.md) | バッチ | Azure |
| [[!DNL Azure Data Explorer]](connectors/databases/data-explorer.md) | バッチ | Azure |
| [[!DNL Azure Table Storage]](connectors/databases/ats.md) | バッチ | Azure |
| [[!DNL GreenPlum]](connectors/databases/greenplum.md) | バッチ | Azure |
| [[!DNL HP Vertica]](connectors/databases/hp-vertica.md) | バッチ | Azure |
| [[!DNL IBM DB2]](connectors/databases/ibm-db2.md) | バッチ | Azure |
| [[!DNL MariaDB]](connectors/databases/mariadb.md) | バッチ | Azure |
| [[!DNL Microsoft SQL Server]](connectors/databases/sql-server.md) | バッチ | Azure |
| [[!DNL MySQL]](connectors/databases/mysql.md) | バッチ | AWS, Azure |
| [[!DNL Oracle]](connectors/databases/oracle.md) | バッチ | AWS, Azure |
| [[!DNL PostgreSQL]](connectors/databases/postgres.md) | バッチ | AWS, Azure |
| [[!DNL Teradata Vantage]](connectors/databases/teradata-vantage.md) | バッチ | Azure |

{style="table-layout:auto"}

### データおよび ID パートナー {#data-partner}

次のソースを使用して、データおよび ID パートナーデータをExperience Platformに取り込むことができます。

| ソース | 取り込みタイプ | Cloud |
| --- | --- | --- |
| [[!DNL Acxiom Data Ingestion]](connectors/data-partners/acxiom-data-ingestion.md) | バッチ | Azure |
| [[!DNL Acxiom Prospecting Data Import]](connectors/data-partners/acxiom-prospecting-data-import.md) | バッチ | Azure |
| [[!DNL Algolia User Profiles]](connectors/data-partners/algolia-user-profiles.md) | バッチ | Azure |
| [[!DNL Bombora Intent]](connectors/data-partners/bombora.md) | バッチ | Azure |
| [[!DNL Demandbase Intent]](connectors/data-partners/demandbase.md) | バッチ | Azure |
| [[!DNL Merkury Enterprise Identity Resolution]](connectors/data-partners/merkury.md) | バッチ | Azure |

{style="table-layout:auto"}

### e コマース {#ecommerce}

次のソースを使用して、e コマースデータをExperience Platformに取り込むことができます。

| ソース | 取り込みタイプ | Cloud |
| --- | --- | --- |
| [[!DNL SAP Commerce]](connectors/ecommerce/sap-commerce.md) | バッチ | Azure |
| [[!DNL Shopify]](connectors/ecommerce/shopify.md) | バッチ | Azure |
| [[!DNL Shopify]](connectors/ecommerce/shopify-streaming.md) | ストリーミング | Azure |

{style="table-layout:auto"}

### ローカルシステム {#local-system}

次のソースを使用して、ローカルシステムからExperience Platformにデータを取り込むことができます。

| ソース | 取り込みタイプ | Cloud |
| --- | --- | --- |
| [ローカルファイルのアップロード](connectors/local-system/local-file-upload.md) | バッチ | Azure |

{style="table-layout:auto"}

### ロイヤルティ {#loyalty}

次のソースを使用して、Experience Platformへのデータロイヤルティを取り込むことができます。

| ソース | 取り込みタイプ | Cloud |
| --- | --- | --- |
| [[!DNL Capillary Streaming Events]](connectors/loyalty/capillary.md) | ストリーミング | Azure |

{style="table-layout:auto"}

### マーケティングオートメーション {#marketing-automation}

次のソースを使用して、マーケティング自動化データをExperience Platformに取り込むことができます。

| ソース | 取り込みタイプ | Cloud |
| --- | --- | --- |
| [[!DNL Braze]](connectors/marketing-automation/braze.md) | ストリーミング | Azure |
| [[!DNL Chatlio]](connectors/marketing-automation/chatlio-webhook.md) | ストリーミング | Azure |
| [[!DNL Customer.io]](connectors/marketing-automation/customerio-webhook.md) | ストリーミング | Azure |
| [[!DNL HubSpot]](connectors/marketing-automation/hubspot.md) | バッチ | Azure |
| [[!DNL Mailchimp]](connectors/marketing-automation/mailchimp.md) | バッチ | Azure |
| [[!DNL Oracle Eloqua]](connectors/marketing-automation/oracle-eloqua.md) | バッチ | Azure |
| [[!DNL Oracle NetSuite]](connectors/marketing-automation/oracle-netsuite.md) | バッチ | Azure |
| [[!DNL PathFactory]](connectors/marketing-automation/pathfactory.md) | バッチ | Azure |
| [[!DNL Relay Connector]](tutorials/ui/create/marketing-automation/relay-connector.md) | ストリーミング | Azure |
| [[!DNL Salesforce Marketing Cloud]](connectors/marketing-automation/salesforce-marketing-cloud.md) | バッチ | AWS, Azure |

{style="table-layout:auto"}

### 支払い {#payments}

次のソースを使用して、支払いデータをExperience Platformに取り込むことができます。

| ソース | 取り込みタイプ | Cloud |
| --- | --- | --- |
| [[!DNL Square]](connectors/payments/square.md) | バッチ | Azure |
| [[!DNL Stripe]](connectors/payments/stripe.md) | バッチ | Azure |

{style="table-layout:auto"}

### ストリーミング {#streaming}

次のソースを使用して、Experience Platformにデータをストリーミングできます。

| ソース | 取り込みタイプ | クラウドサポート |
| --- | --- | --- |
| [[!DNL HTTP API]](connectors/streaming/http.md) | ストリーミング | AWS, Azure |

{style="table-layout:auto"}

### プロトコル {#protocols}

次のソースを使用して、プロトコルデータをExperience Platformに取り込むことができます。

| ソース | 取り込みタイプ | クラウドサポート |
| --- | --- | --- |
| [[!DNL Generic OData]](connectors/protocols/odata.md) | バッチ | Azure |
| [[!DNL Generic REST API]](connectors/protocols/generic-rest.md) | バッチ | Azure |

{style="table-layout:auto"}

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
>属性ベースのアクセス制御は次のように動作します。**roles** を作成して、Experience Platform インスタンスとやり取りするユーザーのタイプを分類します。 **ラベル** は、**役割** に適用され、その役割のアクセスを指定します。 **ラベル** は、スキーマフィールドやセグメントなどのリソースにも適用されます。 ユーザーが特定のスキーマフィールドおよびセグメントにアクセスできるようにするには、それらを *クエリされたリソースに割り当てられたのと同じラベルの役割* に追加する必要があります。 詳しくは、[&#x200B; 属性ベースのアクセス制御エンドツーエンドガイド &#x200B;](../access-control/abac/end-to-end-guide.md) を参照してください。

- スキーマフィールドにラベルを適用して、組織内の特定のスキーマフィールドへのアクセスを定義します。 特定のスキーマフィールドへのアクセスが確立されると、ユーザーは、アクセス権のあるフィールドのマッピングのみを作成できるようになります。
- 適切な役割を持たないユーザーは、アクセスできないスキーマフィールドを含むマッピングを使用したデータフローを作成または更新できません。 さらに、権限のないユーザーは、アクセスできないスキーマフィールドを含む既存のデータフローを更新、削除、有効化または無効化できません。
- さらに、データフローは、マッピング、ターゲットデータセット、ターゲット接続でまったく同じスキーマ ID とバージョンを持つ必要があります。 これは、標準 XDM スキーマとモデルベースのスキーマの両方に適用されます。

>[!NOTE]
>
>モデルベースのスキーマには、プライマリキーやバージョン識別子のフィールドなど、追加の要件があります。 詳しくは、[&#x200B; モデルベースのスキーマの概要 &#x200B;](../xdm/schema/model-based.md) を参照してください。

属性ベースのアクセス制御の詳細については、[属性ベースのアクセス制御の概要](../access-control/abac/overview.md)を参照してください。

## 利用条件 {#terms-and-conditions}

「ベータ版」としてラベル付けされたソースを使用することにより、お客様は、ベータ版が&#x200B;***「現状のまま」でいかなる保証もなく***&#x200B;提供されていることを承諾します。

アドビは、ベータ版を維持、訂正、更新、変更、修正、またはその他の方法でサポートする義務を負いません。 お客様は、情報を使用し、そのようなBetaおよび/または付属の資料の正しい機能やパフォーマンスに何ら依存しないことをお勧めします。 ベータ版はアドビの機密情報と見なされます。

お客様がアドビに提供するあらゆる「フィードバック」（ベータ版の使用中に発生した問題や欠陥、提案、改善、レコメンデーションを含むがこれに限定されないベータ版に関する情報）は、このようなフィードバックに含まれる、およびフィードバックに対するすべての権利、所有権、利益を含め、アドビに帰属します。

公開フィードバックを送信するかサポートチケットを作成して、お客様の提案を共有したりバグを報告したりして、機能強化にご協力ください。
