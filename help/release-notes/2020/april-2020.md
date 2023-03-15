---
title: Adobe Experience Platform リリースノート 2020年4月
description: Adobe Experience Platform の 2020年4月のリリースノート。
doc-type: release notes
last-update: April 13, 2020
author: ens71067
keywords: リリースノート;
exl-id: 0f68c71e-3c9d-453b-a953-1cd1b6ca2e35
source-git-commit: 14e3eff3ea2469023823a35ee1112568f5b5f4f7
workflow-type: tm+mt
source-wordcount: '983'
ht-degree: 68%

---

# Adobe Experience Platform リリースノート

**リリース日：2020年4月8日（PT）**

Adobe Experience Platform の新機能：
* [[!DNL Intelligent Services]](#intelligent)

既存の機能の更新：
* [[!DNL Experience Data Model (XDM)]](#xdm)
* [データガバナンス](#governance)
* [[!DNL Destinations]](#destinations)
* [[!DNL Privacy Service]](#privacy)
* [[!DNL Sources]](#sources)

## [!DNL Intelligent Services] {#intelligent}

[!DNL Intelligent Services] は、マーケティングアナリストや従事者に対して、顧客体験の使用例で人工知能とマシンラーニングの機能を活用する機能を提供します。これにより、マーケティングアナリストは、データ科学の専門知識を必要とせずに、ビジネスレベルの設定を使用して、会社のニーズに固有の予測を設定できます。さらに、マーケターは、Adobe Experience Cloud、Adobe Experience Platform およびサードパーティアプリケーションで予測をアクティブ化できます。

**主な特長**

| 機能 | 説明 |
|---|---|
| [!DNL Customer AI] | [!DNL Customer AI] は、マーケターに、個人レベルで説明を含む顧客予測を生み出す力を提供します。 影響力のある要因の助けを借りて [!DNL Customer AI] は、お客様が何をする可能性が高いかとその理由を示します。 さらに、マーケターは、 [!DNL Customer AI] 最も適切なオファーとメッセージを提供することで、顧客体験をパーソナライズするための予測とインサイト。 |
| [!DNL Attribution AI] | [!DNL Attribution AI] は、指定した結果に対する顧客のインタラクションの影響と増分的な影響を計算する、複数チャネルのアルゴリズムによるアトリビューションサービスです。[!DNL Attribution AI] を使用すると、マーケターは、カスタマージャーニーの各段階における個々の顧客インタラクションの影響を把握することで、マーケティング費用と広告費用を測定し、最適化できます。 |

**既知の問題**

* 現在、既知の問題はありません。

詳しくは、 [!DNL Intelligent Services] そして何を提供すべきか、 [インテリジェントサービスの概要](../../intelligent-services/home.md).

## [!DNL Experience Data Model]（XDM）システム {#xdm}

標準化と相互運用性は、[!DNL Experience Platform] の背後にある重要な概念です。アドビが推進する [!DNL Experience Data Model]（XDM）は、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

XDM はパブリックに文書化された仕様であり、デジタルエクスペリエンスのパワーを向上させるために設計されています。Adobe Experience Platform 上のサービスと通信するすべてのアプリケーションに共通の構造と定義を提供します。XDM 標準規格に準拠することで、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| 代替表示情報の自動化 | この [!DNL Schema Registry] 自動的に、 `alternateDisplayInfo` 記述子。 |
| スカラーフィールドの制限 | この [!DNL Schema Registry] では、1 つのスキーマで 6,000 個を超えるスカラーフィールドを使用できません。 |
| パフォーマンスの見直し | この [!DNL Schema Registry] 要求を満たすために見直され [!DNL Experience Platform] もっと良い。 |

**バグの修正**

* 標準の XDM の入れ子 URI フィールドで、より簡潔な XED 形式をサポートするように、XDM を XED に変換しました。

**既知の問題**

* 既知

## データガバナンス {#governance}

Adobe Experience Platform データガバナンスは、顧客データを管理し、データの使用に適用される規制、制限、ポリシーへの準拠を確保するために使用される一連の戦略とテクノロジーです。これは、[!DNL Experience Platform] において様々なレベルで重要な役割を果たします。例えば、カタログ作成、データ系列、データ使用のラベル付け、データアクセスポリシー、マーケティングアクションのデータに関するアクセス制御などです。

データガバナンスを開始するには、お客様のデータに適用される規制、契約上の義務、および企業ポリシーを十分に理解する必要があります。ここから、適切なデータ使用ラベルを適用してデータを分類し、その使用をデータ使用ポリシーの定義を通じて制御できます。

データガバナンスフレームワークは、 [!DNL Experience Platform] ユーザーインターフェイスと [!DNL Policy Service] API

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| UI でのデータ使用ポリシーの管理 | データ使用ポリシーは、 UI の&#x200B;**ポリシー**&#x200B;ワークスペース内で管理できるようになっています。[!DNL Experience Platform]詳しくは、『[ポリシーユーザガイド](../../data-governance/policies/user-guide.md)』を参照してください。 |

**既知の問題**

* なし。

詳しくは、「[データガバナンスの概要](../../data-governance/home.md)」を参照してください。


## 宛先 {#destinations}

In [Real-time Customer Data Platform](../../rtcdp/overview.md)の宛先は、宛先プラットフォームとの事前に構築された統合であり、これらのパートナーに対するデータをシームレスにアクティブ化します。

**新しい宛先**

Real-Time CDPは現在、50 を超えるデータのアクティブ化をサポートしています [!DNL Experience Cloud Launch] 拡張機能を使用して、analytics、パーソナライゼーション、その他の使用例を有効にします。 詳しくは、以下を参照してください。

| ドキュメント | 説明 |
|--- | ---|
| [宛先のタイプとカテゴリ](../../destinations/destination-types.md) | この記事では、Real-Time CDPインターフェイスの接続と拡張機能の違いを説明し、これらの各宛先を使用する場合を推奨します。 |
| [Experience Platform Launch 拡張機能](../../destinations/catalog/launch-extensions/overview.md) | このページでは、 [!DNL Launch] の拡張機能、使用例のリスト、および各ドキュメントのリンクです。 [!DNL Launch] Real-Time CDPの拡張機能。 |

詳しくは、『[宛先の概要](../../destinations/home.md)』を参照してください。

## [!DNL Privacy Service] {#privacy}

新しい法規制や組織の規制により、ユーザーはリクエストによってデータストアから個人データにアクセスしたり削除したりする権利を与えられています。Adobe Experience Platform [!DNL Privacy Service] には、顧客からのこれらのデータリクエストを管理するのに役立つ RESTful API とユーザーインターフェイスが用意されています。 を使用 [!DNL Privacy Service]を使用すると、Adobe Experience Cloudアプリケーションから非公開または個人の顧客データに対するアクセスおよび削除のリクエストを送信でき、法規制や組織のプライバシー規制への自動コンプライアンスが容易になります。

**新機能**

| 機能 | 説明 |
| --- | --- |
| PDPA のサポート | タイの個人データ保護法（PDPA）に基づいて、プライバシーリクエストを作成し、トラックできるようになりました。API でプライバシーリクエストを実行する場合、`regulation` 配列は「pdpa_tha」の値を受け取ります。 |
| UI の名前空間タイプ | [!DNL Privacy Service] UI のリクエストビルダーで、様々な名前空間タイプを指定できるようになりました。詳しくは、[ユーザガイド](../../privacy-service/ui/user-guide.md)を参照してください。 |
| 古いエンドポイントの廃止 | 古い API エンドポイント（`data/privacy/gdpr`）は廃止されました。 |

既知の問題

* なし

詳しくは、 [!DNL Privacy Service]次を読んでください： [Privacy Serviceの概要](../../privacy-service/home.md).

## ソース {#sources}

Adobe Experience Platformで外部ソースからデータを取り込みながら、を使用してデータの構造化、ラベル付け、拡張をおこなうことができます。 [!DNL Platform] サービス。 アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| データベースの API と UI のサポート | 用の新しいソースコネクタ [!DNL Apache Spark] （HDInsights に関して） [!DNL Azure Synapse Analytics], [!DNL Azure Table Storage], [!DNL Hive] （HDInsights に関する） [!DNL Phoenix]. |
| 支払いベースのアプリケーションの API と UI のサポート | 用の新しいソースコネクタ [!DNL PayPal]. |
| プロトコルベースのアプリケーションの API と UI のサポート | 用の新しいソースコネクタ [!DNL Generic OData]. |

**既知の問題**

* なし

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
