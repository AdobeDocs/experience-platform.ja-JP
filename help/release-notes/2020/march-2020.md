---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2020年3月12日）
doc-type: release notes
last-update: March 10, 2020
author: ens71067
keywords: release notes;
translation-type: tm+mt
source-git-commit: f881c1365684b1ca9e6bf9a8ce866d234dc54128
workflow-type: tm+mt
source-wordcount: '848'
ht-degree: 8%

---


# Adobe Experience Platform リリースノート

**リリース日：2020 年 3 月 11 日**

Adobe Experience Platform内の既存の機能の更新：

* [!DNL Data Governance](#governance)
* [!DNL Data Ingestion](#ingestion)
* [!DNL Destinations](#destinations)
* [!DNL Identity Service](#identity)
* [!DNL Sources](#sources)

## [!DNL Data Governance] {#governance}

[!DNL Experience Platform] 会社は、複数のエンタープライズシステムからのデータを統合し、マーケティング担当者が顧客を特定、理解、関与できるようにします。 [!DNL Experience Platform] には、システム間でデータを適切に使用し、システム間で共有する場合に、データの適切な使用を保証するための、Data Usage Labeling and Enforcement(DULE)など、エンドツーエンドのデータ管理インフラストラクチャが含ま [!DNL Platform] れます。

Adobe Experience Platform [!DNL Data Governance] は、お客様のデータを管理し、データの使用に適用される規制、制限、ポリシーの遵守を確保するために使用される一連の戦略とテクノロジーです。 カタログ化、データ系列、データ使用状況のラベル付け、データアクセスポリシー、マーケティング活動 [!DNL Experience Platform] のためのデータに関するアクセス制御など、様々なレベルで重要な役割を果たします。

**新機能**

>[!NOTE]
>
>次の新機能の一部は現在ベータ版であり、一部のユーザーはご利用いただけません。 ベータ版の機能は変更される可能性があります。

| 機能 | 説明 |
| ------- | ----------- |
| データ使用ポリシーの自動適用 [!DNL Real-time Customer Data Platform] | データ使用ポリシーは、宛先に対するデータのアクティブ化のワークフローに適用されるようになりました。 [!DNL Data Governance] は、既存のアクティベーションに影響を与える変更（データセットのラベル、結合ポリシー、セグメント定義などの変更）を行う場合にも埋め込まれ、適用されます。 |
| 強制のデータ系列 | リアルタイムCDPでデータ使用ポリシーに違反した場合、UIは、データ系列情報を含む通知を表示し、ポリシーが違反された理由と、違反を解決するためにユーザーが実行できる操作を理解します。 |


**既知の問題**

* None

詳細については、「 [!DNL Data Governance]Data Governance overview [](../../data-governance/home.md)」を参照してください。

## データ収集 {#ingestion}

Adobe Experience Platformは、あらゆる種類のデータや遅延を取り込むための豊富な機能セットを提供します。 Adobe Experience Platform [!DNL Data Ingestion] は、Batch API、Streaming API、ネイティブのAdobe Connectors、Data Integrationパートナー、Adobe Experience PlatformUIなど、データを取り込むための複数の代替手段を提供します。

**新機能**

| 機能 | 説明 |
|------- | -----------|
| 部分的なバッチ取り込み | 部分的なバッチ取り込みとは、エラーを含むデータを特定のしきい値まで取り込む機能です。 この機能を使用すると、ユーザーは正しいデータをすべてAdobe Experience Platformに取り込み、誤ったデータはすべて個別にバッチ処理できます。 失敗したバッチに、検証に合格しなかった理由を説明する詳細が追加されます。 部分的なバッチ取り込みについて詳しくは、 [部分的なバッチ取り込みドキュメントを参照してください](../../ingestion/batch-ingestion/partial.md)。 |

**既知の問題**

* None

データをPlatformに取り込む方法の詳細については、『 [データ取り込みドキュメント](../../ingestion/home.md)』を参照してください。


## 宛先 {#destinations}

ア [ドビのリアルタイム顧客データPlatformでは](../../rtcdp/overview.md)、宛先は、目的のプラットフォームとの事前に構築された統合であり、これらのパートナーに対してシームレスにデータをアクティブ化します。

**新しい宛先**

Adobe Experience Platformデータをアクティブ化できる新しい宛先が利用できます。 詳しくは、以下を参照してください。

| 宛先 | 説明 |
|--- | ---|
| クラウドストレージの宛先 | Adobe Real-time CDP can now deliver your segments as data files to your [!DNL Amazon S3] or SFTP cloud storage locations. これにより、オーディエンスとそのプロファイル属性を CSV またはタブ区切りファイル経由で内部システムに送信できます。 |
| 広告の宛先 | 現在は、 [!DNL Google] Adobe Real-time CDPで現在サポートされている3つの異なる [!DNL Google] プラットフォーム用に、デスティネーションカードが3つのデスティネーションカードに分割されます。 [!DNL Google Ads]、 [!DNL Google Ad Manager]、 [!DNL Google] 表示、ビデオ360。 |

詳しくは、 [宛先の概要を参照してください](../../rtcdp/destinations/destinations-overview.md)

## [!DNL Identity Service] {#identity}

関連するデジタルエクスペリエンスを提供するには、顧客に関する完全な理解が必要です。 これは、お客様のデータが異なる複数のシステムに断片化されている場合に、より難しくなり、個々のお客様が複数の「アイデンティティ」を持っているように見える原因となります。

Adobe Experience Platform [!DNL Identity Service] を使用すると、デバイスやシステム間でIDをつなぐことで、顧客とその行動をより良く表示でき、効果的な個人のデジタルエクスペリエンスをリアルタイムで提供できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 強化されたプライベートグラフ | プライベートグラフ機能が強化され、週別のバッチ処理から日別に更新されたグラフへのグラフ生成待ち時間が短縮され、 [!DNL Identity Service] 顧客は最新のIDのグラフやリンクにアクセスできるようになりました。 |

**既知の問題**

* None

詳しくは、「 [!DNL Identity Service]IDサービスの概要 [](../../identity-service/home.md)」を参照してください。

## ソース {#sources}

Adobe Experience Platformは、外部ソースからデータを取り込みながら、サー [!DNL Platform] ビスを使用してデータの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRMシステムなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] には、様々なデータプロバイダーのソース接続を簡単に設定できる、RESTful APIとインタラクティブUIが用意されています。 これらのソース接続を使用すると、外部のストレージシステムやCRMサービスの認証と接続、取り込みの実行時間の設定、データ取り込みスループットの管理を行うことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| Adobe Audience Managerコネクタの非推奨シグナル | オーディエンスマネージャーからの信号レベルのデータは送信されなくなります。 特性とセグメントのセグメントのメンバーシップは、引き続き含まれます。 この変更の結果、受信データセットは生成されなくなります。 |
| 名前が変更されたデータセット | オーディエンスマネージャーコネクタによって生成されるデータセットには、名前と説明が更新されます。 |
| オーディエンスマネージャーで [!DNL Profile] 切り替えを有効にする | [!DNL Profile] の切り替えを有効または無効にして、データセットをにプロモートすることができ [!DNL Real-time Customer Profile]ます。 切り替えはデフォルトで有効になります。 |
| クラウドストレージシステムのUIのサポート | UIで使用する新しいソースコネクタ [!DNL Azure Data Lake Storage Gen2] です。 |
| CRMシステムのUIのサポート | UIの、、 [!DNL HubSpot]およびの新しいソースコネクタ [!DNL Salesforce Service Cloud][!DNL ServiceNow] 。 |
| データベースシステムのUIのサポート | UIの、、、、 [!DNL AWS Redshift]およびの新しいソースコネクタ [!DNL Google BigQuery][!DNL MariaDB][!DNL Microsoft SQL Server][!DNL MySQL] 。 |

**既知の問題**

* None

ソースについて詳しくは、 [ソースの概要を参照してください](../../sources/home.md)。