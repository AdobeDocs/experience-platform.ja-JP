---
title: Adobe Experience Platform リリースノート（2020年3月）
description: Adobe Experience Platform の 2020年3月のリリースノート。
doc-type: release notes
last-update: March 10, 2020
author: ens71067
keywords: リリースノート；
exl-id: 407c2bac-4c8a-4939-b3dd-788250f15650
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '859'
ht-degree: 61%

---

# Adobe Experience Platform リリースノート

**リリース日：2020年3月11日**

Adobe Experience Platform の既存の機能に対するアップデート：

* [データガバナンス](#governance)
* [[!DNL Data Ingestion]](#ingestion)
* [[!DNL Destinations]](#destinations)
* [[!DNL Identity Service]](#identity)
* [[!DNL Sources]](#sources)

## データガバナンス {#governance}

[!DNL Experience Platform] を使用すると、企業は複数の企業システムのデータを統合して、マーケターが顧客を識別かつ理解し、惹きつけられるようになります。 [!DNL Experience Platform] には、システム内およびシステム間で共有される際にデータが適切に使用されるように、エンドツーエンドのデータガバナンスインフラストラクチャが含まれて [!DNL Experience Platform] ます。

Adobe Experience Platform データガバナンスは、顧客データを管理し、データの使用に適用される規制、制限、ポリシーへの準拠を確保するために使用される一連の戦略とテクノロジーです。これは、[!DNL Experience Platform] において様々なレベルで重要な役割を果たします。例えば、カタログ作成、データ系列、データ使用のラベル付け、データアクセスポリシー、マーケティングアクションのデータに関するアクセス制御などです。

**新機能**

>[!NOTE]
>
>次の新機能の一部は現在ベータ版で、すべてのユーザーが利用できるわけではありません。 ベータ版の機能は変更されることがあります。

| 機能 | 説明 |
| ------- | ----------- |
| [!DNL Real-Time Customer Data Platform] のデータ使用ポリシーの自動適用 | 宛先に対してデータをアクティブ化するワークフローにデータ使用ポリシーが適用されるようになりました。また、データガバナンスは、既存のアクティベーションに影響を与える変更（データセットラベル、結合ポリシー、セグメント定義の変更など）をおこなう場合にも、組み込まれて適用されます。 |
| データリネージの適用 | Real-Time CDPでデータ使用ポリシーに違反すると、UI にデータ系列情報を含む通知が表示されます。この通知は、ポリシー違反の理由と違反解決に役立ちます。 |


**既知の問題**

* なし

データガバナンスについて詳しくは、「[データガバナンスの概要](../../data-governance/home.md)」を参照してください。

## データ取得 {#ingestion}

Adobe Experience Platform には、あらゆる種類のデータやデータのレイテンシを取り込むための豊富な機能が用意されています。Adobe Experience Platform [!DNL Data Ingestion] は、データ取り込みに複数の代替手段を提供します。これには、バッチ API、ストリーミング API、ネイティブのAdobe コネクタ、データ統合パートナー、Adobe Experience Platform UI などがあります。

**新機能**

| 機能 | 説明 |
|------- | -----------|
| バッチの部分取り込み | バッチの部分取り込みは、エラーを含むデータを特定のしきい値まで取り込む機能です。この機能を使用すると、ユーザーは正しいデータをすべて Adobe Experience Platform に取り込める一方で、不正なデータはすべて別々にバッチ処理できます。失敗したバッチには、検証に合格しなかった理由を示す詳細が追加されます。バッチの部分取り込みについて詳しくは、[バッチの部分取り込みに関するドキュメント](../../ingestion/batch-ingestion/partial.md)を参照してください。 |

**既知の問題**

* なし

データをExperience Platformに取り込む方法については、[&#x200B; データ取得に関するドキュメント &#x200B;](../../ingestion/home.md) を参照してください。


## 宛先 {#destinations}

[Real-Time Customer Data Platform](../../rtcdp/overview.md) では、宛先は、宛先プラットフォームとの事前定義済みの統合であり、シームレスな方法でこれらのパートナーにデータをアクティブ化します。

**新しい宛先**

新しい宛先は、Adobe Experience Platform データをアクティブ化する際に使用できます。詳しくは、以下を参照してください。

| 宛先 | 説明 |
|--- | ---|
| クラウドストレージの宛先 | Real-Time CDPは、セグメントをデータファイルとして [!DNL Amazon S3] または SFTP クラウドストレージの場所に配信できるようになりました。 これにより、オーディエンスとそのプロファイル属性を CSV またはタブ区切りファイル経由で内部システムに送信できます。 |
| 広告の宛先 | [!DNL Google] の宛先カードは、現在Real-Time CDPでサポートされている 3 つの異なる [!DNL Google] プラットフォーム（[!DNL Google Ads]、[!DNL Google Ad Manager]、[!DNL Google] Display &amp; Video 360）に対して、3 つの宛先カードに分割されるようになりました。 |

詳しくは、[宛先の概要](../../destinations/home.md)を参照してください。

## [!DNL Identity Service] {#identity}

適切なデジタルエクスペリエンスを提供するには、顧客を完全に理解する必要があります。顧客データが異なる複数のシステムに断片化されており、そのため各顧客が複数の「ID」を持つと考えられる場合、顧客を理解するのはさらに困難になります。

Adobe Experience Platform [!DNL Identity Service]を使用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動をわかりやすく表示できます。これによって、インパクトのある個人的なデジタル体験をリアルタイムで提供できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 強化されたプライベートグラフ | プライベートグラフの機能が強化され、毎週のバッチプロセスから毎日更新されるグラフに至るまで、グラフ生成の待ち時間が短縮されました。これにより、[!DNL Identity Service] 顧客は、より新しい最新の ID グラフとリンクにアクセスできるようになりました。 |

**既知の問題**

* なし

[!DNL Identity Service] について詳しくは、「[ID サービスの概要 &#x200B;](../../identity-service/home.md)」を参照してください。

## ソース {#sources}

Adobe Experience Platformでは、外部ソースからデータを取り込むときに、[!DNL Experience Platform] サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を提供します。 これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| Adobe Audience Manager コネクタの廃止された信号 | Audience Manager から信号レベルのデータは送信されなくなります。特性とセグメントのセグメントメンバーシップは、引き続き送信されることに注意してください。この変更の結果、受信データセットは生成されなくなります。 |
| データセット名の変更 | Audience Manager コネクタで生成されたデータセットの名前と説明が更新されます。 |
| Audience Manger[!DNL Profile] 有効にする切り替え | デ [!DNL Profile] タセットを [!DNL Real-Time Customer Profile] に昇格させるには、切替スイッチを有効または無効にできます。 デフォルトでは、切り替えが有効になっています。 |
| クラウドストレージシステムの UI のサポート | UI の [!DNL Azure Data Lake Storage Gen2] の新しいソースコネクタ。 |
| CRM システムの UI のサポート | UI の [!DNL HubSpot]、[!DNL Salesforce Service Cloud] および [!DNL ServiceNow] の新しいソースコネクタ。 |
| データベースシステムの UI のサポート | UI の [!DNL AWS Redshift]、[!DNL Google BigQuery]、[!DNL MariaDB]、[!DNL Microsoft SQL Server] および [!DNL MySQL] 用の新しいソースコネクタ。 |

**既知の問題**

* なし

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
