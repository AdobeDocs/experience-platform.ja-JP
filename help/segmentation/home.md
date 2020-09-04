---
keywords: Experience Platform;home;popular topics;segmentation;Segmentation;segment service;segment;Segment;Segments;segments
solution: Experience Platform
title: Adobe Experience Platform セグメント化サービス
topic: overview
description: このドキュメントでは、セグメント化サービスの概要と Adobe Experience Platform での役割について説明します。
translation-type: tm+mt
source-git-commit: 5dd07bf9afe96be3a4c3f4a4d4e3b23aef4fde70
workflow-type: tm+mt
source-wordcount: '1387'
ht-degree: 68%

---


# Adobe Experience Platform [!DNL Segmentation Service] overview

Adobe Experience Platform [!DNL Segmentation Service] provides a user interface and RESTful API that allows you to build segments and generate audiences from your [!DNL Real-time Customer Profile] data. These segments are centrally configured and maintained on [!DNL Platform], and are readily accessible by any Adobe solution.

This document provides an overview of [!DNL Segmentation Service] and the role it plays in Adobe Experience Platform.

## Getting started with [!DNL Segmentation Service]

このドキュメント全体で使用される以下の主要用語を理解しておくことが重要です。

- **セグメント**：多数の個人（顧客、見込み客、ユーザーまたは組織など）を、類似の特性を共有しマーケティング戦略に対して同様の対応をする小さなグループに分割します。
- **セグメント定義**：ターゲットオーディエンスの主要な特性や動作を記述するルールセットです。概念化が完了すると、セグメント定義で記述されているルールを使用して、セグメントの適格なオーディエンスメンバーが決定されます。
- **オーディエンス**：セグメント定義の条件を満たすプロファイルの結果セットです。

## セグメント化の仕組み

セグメント化とは、プロファイルストアにあるプロファイルのサブセットによって共有される特定の属性や行動を定義し、マーケティング可能な人々のグループを顧客ベースから選別するプロセスです。例えば、「スニーカーを購入し忘れましたか？」という電子メールキャンペーンでは、過去 30 日間にランニングシューズを検索したが購入を完了しなかったすべてのユーザーのオーディエンスが必要な場合があります。

Once a segment has been conceptually defined it is built in [!DNL Experience Platform]. セグメントは通常、マーケターまたはオーディエンススペシャリストが作成しますが、データアナリストと協力してマーケティング部門が作成することを望む組織もあります。Upon reviewing the data being sent to [!DNL Platform], the data analyst composes the segment definition by selecting which fields and values will be used to build the rules or conditions of the segment. これは、UI または API を使用しておこないます。

## セグメントの作成

APIを使用して作成する場合も、を使用する場合も、最終的に [!DNL Segment Builder]は [!DNL Profile Query Language] (PQL)を使用して定義されます。 ここでは、条件を満たすプロファイルを取得するために設計された言語で概念セグメント定義を記述します。詳しくは、[PQL の概要](./pql/overview.md)を参照してください。

To learn how to create and use segments in the [!DNL Segment Builder] (the UI implementation of [!DNL Segmentation Service]), see the [Segment Builder guide](./ui/overview.md).

API を使用したセグメント定義の作成について詳しくは、[API を使用したオーディエンスセグメントの作成](./tutorials/create-a-segment.md)に関するチュートリアルを参照してください。

>[!NOTE]
>
> イベントでは、スキーマが拡張され、以降のすべてのアップロードで、新しく追加されたフィールドを適宜更新する必要があります。For more information on customizing [!DNL Experience Data Model] (XDM), visit the [Schema Editor tutorial](../xdm/tutorials/create-schema-ui.md).

## セグメントの評価

### ストリーミングセグメント化

ストリーミングセグメント化は、ユーザーのアクティビティに応じてセグメントを更新する継続的なデータ選択プロセスです。Once a segment has been built and saved, the segment definition is applied against incoming data to [!DNL Real-time Customer Profile]. セグメントの追加と削除は定期的に処理され、ターゲットオーディエンスの関連性が維持されます。

ストリーミングセグメント化について詳しくは、[ストリーミングセグメント化のドキュメント](./api/streaming-segmentation.md)を参照してください。

### バッチセグメント化

継続的なデータ選択プロセスの代わりに、バッチセグメント化では、セグメント定義を介してすべてのプロファイルデータを一括して移動し、対応するオーディエンスを生成します。作成したセグメントは保存されて、使用時にエクスポートできるようになります。

セグメントの評価方法については、[セグメント評価のチュートリアル](./tutorials/evaluate-a-segment.md)を参照してください。

## セグメント化の結果へのアクセス

エクスポートしたセグメントにアクセスする方法については、[セグメント評価のチュートリアル](./tutorials/evaluate-a-segment.md)を参照してください。

## セグメントメタデータ

セグメントメタデータを使用すると、イベント内でのインデックス作成が容易になり、セグメントの再利用や組み合わせが可能になります。

Composing your segments (through either the API or [!DNL Segment Builder]) requires that you to define a segment name and merge policy.

### セグメント名

新しいセグメントを作成する場合は、セグメント名を指定する必要があります。The segment name is used to identify a particular segment amongst the collection built by [!DNL Segmentation Service]. したがって、セグメント名は説明的で簡潔かつ一意である必要があります。

>[!NOTE]
>
> セグメントを計画する際、セグメントを他のセグメントから参照したり、他のセグメントと組み合わせたりできます。名前を選択する際は、セグメントに再利用可能な部分が含まれている可能性を考慮してください。

