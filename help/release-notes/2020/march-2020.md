---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート（2020年3月11日）
doc-type: release notes
last-update: March 10, 2020
author: ens71067
keywords: リリースノート;
exl-id: 407c2bac-4c8a-4939-b3dd-788250f15650
source-git-commit: 03e7863f38b882a2fbf6ba0de1755e1924e8e228
workflow-type: tm+mt
source-wordcount: '851'
ht-degree: 71%

---

# Adobe Experience Platform リリースノート

**リリース日：2020 年 3 月 11 日（PT）**

Adobe Experience Platform の既存の機能のアップデート：

* [データガバナンス](#governance)
* [[!DNL Data Ingestion]](#ingestion)
* [[!DNL Destinations]](#destinations)
* [[!DNL Identity Service]](#identity)
* [[!DNL Sources]](#sources)

## データガバナンス {#governance}

[!DNL Experience Platform] を使用すると、会社は複数のエンタープライズシステムからデータを統合し、マーケティング担当者が顧客をより良く識別、理解し、惹きつけることができるようになります。[!DNL Experience Platform] には、内でのデータの適切な使用を保証するエンドツーエンドのデータガバナンスインフラストラクチャが含まれます [!DNL Platform] とは、システム間で共有される場合に使用されます。

Adobe Experience Platform データガバナンスは、顧客データを管理し、データの使用に適用される規制、制限、ポリシーへの準拠を確保するために使用される一連の戦略とテクノロジーです。内で重要な役割を果たします [!DNL Experience Platform] 様々なレベル（カタログ化、データ系列、データ使用のラベル付け、データアクセスポリシー、マーケティングアクションのデータに対するアクセス制御など）で使用できます。

**新機能**

>[!NOTE]
>
>現在、次の新機能の一部はベータ版です。このため、一部のユーザーにはご利用いただけません。ベータ版の機能は変更されることがあります。

| 機能 | 説明 |
| ------- | ----------- |
| のデータ使用ポリシーの自動適用 [!DNL Real-time Customer Data Platform] | 宛先に対してデータをアクティブ化するワークフローにデータ使用ポリシーが適用されるようになりました。また、データガバナンスは、既存のアクティベーションに影響を与える変更（データセットラベル、結合ポリシー、セグメント定義の変更など）をおこなう場合にも、組み込まれて適用されます。 |
| データリネージの適用 | リアルタイム CDP でデータ使用ポリシーが侵された場合、UI にはデータリネージ情報を含む通知が表示されます。このため、ユーザーは、ポリシー違反の理由と、違反を解決するために必要な操作を理解できます。 |


**既知の問題**

* なし

データガバナンスについて詳しくは、「[データガバナンスの概要](../../data-governance/home.md)」を参照してください。

## データ取得 {#ingestion}

Adobe Experience Platform には、あらゆる種類のデータやデータのレイテンシを取り込むための豊富な機能が用意されています。Adobe Experience Platform [!DNL Data Ingestion] では、Batch API、Streaming API、ネイティブAdobeコネクタ、データ統合パートナー、Adobe Experience Platform UI など、データを取り込むための複数の方法を提供します。

**新機能**

| 機能 | 説明 |
|------- | -----------|
| バッチの部分取り込み | バッチの部分取り込みは、エラーを含むデータを特定のしきい値まで取り込む機能です。この機能を使用すると、ユーザーは正しいデータをすべて Adobe Experience Platform に取り込める一方で、不正なデータはすべて別々にバッチ処理できます。失敗したバッチには、検証に合格しなかった理由を示す詳細が追加されます。バッチの部分取り込みについて詳しくは、[バッチの部分取り込みに関するドキュメント](../../ingestion/batch-ingestion/partial.md)を参照してください。 |

**既知の問題**

* なし

データを Platform に取り込む方法については、[データ取得に関するドキュメント](../../ingestion/home.md)を参照してください。


## 宛先 {#destinations}

In [Real-time Customer Data Platform](../../rtcdp/overview.md)の宛先は、宛先プラットフォームとの事前に構築された統合であり、これらのパートナーに対するデータをシームレスにアクティブ化します。

**新しい宛先**

新しい宛先は、Adobe Experience Platform データをアクティブ化する際に使用できます。詳しくは、以下を参照してください。

| 宛先 | 説明 |
|--- | ---|
| クラウドストレージの宛先 | リアルタイム CDP は、セグメントをデータファイルとして [!DNL Amazon S3] または SFTP クラウドのストレージの場所。 これにより、オーディエンスとそのプロファイル属性を CSV またはタブ区切りファイル経由で内部システムに送信できます。 |
| 広告の宛先 | この [!DNL Google] 宛先カードは、3 つの異なる [!DNL Google] 現在、リアルタイム CDP でサポートされているプラットフォーム： [!DNL Google Ads], [!DNL Google Ad Manager], [!DNL Google] Display &amp; Video 360。 |

詳しくは、[宛先の概要](../../destinations/home.md)を参照してください。

## [!DNL Identity Service] {#identity}

関連性のあるデジタルエクスペリエンスを提供するには、顧客を完全に理解しておく必要があります。顧客データが異なる複数のシステムに断片化されており、そのため各顧客が複数の「ID」を持つと考えられる場合、顧客を理解するのはさらに困難になります。

Adobe Experience Platform [!DNL Identity Service]を使用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動をわかりやすく表示できます。これによって、インパクトのある個人的なデジタル体験をリアルタイムで提供できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 強化されたプライベートグラフ | プライベートグラフ機能が強化され、週別のバッチ処理から日別の更新済みグラフにグラフ生成の待ち時間が短縮され、 [!DNL Identity Service] をご利用のお客様が最新の id グラフおよびリンクにアクセスできるようになりました。 |

**既知の問題**

* なし

詳しくは、 [!DNL Identity Service]を参照し、 [ID サービスの概要](../../identity-service/home.md).

## ソース {#sources}

Adobe Experience Platformで外部ソースからデータを取り込みながら、を使用してデータの構造化、ラベル付け、拡張をおこなうことができます。 [!DNL Platform] サービス。 アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| Adobe Audience Manager コネクタの廃止された信号 | Audience Manager から信号レベルのデータは送信されなくなります。特性とセグメントのセグメントメンバーシップは、引き続き送信されることに注意してください。この変更の結果、受信データセットは生成されなくなります。 |
| データセット名の変更 | Audience Manager コネクタで生成されたデータセットの名前と説明が更新されます。 |
| 有効にする [!DNL Profile] Audience Manager で切り替え | [!DNL Profile] 切り替えは、有効または無効にして、データセットを次に昇格させることができます。 [!DNL Real-time Customer Profile]. デフォルトでは、切り替えが有効になっています。 |
| クラウドストレージシステムの UI のサポート | 用の新しいソースコネクタ [!DNL Azure Data Lake Storage Gen2] を使用します。 |
| CRM システムの UI のサポート | 用の新しいソースコネクタ [!DNL HubSpot], [!DNL Salesforce Service Cloud]、および [!DNL ServiceNow] を使用します。 |
| データベースシステムの UI のサポート | 用の新しいソースコネクタ [!DNL AWS Redshift], [!DNL Google BigQuery], [!DNL MariaDB], [!DNL Microsoft SQL Server]、および [!DNL MySQL] を使用します。 |

**既知の問題**

* なし

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
