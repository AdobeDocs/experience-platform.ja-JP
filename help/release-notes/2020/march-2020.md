---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート（2020年3月11日）
doc-type: release notes
last-update: March 10, 2020
author: ens71067
keywords: リリースノート;
translation-type: tm+mt
source-git-commit: 126b3d1cf6d47da73c6ab045825424cf6f99e5ac
workflow-type: tm+mt
source-wordcount: '841'
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

[!DNL Experience Platform] を使用すると、会社は複数のエンタープライズシステムからデータを統合し、マーケティング担当者が顧客をより良く識別、理解し、惹きつけることができるようになります。[!DNL Experience Platform] には、システム間でデータを適切に使用し、システム間で共有する場合にデータを適切に使用するための、エンドツーエンドのデータ管理インフラストラクチャ [!DNL Platform] が含まれます。

Adobe Experience Platform[!DNL Data Governance]は、お客様のデータを管理し、データの使用に適用される規制、制限、ポリシーの遵守を確保するために使用される一連の戦略とテクノロジーです。 [!DNL Experience Platform]内では、カタログ化、データ系列、データ使用状況のラベル付け、データアクセスポリシー、マーケティング活動のデータに関するアクセス制御など、様々なレベルで重要な役割を果たします。

**新機能**

>[!NOTE]
>
>現在、次の新機能の一部はベータ版です。このため、一部のユーザーにはご利用いただけません。ベータ版の機能は変更されることがあります。

| 機能 | 説明 |
| ------- | ----------- |
| [!DNL Real-time Customer Data Platform]に対するデータ使用ポリシーの自動適用 | 宛先に対してデータをアクティブ化するワークフローにデータ使用ポリシーが適用されるようになりました。[!DNL Data Governance] は、既存のアクティベーションに影響を与える変更（データセットのラベル、結合ポリシー、セグメント定義などの変更）を行う場合にも埋め込まれ、適用されます。 |
| データリネージの適用 | リアルタイム CDP でデータ使用ポリシーが侵された場合、UI にはデータリネージ情報を含む通知が表示されます。このため、ユーザーは、ポリシー違反の理由と、違反を解決するために必要な操作を理解できます。 |


**既知の問題**

* なし

[!DNL Data Governance]の詳細については、[データガバナンスの概要](../../data-governance/home.md)を参照してください。

## データ取得 {#ingestion}

Adobe Experience Platform には、あらゆる種類のデータやデータのレイテンシを取り込むための豊富な機能が用意されています。Adobe Experience Platform[!DNL Data Ingestion]は、Batch API、Streaming API、ネイティブAdobeコネクタ、Data Integrationパートナー、Adobe Experience PlatformUIなど、データを取り込むための複数の代替手段を提供します。

**新機能**

| 機能 | 説明 |
|------- | -----------|
| バッチの部分取り込み | バッチの部分取り込みは、エラーを含むデータを特定のしきい値まで取り込む機能です。この機能を使用すると、ユーザーは正しいデータをすべて Adobe Experience Platform に取り込める一方で、不正なデータはすべて別々にバッチ処理できます。失敗したバッチには、検証に合格しなかった理由を示す詳細が追加されます。バッチの部分取り込みについて詳しくは、[バッチの部分取り込みに関するドキュメント](../../ingestion/batch-ingestion/partial.md)を参照してください。 |

**既知の問題**

* なし

データを Platform に取り込む方法については、[データ取得に関するドキュメント](../../ingestion/home.md)を参照してください。


## 宛先 {#destinations}

[リアルタイム顧客データプラットフォーム](../../rtcdp/overview.md)では、宛先は、それらのパートナーに対してシームレスにデータをアクティブ化する目的のプラットフォームとの事前に構築された統合です。

**新しい宛先**

新しい宛先は、Adobe Experience Platform データをアクティブ化する際に使用できます。詳しくは、以下を参照してください。

| 宛先 | 説明 |
|--- | ---|
| クラウドストレージの宛先 | リアルタイムCDPは、セグメントをデータファイルとして[!DNL Amazon S3]またはSFTPクラウドストレージの場所に配信できるようになりました。 これにより、オーディエンスとそのプロファイル属性を CSV またはタブ区切りファイル経由で内部システムに送信できます。 |
| 広告の宛先 | [!DNL Google]宛先カードは、現在Real-time CDPでサポートされている3つの異なる[!DNL Google]プラットフォーム用に、3つの宛先カードに分割されます。[!DNL Google Ads]、[!DNL Google Ad Manager]、[!DNL Google]ディスプレイ&amp;ビデオ360。 |

詳しくは、[宛先の概要](../../destinations/home.md)を参照してください。

## [!DNL Identity Service] {#identity}

関連するデジタルエクスペリエンスを提供するには、顧客を完全に理解する必要があります。顧客データが異なる複数のシステムに断片化されており、そのため各顧客が複数の「ID」を持つと考えられる場合、顧客を理解するのはさらに困難になります。

Adobe Experience Platform[!DNL Identity Service]は、デバイスやシステム間でIDをつなぐことで、顧客とその行動をより良く表示できるように支援します。これにより、インパクトなパーソナルデジタルエクスペリエンスをリアルタイムで提供できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 強化されたプライベートグラフ | プライベートグラフ機能が強化され、週別のバッチ処理から日別に更新されたグラフにグラフ生成の待ち時間が短縮され、[!DNL Identity Service]ユーザーはより最新のIDグラフやリンクにアクセスできるようになりました。 |

**既知の問題**

* なし

[!DNL Identity Service]について詳しくは、[IDサービスの概要](../../identity-service/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platformは外部ソースからデータを取り込みながら、[!DNL Platform]サービスを使ってデータの構造、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できるようにする RESTful API とインタラクティブな UI を提供します。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| Adobe Audience Manager コネクタの廃止された信号 | Audience Manager から信号レベルのデータは送信されなくなります。特性とセグメントのセグメントメンバーシップは、引き続き送信されることに注意してください。この変更の結果、受信データセットは生成されなくなります。 |
| データセット名の変更 | Audience Manager コネクタで生成されたデータセットの名前と説明が更新されます。 |
| オーディエンスマネージャーで[!DNL Profile]切り替えを有効にする | [!DNL Profile] の切り替えを有効または無効にして、データセットをにプロモートすることができ [!DNL Real-time Customer Profile]ます。デフォルトでは、切り替えが有効になっています。 |
| クラウドストレージシステムの UI のサポート | UIの[!DNL Azure Data Lake Storage Gen2]の新しいソースコネクタ。 |
| CRM システムの UI のサポート | UIの[!DNL HubSpot]、[!DNL Salesforce Service Cloud]、および[!DNL ServiceNow]の新しいソースコネクタ。 |
| データベースシステムの UI のサポート | UIの[!DNL AWS Redshift]、[!DNL Google BigQuery]、[!DNL MariaDB]、[!DNL Microsoft SQL Server]、および[!DNL MySQL]の新しいソースコネクタ。 |

**既知の問題**

* なし

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。