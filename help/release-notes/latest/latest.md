---
title: Adobe Experience Platform リリースノート
description: 2021年2月25日Experience Platformリリースノート
doc-type: release notes
last-update: February 24, 2021
author: ens70167
translation-type: tm+mt
source-git-commit: d3151d9ccaf61e6ec1a88e147f3694292e953de0
workflow-type: tm+mt
source-wordcount: '770'
ht-degree: 36%

---


# Adobe Experience Platform リリースノート

**リリース日：2021 年 2 月 24 日**

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Data Science Workspace]](#dsw)
- [[!DNL Dataflows]](#dataflows)
- [[!DNL Experience Data Model (XDM) System]](#xdm)
- [[!DNL Identity Service]](#identity)
- [[!DNL Sources]](#sources)

## [!DNL Data Science Workspace] {#dsw}

Data Science Workspaceは、機械学習と人工知能を使用して、データからインサイトを作成します。 Adobe Experience Platform に統合された Data Science Workspace は、アドビソリューションでコンテンツやデータアセットを使用して予測をおこなうことを支援します。

**新機能**

| 機能 | 説明 |
| --- | --- |
| JupyterLab EDAノートブック | 探索的データ分析(EDA)PythonノートブックがJupyterlabで入手できるようになりました。 このノートブックは、データのパターンの検出、データの正確性のチェック、および予測モデルに関する関連データの要約を行うのに役立つように設計されています。 詳しくは、[予測モデルに関するWebベースのデータの調査](../../data-science-workspace/jupyterlab/eda-notebook.md)のチュートリアルを参照してください。 |

Data Science Workspaceの一般的な情報については、[データサイエンスワークスペースの概要](../../data-science-workspace/home.md)を参照してください。

## [!DNL Dataflows] {#dataflows}

Adobe Experience Platformでは、様々なソースからデータを取り込み、Experience Platform内で分析し、様々な目的地に活性化する。 プラットフォームでは、データフローに透明性を提供することで、この非線形の可能性があるデータフローの追跡プロセスを容易にします。

データフローは、Platform 間でデータを移動するデータジョブを表します。これらのデータフローは様々なサービスで構成され、ソースコネクタからターゲットデータセットにデータを移動できます。その後、[!DNL Identity Service]と[!DNL Real-time Customer Profile]がデータを利用してから、最終的に[!DNL Destinations]にアクティブ化します。

**新機能**

| 機能 | 説明 |
| --- | --- |
| 新しい監視ダッシュボード | ソースデータインジェクションに対して、クロスサービスの透過性と実行可能なインサイトの監視ダッシュボードを使用できるようになりました。 新しい監視ダッシュボードは、[!DNL Data Lake]から[!DNL Identity Service]、[!DNL Profile]に処理されるデータの包括的な表示を提供します。また、取り込み率、成功数、失敗数を監視することもできます。 詳細については、UI](../../dataflows/ui/monitor-sources.md)の[監視ソースのデータフローのチュートリアルを参照してください。 |

データフローの一般的な情報については、[データフローの概要](../../dataflows/home.md)を参照してください。

## [!DNL Experience Data Model (XDM) System] {#xdm}

標準化と相互運用性は、[!DNL Experience Platform]の背後にある重要な概念です。 [!DNL Experience Data Model] (XDM)は、Adobeに基づいて、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

XDM はパブリックに文書化された仕様であり、デジタルエクスペリエンスのパワーを向上させるために設計されています。Adobe Experience Platform 上のサービスと通信するすべてのアプリケーションに共通の構造と定義を提供します。XDM 標準規格に準拠することで、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| アップグレードされた検索UI | [!UICONTROL スキーマ]ワークスペースの[!UICONTROL 参照]タブと[!DNL Schema Editor]のミックスイン選択ダイアログで、検索機能が強化されました。<br><br>以前にキーワードを検索した場合、検索クエリと名前が一致するXDMリソースのみが結果に含まれます。現在は、クエリと名前が一致するリソースに加えて、キーワードに一致する個々の属性を含むリソースも含まれます。 これにより、XDMリソースを、リソース名ではなく、属性を含むXDMリソースに基づいて検索できます。<br><br>詳細は、XDM [リソースの](../../xdm/ui/explore.md) 調査とUIでのスキーマの [](../../xdm/ui/resources/schemas.md) 管理に関するドキュメントを参照してください。 |

XDMの一般的な情報については、[XDMシステム概要](../../xdm/home.md)を参照してください。

## [!DNL Identity Service] {#identity}

関連するデジタルエクスペリエンスを提供するには、顧客を完全に理解する必要があります。顧客データが異なる複数のシステムに断片化されており、そのため各顧客が複数の「ID」を持つと考えられる場合、顧客を理解するのはさらに困難になります。

Adobe Experience Platform[!DNL Identity Service]は、デバイスやシステム間でIDをつなぐことで、顧客とその行動をより良く表示できるように支援します。これにより、インパクトの高い個人的なデジタルエクスペリエンスをリアルタイムで提供できます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| ID グラフ viewer | IDグラフビューアでは、UIで繋ぎ合わされたIDを検証および視覚化でき、デバッグと透明度を向上できます。 詳しくは、[IDグラフビューアのドキュメント](../../identity-service/ui/identity-graph-viewer.md)を参照してください。 |

[!DNL Identity Service]の一般的な情報については、[IDサービスの概要](../../identity-service/home.md)を参照してください。

## [!DNL Sources] {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新しいソース**

| 機能 | 説明 |
| --- | --- |
| [!DNL Google PubSub] | [!DNL Flow Service] APIまたはUIを使用して、[!DNL Google PubSub]を[!DNL Experience Platform]に接続できるようになりました。 詳しくは、[[!DNL Google PubSub] コネクタの概要](../../sources/connectors/cloud-storage/google-pubsub.md)を参照してください。 |
| [!DNL Oracle Object Storage] | [!DNL Flow Service] APIまたはUIを使用して、[!DNL Oracle Object Storage]を[!DNL Experience Platform]に接続できるようになりました。 詳しくは、[[!DNL Oracle Object Storage] コネクタの概要](../../sources/connectors/cloud-storage/oracle-object-storage.md)を参照してください。 |

ソースの一般的な情報については、[ソースの概要](../../sources/home.md)を参照してください。