### 結合ポリシー

Merge policies are rules used by [!DNL Profile] to determine how data will be prioritized and combined into a unified view under certain conditions.
If a merge policy is not defined, the default [!DNL Platform] merge policy is used. 組織に固有の結合ポリシーを使用する場合は、独自の結合ポリシーを作成し、組織のデフォルトとすることができます。

結合ポリシーの詳細については、 [結合ポリシーガイドを参照してください](../profile/api/merge-policies.md)。

>[!NOTE]
>
> オーディエンスサイズの推定は、組織のデフォルトのプロファイル結合ポリシーに基づいておこなわれます。

### その他のセグメントメタデータ

In addition to segment name and merge policy, [!DNL Segment Builder] offers you an additional &quot;segment description&quot; metadata field where you can summarize your segment definition&#39;s purpose.

## 高度なセグメント化機能

セグメントは、[ストリーミングデータの取り込み](../ingestion/streaming-ingestion/overview.md)と以下の高度なセグメント化機能のいずれかを組み合わせることで、継続的にオーディエンスを生成するように設定できます。
- [順次セグメント化](#sequential)
- [動的セグメント化](#dynamic)
- [マルチエンティティセグメント化](#multi-entity)

これらの高度な機能について、以降の節で詳しく説明します。

## 順次セグメント化 {#sequential}

標準的なユーザージャーニーは、本質的に順次的です。Adobe Experience Platform では、一連のセグメントを定義してこのジャーニーを反映させ、発生したイベントのシーケンスをキャプチャできます。You can arrange events into their desired order by using the visual event timeline in the [!DNL Segment Builder].

順次セグメント化を必要とする顧客のジャーニーの例としては、製品表示／製品追加／チェックアウト／購入なしがあります。

## 動的セグメント化 {#dynamic}

動的セグメント化は、マーケティングキャンペーンのセグメントを作成する際に従来マーケターが直面していたスケーラビリティの問題を解決します。

すべての可能なユースケースを明示的に繰り返しキャプチャする必要がある静的セグメント化とは異なり、動的セグメント化では変数を使用してルールロジックを作成し、関係を動的に表現します。

### ユースケース：自宅以外で購入する顧客を探す

この高度なセグメント化機能の有用性を説明するために、ここでは、データアーキテクトがマーケターの協力を得て、自宅以外で購入した顧客を特定するとしましょう。

**問題**

静的セグメント化では、一意の「自宅の州」属性を持つ個々のセグメントを定義してから、「自宅の州」に一致しない購入イベントをフィルタリングで特定する必要があります。このタイプの明示的なセグメントは、例えば、「ユタ州在住で購入の州がユタでない人を探しています」となるでしょう。この方法を使用してオーディエンスを作成するには、米国の州ごとに 1 つのセグメントを定義し、合計 50 個のセグメントを定義する必要があります。

拡張するにつれて様々なセグメントの組み合わせが必然的に発生する結果、静的セグメント化に必要な手動プロセスにますます時間がかかり、全体の効率が低下します。

**解決策**

動的セグメント化では、購入州属性に変数を割り当てることで、「購入の州が顧客の自宅の州と異なる購入を見つける」ことが簡単になります。これにより、50 個の静的セグメントを 1 つの動的セグメントに統合できます。

## マルチエンティティセグメント化 {#multi-entity}

高度なマルチエンティティセグメント機能を使用すると、製品、店舗、または他の非人物（「ディメンション」エンティティとも呼ばれる）に基づいて、追加のデータを使用して [!DNL Real-time Customer Profile] データを拡張できます。 As a result, [!DNL Segmentation Service] can access additional fields during segment definition as if they were native to the [!DNL Profile] data store. マルチエンティティのセグメント化は、お客様固有のビジネスニーズに関連するデータに基づいてオーディエンスを識別する際に柔軟性を提供します。 使用事例やワークフローなどの詳細については、『 [マルチエンティティセグメント化ガイド](multi-entity-segmentation.md)』を参照してください。

## [!DNL Segmentation Service] データ型

[!DNL Segmentation Service] は、様々なプリミティブデータ型と複雑なデータ型をサポートしています。 サポートされるデータタイプのリストなど、詳細な情報については、 [サポートされるデータタイプガイドを参照してください](./data-types.md)。

## 次の手順

[!DNL Segmentation Service] は、データからセグメントを作成するための統合的なワークフローを提供し [!DNL Real-time Customer Profile] ます。 まとめ：

- [!DNL Segmentation] は、プロファイルストアからプロファイルのサブセットを定義するプロセスで、目的のマーケティング可能なグループの行動や属性を特徴付けることができます。 [!DNL Segmentation Service] このプロセスを可能にします。
- セグメントを計画する際、セグメントを他のセグメントから参照したり、他のセグメントと組み合わせたりできます。
- セグメントは、プロファイルデータ、関連する時系列データ、またはその両方に基づくルールから作成できます。
- セグメントは、オンデマンドで評価することも継続的に評価することもできます。オンデマンドで評価する場合は、すべてのプロファイルデータがセグメント定義を一度に通過します。When evaluated continuously, data streams through segment definitions as it enters [!DNL Platform].

UI でセグメントを定義する方法については、[セグメントビルダーガイド](./ui/overview.md)を参照してください。API を使用したセグメント定義の作成について詳しくは、[API を使用したセグメントの作成](./tutorials/create-a-segment.md)に関するチュートリアルを参照してください。