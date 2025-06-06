---
solution: Experience Platform
title: セグメント化サービスの概要
description: Adobe Experience Platform セグメント化サービスと、それがExperience Platform エコシステムで果たす役割について説明します。
exl-id: 2c18a806-88ed-4659-bdfd-2377f5a09a1a
source-git-commit: 0a9028beca36b46d6228c0038366bbac5d32603c
workflow-type: tm+mt
source-wordcount: '1679'
ht-degree: 88%

---

# [!DNL Segmentation Service] の概要

Adobe Experience Platform [!DNL Segmentation Service] は、[!DNL Real-Time Customer Profile] データからセグメント定義や他のソースを通じてオーディエンスを作成できるユーザーインターフェイスおよび RESTful API を提供します。これらのオーディエンスは、[!DNL Experience Platform] 上で一元的に設定および管理され、アドビのどのソリューションからでも簡単にアクセスできます。

このドキュメントでは、[!DNL Segmentation Service] の概要と Adobe Experience Platform での役割について説明します。

## [!DNL Segmentation Service] 入門

このドキュメント全体で使用される次の主な用語を理解する必要があります。

- **オーディエンス**：類似した行動や特性を共有する人の集まりです。このユーザーグループは、セグメント定義を使用したAdobe Experience Platform（Experience-Platform で生成されたオーディエンス）または外部ソース（外部で生成されたオーディエンス）から生成できます。
- **セグメント定義**：Adobe Experience Platform によって使用されるルールセット。ターゲットオーディエンスの主要な特性や行動を説明します。
- **セグメント**：プロファイルをオーディエンスに分割する行為です。

## セグメント化の仕組み

セグメント化は、プロファイルストアからのプロファイルのサブセットで共有される特定の属性や行動を定義して、マーケティング可能なユーザーグループを顧客ベースと区別するプロセスです。 例えば、「スニーカーを購入し忘れましたか？」というメールキャンペーンでは、過去 30 日間にランニングシューズを検索したが購入を完了しなかったすべてのユーザーのオーディエンスが必要な場合があります。

オーディエンスを概念的に定義したら、[!DNL Experience Platform] で作成されます。オーディエンスは通常、マーケターまたはオーディエンススペシャリストによって作成されますが、データアナリストと協力してマーケティング部門が作成することを望む組織もあります。[!DNL Experience Platform] に送信されるデータを確認する際、データアナリストは、2 つの方法（セグメント定義を作成する際にオーディエンスのルールや条件の作成に使用するフィールドや値を選択する、またはオーディエンス構成を使用してオーディエンスを構成する）でオーディエンスを作成できます。

## オーディエンスを作成

オーディエンスは、コンポジション、セグメント定義、Federated Data および Data Distillerなど、Adobe Experience Platform上で複数の方法で作成できます。

### オーディエンス構成

Experience Platformでオーディエンスを直接構成する場合は、オーディエンス構成を使用できます。 オーディエンス構成を使用してオーディエンスを作成する方法について詳しくは、[オーディエンス構成ガイド](./ui/audience-composition.md)を参照してください。

### セグメント定義

API を使用して作成した場合でも、[!DNL Segment Builder] を使用した場合でも、セグメント定義は最終的に [!DNL Profile Query Language]（PQL）を使用して定義されます。ここでは、条件を満たすプロファイルを取得するために設計された言語で概念セグメント定義を記述します。詳しくは、[PQL の概要](./pql/overview.md)を参照してください。

[!DNL Segment Builder]（[!DNL Segmentation Service] の UI 実装）でセグメントを作成して使用する方法については、[セグメントビルダーガイド](./ui/segment-builder.md)を参照してください。

API を使用したセグメント定義の作成について詳しくは、[API を使用したセグメント定義の作成](./tutorials/create-a-segment.md)に関するチュートリアルを参照してください。

>[!NOTE]
>
>イベントでは、スキーマが拡張され、以降のすべてのアップロードで、新しく追加されたフィールドを適宜更新する必要があります。[!DNL Experience Data Model]（XDM）のカスタマイズについて詳しくは、[スキーマエディターのチュートリアル](../xdm/tutorials/create-schema-ui.md)を参照してください。
>
>さらに、データセットでエクスペリエンスイベントの有効期限の値が有効になっている場合、これは、作成したセグメント定義のメンバーシップに影響を与える可能性があります。この機能がセグメント化に与える影響について詳しくは、[エクスペリエンスイベントの有効期限](../profile/event-expirations.md)に関するガイドを参照してください。

### 連合オーディエンス構成 {#fac}

