---
keywords: Experience Platform;ホーム;人気のトピック;ソースコネクタ;ソースコネクタ;ソース;データソース;データソース;データソース接続
solution: Experience Platform
title: ソースコネクタの概要
description: Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。
exl-id: efdbed4d-5697-43ef-a47a-a8bcf0f13237
source-git-commit: 7c83a946ca30f1bf016cc42245d7d52f2c034f18
workflow-type: tm+mt
source-wordcount: '1437'
ht-degree: 77%

---

# ソースコネクタの概要

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[!DNL Flow Service] を使用すると、様々な異なるソースから顧客データを収集し、Platform で一元化できます。このサービスにはユーザーインターフェイスおよび RESTful API が用意されており、様々なデータプロバイダーへのソース接続を簡単に設定できます。これらのソース接続を使用すると、サードパーティ製システムの認証、データ取り込み時間の設定、データ取り込みスループットの管理を行うことができます。

Experience Platform を使用すると、異なるソースから収集したデータを一元管理し、得たインサイトを利用して、より多くの作業を行うことができます。

<div id="recs-overview-body-1"></div>
<div id="recs-overview-body-2"></div>
<div id="recs-overview-body-3"></div>
<div id="recs-overview-body-4"></div>
<div id="recs-overview-body-5"></div>
<div id="recs-overview-body-6"></div>

## Adobeで作成されたソースとパートナーで作成されたソース {#adobe-and-partner-built-sources}

Experience Platformソースカタログ内のコネクタの一部はAdobeによって作成および管理され、その他はパートナー企業によって作成および管理されます。 [ソース SDK](/help/sources/sources-sdk/overview.md). ソースがパートナーによって作成および管理されている場合は、各パートナーが作成したコネクタのドキュメントページ上部にあるメモが呼び出されます。 例えば、 [Amazon S3 コネクタ](/help/sources/connectors/cloud-storage/s3.md) はAdobeによって作成され、 [RainFocus コネクタ](/help/sources/connectors/analytics/rainfocus.md) は、RainFocus チームによって作成および管理されます。

パートナーが作成および管理するコネクタの場合、コネクタに関する問題をパートナーチームが解決する必要が生じる場合があります（ドキュメントページのメモに記載されている連絡先方法）。 Adobeが作成および維持するコネクタに関する問題については、Adobe担当者またはカスタマーケアにお問い合わせください。

## ソースのタイプ

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
- [Adobe Commerceソースの概要](connectors/adobe-applications/commerce.md)
- [Adobe Data Collection ソースの概要](connectors/adobe-applications/data-collection.md)
   - [UI での Customer Attributes ソース接続の作成](./tutorials/ui/create/adobe-applications/customer-attributes.md)
- [[!DNL Marketo Engage] ソースの概要](connectors/adobe-applications/marketo/marketo.md)
   - [UI での  [!DNL Marketo Engage]  ソース接続の作成](./tutorials/ui/create/adobe-applications/marketo.md)
   - [の作成 [!DNL Marketo Engage] カスタムアクティビティデータのソース接続とデータフロー](./tutorials/ui/create/adobe-applications/marketo-custom-activities.md)

### 広告 {#advertising}

Adobe Experience Platform には、サードパーティの広告システムからデータを取り込む機能が用意されています。特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [Google 広告](connectors/advertising/ads.md)

### Analytics {#analytics}

Experience Platform には、サードパーティの分析プラットフォームからデータを取り込む機能が用意されています。詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Mixpanel]](connectors/analytics/mixpanel.md)
- [[!DNL Pendo]](connectors/analytics/pendo-webhook.md)
- [[!DNL RainFocus]](connectors/analytics/rainfocus.md)

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
- [[!DNL SugarCRM]](connectors/crm/sugarcrm.md)
- [[!DNL Veeva CRM]](connectors/crm/veeva.md)
- [[!DNL Zoho CRM]](connectors/crm/zoho.md)

### カスタマーサクセス {#customer-success}

Experience Platform は、サードパーティの顧客成功アプリケーションからデータを取得する機能を備えています。詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Oracle Service Cloud]](connectors/customer-success/oracle-service-cloud.md)
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
- [[!DNL Snowflake (Streaming)]](connectors/databases//snowflake-streaming.md)
- [[!DNL Snowflake]](connectors/databases/snowflake.md)
- [[!DNL Teradata Vantage]](connectors/databases/teradata-vantage.md)

### e コマース {#ecommerce}

Adobe Experience Platform には、サードパーティの e コマースシステムからデータを取り込む機能が用意されています。特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL SAP Commerce]](connectors/ecommerce/sap-commerce.md)
- [[!DNL Shopify]](connectors/ecommerce/shopify.md)
- [[!DNL Shopify (Streaming)]](connectors/ecommerce/shopify-streaming.md)

