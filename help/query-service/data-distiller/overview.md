---
title: Data Distillerの概要
description: ライセンス付与に関する、クエリサービスデータの Data Distillerの使用制限の概要です。
source-git-commit: ae4ecd43071a198592193a1a598a064cdc6be2f6
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 1%

---

# Data Distillerの概要

Data Distillerは、Adobe Experience Platformの機能のサブセットを含むパッケージ提供です。 Data Distillerを使用すると、クエリサービスでバッチクエリを実行することで、リアルタイム顧客プロファイルや分析的な使用例に対して、取り込み後のデータの準備（クリーニング、整形、操作など）を実行できます。 Data Distillerの使用は、Platform ベースのアプリケーションの使用権限に応じて異なります。

## ライセンス使用状況 {#license-usage}

この  [Data Distiller License Usage Dashboard](./license-usage.md) は、Data Distiller Compute 時間を購入すると利用できます。 ライセンス使用状況ダッシュボードを使用すると、使用権限を付与されたコンピューティング時間の消費状況を監視できます。 詳しくは、 [Data Distillerライセンス使用状況ドキュメント](./license-usage.md) を参照して、組織のクエリサービスライセンスの使用状況に関する重要な情報を確認してください。

<!-- Update these descriptions post 23.3 release
## Scoping parameters {#scoping-parameters}

Scoping parameters are usage limits that relate to the scoping of your required set up, and are defined by your license capacity. Without add-ons, Data Distiller's scoping parameters are as follows: 

* **Compute Hours**: You can use PSQL or the Query Service API to run batch queries executed in any sandbox (scheduled or otherwise) to scan and write data. This uses your allotted Compute Hours per year as determined in the scoping process of your license agreement. Total Compute Hours is accumulated across all Sandboxes.
* **Data Ingested**: The data ingested into Adobe Experience Platform which can be queried using Data Distiller is subject to the limitations described in your then-current license to Adobe Real-Time Customer Data Platform, Customer Journey Analytics, and/or Adobe Journey Optimizer.
* **Data Lake Storage**: The data lake storage provided in your then-current license to Adobe Real-Time Customer Data Platform, Customer Journey Analytics, and/or Adobe Journey Optimizer may also be used with Data Distiller. Data Lake Storage is a shared feature.
* **Query Service Users**: The number of Query Service users detailed in your then-current license to Adobe Real-Time Customer Data Platform, Customer Journey Analytics, and/or Adobe Journey Optimizer may also be used with Data Distiller. Query Service Users is a shared feature. 
-->

## ガードレール

詳しくは、 [クエリサービスガードレール](../guardrails.md) ライセンス付与に関するクエリサービスデータのデフォルトの使用制限に関するドキュメント。

<!-- Update these descriptions post 23.3 release
## Static limits

A static limit is the usage limit that relates to the functional boundaries of Adobe Experience Platform Activation. [More information on Adobe Experience Platform Activation](https://helpx.adobe.com/ca/legal/product-descriptions/adobe-experience-platform0.html) can be found in the Adobe help documents. A summary of Data Distiller static limits are listed below, for more complete information please refer to the Query Service guardrail document.  

* **Batch Queries**: Scheduled batch queries time out after 24 hours.
* **Query Service**: You can use Query Service for the following purposes: 
    * To run SQL queries for data analysis and post ingestion data preparation (cleaning, shaping, and manipulation).
    * To run SQL queries to create roll-up metrics to surface directly into a BI tool.
    * To quickly inspect data within Adobe Experience Platform.
    * To generate meaningful insights from your data.
* **Reporting API Call**: To ensure queries run on aggregated data using the reporting API have enough resources to execute efficiently. This includes queries that enhance existing data models such as those provided by Real-Time Customer Data Platform. The reporting API tracks resource utilization by assigning concurrency slots to each query. A maximum of four reporting API calls are available concurrently. If you access the reporting API through a BI tool and require more concurrency slots, a BI server is required.
-->

