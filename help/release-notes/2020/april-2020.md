---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2020年4月9日）
doc-type: release notes
last-update: April 13, 2020
author: ens71067
keywords: release notes;
translation-type: tm+mt
source-git-commit: f881c1365684b1ca9e6bf9a8ce866d234dc54128
workflow-type: tm+mt
source-wordcount: '975'
ht-degree: 5%

---


# Adobe Experience Platform リリースノート

**リリース日：2020 年 4 月 8 日**

Adobe Experience Platformの新機能：
* [!DNL Intelligent Services](#intelligent)

既存の機能の更新：
* [!DNL Experience Data Model (XDM)](#xdm)
* [!DNL Data Governance](#governance)
* [!DNL Destinations](#destinations)
* [!DNL Privacy Service](#privacy)
* [!DNL Sources](#sources)

## [!DNL Intelligent Services] {#intelligent}

[!DNL Intelligent Services] マーケティングアナリストや専門家が、顧客体験の使用事例で人工知能と機械学習の能力を活用できるように支援します。 これにより、マーケティングアナリストは、データ科学の専門知識を必要とせずに、ビジネスレベルの設定を使用して、会社のニーズに固有の予測を設定できます。 また、マーケティングの専門家は、Adobe Experience Cloud、Adobe Experience Platform、サードパーティの各アプリケーションで、予測をアクティブにすることができます。

**主な特長**

| 機能 | 説明 |
|---|---|
| [!DNL Customer AI] | [!DNL Customer AI] マーケターに、説明を加えながら個々のレベルで顧客予測を行う力を提供します。 影響力のある要因の支援を受け [!DNL Customer AI] て、顧客が何を行う可能性があるかと、その理由を把握できます。 さらに、マーケターは、 [!DNL Customer AI] 予測とインサイトからメリットを得て、最も適切なオファーとメッセージを提供することで顧客の体験をパーソナライズできます。 |
| [!DNL Attribution AI] | [!DNL Attribution AI] は、特定の結果に対する顧客のインタラクションの影響と増分的な影響を計算する、複数チャネルのアルゴリズムアトリビューションサービスです。 マーケター [!DNL Attribution AI]は、顧客の遍歴の各段階における個々の顧客のインタラクションの影響を把握することで、マーケティング費用と広告費用を測定し、最適化できます。 |

**既知の問題**

* 現在、既知の問題はありません。

オファーに関する詳細 [!DNL Intelligent Services] とその内容については、 [インテリジェントサービスの概要](../../intelligent-services/home.md)を参照してください。

## [!DNL Experience Data Model] (XDM)システム {#xdm}

標準化と相互運用性は、重要な概念 [!DNL Experience Platform]です。 [!DNL Experience Data Model] (XDM)は、アドビが推進する機能で、カスタマーエクスペリエンスデータを標準化し、カスタマーエクスペリエンス管理のスキーマを定義するための取り組みです。

XDMは、デジタルエクスペリエンスのパワーを向上させるために設計された、公開された仕様です。 Adobe Experience Platform上のサービスと通信するアプリケーションの共通の構造と定義を提供します。 XDM標準を守ることで、すべての顧客体験データを共通の表現に組み込むことができ、より迅速で統合的な方法でインサイトを提供できます。 顧客のアクションから貴重なインサイトを得たり、セグメントを通して顧客オーディエンスを定義したり、顧客属性を使用してパーソナライズを図ることができます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| 代替表示情報の自動表示 | は、記述子に設定されたカスタマイズされたタイトルと説明の値を [!DNL Schema Registry] 自動的に適用し `alternateDisplayInfo` ます。 |
| スカラーフィールドの制限 | 1つのスキーマ [!DNL Schema Registry] に6000個を超えるスカラーフィールドは使用できません。 |
| パフォーマンスの概要 | こ [!DNL Schema Registry] れは、より良い要求を満たすために、行き過ぎにな [!DNL Experience Platform] った。 |

**バグの修正**

* 標準のXDMの入れ子URIフィールドに対して、よりクリーンなXED形式をサポートするように、XDMをXEDに変換しました。

**既知の問題**

* 既知

## [!DNL Data Governance] {#governance}

Adobe Experience Platform [!DNL Data Governance] は、お客様のデータを管理し、データの使用に適用される規制、制限、ポリシーの遵守を確保するために使用される一連の戦略とテクノロジーです。 カタログ化、データ系列、データ使用状況のラベル付け、データアクセスポリシー、マーケティング活動 [!DNL Experience Platform] のためのデータに関するアクセス制御など、様々なレベルで重要な役割を果たします。

データ・ガバナンスを使用するには、お客様のデータに適用される規制、契約上の義務、および企業ポリシーを十分に理解する必要があります。 ここから適切なデータ使用ラベルを適用してデータを分類し、その使用をデータ使用ポリシーの定義によって制御できます。

DULEフレームワークは、ユーザー・インターフェースとDULE [!DNL Experience Platform][!DNL Policy Service] APIを通じて、データの分類とデータ使用ポリシーの作成のプロセスを簡素化および合理化します。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| UIでのデータ使用ポリシーの管理 | データ使用ポリシーは、 _UIのポリシー_[!DNL Experience Platform] ワークスペース内で管理できるようになりました。 詳しくは、 [ポリシーユーザーガイド](../../data-governance/policies/user-guide.md) を参照してください。 |

**既知の問題**

* None.

詳しくは、「 [データ管理の概要](../../data-governance/home.md)」を参照してください。


## 宛先 {#destinations}

ア [ドビのリアルタイム顧客データPlatformでは](../../rtcdp/overview.md)、宛先は、目的のプラットフォームとの事前に構築された統合であり、これらのパートナーに対してシームレスにデータをアクティブ化します。

**新しい宛先**

Adobe Real-time CDPは、50を超える [!DNL Experience Cloud Launch] 拡張に対するデータアクティベーションをサポートし、解析、パーソナライゼーション、その他の使用例を可能にします。 詳しくは、以下を参照してください。

| ドキュメント | 説明 |
|--- | ---|
| [宛先のタイプとカテゴリ](/help/rtcdp/destinations/destination-types.md) | この記事では、Adobe Real-time CDPインターフェイスの接続と拡張の違いを説明し、これらの各宛先を使用する場合を推奨します。 |
| [Experience Platform Launch拡張](/help/rtcdp/destinations/experience-platform-launch-extensions.md) | このページでは、拡張機能の概要、 [!DNL Launch] リストが拡張機能を使用する場合の使用例、およびAdobe Real-time CDPの各拡張機能に関するドキュメントへのリンクを説明し [!DNL Launch] ます。 |

詳しくは、 [宛先の概要を参照してください](/help/rtcdp/destinations/destinations-overview.md)。

## [!DNL Privacy Service] {#privacy}

新しい法規制や組織規制により、ユーザーは要求に応じて、データストアから個人データにアクセスしたり削除したりする権利を持つようになります。 Adobe Experience Platform [!DNL Privacy Service] は、RESTful APIとユーザーインターフェイスを提供し、顧客からのこれらのデータリクエストを管理するのに役立ちます。 また、Adobe Experience Cloudアプリケーションから個人または個人の顧客データにアクセスする要求を送信したり、Adobe Experience Cloudアプリケーションから削除したりできるので、法律や組織のプライバシーに関する規制への準拠を自動化できます。 [!DNL Privacy Service]

**新機能**

| 機能 | 説明 |
| --- | --- |
| PDPAのサポート | タイのPersonal Data Protection Act(PDPA)で、プライバシー要求を作成し、追跡できるようになりました。 APIでプライバシーリクエストを行う場合、 `regulation` 配列は値「pdpa_tha」を受け入れます。 |
| UIの名前空間タイプ | UIのリクエストビルダーで異なる名前空間タイプを指定できるようになり [!DNL Privacy Service] ました。 詳しくは、 [ユーザガイド](../../privacy-service/ui/user-guide.md) を参照してください。 |
| 古いエンドポイントの廃止 | 古いAPIエンドポイント(`data/privacy/gdpr`)は非推奨となりました。 |

既知の問題

* None

詳細については、 [!DNL Privacy Service]Privacy Serviceの概要を読んで開始してください [](../../privacy-service/home.md)。

## ソース {#sources}

Adobe Experience Platformは、外部ソースからデータを取り込みながら、サー [!DNL Platform] ビスを使用してデータの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRMシステムなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] には、様々なデータプロバイダーのソース接続を簡単に設定できる、RESTful APIとインタラクティブUIが用意されています。 これらのソース接続を使用すると、外部のストレージシステムやCRMサービスの認証と接続、取り込みの実行時間の設定、データ取り込みスループットの管理を行うことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| データベース用のAPIとUIのサポート | （HDInsightsで）、 [!DNL Apache Spark] 、 [!DNL Azure Synapse Analytics]、（HDInsightsで）およびの新しいソースコネクタ [!DNL Azure Table Storage]ー [!DNL Hive][!DNL Phoenix]。 |
| 支払いベースのアプリケーションのAPIとUIのサポート | の新しいソースコネクタ [!DNL PayPal]。 |
| プロトコルベースのアプリケーションのAPIとUIのサポート | の新しいソースコネクタ [!DNL Generic OData]。 |

**既知の問題**

* None

ソースについて詳しくは、 [ソースの概要を参照してください](../../sources/home.md)。
