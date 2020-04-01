---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform Source Connectorsの概要
topic: overview
translation-type: tm+mt
source-git-commit: 291d669cc0f5b009b23dc2771688ee54b53d08db

---


# ソースコネクタの概要

Adobe Experience Platformを使用すると、外部ソースからデータを取り込み、プラットフォームサービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

エクスペリエンスプラットフォームは、様々なデータプロバイダーへのソース接続を簡単に設定できるRESTful APIとインタラクティブUIを提供します。 これらのソース接続を使用すると、サードパーティ製システムの認証、取り込みの実行時間の設定、データ取り込みのスループットの管理を行うことができます。

エクスペリエンスプラットフォームを使用すると、異なるソースから収集したデータを一元管理し、得た洞察を利用して、より多くの作業を行うことができます。

## ソースのタイプ

エクスペリエンスプラットフォームのソースは、次のカテゴリに分類されます。

### アドビアプリケーション

Experience Platformを使用すると、Adobe Analytics、Adobe Platform Manager、Experience Platform Launchなど、他のAdobeオーディエンスからデータを取り込むことができます。 詳しくは、次の関連ドキュメントを参照してください。

- [Adobeオーディエンスマネージャーコネクタの概要](./ui/adobe-applications/audience-manager.md)
- [UIでのAdobeオーディエンスマネージャーソースコネクタの作成](https://www.adobe.io/apis/experienceplatform/home/tutorials/sources-ui-tutorials.html#!api-specification/markdown/narrative/tutorials/sources_tutorial/ui/adobe-applications/aam-ui-tutorial.md)
- [Adobe Analytics data connectorの概要](./ui/adobe-applications/analytics.md)
- [UIでのAdobe Analyticsソースコネクタの作成](https://www.adobe.io/apis/experienceplatform/home/tutorials/sources-ui-tutorials.html#!api-specification/markdown/narrative/tutorials/sources_tutorial/ui/adobe-applications/adobe-analytics-ui-tutorial.md)

### クラウドストレージ

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードを行う必要なく、独自のデータをプラットフォームに取り込むことができます。 プロセスのすべての手順は、ユーザーインターフェイスを使用してSourcesワークフローに統合されます。 クラウドストレージプロバイダーのサポートには、Amazon S3、Azure Blob、FTPサーバー、SFTPサーバーが含まれます。 詳しくは、次の関連ドキュメントを参照してください。

- [UIでAzure BlobまたはAmazon S3ソースコネクタを作成する](https://www.adobe.io/apis/experienceplatform/home/tutorials/sources-ui-tutorials.html#!api-specification/markdown/narrative/tutorials/sources_tutorial/ui/cloud-storages/amazon-s3-ui-tutorial.md)
- [UIでAzure Data LakeストレージGen2ソースコネクタを作成する](https://www.adobe.io/apis/experienceplatform/home/tutorials/sources-ui-tutorials.html#!api-specification/markdown/narrative/tutorials/sources_tutorial/ui/cloud-storages/adls-gen2-ui-tutorial.md)
- [UIでのFTPまたはSFTPソースコネクタの作成](https://www.adobe.io/apis/experienceplatform/home/tutorials/sources-ui-tutorials.html#!api-specification/markdown/narrative/tutorials/sources_tutorial/ui/cloud-storages/ftp-sftp-ui-tutorial.md)
- [UIでのGoogle Cloudストレージソースコネクタの作成](https://www.adobe.io/apis/experienceplatform/home/tutorials/sources-ui-tutorials.html#!api-specification/markdown/narrative/tutorials/sources_tutorial/ui/cloud-storages/google-cloud-storage-ui-tutorial.md)

### 顧客関係管理（CRM）

CRMシステムは顧客との関係を築くのに役立つデータを提供し、顧客との忠誠度を高め、顧客保持率を高めます。 エクスペリエンスプラットフォームは、Microsoft Dynamics 365およびSalesforceからCRMデータを取り込むためのサポートを提供します。 詳しくは、次の関連ドキュメントを参照してください。

- [UIでMicrosoft Dynamics 365またはSalesforceソースコネクタを作成する](https://www.adobe.io/apis/experienceplatform/home/tutorials/sources-ui-tutorials.html#!api-specification/markdown/narrative/tutorials/sources_tutorial/ui/crm/dynamics-salesforce-ui-tutorial.md)
- [UIでのPayPalソースコネクタの作成](https://www.adobe.io/apis/experienceplatform/home/tutorials/sources-ui-tutorials.html#!api-specification/markdown/narrative/tutorials/sources_tutorial/ui/crm/paypal-tutorial.md)

### 顧客の成功(CS)

エクスペリエンスプラットフォームは、サードパーティの顧客成功アプリケーションからデータを取り込むためのサポートを提供します。 詳しくは、次の関連ドキュメントを参照してください。

- [UIでのSalesforceサービスクラウドソースコネクタの作成](https://www.adobe.io/apis/experienceplatform/home/tutorials/sources-ui-tutorials.html#!api-specification/markdown/narrative/tutorials/sources_tutorial/ui/customer-success/salesforce-service-cloud-tutorial.md)
- [UIでのServiceNowソースコネクタの作成](https://www.adobe.io/apis/experienceplatform/home/tutorials/sources-ui-tutorials.html#!api-specification/markdown/narrative/tutorials/sources_tutorial/ui/customer-success/servicenow-ui-tutorial.md)

### データベース

エクスペリエンスプラットフォームは、サードパーティのデータベースからデータを取り込む機能を提供します。 特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

- [UIでAWS Redshiftソースコネクタを作成する](https://www.adobe.io/apis/experienceplatform/home/tutorials/sources-ui-tutorials.html#!api-specification/markdown/narrative/tutorials/sources_tutorial/ui/databases/amazon-redshift-ui-tutorial.md)
- [UIでAzure Synapse Analyticsソースコネクタを作成する](https://www.adobe.io/apis/experienceplatform/home/tutorials/sources-ui-tutorials.html#!api-specification/markdown/narrative/tutorials/sources_tutorial/ui/databases/azure-synapse-analytics-ui-tutorial.md)
- [UIでのGoogle BigQueryソースコネクタの作成](https://www.adobe.io/apis/experienceplatform/home/tutorials/sources-ui-tutorials.html#!api-specification/markdown/narrative/tutorials/sources_tutorial/ui/databases/google-big-query-ui-tutorial.md)
- [UIでのMariaDBソースコネクタの作成](https://www.adobe.io/apis/experienceplatform/home/tutorials/sources-api-tutorials.html#!api-specification/markdown/narrative/tutorials/sources_tutorial/api/database-nosql/mariadb-api-tutorial.md)
- [UIでのMicrosoft SQL Serverソースコネクタの作成](https://www.adobe.io/apis/experienceplatform/home/tutorials/sources-ui-tutorials.html#!api-specification/markdown/narrative/tutorials/sources_tutorial/ui/databases/sql-server-ui-tutorial.md)
- [UIでのMySQLソースコネクタの作成](https://www.adobe.io/apis/experienceplatform/home/tutorials/sources-ui-tutorials.html#!api-specification/markdown/narrative/tutorials/sources_tutorial/ui/databases/mysql-ui-tutorial.md)
- [UIでPostgreSQLソースコネクタを作成する](https://www.adobe.io/apis/experienceplatform/home/tutorials/sources-ui-tutorials.html#!api-specification/markdown/narrative/tutorials/sources_tutorial/ui/databases/postgresql-tutorial.md)

### マーケティングの自動化

エクスペリエンスプラットフォームは、サードパーティのマーケティング自動化システムからデータを取り込むためのサポートを提供します。 特定のソースコネクタの詳細については、次の関連ドキュメントを参照してください。

- [UIでのHubSpotソースコネクタの作成](https://www.adobe.io/apis/experienceplatform/home/tutorials/sources-ui-tutorials.html#!api-specification/markdown/narrative/tutorials/sources_tutorial/ui/marketing-automation/hubspot-tutorial.md)

## APIチュートリアル

フローサービスAPIを使用してソースコネクタを作成できます。 詳しくは、ソースAPIチュートリアル [ドキュメント](https://www.adobe.io/apis/experienceplatform/home/tutorials/sources-api-tutorials.html#!api-specification/markdown/narrative/tutorials/sources_tutorial/api/sources-api-tutorial.md)。

## アクセス制御取り込み時のソースのデータ

データ取り込みのソースに対する権限は、アドビの管理コンソールで管理できます。 権限には、特定の製品プロファイルの「権 *限* 」タブからアクセスできます。 権限を編集 **パネルから** 、データ取り込みメニューのエントリを使用して、ソースに関連する権 *限にアクセスできます* 。 「 **** 表示ソース *」権限は、「* Catalog ****** 」タブの使用可能なソースと「認証済みのソース」に対する読み取り専用アクセス権を付与し、「ソースの管理」参照権限は、ソースの作成、編集および無効化に対するフルアクセス権を付与します。

次の表に、これらの権限の様々な組み合わせに基づくUIの動作の概要を示します。

| 権限レベル | 説明 |
| ---- | ----|
| **表示ソース** オン | 「 *Catalog* 」タブの各ソースタイプ、および「 *Browse**」、「Accounts」、「DataFlow」タブの各ソースタイプのソースに読み取り専用アクセス権を付*** 与します。 |
| **ソースの管理** : | 表示ソースに含まれる機能に加えて、 **Connect Source** Option Catalogの *Connect Source* Option、Select Data Select Browse Data Option Browse Browse Browse ****** Optionに対するアクセス権を付与します。 **また、「ソースの管理** 」では *、DataFlowの有効/無効を切り替えたり、スケジ* ュールを編集したりできます。 |
| **表示ソース** Offおよび **Manage Sources** Off | ソースへのすべてのアクセスを取り消します。 |

管理コンソールから付与される使用可能な権限（これら4つのソースを含む）の詳細については、 [アクセス制御の概要](../access-control/home.md)。
