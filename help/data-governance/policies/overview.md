---
keywords: Experience Platform;home;popular topics;dule;DULE
solution: Experience Platform
title: データ使用ポリシーの概要
topic: policies
description: データ使用状況ラベルがデータのコンプライアンスを効果的にサポートするためには、データ使用ポリシーを実施する必要があります。データ使用ポリシーは、Experience Platform 内のデータに対して実行を許可または制限するマーケティングアクションの種類を記述するルールです
translation-type: tm+mt
source-git-commit: 259c26a9d3b6ef397acd552e255f68ecb25b2dd1
workflow-type: tm+mt
source-wordcount: '1003'
ht-degree: 19%

---


# データ使用ポリシーの概要

データ使用状況ラベルがデータのコンプライアンスを効果的にサポートするためには、データ使用ポリシーを実施する必要があります。Data usage policies are rules that describe the kinds of marketing actions that you are allowed to, or restricted from, performing on data within [!DNL Experience Platform].

このドキュメントでは、データ使用ポリシーの概要を説明し、UIまたはAPIでポリシーを操作するための詳細なドキュメントへのリンクを示します。

## マーケティングアクション {#marketing-actions}

マーケティングアクション（マーケティングの使用例とも呼ばれます）は、データ管理フレームワークの観点から見ると、 [!DNL Experience Platform] データ消費者が行うことのできるアクションで、組織でデータの使用を制限したいと考えています。 そのため、データ使用ポリシーは次の方法で定義します。

1. 特定のマーケティングアクション
2. アクションの実行が制限されているデータ使用ラベル

マーケティングアクションの例としては、データセットをサードパーティのサービスにエクスポートする場合などがあります。特定の種類のデータ(PII(Personally Identifial Information)など)はエクスポートできないというポリシーが設定されている場合、「I」ラベル(Identity data)を含むデータセットをエクスポートしようとすると、データ使用ポリシーに違反したことを伝える応答が返されます。 [!DNL Policy Service]

>[!NOTE]
>
>マーケティングアクション自体は、データの使用を制限しません。 これらのアクションをポリシー違反で評価するには、有効なデータ使用ポリシーに含める必要があります。

組織のサービスでデータの使用が発生した場合、ポリシー違反を識別できるように、関連するマーケティングアクションを示す必要があります。 その後、 [Policy Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml) （ポリシーサービスAPI）を使用して、統合内のポリシー違反を確認できます。

>[!NOTE]
>
>を使用している場合 [!DNL Real-time Customer Data Platform]は、宛先にマーケティングの使用例を設定して、ポリシーの適用を自動化できます。 詳細は、Real-time CDPの [Data Governanceに関するドキュメントを参照してください](../../rtcdp/privacy/data-governance-overview.md) 。

