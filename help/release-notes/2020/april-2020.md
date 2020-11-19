---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート（2020 年 4 月 9 日）
doc-type: release notes
last-update: April 13, 2020
author: ens71067
keywords: release notes;
translation-type: tm+mt
source-git-commit: 0232acdc64019b9d93888e8137ef9bc8e114779b
workflow-type: tm+mt
source-wordcount: '972'
ht-degree: 60%

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
| [!DNL Customer AI] | [!DNL Customer AI] マーケターに、説明を加えながら個々のレベルで顧客予測を行う力を提供します。 With the help of influential factors, [!DNL Customer AI] can tell you what a customer is likely to do and why. Additionally, marketers can benefit from [!DNL Customer AI] predictions and insights to personalize customer experiences by serving the most appropriate offers and messaging. |
| [!DNL Attribution AI] | [!DNL Attribution AI] は、指定した結果に対する顧客のインタラクションの影響と増分的な影響を計算する、複数チャネルのアルゴリズムによるアトリビューションサービスです。[!DNL Attribution AI] を使用すると、マーケターは、カスタマージャーニーの各段階における個々の顧客インタラクションの影響を把握することで、マーケティング費用と広告費用を測定し、最適化できます。 |

**既知の問題**

* 現在、既知の問題はありません。

For more information on [!DNL Intelligent Services] and what it has to offer, see the [Intelligent Services overview](../../intelligent-services/home.md).

## [!DNL Experience Data Model] (XDM)システム {#xdm}

Standardization and interoperability are key concepts behind [!DNL Experience Platform]. [!DNL Experience Data Model] (XDM)は、Adobeに基づいて、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

XDM はパブリックに文書化された仕様であり、デジタルエクスペリエンスのパワーを向上させるために設計されています。Adobe Experience Platform 上のサービスと通信するすべてのアプリケーションに共通の構造と定義を提供します。XDM 標準規格に準拠することで、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| 代替表示情報の自動化 | The [!DNL Schema Registry] automatically applies the customized title and description values configured in the `alternateDisplayInfo` descriptor. |
| スカラーフィールドの制限 | The [!DNL Schema Registry] does not allow more than 6000 scalar fields in a single schema. |
| パフォーマンスの見直し | The [!DNL Schema Registry] has been overhauled to perform and meet the demands of [!DNL Experience Platform] better. |

**バグの修正**

* 標準の XDM の入れ子 URI フィールドで、より簡潔な XED 形式をサポートするように、XDM を XED に変換しました。

**既知の問題**

* 既知

## [!DNL Data Governance] {#governance}

Adobe Experience Platform [!DNL Data Governance] is a series of strategies and technologies used to manage customer data and ensure compliance with regulations, restrictions, and policies applicable to data usage. It plays a key role within [!DNL Experience Platform] at various levels, including cataloging, data lineage, data usage labeling, data access policies, and access control on data for marketing actions.

データガバナンスを開始するには、お客様のデータに適用される規制、契約上の義務、および企業ポリシーを十分に理解する必要があります。ここから、適切なデータ使用ラベルを適用してデータを分類し、その使用をデータ使用ポリシーの定義を通じて制御できます。

The [!DNL Data Governance] framework simplifies and streamlines the process of categorizing data and creating data usage policies through the [!DNL Experience Platform] user interface and [!DNL Policy Service] API.

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| UI でのデータ使用ポリシーの管理 | データ使用ポリシーは、 UI の&#x200B;**ポリシー**&#x200B;ワークスペース内で管理できるようになっています。[!DNL Experience Platform]詳しくは、『[ポリシーユーザガイド](../../data-governance/policies/user-guide.md)』を参照してください。 |

**既知の問題**

* なし。

詳しくは、「[データガバナンスの概要](../../data-governance/home.md)」を参照してください。


## 宛先 {#destinations}

In [Real-time Customer Data Platform](../../rtcdp/overview.md), destinations are pre-built integrations with destination platforms that activate data to those partners in a seamless way.

**新しい宛先**

Adobe Real-time CDP now supports data activation to over fifty [!DNL Experience Cloud Launch] extensions, enabling analytics, personalization, and other use cases. 詳しくは、以下を参照してください。

| ドキュメント | 説明 |
|--- | ---|
| [宛先のタイプとカテゴリ](/help/rtcdp/destinations/destination-types.md) | この記事では、アドビのリアルタイム CDP インターフェイスの接続と拡張機能の違いを説明し、これらの各宛先を使用する場合を推奨します。 |
| [Experience Platform Launch 拡張機能](/help/rtcdp/destinations/experience-platform-launch-extensions.md) | This page explains what [!DNL Launch] extensions are, lists use cases for using them, and links to documentation for each [!DNL Launch] extension in Adobe Real-time CDP. |

詳しくは、『[宛先の概要](/help/rtcdp/destinations/destinations-overview.md)』を参照してください。

## [!DNL Privacy Service] {#privacy}

新しい法規制や組織の規制により、ユーザーはリクエストによってデータストアから個人データにアクセスしたり削除したりする権利を与えられています。Adobe Experience Platform [!DNL Privacy Service] provides a RESTful API and user interface to help you manage these data requests from your customers. With [!DNL Privacy Service], you can submit requests to access and delete private or personal customer data from Adobe Experience Cloud applications, facilitating automated compliance with legal and organizational privacy regulations.

**新機能**

| 機能 | 説明 |
| --- | --- |
| PDPA のサポート | タイの個人データ保護法（PDPA）に基づいて、プライバシーリクエストを作成し、トラックできるようになりました。API でプライバシーリクエストを実行する場合、`regulation` 配列は「pdpa_tha」の値を受け取ります。 |
| UI の名前空間タイプ | You can now specify different namespace types in the Request Builder in the [!DNL Privacy Service] UI. 詳しくは、[ユーザガイド](../../privacy-service/ui/user-guide.md)を参照してください。 |
| 古いエンドポイントの廃止 | 古い API エンドポイント（`data/privacy/gdpr`）は廃止されました。 |

既知の問題

* なし

For more information about [!DNL Privacy Service], please start by reading the [Privacy Service overview](../../privacy-service/home.md).

## ソース {#sources}

Adobe Experience Platform can ingest data from external sources while allowing you to structure, label, and enhance that data using [!DNL Platform] services. アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できるようにする RESTful API とインタラクティブな UI を提供します。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| データベースの API と UI のサポート | （HDInsightsで）、 [!DNL Apache Spark] 、 [!DNL Azure Synapse Analytics]、（HDInsightsで）およびの新しいソースコネクタ [!DNL Azure Table Storage]ー [!DNL Hive][!DNL Phoenix]。 |
| 支払いベースのアプリケーションの API と UI のサポート | New source connectors for [!DNL PayPal]. |
| プロトコルベースのアプリケーションの API と UI のサポート | New source connectors for [!DNL Generic OData]. |

**既知の問題**

* なし

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