オーディエンスの構成とセグメントの定義に加えて、Adobe Federated Audience Composition を使用すると、基になるデータをコピーせずにエンタープライズデータセットから新しいオーディエンスを作成し、それらのオーディエンスをAdobe Experience Platform Audience Portal に保存できます。 また、Enterprise Data Warehouse からフェデレーションされた作成済みオーディエンスデータを利用して、Adobe Experience Platformの既存のオーディエンスを強化することもできます。 [連合オーディエンス構成](https://experienceleague.adobe.com/ja/docs/federated-audience-composition/using/home)に関するガイドを参照してください。

## オーディエンスを評価 {#evaluate-segments}

>[!CONTEXTUALHELP]
>id="platform_segments_evaluation"
>title="評価方法"
>abstract="Experience Platform は、現在、ストリーミングセグメント化、バッチセグメント化、エッジセグメント化の 3 つのオーディエンス評価方法をサポートしています。"

>[!CONTEXTUALHELP]
>id="platform_segments_evaluation_streaming"
>title="ストリーミング評価"
>abstract="ストリーミングセグメント化は、ユーザーアクティビティに応じてオーディエンスを更新する継続的なデータ選択プロセスです。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/segmentation/methods/streaming-segmentation.html?lang=ja" text="ストリーミングセグメント化を使用してほぼリアルタイムでイベントを評価する"

Experience Platform は、現在、ストリーミングセグメント化、バッチセグメント化、エッジセグメント化の 3 つのオーディエンス評価方法をサポートしています。

### ストリーミングセグメント化 {#streaming}

ストリーミングセグメント化は、ユーザーアクティビティに応じてオーディエンスを更新する継続的なデータ選択プロセスです。オーディエンスが作成され保存されると、受信データに対してセグメント定義が [!DNL Real-Time Customer Profile] に適用されます。ターゲットオーディエンスの関連性が維持されるよう、オーディエンスへの追加および削除は定期的に処理されます。

ストリーミングセグメント化について詳しくは、[ストリーミングセグメント化のドキュメント](./methods/streaming-segmentation.md)を参照してください。

### バッチセグメント化 {#batch}

>[!CONTEXTUALHELP]
>id="platform_segments_evaluation_batch"
>title="バッチ評価"
>abstract="継続的なデータ選択プロセスの代わりに、バッチセグメント化では、セグメント定義を介してすべてのプロファイルデータを一括して移動し、対応するオーディエンスを生成します。作成したオーディエンスは保存されるので、書き出して使用できます。"

継続的なデータ選択プロセスに代わる手段として、バッチセグメント化では、セグメント定義を通じてすべてのプロファイルデータを一度に移動して、対応するオーディエンスを生成します。作成したオーディエンスは保存されるので、書き出して使用できます。

バッチオーディエンスは、24 時間ごとに自動的に評価されます。バッチオーディエンスをオンデマンドで評価する場合は、セグメントジョブを使用できます。セグメントジョブについて詳しくは、[セグメントジョブのドキュメント](./api/segment-jobs.md)を参照してください。

### エッジのセグメント化 {#edge}

>[!CONTEXTUALHELP]
>id="platform_segments_evaluation_edge"
>title="エッジ評価"
>abstract="エッジセグメント化は、Experience Platform のセグメントを Edge Network 上で瞬時に評価する機能で、同じページや次のページのパーソナライゼーションのユースケースを可能にします。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/segmentation/methods/edge-segmentation.html?lang=ja" text="エッジセグメント化ガイド"

Edgeのセグメント化は、Experience Platform内のセグメントを [Edge Network上で ](../landing/edge-and-hub-comparison.md) 瞬時に評価する機能で、同じページでのパーソナライゼーションや次のページのパーソナライゼーションのユースケースを可能にします。

エッジセグメント化について詳しくは、[ エッジセグメント化の概要 ](./methods/edge-segmentation.md) を参照してください。

## セグメント化の結果へのアクセス

書き出したオーディエンスにアクセスする方法について詳しくは、[セグメント定義の評価に関するチュートリアル](./tutorials/evaluate-a-segment.md)を参照してください。

## セグメント定義メタデータ

任意のオーディエンスを再利用または組み合わせる場合は、セグメント定義メタデータを使用するとインデックス作成が便利になります。

（API または [!DNL Segment Builder] を使用して）セグメント定義を作成するには、名前および結合ポリシーを定義する必要があります。

### セグメント定義名

新しいセグメント定義を作成する場合は、名前を指定する必要があります。セグメント定義名は、[!DNL Segmentation Service] で作成されたコレクションの中の特定のセグメント定義を識別するために使用されます。したがって、セグメント定義名は説明的で簡潔かつ一意である必要があります。

>[!NOTE]
>
>セグメント定義を計画する際、セグメント定義を他のセグメント定義から参照したり、他のセグメント定義と組み合わせたりできます。名前を選択する際は、セグメント定義に再利用可能な部分が含まれているかもしれないことを考慮してください。

### 結合ポリシー

結合ポリシーは、特定の条件下でデータを優先順位付けし統合ビューにまとめる方法を [!DNL Profile] が決定する場合に使用するルールです。

結合ポリシーが定義されていない場合、デフォルトの [!DNL Experience Platform] 結合ポリシーが使用されます。組織に特有の結合ポリシーを使用する場合は、独自の結合ポリシーを作成し、組織のデフォルトとすることができます。

結合ポリシーについて詳しくは、[結合ポリシーガイド](../profile/api/merge-policies.md)を参照してください。

>[!NOTE]
>
>オーディエンスサイズの推定は、組織のデフォルトのプロファイル結合ポリシーに基づいて行われます。

### その他のセグメント定義のメタデータ

名前と結合ポリシーに加えて、[!DNL Segment Builder] には、セグメント定義の目的を要約できる追加の説明メタデータフィールドが用意されています。

## 高度なセグメント化機能

セグメント定義は、[ストリーミングデータの取り込み](../ingestion/streaming-ingestion/overview.md)と以下の高度なセグメント化機能のいずれかを組み合わせることで、継続的にオーディエンスを生成するように設定できます。

- [順次セグメント化](#sequential)
- [動的セグメント化](#dynamic)
- [マルチエンティティのセグメント化](#multi-entity)

これらの高度な機能について、以降の節で詳しく説明します。

### 順次セグメント化 {#sequential}

標準的なユーザージャーニーは、本質的に順次的です。Adobe Experience Platform では、一連のオーディエンスを定義してこのジャーニーを反映させ、発生したイベントのシーケンスをキャプチャできます。[!DNL Segment Builder] の視覚的なイベントタイムラインを使用することで、イベントを目的の順序に並べ替えることができます。

順次セグメント化を必要とする顧客のジャーニーの例としては、製品表示／製品追加／チェックアウト／購入なしがあります。

### 動的セグメント化 {#dynamic}

動的セグメンテーションは、マーケティングキャンペーンのオーディエンスを作成する際に従来マーケターが直面していたスケーラビリティの問題を解決します。

すべての可能なユースケースを明示的に繰り返しキャプチャする必要がある静的セグメント化とは異なり、動的セグメント化では変数を使用してルールロジックを作成し、関係を動的に表現します。

この高度なセグメント化機能の有用性を説明するために、ここでは、データアーキテクトがマーケターと協力して、自宅の州以外で購入した顧客を特定するとします。

静的セグメント化では、一意の「自宅の州」属性を持つ個々のセグメントを定義してから、「自宅の州」に一致しない購入イベントをフィルタリングで特定する必要があります。このタイプの明示的なセグメント定義は、例えば、「ユタ州在住で、かつユタ州以外で購入した人物を探しています」のようになります。この方法を使用してオーディエンスを作成するには、米国の州ごとに 1 つのセグメント定義を定義し、合計 50 個のセグメントを定義する必要があります。

拡張するにつれて必然的に、様々なセグメント定義の組み合わせが発生した結果、静的セグメント化に必要な手動プロセスにかかる時間が増え、全体の効率が低下します。

動的セグメント定義では、購入州属性に変数を割り当てることで、「購入の州が顧客の自宅の州と異なる購入を見つける」ことが簡単になります。これにより、50 個の静的セグメントを 1 つの動的セグメント定義へと統合できます。

### マルチエンティティのセグメント化 {#multi-entity}

高度なマルチエンティティのセグメント化機能により、製品、ストア、その他の非人物（「ディメンション」エンティティとも呼ばれる）に基づく追加データで [!DNL Real-Time Customer Profile] データを拡張できます。その結果、[!DNL Segmentation Service] は、セグメント定義時に、[!DNL Profile] データストア本来の方法と同じように、追加のフィールドにアクセスできます。マルチエンティティのセグメント化は、独自のビジネスニーズに関連するデータに基づいて、オーディエンスを識別する際に、柔軟性をもたらします。ユースケースおよびワークフローを含め、詳しくは[マルチエンティティのセグメント化ガイド](./tutorials/multi-entity-segmentation.md)を参照してください。

## [!DNL Segmentation Service] データタイプ

[!DNL Segmentation Service] は、様々なプリミティブおよび複雑なデータタイプをサポートします。サポートされるデータタイプのリストを含め、詳しくは、[サポートされるデータタイプガイド](./data-types.md)を参照してください。

## 次の手順

[!DNL Segmentation Service] は、[!DNL Real-Time Customer Profile] データからオーディエンスを作成するための統合ワークフローを提供します。

セグメント化サービス UI の使用方法の詳細については、[セグメント化サービス UI の概要](./ui/overview.md)を参照してください。

UI でオーディエンスを構成する方法については、[オーディエンス構成ガイド](./ui/audience-composition.md)を参照してください。UI でセグメント定義を定義する方法については、[セグメントビルダーガイド](./ui/overview.md)を参照してください。API を使用したセグメント定義の作成について詳しくは、[API を使用したセグメント定義の作成](./tutorials/create-a-segment.md)に関するチュートリアルを参照してください。