使用可能なAdobe定義のマーケティングアクションのリストについては、このドキュメントの付録 [を参照してください](#core-actions)。 また、 [!DNL Policy Service] APIまたはユー [!DNL Experience Platform ]ザーインターフェイスを使用して、独自のカスタムマーケティングアクションを定義することもできます。 マーケティングアクションとポリシーの操作について詳しくは、次のセクションを参照してください。

<!-- (Add after AAM DEC mapping doc is published)
### Inheritance from Adobe Audience Manager Data Export Controls

Experience Platform has the ability to share segments with Adobe Audience Manager. Any Data Export Controls that have been applied to Audience Manager segments are translated to equivalent marketing use cases recognized by Experience Platform Data Governance.

For a reference on how specific Data Export Controls map to marketing actions in Platform, please refer to the [Audience Manager documentation](https://docs.adobe.com/content/help/en/audience-manager/user-guide/features/data-export-controls.html).
-->

## Managing data usage policies {#manage}

データ使用ラベルが適用されると、データステッダーは [!DNL Policy Service] APIまたは [!DNL Experience Platform] UIを使用して、データ使用ラベルを含むデータに対して行われるマーケティングアクションに関連するポリシーを管理および評価できます。 ポリシーの作成と更新、ポリシーのステータスの決定、およびマーケティングアクションを使用して、特定のアクションがデータ使用ポリシーに違反しているかどうかを評価できます。

>[!IMPORTANT]
>
>すべてのデータ使用ポリシー(Adobeが提供するコアポリシーを含む)は、デフォルトで無効になっています。 個々のポリシーの適用が考慮されるようにするには、APIまたはUIを使用して手動でそのポリシーを有効にする必要があります。

APIでマーケティングアクションとデータ使用ポリシーを使用する手順については、データ使用ポリシーの [作成と評価に関するチュートリアルを参照してください](create.md)。 For more information the key operations provided by the [!DNL Policy Service] API, see the [Policy Service developer guide](../api/getting-started.md).

UIでマーケティングアクションとマーケティングポリシーを使用する方法について詳しくは、『 [!DNL Platform] data usage policy user guide [](./user-guide.md)』を参照してください。

## 次の手順

このドキュメントでは、フレームワーク内のデータ使用ポリシーの概要を説明し [!DNL Data Governance] ました。 このガイド全体にリンクされているプロセスドキュメントを読み続けて、APIとUIでポリシーを使用する方法について詳しく学ぶことができます。

## 付録

次の節では、データ使用ポリシーに関する追加情報について説明します。

### Adobe定義のマーケティングアクション {#core-actions}

次の表に、Adobeがすぐに使用できる主要なマーケティングアクションを示します。

>[!NOTE]
>
>どの使用ポリシーを作成し違反を確認するかを特定するために、主要なマーケティングアクションを出発点として見なす必要があります。 定義とその解釈方法は、組織のニーズとポリシーに応じて異なります。

| マーケティングアクション | 説明 |
| --- | --- |
| Analytics | 組織のサイトやアプリの顧客の使用状況に関する測定、分析、レポートなど、分析目的でデータを使用するアクション。 |
| PII と組み合わせる | 任意の個人識別情報(PII)と匿名データを組み合わせたアクション。 広告ネットワーク、広告サーバー、およびサードパーティのデータプロバイダーが提供するデータの契約には、直接識別可能なデータを含むデータの使用に関し、特定の契約上の禁止が含まれることがよくあります。 |
| クロスサイトターゲティング | データをクロスサイト広告ターゲットに使用するアクション。 オンサイトデータとオフサイトデータの組み合わせや、複数のオフサイトソースから得られるデータの組み合わせなど、複数サイトのデータの組み合わせは、クロスサイトデータと呼ばれます。通常、クロスサイトデータは収集および処理され、ユーザーの興味に関する推測が行われます。 |
| データ科学 | データサイエンスのワークフローにデータを使用するアクション。 一部には、データサイエンスへのデータの使用を明示的に禁止している契約があります。これらには、人工知能（AI）、機械学習（ML）、モデリングへのデータの使用を禁じる条項が含まれている場合があります。 |
| 電子メールのターゲット設定 | 電子メールターゲティングキャンペーンのデータを使用するアクション。 |
| サードパーティにエクスポート | 顧客と直接の関係を持たないプロセッサーおよびエンティティにデータをエクスポートするアクション。 多くのデータプロバイダは、契約に含まれる条件の中で、最初に収集された場所からのデータのエクスポートを禁止しています。 例えば、ソーシャルネットワークの契約では、多くの場合、ソーシャルネットワークから受け取ったデータの転送を制限しています。 |
| オンサイト広告 | 組織のWebサイトやアプリに対する広告の選択と配信、またはそのような広告の配信と効果を測定するなど、オンサイト広告にデータを使用するアクション。 |
| オンサイトのパーソナライズ機能 | オンサイトコンテンツのパーソナライゼーションにデータを使用するアクション。 オンサイトパーソナライゼーションは、ユーザーの興味に関する推論を行うために使用され、それらの推論に基づいて提供されるコンテンツや広告を選択するために使用されるデータです。 |
| 単一IDパーソナライゼーション | 複数のソースからIDをステッチするのではなく、単一のIDをパーソナライズの目的で使用する必要があるアクション。 |
