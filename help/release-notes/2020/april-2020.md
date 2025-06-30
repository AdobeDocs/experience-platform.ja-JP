---
title: Adobe Experience Platform リリースノート 2020年4月
description: Adobe Experience Platform の 2020年4月のリリースノート。
doc-type: release notes
last-update: April 13, 2020
author: ens71067
keywords: リリースノート；
exl-id: 0f68c71e-3c9d-453b-a953-1cd1b6ca2e35
source-git-commit: 104db777446b19fa9e3ea7538ae1dda6f51a00b1
workflow-type: tm+mt
source-wordcount: '972'
ht-degree: 58%

---

# Adobe Experience Platform リリースノート

**リリース日：2020年4月8日**

Adobe Experience Platform の新機能：
* [[!DNL Intelligent Services]](#intelligent)

既存の機能の更新：
* [[!DNL Experience Data Model (XDM)]](#xdm)
* [データガバナンス](#governance)
* [[!DNL Destinations]](#destinations)
* [[!DNL Privacy Service]](#privacy)
* [[!DNL Sources]](#sources)

## [!DNL Intelligent Services] {#intelligent}

[!DNL Intelligent Services] は、マーケティングアナリストや実務担当者に対して、顧客体験のユースケースで人工知能と機械学習の機能を活用する機能を提供します。 これにより、マーケティングアナリストは、データ科学の専門知識を必要とせずに、ビジネスレベルの設定を使用して、会社のニーズに固有の予測を設定できます。さらに、マーケターは、Adobe Experience Cloud、Adobe Experience Platform およびサードパーティアプリケーションで予測をアクティブ化できます。

**主な特長**

| 機能 | 説明 |
|---|---|
| [!DNL Customer AI] | マーケターは、[!DNL Customer AI] を使用して、顧客予測を個人レベルで説明とともに生成することができます。 影響力のある要因の助けを借 [!DNL Customer AI] て、顧客が何をする可能性があるかとその理由を知ることができます。 さらに、マーケターは、[!DNL Customer AI] の予測とインサイトを活用して、最も適切なオファーとメッセージを提供することで顧客体験をパーソナライズできます。 |
| [!DNL Attribution AI] | [!DNL Attribution AI] は、顧客とのやり取りの影響と増分的な影響を指定した成果に照らして計算する、マルチチャネルのアルゴリズムアトリビューションサービスです。 [!DNL Attribution AI] を使用すると、マーケターは、カスタマージャーニーの各段階における個々の顧客インタラクションの影響を把握することで、マーケティング費用と広告費用を測定し、最適化できます。 |

**既知の問題**

* 現在、既知の問題はありません。

[!DNL Intelligent Services] とその機能について詳しくは、[ インテリジェントサービスの概要 ](../../intelligent-services/home.md) を参照してください。

## [!DNL Experience Data Model]（XDM）システム {#xdm}

標準化と相互運用性は、[!DNL Experience Platform] の背後にある重要な概念です。アドビが推進する [!DNL Experience Data Model]（XDM）は、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

XDM はパブリックに文書化された仕様であり、デジタルエクスペリエンスのパワーを向上させるために設計されています。Adobe Experience Platform 上のサービスと通信するすべてのアプリケーションに共通の構造と定義を提供します。XDM 標準規格に準拠することで、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| 代替表示情報の自動化 | [!DNL Schema Registry] は、`alternateDisplayInfo` 記述子で設定されたカスタマイズされたタイトルと説明の値を自動的に適用します。 |
| スカラーフィールドの制限 | [!DNL Schema Registry] では、1 つのスキーマに 6000 を超えるスカラーフィールドを使用することはできません。 |
| パフォーマンスの見直し | [!DNL Schema Registry] は、パフォーマンスが向上し、より良い [!DNL Experience Platform] の要求を満たすために見直されました。 |

**バグの修正**

* 標準の XDM の入れ子 URI フィールドで、より簡潔な XED 形式をサポートするように、XDM を XED に変換しました。

**既知の問題**

* 既知

## データガバナンス {#governance}

Adobe Experience Platform データガバナンスは、顧客データを管理し、データの使用に適用される規制、制限、ポリシーへの準拠を確保するために使用される一連の戦略とテクノロジーです。これは、[!DNL Experience Platform] において様々なレベルで重要な役割を果たします。例えば、カタログ作成、データ系列、データ使用のラベル付け、データアクセスポリシー、マーケティングアクションのデータに関するアクセス制御などです。

データガバナンスを開始するには、お客様のデータに適用される規制、契約上の義務、および企業ポリシーを十分に理解する必要があります。ここから、適切なデータ使用ラベルを適用してデータを分類し、その使用をデータ使用ポリシーの定義を通じて制御できます。

データガバナンスフレームワークは、[!DNL Experience Platform] ユーザーインターフェイスと [!DNL Policy Service] API を通じて、データの分類プロセスや、データ使用ポリシーの作成プロセスを簡素化および効率化します。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| UI でのデータ使用ポリシーの管理 | データ使用ポリシーを、[!DNL Experience Platform] UI の **ポリシー** ワークスペース内で管理できるようになりました。 詳しくは、『[ポリシーユーザガイド](../../data-governance/policies/user-guide.md)』を参照してください。 |

**既知の問題**

* なし。

詳しくは、「[データガバナンスの概要](../../data-governance/home.md)」を参照してください。


## 宛先 {#destinations}

[Real-Time Customer Data Platform](../../rtcdp/overview.md) では、宛先は、宛先プラットフォームとの事前定義済みの統合であり、シームレスな方法でこれらのパートナーにデータをアクティブ化します。

**新しい宛先**

Real-Time CDPでは、50 を超える拡張機能へのデータのアクティベーションをサポートし、分析、パーソナライゼーション、その他のユースケースを可能に [!DNL Experience Cloud Launch] るようになりました。 詳しくは、以下を参照してください。

| ドキュメント | 説明 |
|--- | ---|
| [宛先のタイプとカテゴリ](../../destinations/destination-types.md) | この記事では、Real-Time CDP インターフェイスの接続と拡張機能の違いを説明し、それぞれの宛先を使用するタイミングを推奨します。 |
| [Experience Platform Launch 拡張機能](../../destinations/catalog/launch-extensions/overview.md) | ここでは、[!DNL Launch] 拡張機能の概要、使用する際のユースケースおよびReal-Time CDPの各 [!DNL Launch] 拡張機能に関するドキュメントへのリンクを示します。 |

詳しくは、『[宛先の概要](../../destinations/home.md)』を参照してください。

## [!DNL Privacy Service] {#privacy}

新しい法規制や組織の規制により、ユーザーはリクエストによってデータストアから個人データにアクセスしたり削除したりする権利を与えられています。Adobe Experience Platform [!DNL Privacy Service] は、お客様からのこれらのデータリクエストを管理するのに役立つ RESTful API とユーザーインターフェイスを提供します。 [!DNL Privacy Service] を使用すると、Adobe Experience Cloud アプリケーションから非公開または個人的な顧客データに対するアクセスおよび削除のリクエストを送信でき、法的および組織のプライバシー規制への自動コンプライアンスが容易になります。

**新機能**

| 機能 | 説明 |
| --- | --- |
| PDPA のサポート | タイの個人データ保護法（PDPA）に基づいて、プライバシーリクエストを作成し、トラックできるようになりました。API でプライバシーリクエストを実行する場合、`regulation` 配列は「pdpa_tha」の値を受け取ります。 |
| UI の名前空間タイプ | [!DNL Privacy Service] UI のリクエストビルダーで、様々な名前空間タイプを指定できるようになりました。詳しくは、[ユーザガイド](../../privacy-service/ui/user-guide.md)を参照してください。 |
| 古いエンドポイントの廃止 | 古い API エンドポイント（`data/privacy/gdpr`）は廃止されました。 |

既知の問題

* なし

[!DNL Privacy Service] の詳細については、[Privacy Serviceの概要 ](../../privacy-service/home.md) を読んで開始してください。

## ソース {#sources}

Adobe Experience Platformでは、外部ソースからデータを取り込むときに、[!DNL Experience Platform] サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。 アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を提供します。 これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| データベースの API と UI のサポート | （HDInsights 上の） [!DNL Apache Spark]、[!DNL Azure Synapse Analytics]、[!DNL Azure Table Storage] および [!DNL Hive] 用の新しいソースコネクタ。 |
| プロトコルベースのアプリケーションの API と UI のサポート | [!DNL Generic OData] の新しいソースコネクタ。 |

**既知の問題**

* なし

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
