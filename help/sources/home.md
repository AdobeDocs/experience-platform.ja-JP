---
keywords: Experience Platform；ホーム；人気のあるトピック；ソースコネクタ；ソースコネクタ；ソース；データソース；データソース；データソース；データソース；データソース接続
solution: Experience Platform
title: ソースコネクタの概要
topic-legacy: overview
description: Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。
exl-id: efdbed4d-5697-43ef-a47a-a8bcf0f13237
translation-type: tm+mt
source-git-commit: 8edcb3697337edd0043f26553b92c31e52d3c87c
workflow-type: tm+mt
source-wordcount: '975'
ht-degree: 55%

---

# ソースコネクタの概要

Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。Adobeアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[!DNL Flow Service] は、プラットフォーム内の様々な異種ソースから顧客データを収集および一元化するために使用します。このサービスは、様々なデータプロバイダーへのソース接続を簡単に設定できるユーザーインターフェイスおよびRESTful APIを提供します。 これらのソース接続を使用すると、サードパーティ製システムの認証、取得の実行時間の設定、データ取得スループットの管理をおこなうことができます。

Experience Platform を使用すると、異なるソースから収集したデータを一元管理し、得たインサイトを利用して、より多くの作業をおこなうことができます。

## ソースのタイプ

Experience Platform のソースは、次のカテゴリに分類されます。

### アドビアプリケーション

Experience Platformは、Adobe Analytics、Adobe Audience Manager、[!DNL Experience Platform Launch]など、他のAdobeアプリケーションからデータを取り込むことを可能にします。 詳しくは、次の関連ドキュメントを参照してください。

- [Adobe Audience Manager コネクタの概要](connectors/adobe-applications/audience-manager.md)
- [UIでのAdobe Audience Managerソース接続の作成](./tutorials/ui/create/adobe-applications/audience-manager.md)
- [Adobe Analytics分類データコネクタの概要](connectors/adobe-applications/classifications.md)
- [UIでのAdobe Analytics分類データソース接続の作成](./tutorials/ui/create/adobe-applications/classifications.md)
- [Adobe Analytics Data コネクタの概要](connectors/adobe-applications/analytics.md)
- [UIでのAdobe Analyticsソース接続の作成](./tutorials/ui/create/adobe-applications/analytics.md)
- [UIでの顧客属性ソース接続の作成](./tutorials/ui/create/adobe-applications/customer-attributes.md)
- [[!DNL Marketo Engage] コネクタの概要](connectors/adobe-applications/marketo/marketo.md)
- [UIで [!DNL Marketo Engage] ソース接続を作成する](./tutorials/ui/create/adobe-applications/marketo.md)

### 広告

Experience Platformは、サードパーティの広告システムからデータを取り込むためのサポートを提供します。 特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

- [[!DNL Google AdWords]](connectors/advertising/ads.md) コネクタ

### クラウドストレージ

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードをおこなう必要なく、独自のデータを Platform に取り込むことができます。取り込んだデータは、XDM JSON、XDM Parket、または区切り形式で形式設定できます。 プロセスのすべての手順は、ユーザーインターフェイスを使用して Sources ワークフローに統合されます。詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Azure Data Lake Storage Gen2] コネクタ](connectors/cloud-storage/adls-gen2.md)
- [[!DNL Azure Blob] コネクタ](connectors/cloud-storage/blob.md)
- [[!DNL Amazon Kinesis] コネクタ](connectors/cloud-storage/kinesis.md)
- [[!DNL Amazon S3] コネクタ](connectors/cloud-storage/s3.md)
- [[!DNL Apache HDFS] コネクタ](connectors/cloud-storage/hdfs.md)
- [[!DNL Azure Event Hubs] コネクタ](connectors/cloud-storage/eventhub.md)
- [[!DNL Azure File Storage] コネクタ](connectors/cloud-storage/azure-file-storage.md)
- [[!DNL FTP] コネクタ](connectors/cloud-storage/ftp.md)
- [[!DNL Google Cloud Storage] コネクタ](connectors/cloud-storage/google-cloud-storage.md)
- [[!DNL Google PubSub] コネクタ](connectors/cloud-storage/google-pubsub.md)
- [[!DNL Oracle Object Storage] コネクタ](connectors/cloud-storage/oracle-object-storage.md)
- [[!DNL SFTP] コネクタ](connectors/cloud-storage/sftp.md)

### 顧客関係管理（CRM）

CRM システムは顧客との関係を築くのに役立つデータを提供し、顧客との忠誠度を高め、顧客保持率を高めます。Experience Platformは、CRMデータを[!DNL Microsoft Dynamics 365]と[!DNL Salesforce]から取り込むためのサポートを提供します。 詳しくは、次の関連ドキュメントを参照してください。

- [[!DNL Microsoft Dynamics] コネクタ](connectors/crm/ms-dynamics.md)
- [[!DNL Salesforce] コネクタ](connectors/crm/salesforce.md)

### 顧客の成功

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

### e コマース

Experience Platformは、サードパーティのeCommerceシステムからデータを取り込むためのサポートを提供します。 特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

- [[!DNL Shopify]](connectors/ecommerce/shopify.md)

### マーケティングの自動処理

Experience Platform は、サードパーティのマーケティング自動化システムからデータを取得する機能を備えています。特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

- [[!DNL HubSpot] コネクタ](connectors/marketing-automation/hubspot.md)

### 支払い

Experience Platformは、サードパーティの支払いシステムからデータを取り込むためのサポートを提供します。 特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

- [[!DNL PayPal] コネクタ](connectors/payments/paypal.md)

### ストリーミング

Experience Platformは、ストリーミングソースからデータを取り込むためのサポートを提供します。 特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

- [[!DNL HTTP API]](connectors/streaming/http.md)


### プロトコル

Experience Platformは、サードパーティのプロトコルシステムからデータを取り込むためのサポートを提供します。 特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

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

## 利用条件{#terms-and-conditions}

ベータ版（「ベータ版」）と表示されたソースを使用することで、お客様は、ベータ版が「現状のまま」提供されることを、いかなる種類の&#x200B;***の保証も受けずに、この旨を了承します。***

Adobeは、ベータ版を保守、修正、更新、変更、またはその他の方法でサポートする義務はありません。 ベータ版および/または付属のマテリアルの正しい機能やパフォーマンスに一切依存しないように注意し、注意が必要です。 ベータ版はAdobeの機密情報と見なされます。

お客様がAdobeに提供する「フィードバック」（ベータ版の使用中に発生した問題や欠陥、提案、改善点、推奨事項を含む、ただしこれに限定しない、ベータ版に関する情報）は、本書により、そのフィードバックに対するすべての権利、役職、関心を含むAdobeに割り当てられます。

Open Feedbackを送信するか、サポートチケットを作成して、ご提案を共有したり、バグを報告したり、機能強化を求めたりします。
