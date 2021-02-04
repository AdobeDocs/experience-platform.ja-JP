---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート（2020 年 4 月 9 日）
doc-type: release notes
last-update: April 13, 2020
author: ens71067
keywords: リリースノート;
translation-type: tm+mt
source-git-commit: adf8e8457c8ffef263223a38d3f9c345cf7c6ab2
workflow-type: tm+mt
source-wordcount: '971'
ht-degree: 58%

---


# Adobe Experience Platform リリースノート

**リリース日：2020 年 4 月 8 日**

Adobe Experience Platform の新機能：
* [[!DNL Intelligent Services]](#intelligent)

既存の機能の更新：
* [[!DNL Experience Data Model (XDM)]](#xdm)
* [[!DNL Data Governance]](#governance)
* [[!DNL Destinations]](#destinations)
* [[!DNL Privacy Service]](#privacy)
* [[!DNL Sources]](#sources)

## [!DNL Intelligent Services] {#intelligent}

[!DNL Intelligent Services] は、マーケティングアナリストや従事者に対して、顧客体験の使用例で人工知能とマシンラーニングの機能を活用する機能を提供します。これにより、マーケティングアナリストは、データ科学の専門知識を必要とせずに、ビジネスレベルの設定を使用して、会社のニーズに固有の予測を設定できます。さらに、マーケターは、Adobe Experience Cloud、Adobe Experience Platform およびサードパーティアプリケーションで予測をアクティブ化できます。

**主な特長**

| 機能 | 説明 |
|---|---|
| [!DNL Customer AI] | [!DNL Customer AI] マーケターに、説明を加えながら個々のレベルで顧客予測を行う力を提供します。[!DNL Customer AI]は、影響力のある要因の助けを借りて、顧客が何をする可能性があるかと、その理由を教えてくれます。 さらに、マーケターは、最も適切なオファーとメッセージを提供することで、[!DNL Customer AI]予測とインサイトを活用して顧客体験をパーソナライズできます。 |
| [!DNL Attribution AI] | [!DNL Attribution AI] は、指定した結果に対する顧客のインタラクションの影響と増分的な影響を計算する、複数チャネルのアルゴリズムによるアトリビューションサービスです。[!DNL Attribution AI] を使用すると、マーケターは、カスタマージャーニーの各段階における個々の顧客インタラクションの影響を把握することで、マーケティング費用と広告費用を測定し、最適化できます。 |

**既知の問題**

* 現在、既知の問題はありません。

[!DNL Intelligent Services]とそのオファーに関する詳細は、[インテリジェントサービスの概要](../../intelligent-services/home.md)を参照してください。

## [!DNL Experience Data Model] (XDM)システム  {#xdm}

標準化と相互運用性は、[!DNL Experience Platform]の背後にある重要な概念です。 [!DNL Experience Data Model] (XDM)は、Adobeに基づいて、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

XDM はパブリックに文書化された仕様であり、デジタルエクスペリエンスのパワーを向上させるために設計されています。Adobe Experience Platform 上のサービスと通信するすべてのアプリケーションに共通の構造と定義を提供します。XDM 標準規格に準拠することで、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| 代替表示情報の自動化 | [!DNL Schema Registry]は、`alternateDisplayInfo`記述子に設定されたカスタマイズされたタイトルと説明の値を自動的に適用します。 |
| スカラーフィールドの制限 | [!DNL Schema Registry]は、1つのスキーマに6000個を超えるスカラーフィールドを使用できません。 |
| パフォーマンスの見直し | [!DNL Schema Registry]は、[!DNL Experience Platform]の要求をより良く満たすように、行き過ぎた。 |

**バグの修正**

* 標準の XDM の入れ子 URI フィールドで、より簡潔な XED 形式をサポートするように、XDM を XED に変換しました。

**既知の問題**

* 既知

## [!DNL Data Governance] {#governance}

Adobe Experience Platform[!DNL Data Governance]は、お客様のデータを管理し、データの使用に適用される規制、制限、ポリシーの遵守を確保するために使用される一連の戦略とテクノロジーです。 [!DNL Experience Platform]内では、カタログ化、データ系列、データ使用状況のラベル付け、データアクセスポリシー、マーケティング活動のデータに関するアクセス制御など、様々なレベルで重要な役割を果たします。

データガバナンスを開始するには、お客様のデータに適用される規制、契約上の義務、および企業ポリシーを十分に理解する必要があります。ここから、適切なデータ使用ラベルを適用してデータを分類し、その使用をデータ使用ポリシーの定義を通じて制御できます。

[!DNL Data Governance]フレームワークは、[!DNL Experience Platform]ユーザーインターフェイスと[!DNL Policy Service] APIを通じて、データの分類とデータ使用ポリシーの作成のプロセスを簡素化および合理化します。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| UI でのデータ使用ポリシーの管理 | データ使用ポリシーは、 UI の&#x200B;**ポリシー**&#x200B;ワークスペース内で管理できるようになっています。[!DNL Experience Platform]詳しくは、『[ポリシーユーザガイド](../../data-governance/policies/user-guide.md)』を参照してください。 |

**既知の問題**

* なし。

詳しくは、「[データガバナンスの概要](../../data-governance/home.md)」を参照してください。


## 宛先 {#destinations}

[リアルタイム顧客データプラットフォーム](../../rtcdp/overview.md)では、宛先は、それらのパートナーに対してシームレスにデータをアクティブ化する目的のプラットフォームとの事前に構築された統合です。

**新しい宛先**

リアルタイムCDPは、50個を超える[!DNL Experience Cloud Launch]拡張に対するデータアクティベーションをサポートし、解析、パーソナライゼーション、その他の使用例を可能にします。 詳しくは、以下を参照してください。

| ドキュメント | 説明 |
|--- | ---|
| [宛先のタイプとカテゴリ](../../destinations/destination-types.md) | この記事では、リアルタイムCDPインターフェイスの接続と拡張の違いを説明し、これらの各宛先を使用する場合を推奨します。 |
| [Experience Platform Launch 拡張機能](../../destinations/catalog/launch-extensions/overview.md) | このページでは、[!DNL Launch]拡張機能の概要、リストの使用例、およびReal-time CDPの各[!DNL Launch]拡張機能のドキュメントへのリンクを説明します。 |

詳しくは、『[宛先の概要](../../destinations/home.md)』を参照してください。

## [!DNL Privacy Service] {#privacy}

新しい法規制や組織の規制により、ユーザーはリクエストによってデータストアから個人データにアクセスしたり削除したりする権利を与えられています。Adobe Experience Platform[!DNL Privacy Service]は、RESTful APIとユーザーインターフェイスを提供し、お客様からのこれらのデータリクエストを管理するのに役立ちます。 [!DNL Privacy Service]を使うと、個人顧客データや個人顧客データにアクセスして削除する要求をAdobe Experience Cloudのアプリケーションから送信でき、法的および組織のプライバシー規制への自動コンプライアンスが容易になります。

**新機能**

| 機能 | 説明 |
| --- | --- |
| PDPA のサポート | タイの個人データ保護法（PDPA）に基づいて、プライバシーリクエストを作成し、トラックできるようになりました。API でプライバシーリクエストを実行する場合、`regulation` 配列は「pdpa_tha」の値を受け取ります。 |
| UI の名前空間タイプ | [!DNL Privacy Service] UIのリクエストビルダーで、異なる名前空間タイプを指定できるようになりました。 詳しくは、[ユーザガイド](../../privacy-service/ui/user-guide.md)を参照してください。 |
| 古いエンドポイントの廃止 | 古い API エンドポイント（`data/privacy/gdpr`）は廃止されました。 |

既知の問題

* なし

[!DNL Privacy Service]の詳細については、[Privacy Serviceの概要](../../privacy-service/home.md)を読んで開始してください。

## ソース {#sources}

Adobe Experience Platformは外部ソースからデータを取り込みながら、[!DNL Platform]サービスを使ってデータの構造、ラベル付け、拡張を行うことができます。 アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できるようにする RESTful API とインタラクティブな UI を提供します。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| データベースの API と UI のサポート | [!DNL Apache Spark] (HDInsights)、[!DNL Azure Synapse Analytics]、[!DNL Azure Table Storage]、[!DNL Hive] (HDInsights)、および[!DNL Phoenix]の新しいソースコネクタ。 |
| 支払いベースのアプリケーションの API と UI のサポート | [!DNL PayPal]の新しいソースコネクタ。 |
| プロトコルベースのアプリケーションの API と UI のサポート | [!DNL Generic OData]の新しいソースコネクタ。 |

**既知の問題**

* なし

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
