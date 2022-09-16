---
title: Adobe Experience Platformリリースノート 2021 年 2 月
description: Adobe Experience Platformの 2021 年 2 月のリリースノート。
doc-type: release notes
last-update: February 24, 2021
author: ens70167
exl-id: 8c3142af-4021-4f7e-acbd-c5277dd188d1
source-git-commit: ce967ae176fce81aa26d92b3f0ee8be006808657
workflow-type: tm+mt
source-wordcount: '1143'
ht-degree: 98%

---

# Adobe Experience Platform リリースノート

**リリース日：2021 年 2 月 24 日（PT）**

Adobe Experience Platform の新機能：

- [（ベータ版）ダッシュボード ](#dashboards)

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Data Science Workspace]](#dsw)
- [[!DNL Dataflows]](#dataflows)
- [[!DNL Destinations]](#destinations)
- [[!DNL Experience Data Model (XDM) System]](#xdm)
- [[!DNL Identity Service]](#identity)
- [[!DNL Real-time Customer Profile]](#profile)
- [[!DNL Sources]](#sources)

## （ベータ版）ダッシュボード  {#dashboards}

Adobe Experience Platform は、毎日のスナップショットで取得した、組織のデータに関する重要な情報を表示できる複数のダッシュボードを提供しています。

**新機能**

| 機能 | 説明 |
| --- | --- |
| プロファイル、セグメント、宛先、およびライセンス使用状況ダッシュボード（ベータ版） | **注意：ダッシュボードの機能は現在ベータ版のため、すべてのユーザーが利用できるわけではありません。ドキュメントと機能は変更される場合があります。**<br/><br/>&#x200B;ダッシュボードは、組織のデータに対してすぐに使用できるレポートを提供し、Platform 内のマーケターワークフローに直接組み込まれます。これらのダッシュボードは、IT サポートの追加や、データウェアハウスの設計と実装を追加してデータをエクスポートおよび処理する時間と労力の必要なしに利用できます。 |

## [!DNL Data Science Workspace] {#dsw}

Data Science Workspace は、機械学習と人工知能を使用して、データからインサイトを作成します。Adobe Experience Platform に統合された Data Science Workspace は、アドビソリューションでコンテンツやデータアセットを使用して予測をおこなうことを支援します。

**新機能**

| 機能 | 説明 |
| --- | --- |
| JupyterLab EDA ノートブック | 探索的データ解析（EDA）Python ノートブックを Jupyterlab で利用できるようになりました。このノートブックは、データのパターンの検出、データサニティのチェック、および予測モデルの関連データの要約に役立つように設計されています。詳しくは、[予測モデルに関する Web ベースのデータ調査](../../data-science-workspace/jupyterlab/eda-notebook.md)のチュートリアルを参照してください。 |

Data Science Workspace の一般的な情報については、[Data Science Workspace の概要](../../data-science-workspace/home.md)を参照してください。

## [!DNL Dataflows] {#dataflows}

Adobe Experience Platform では、様々なソースからデータを取り込み、Experience Platform 内で分析し、様々な目的でアクティブ化します。Platform では、データフローに透明性を提供することで、この非線形の可能性があるデータフローのトラッキングプロセスを容易にします。

データフローは、Platform 間でデータを移動するデータジョブを表します。これらのデータフローは様々なサービスで構成され、ソースコネクタからターゲットデータセットにデータを移動できます。その後、[!DNL Identity Service] と [!DNL Real-time Customer Profile] がデータを利用してから、最終的に [!DNL Destinations] に対してアクティブ化します。

**新機能**

| 機能 | 説明 |
| --- | --- |
| 新しい監視ダッシュボード | ソースデータの取り込みに対して、サービス間の透過性とアクションにつながるインサイトを備える監視ダッシュボードを使用できるようになりました。新しい監視ダッシュボードは、[!DNL Data Lake] から [!DNL Identity Service] および [!DNL Profile] に処理されるデータの包括的な表示を提供します。また、取り込み率、成功数、失敗数を監視することもできます。詳細については、[UI でのソースデータフローの監視](../../dataflows/ui/monitor-sources.md)に関するチュートリアルを参照してください。 |

データフローの一般的な情報については、[データフローの概要](../../dataflows/home.md)を参照してください。

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新しい宛先**

| 宛先 | 説明 |
| ----------- | ----------- |
| [[!DNL LinkedIn Matched Audiences]](../../destinations/catalog/social/linkedin.md) | [!DNL LinkedIn Matched Audiences] 接続を使用すると、[!DNL LinkedIn] ソーシャルプラットフォームでオーディエンスをアクティブ化できます。 |

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## [!DNL Experience Data Model (XDM) System] {#xdm}

標準化と相互運用性は、[!DNL Experience Platform] の背後にある重要な概念です。アドビが推進する [!DNL Experience Data Model]（XDM）は、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

XDM はパブリックに文書化された仕様であり、デジタルエクスペリエンスのパワーを向上させるために設計されています。Adobe Experience Platform 上のサービスと通信するすべてのアプリケーションに共通の構造と定義を提供します。XDM 標準規格に準拠することで、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| 検索 UI のアップグレード | [!UICONTROL スキーマ]ワークスペースの「[!UICONTROL 参照]」タブと、[!DNL Schema Editor] のスキーマフィールドグループ選択ダイアログで、検索機能が強化されました。<br><br>キーワードを検索した場合、検索クエリと名前が一致する XDM リソースのみが結果に含まれます。現在は、クエリと名前が一致するリソースに加えて、キーワードに一致する個々の属性を含むリソースも含まれます。これにより、リソース名ではなく、属性に基づいて XDM リソースを検索できます。<br><br>詳細は、[XDM リソースの調査](../../xdm/ui/explore.md)と[ UI でのスキーマの管理](../../xdm/ui/resources/schemas.md)に関するドキュメントを参照してください。 |

XDM の一般的な情報について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## [!DNL Identity Service] {#identity}

関連性のあるデジタルエクスペリエンスを提供するには、顧客を完全に理解しておく必要があります。顧客データが異なる複数のシステムに断片化されており、そのため各顧客が複数の「ID」を持つと考えられる場合、顧客を理解するのはさらに困難になります。

Adobe Experience Platform [!DNL Identity Service]を使用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動をわかりやすく表示できます。これによって、インパクトのある個人的なデジタル体験をリアルタイムで提供できます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| ID グラフビューア | ID グラフビューアでは、UI でステッチされ ID を検証および視覚化し、デバッグ精度と透過性を向上できます。詳しくは、[ID グラフビューアのドキュメント](../../identity-service/ui/identity-graph-viewer.md)を参照してください。 |

[!DNL Identity Service] の一般的な情報について詳しくは、[ID サービスの概要](../../identity-service/home.md)を参照してください。

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、いつでもどこでもブランドとのやり取りが顧客に対して調整され、一貫性と関連性のあるエクスペリエンスを提供できます。リアルタイム顧客プロファイルでは、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせた、各顧客の全体像を確認できます。[!DNL Profile] を使用すると、個別の顧客データを統合ビューに取り込み、顧客インタラクションごとにアクションにつながる、タイムスタンプ付きのアカウントを提供できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 計算済み属性（アルファ版） | ***注意：この機能は現在アルファ版であり、一部のユーザーのみが使用できます。ドキュメントと機能は変更される場合があります。*** <br/><br/>計算済み属性は、イベントレベルのデータをプロファイルレベルの属性に集計するために使用される関数です。その後、集計をセグメント化、有効化、パーソナライズ機能に使用できます。これらの関数の例としては、count、sum、average、min、max、true/false があります。計算済み属性は、現在 API でのみ使用できます。詳しくは、[計算済み属性の概要](../../profile/computed-attributes/overview.md)を参照してください。 |

[!DNL Profile] データを操作するためのチュートリアルやベストプラクティスなど、リアルタイム顧客プロファイルの詳細については、[リアルタイム顧客プロファイルの概要](../../profile/home.md)を参照してください。

## [!DNL Sources] {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新しいソース**

| 機能 | 説明 |
| --- | --- |
| [!DNL Google PubSub] | [!DNL Flow Service] API または UI を使用して、[!DNL Google PubSub] を [!DNL Experience Platform] に接続できるようになりました。詳しくは、[[!DNL Google PubSub] コネクタの概要](../../sources/connectors/cloud-storage/google-pubsub.md)を参照してください。 |
| [!DNL Oracle Object Storage] | [!DNL Flow Service] API または UI を使用して、[!DNL Oracle Object Storage] を [!DNL Experience Platform] に接続できるようになりました。詳しくは、[[!DNL Oracle Object Storage] コネクタの概要](../../sources/connectors/cloud-storage/oracle-object-storage.md)を参照してください。 |

ソースの一般的な情報については、[ソースの概要](../../sources/home.md)を参照してください。
