---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート（2020年3月11日）
doc-type: release notes
last-update: March 10, 2020
author: ens71067
keywords: release notes;
translation-type: tm+mt
source-git-commit: 0232acdc64019b9d93888e8137ef9bc8e114779b
workflow-type: tm+mt
source-wordcount: '840'
ht-degree: 60%

---


# Adobe Experience Platform リリースノート

**リリース日：2020年3月11日**

Adobe Experience Platform の既存の機能のアップデート：

* [[!DNL Data Governance]](#governance)
* [[!DNL Data Ingestion]](#ingestion)
* [[!DNL Destinations]](#destinations)
* [[!DNL Identity Service]](#identity)
* [[!DNL Sources]](#sources)

## [!DNL Data Governance] {#governance}

[!DNL Experience Platform] を使用すると、会社は複数のエンタープライズシステムからデータを統合し、マーケティング担当者が顧客をより良く識別、理解し、惹きつけることができるようになります。[!DNL Experience Platform] には、システム間でデータを適切に使用し、共有する場合に、データを適切に使用するためのエンドツーエンドのデータ管理インフラストラクチャ [!DNL Platform] が含まれます。

Adobe Experience Platform [!DNL Data Governance] is a series of strategies and technologies used to manage customer data and ensure compliance with regulations, restrictions, and policies applicable to data usage. It plays a key role within [!DNL Experience Platform] at various levels, including cataloging, data lineage, data usage labeling, data access policies, and access control on data for marketing actions.

**新機能**

>[!NOTE]
>
>現在、次の新機能の一部はベータ版です。このため、一部のユーザーにはご利用いただけません。ベータ版の機能は変更されることがあります。

| 機能 | 説明 |
| ------- | ----------- |
| データ使用ポリシーの自動適用 [!DNL Real-time Customer Data Platform] | 宛先に対してデータをアクティブ化するワークフローにデータ使用ポリシーが適用されるようになりました。[!DNL Data Governance] は、既存のアクティベーションに影響を与える変更（データセットのラベル、結合ポリシー、セグメント定義などの変更）を行う場合にも埋め込まれ、適用されます。 |
| データリネージの適用 | リアルタイム CDP でデータ使用ポリシーが侵された場合、UI にはデータリネージ情報を含む通知が表示されます。このため、ユーザーは、ポリシー違反の理由と、違反を解決するために必要な操作を理解できます。 |


**既知の問題**

* なし

For more information about [!DNL Data Governance], see the [Data Governance overview](../../data-governance/home.md).

## データ取得 {#ingestion}

Adobe Experience Platform には、あらゆる種類のデータやデータのレイテンシを取り込むための豊富な機能が用意されています。Adobe Experience Platform [!DNL Data Ingestion] provides multiple alternatives for ingesting data including Batch APIs, Streaming APIs, native Adobe connectors, data integration partners, or the Adobe Experience Platform UI.

**新機能**

| 機能 | 説明 |
|------- | -----------|
| バッチの部分取り込み | バッチの部分取り込みは、エラーを含むデータを特定のしきい値まで取り込む機能です。この機能を使用すると、ユーザーは正しいデータをすべて Adobe Experience Platform に取り込める一方で、不正なデータはすべて別々にバッチ処理できます。失敗したバッチには、検証に合格しなかった理由を示す詳細が追加されます。バッチの部分取り込みについて詳しくは、[バッチの部分取り込みに関するドキュメント](../../ingestion/batch-ingestion/partial.md)を参照してください。 |

**既知の問題**

* なし

データを Platform に取り込む方法については、[データ取得に関するドキュメント](../../ingestion/home.md)を参照してください。


## 宛先 {#destinations}

In [Real-time Customer Data Platform](../../rtcdp/overview.md), destinations are pre-built integrations with destination platforms that activate data to those partners in a seamless way.

**新しい宛先**

新しい宛先は、Adobe Experience Platform データをアクティブ化する際に使用できます。詳しくは、以下を参照してください。

| 宛先 | 説明 |
|--- | ---|
| クラウドストレージの宛先 | Adobe Real-time CDP can now deliver your segments as data files to your [!DNL Amazon S3] or SFTP cloud storage locations. これにより、オーディエンスとそのプロファイル属性を CSV またはタブ区切りファイル経由で内部システムに送信できます。 |
| 広告の宛先 | The [!DNL Google] destination card is now split into three destination cards, for the three different [!DNL Google] platforms currently supported in Adobe Real-time CDP: [!DNL Google Ads], [!DNL Google Ad Manager], [!DNL Google] Display &amp; Video 360. |

詳しくは、[宛先の概要](../../rtcdp/destinations/destinations-overview.md)を参照してください。

## [!DNL Identity Service] {#identity}

関連するデジタルエクスペリエンスを提供するには、顧客を完全に理解する必要があります。顧客データが異なる複数のシステムに断片化されており、そのため各顧客が複数の「ID」を持つと考えられる場合、顧客を理解するのはさらに困難になります。

Adobe Experience Platform [!DNL Identity Service] helps you to gain a better view of your customer and their behavior by bridging identities across devices and systems, allowing you to deliver impactful, personal digital experiences in real-time.

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 強化されたプライベートグラフ | Private Graph functionality has been enhanced to reduce graph generation latency from a weekly batch process to a daily refreshed graph, allowing [!DNL Identity Service] customers to access more up-to-date identity graphs and linkages. |

**既知の問題**

* なし

For more information about [!DNL Identity Service], see the [Identity Service overview](../../identity-service/home.md).

## ソース {#sources}

Adobe Experience Platform can ingest data from external sources while allowing you to structure, label, and enhance that data using [!DNL Platform] services. アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できるようにする RESTful API とインタラクティブな UI を提供します。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| Adobe Audience Manager コネクタの廃止された信号 | Audience Manager から信号レベルのデータは送信されなくなります。特性とセグメントのセグメントメンバーシップは、引き続き送信されることに注意してください。この変更の結果、受信データセットは生成されなくなります。 |
| データセット名の変更 | Audience Manager コネクタで生成されたデータセットの名前と説明が更新されます。 |
| Enable [!DNL Profile] toggle in Audience Manger | [!DNL Profile] の切り替えを有効または無効にして、データセットをにプロモートすることができ [!DNL Real-time Customer Profile]ます。 デフォルトでは、切り替えが有効になっています。 |
| クラウドストレージシステムの UI のサポート | New source connector for [!DNL Azure Data Lake Storage Gen2] in the UI. |
| CRM システムの UI のサポート | New source connector for [!DNL HubSpot], [!DNL Salesforce Service Cloud], and [!DNL ServiceNow] in the UI. |
| データベースシステムの UI のサポート | New source connector for [!DNL AWS Redshift], [!DNL Google BigQuery], [!DNL MariaDB], [!DNL Microsoft SQL Server], and [!DNL MySQL] in the UI. |

**既知の問題**

* なし

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。