### ローカルシステム {#local-system}

Adobe Experience Platform には、ローカルシステムからデータを取り込む機能が用意されています。 特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [ローカルファイルのアップロード](connectors/local-system/local-file-upload.md)

### マーケティングの自動処理 {#marketing-automation}

Experience Platform は、サードパーティのマーケティング自動化システムからデータを取得する機能を備えています。特定のソースコネクタについて詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Chatlio]](connectors/marketing-automation/chatlio-webhook.md)
- [[!DNL Customer.io]](connectors/marketing-automation/customerio-webhook.md)
- [[!DNL HubSpot]](connectors/marketing-automation/hubspot.md)
- [[!DNL Mailchimp]](connectors/marketing-automation/mailchimp.md)
- [[!DNL Oracle Eloqua]](connectors/marketing-automation/oracle-eloqua.md)
- [[!DNL Salesforce Marketing Cloud]](connectors/marketing-automation/salesforce-marketing-cloud.md)
<!-- 
- [[!DNL Oracle Responsys]](connectors/marketing-automation/oracle-responsys.md)
-->

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

Adobe Permissions を通じて付与される使用可能な権限の詳細については、[アクセス制御の概要](../access-control/home.md)を参照してください。

### 属性ベースのアクセス制御

Adobe Experience Platform での属性ベースのアクセス制御では、管理者が属性に基づいて特定のオブジェクトや機能へのアクセスを制御できます。

属性ベースのアクセス制御を使用すると、権限を持つフィールドにマッピング設定を適用できます。さらに、データセット内のすべてのフィールドにアクセスできない場合は、データセットにデータを取り込むことはできません。

#### ソースでの属性ベースのアクセス制御のサポート

>[!TIP]
>
>属性ベースのアクセス制御は、次のように機能します。 **役割** は、Platform インスタンスとやり取りするユーザーのタイプを分類するために作成されます。 **ラベル** が適用される **役割** を使用して、特定のロールへのアクセスを指定します。 **ラベル** また、は、スキーマフィールドやセグメントなどのリソースにも適用されます。 ユーザーが特定のスキーマフィールドやセグメントにアクセスできるようにするには、次のように追加する必要があります。 *問い合わせ先のリソースに割り当てられた同じラベルを持つロール*. 詳しくは、 [属性ベースのアクセス制御エンドツーエンドガイド](../access-control/abac/end-to-end-guide.md).

- スキーマフィールドにラベルを適用して、組織内の特定のスキーマフィールドへのアクセスを定義します。 特定のスキーマフィールドへのアクセスが確立されると、ユーザーは、アクセス権を持つフィールドのマッピングのみ作成できます。
- 適切な役割を持たないユーザーは、アクセスできないスキーマフィールドを含むマッピングを使用してデータフローを作成または更新できません。 また、権限のないユーザーは、アクセスできないスキーマフィールドを持つ既存のデータフローを更新、削除、有効化、無効化することはできません。
- さらに、データフローのマッピング、ターゲットデータセット、ターゲット接続には、完全に同じスキーマ ID とバージョンが必要です。

属性ベースのアクセス制御の詳細については、[属性ベースのアクセス制御の概要](../access-control/abac/overview.md)を参照してください。

## 利用条件 {#terms-and-conditions}

「ベータ版」としてラベル付けされたソースを使用することにより、お客様は、ベータ版が&#x200B;***「現状のまま」でいかなる保証もなく***&#x200B;提供されていることを承諾します。

アドビは、ベータ版を維持、訂正、更新、変更、修正、またはその他の方法でサポートする義務を負いません。このようなベータ版および付属の資料またはそのいずれかが、正しく機能しパフォーマンスを提供することに関して、お客様は注意を払い、いかなる形でも依存しないことをお勧めします。ベータ版はアドビの機密情報と見なされます。

お客様がアドビに提供するあらゆる「フィードバック」（ベータ版の使用中に発生した問題や欠陥、提案、改善、推奨事項を含むがこれに限定されないベータ版に関する情報）は、このようなフィードバックに含まれる、およびフィードバックに対するすべての権利、所有権、利益を含め、アドビに帰属します。

公開フィードバックを送信するかサポートチケットを作成して、お客様の提案を共有したりバグを報告したりして、機能強化にご協力ください。
