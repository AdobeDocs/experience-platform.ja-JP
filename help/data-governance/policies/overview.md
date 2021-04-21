---
keywords: Experience Platform；ホーム；人気のあるトピック；スケジュール；DULE
solution: Experience Platform
title: データ使用ポリシーの概要
topic-legacy: policies
description: データ使用ラベルがデータのコンプライアンスを効果的にサポートするためには、データ使用ポリシーを実装する必要があります。データ使用ポリシーは、Experience Platform 内のデータに対して実行を許可または制限するマーケティングアクションの種類を記述するルールです
exl-id: 1b372aa5-3e49-4741-82dc-5701a4bc8469
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1010'
ht-degree: 18%

---

# データ使用ポリシーの概要

データ使用状況ラベルがデータのコンプライアンスを効果的にサポートするためには、データ使用ポリシーを実施する必要があります。データ使用ポリシーとは、[!DNL Experience Platform]内のデータに対して実行を許可（制限）されるマーケティングアクションの種類を記述するルールです。

このドキュメントでは、データ使用ポリシーの概要を説明し、UIまたはAPIでポリシーを操作するための詳細なドキュメントへのリンクを示します。

## マーケティングアクション {#marketing-actions}

マーケティングアクション（マーケティングの使用例とも呼ばれます）は、データ管理フレームワークの観点から見ると、[!DNL Experience Platform]データ消費者が行うことのできるアクションで、組織がデータの使用を制限したいと考えています。 そのため、データ使用ポリシーは次の方法で定義します。

1. 特定のマーケティングアクション
2. アクションの実行が制限されているデータ使用ラベル

マーケティングアクションの例としては、データセットをサードパーティのサービスにエクスポートする場合などがあります。特定の種類のデータ(PII(Personally Identifial Information)など)はエクスポートできないというポリシーが設定されている場合、「I」ラベル(Identity data)を含むデータセットをエクスポートしようとすると、[!DNL Policy Service]からデータ使用ポリシーに違反した旨の応答が返されます。

>[!NOTE]
>
>マーケティングアクション自体は、データの使用を制限しません。 これらのアクションをポリシー違反で評価するには、有効なデータ使用ポリシーに含める必要があります。

組織のサービスでデータの使用が発生した場合、ポリシー違反を識別できるように、関連するマーケティングアクションを示す必要があります。 その後、[Policy Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml)を使用して、統合内のポリシー違反を確認できます。

>[!NOTE]
>
>[!DNL Real-time Customer Data Platform]を使用している場合は、宛先に対するマーケティングの使用例を設定して、ポリシーの適用を自動化できます。 詳細は、[Data Governance in Real-time CDP](../../rtcdp/privacy/data-governance-overview.md)のドキュメントを参照してください。

