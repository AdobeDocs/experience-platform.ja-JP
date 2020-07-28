---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform セグメント化サービス
topic: overview
translation-type: tm+mt
source-git-commit: 96b6f820e5d372446c4927e7719aedadb2b11bc9
workflow-type: tm+mt
source-wordcount: '1974'
ht-degree: 73%

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

高度なマルチエンティティセグメント化機能を使用すると、複数の XDM クラスを使用してセグメントを作成し、個人のスキーマに拡張機能を追加できます。As a result, [!DNL Segmentation Service] can access additional fields during segment definition as if they were native to the profile data store.

マルチエンティティセグメント化は、ビジネスニーズに関係のあるデータに基づいてオーディエンスを特定する場合に必要な柔軟性を提供します。このプロセスは、データベースのクエリに関する専門知識がなくても、すばやく簡単に実行できます。これにより、データストリームにコストの高い変更を加えたり、バックエンドでのデータ結合を待機したりしなくても、セグメントに主要なデータを追加できます。

次のビデオでは、マルチエンティティのセグメント化について理解を深めることを目的としており、マルチエンティティのセグメント化とセグメントのコンテキスト（セグメントのペイロード）の概要を説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/28947?quality=12&learn=on)

### ユースケース：価格主導型プロモーション

この高度なセグメント機能の有用性を説明するために、データアーキテクトがマーケターと協力するとしましょう。

In this example, the data architect is joining data for an individual (made up of schemas with [!DNL XDM Individual Profile] and [!DNL XDM ExperienceEvent] as their base classes) to another class using a key. 結合後、データアーキテクトまたはマーケターは、セグメント定義時に、これらの新しいフィールドを基本クラスのスキーマ本来の方法と同じように使用できます。

**問題**

データアーキテクトとマーケターのクライアントは同じ衣料品店です。その小売店は、北米全域で 1,000 店を超える店舗を持ち、ライフサイクル全体で定期的に製品価格を引き下げます。そのため、マーケターは、特別なキャンペーンを実施して、同店で買い物をした客に、割引価格で買い物ができるチャンスを提供したいと考えています。

データアーキテクトのリソースには、顧客の Web 閲覧データへのアクセス、および製品の SKU 識別子を含んだ買い物かごの追加データへのアクセスが含まれています。また、別の「products」クラスにもアクセスできます。このクラスには、追加の製品情報（製品価格など）が格納されています。アドバイスとしては、過去 14 日間に製品を買い物かごに追加したが購入に至らなかった顧客に注目することです。この製品の価格は現在下がっています。

**解決策**

>[!NOTE]
>
> この例では、データアーキテクトが既に ID 名前空間を確立しているとします。

Using the API, the data architect relates the key from the [!DNL ExperienceEvent] schema with the &quot;products&quot; class. Doing so allows the data architect to make use of the additional fields from the &quot;products&quot; class as if they are native to the [!DNL ExperienceEvent] schema. As the final step of the configuration work, the data architect needs to bring the appropriate data into [!DNL Real-time Customer Profile]. This is done by enabling the &quot;products&quot; dataset for use with [!DNL Profile]. With the configuration work complete, either the data architect or the marketer can build the target segment in [!DNL Segment Builder].

XDM クラス間の関係を定義する方法については、[スキーマ構成の概要](../xdm/schema/composition.md#union)を参照してください。

<!-- ## Personalization payload

Segments can now carry a payload of contextual details to enable deep personalization of Adobe Solutions as well as external non-Adobe applications. These payloads can be added while defining your target segment.

With contextual data built into the segment itself, this advanced Segmentation Service feature allows you to better connect with your customer.

Segment Payload helps you answer questions surrounding your customer’s frame of reference such as:
- What: What product was purchased? What product should be recommended next?
- When: At what time and date did the purchase occur?
- Where: In which store or city did the customer make their purchase?

While this solution does not change the binary nature of segment membership, it does add additional context to each profile through an associated segment membership object. Each segment membership object has the capacity to include three kinds of contextual data:

- **Identifier**: this is the ID for the segment 
- **Attributes**: this would include information about the segment ID such as last qualification time, XDM version, status and so on.
- **Event data**: Specific aspects of experience events which resulted in the profile qualifying for the segment

Adding this specific data to the segment itself allows execution engines to personalize the experience for the customers in their target audience. -->

### ユースケース

この高度なセグメント機能の有用性を説明するために、セグメントペイロードの機能強化がおこなわれる以前にマーケティングアプリケーションが抱えていた課題を示す 3 つの標準的なユースケースを検討しましょう。
- E メールのパーソナライゼーション
- E メールのリターゲティング
- 広告のリターゲティング

**E メールのパーソナライゼーション**

E メールキャンペーンを作成しているマーケターは、過去 3 ヶ月間における店舗での顧客の購入履歴を使用して、ターゲットオーディエンスのセグメントを作成しようとしたかもしれません。理想的には、商品名と購入がおこなわれた店舗の名前の両方がこのセグメントに必要です。機能強化がおこなわれる前は、購入イベントからストア ID を取り込み、それをその顧客のプロファイルに割り当てていました。

**E メールのリターゲティング**

多くの場合、「買い物かごの放棄」をターゲットとする E メールキャンペーンのセグメントを作成し認定するのは複雑な作業です。この機能強化の前は、必要なデータが利用できないため、パーソナライズされたメッセージに含める製品を把握するのは困難でした。製品が破棄された場合のデータは、以前はデータの監視と抽出が困難だったエクスペリエンスイベントに結び付けられます。

**広告のリターゲティング**

マーケターにとって、従来のもう 1 つの課題は、買い物かごの品目を破棄した顧客を再度ターゲットにする広告を作成することでした。セグメント定義はこの課題に対応しましたが、機能強化の前は、購入した製品と破棄した製品を区別する系統的な方法はありませんでした。セグメントの定義中に、特定のターゲットセットを定義できるようになりました。

## [!DNL Segmentation Service] データ型

[!DNL Segmentation Service] は、次のような様々なデータ型をサポートしています。

- 文字列
- URI（統一リソース識別子）
- 列挙
- 数値
- 長整数
- 整数
- 短整数
- バイト
- ブール値
- 日付
- 日時
- 配列
- オブジェクト
- マップ
- イベント
- 外部オーディエンス
- セグメント

サポートされるこれらのデータ型について詳しくは、 [サポートされるデータ型のドキュメントを参照してください](./data-types.md)。

## 次の手順

[!DNL Segmentation Service] は、データからセグメントを作成するための統合的なワークフローを提供し [!DNL Real-time Customer Profile] ます。 まとめ：

- [!DNL Segmentation] は、プロファイルストアからプロファイルのサブセットを定義するプロセスで、目的のマーケティング可能なグループの行動や属性を特徴付けることができます。 [!DNL Segmentation Service] このプロセスを可能にします。
- セグメントを計画する際、セグメントを他のセグメントから参照したり、他のセグメントと組み合わせたりできます。
- セグメントは、プロファイルデータ、関連する時系列データ、またはその両方に基づくルールから作成できます。
- セグメントは、オンデマンドで評価することも継続的に評価することもできます。オンデマンドで評価する場合は、すべてのプロファイルデータがセグメント定義を一度に通過します。When evaluated continuously, data streams through segment definitions as it enters [!DNL Platform].

UI でセグメントを定義する方法については、[セグメントビルダーガイド](./ui/overview.md)を参照してください。API を使用したセグメント定義の作成について詳しくは、[API を使用したセグメントの作成](./tutorials/create-a-segment.md)に関するチュートリアルを参照してください。