[利用可能なAdobe定義のマーケティングアクション](#core-actions)のリストについては、このドキュメントの付録を参照してください。 また、[!DNL Policy Service] APIまたは[!DNL Experience Platform ]ユーザーインターフェイスを使用して、独自のカスタムマーケティングアクションを定義することもできます。 マーケティングアクションとポリシーの操作について詳しくは、次のセクションを参照してください。

<!-- (Add after AAM DEC mapping doc is published)
### Inheritance from Adobe Audience Manager Data Export Controls

Experience Platform has the ability to share segments with Adobe Audience Manager. Any Data Export Controls that have been applied to Audience Manager segments are translated to equivalent marketing use cases recognized by Experience Platform Data Governance.

For a reference on how specific Data Export Controls map to marketing actions in Platform, please refer to the [Audience Manager documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/data-export-controls.html).
-->

## データ使用ポリシーの管理{#manage}

データ使用ラベルが適用されると、データステワードは[!DNL Policy Service] APIまたは[!DNL Experience Platform] UIを使用して、データ使用ラベルを含むデータに対して行われるマーケティングアクションに関連するポリシーを管理および評価できます。 ポリシーの作成と更新、ポリシーのステータスの決定、およびマーケティングアクションを使用して、特定のアクションがデータ使用ポリシーに違反しているかどうかを評価できます。

>[!IMPORTANT]
>
>すべてのデータ使用ポリシー(Adobeが提供するコアポリシーを含む)は、デフォルトで無効になっています。 個々のポリシーの適用が考慮されるようにするには、APIまたはUIを使用して手動でそのポリシーを有効にする必要があります。

APIでマーケティングアクションとデータ使用ポリシーを使用する手順については、[データ使用ポリシーの作成と評価](create.md)のチュートリアルを参照してください。 [!DNL Policy Service] APIが提供する主な操作について詳しくは、[Policy Service開発者ガイド](../api/getting-started.md)を参照してください。

[!DNL Platform] UIのマーケティングアクションとポリシーの使用方法について詳しくは、[data usage policyユーザーガイド](./user-guide.md)を参照してください。

## 次の手順

このドキュメントは、[!DNL Data Governance]フレームワーク内のデータ使用ポリシーの紹介を提供しました。 このガイド全体にリンクされているプロセスドキュメントを読み続けて、APIとUIでポリシーを使用する方法について詳しく学ぶことができます。

## 付録

次の節では、データ使用ポリシーに関する追加情報について説明します。

### Adobe定義のマーケティングアクション{#core-actions}

次の表に、Adobeがすぐに使用できる主要なマーケティングアクションを示します。

>[!NOTE]
>
>どの使用ポリシーを作成し違反を確認するかを特定するために、主要なマーケティングアクションを出発点として見なす必要があります。 定義とその解釈方法は、組織のニーズとポリシーに応じて異なります。

| マーケティングアクション | 説明 |
| --- | --- |
| Analytics | 組織のサイトやアプリの顧客の使用状況に関する測定、分析、レポートなど、分析目的でデータを使用するアクション。 |
| PII と組み合わせる | 任意の個人識別情報(PII)と匿名データを組み合わせたアクション。 広告ネットワーク、広告サーバー、およびサードパーティのデータプロバイダーが提供するデータの契約には、直接識別可能なデータを含むデータの使用に関し、特定の契約上の禁止が含まれることがよくあります。 |
| クロスサイトターゲティング | データをクロスサイト広告ターゲットに使用するアクション。 オンサイトデータとオフサイトデータの組み合わせや、複数のオフサイトソースから得られるデータの組み合わせなど、複数サイトのデータの組み合わせは、クロスサイトデータと呼ばれます。通常、クロスサイトデータは収集および処理され、ユーザーの興味に関する推測が行われます。 |
| データサイエンス | データサイエンスのワークフローにデータを使用するアクション。 一部には、データサイエンスへのデータの使用を明示的に禁止している契約があります。これらには、人工知能（AI）、機械学習（ML）、モデリングへのデータの使用を禁じる条項が含まれている場合があります。 |
| 電子メールのターゲット設定 | 電子メールターゲティングキャンペーンのデータを使用するアクション。 |
| サードパーティにエクスポート | 顧客と直接の関係を持たないプロセッサーおよびエンティティにデータをエクスポートするアクション。 多くのデータプロバイダは、契約に含まれる条件の中で、最初に収集された場所からのデータのエクスポートを禁止しています。 例えば、ソーシャルネットワークの契約では、多くの場合、ソーシャルネットワークから受け取ったデータの転送を制限しています。 |
| オンサイト広告 | 組織のWebサイトやアプリに対する広告の選択と配信、またはそのような広告の配信と効果を測定するなど、オンサイト広告にデータを使用するアクション。 |
| オンサイトのパーソナライズ機能 | オンサイトコンテンツのパーソナライゼーションにデータを使用するアクション。 オンサイトパーソナライゼーションは、ユーザーの興味に関する推論を行うために使用され、それらの推論に基づいて提供されるコンテンツや広告を選択するために使用されるデータです。 |
| 単一IDパーソナライゼーション | 複数のソースからIDをステッチするのではなく、単一のIDをパーソナライズの目的で使用する必要があるアクション。 |
