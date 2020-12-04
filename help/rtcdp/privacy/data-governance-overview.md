---
keywords: data governance rtcdp;rtcdp data governance;real time customer data profile data governance
title: データガバナンスの概要
seo-title: リアルタイム顧客データプラットフォームにおけるデータガバナンス
description: 'データガバナンスを使用すると、顧客データを管理し、データの使用に適した規制、制限、ポリシーへのコンプライアンスを確保できます。 '
seo-description: 'データガバナンスを使用すると、顧客データを管理し、データの使用に適した規制、制限、ポリシーへのコンプライアンスを確保できます。 '
translation-type: tm+mt
source-git-commit: 91b60539010318ea8d545bff4e5cc7e2d0aa70fc
workflow-type: tm+mt
source-wordcount: '1588'
ht-degree: 30%

---


# [!DNL Data Governance] リアルタイムCDP

[!DNL Real-time Customer Data Platform] （リアルタイムCDP）は、複数のエンタープライズ・システムからのデータを統合し、マーケティング担当者が顧客をより良く識別、理解、関与できるようにします。 このデータは、組織または法規制によって定義された使用制限の対象となる場合があります。したがって、リアルタイム CDP が使用ポリシーに準拠していることを確認し、データを処理することが重要です。

Adobe Experience Platform [!DNL Data Governance] allows you to manage customer data and ensure compliance with regulations, restrictions, and policies applicable to data use. データガバナンスは リアルタイム CDP 内で重要な役割を果たし、使用ポリシーの定義、それらのポリシーに基づくデータの分類、特定のマーケティングアクションの実行時のポリシー違反を確認できるようになります。

Real-time CDP is built on top of Adobe Experience Platform, and therefore the majority of [!DNL Data Governance] capabilities are covered in the [!DNL Experience Platform] documentation. 本書は、 の『[データガバナンスの概要](../../data-governance/home.md)』を補完するものであり、リアルタイム CDP で利用可能なガバナンス機能の概要を説明しています。[!DNL Experience Platform]以下のトピックを取り上げます。

* [データへの使用状況ラベルの適用](#labels)
* [データ使用ポリシーの管理](#policies)
* [データ使用コンプライアンスの実施](#enforce-data-usage-compliance)

## データへの使用状況ラベルの適用 {#labels}

[!DNL Data Governance] 使用状況ラベルをデータセットレベルまたはデータセットフィールドレベルでデータに適用できます。 データ使用状況ラベルを使用すると、データに適用される使用ポリシーに従ってデータを分類できます。

データ使用状況ラベルの使用について詳しくは、Adobe Experience Platform の『[データ使用ラベルユーザーガイド](../../data-governance/labels/overview.md)』を参照してください。

## 宛先のマーケティング使用例の設定 {#destinations}

宛先にデータの使用制限を設定するには、その宛先に対するマーケティングの使用例（マーケティングアクションとも呼ばれます）を定義します。 宛先のマーケティングの使用例は、その宛先にエクスポートされるデータの意図を示します。

>[!NOTE]
>
>マーケティングアクションとデータ使用ポリシーでのその使用について詳しくは、ドキュメントの [データ使用ポリシーの概要](../../data-governance/policies/overview.md)[!DNL Experience Platform] を参照してください。

宛先に対するマーケティングの使用例を定義すると、それらの宛先に送信されるプロファイルやセグメントがデータ使用ポリシーに確実に準拠していることを確認できます。 したがって、アクティベーションに対するポリシー制限を実施するための組織のニーズに基づいて、目的のマーケティングの使用例を宛先に追加する必要があります。

マーケティングの使用例は、宛先を初めて設定する場合にのみ選択できます。 操作している宛先のタイプに応じて、マーケティングの使用例を設定するオポチュニティは、セットアップワークフローの様々なポイントに表示されます。 特定の [宛先を設定する手順については、](../destinations/overview.md) destinationsのドキュメントを参照してください。

## データ使用ポリシーの管理 {#policies}

データ使用状況ラベルがデータのコンプライアンスを効果的にサポートするためには、データ使用ポリシーを定義し、有効にする必要があります。データ使用ポリシーは、リアルタイム CDP 内のデータに対して実行を許可（／制限）するマーケティングアクションの種類を記述するルールです詳しくは、 で『[!DNL Experience Platform][データガバナンスの概要](../../data-governance/home.md)』の「データ使用ポリシー」の節を参照してください。

Adobe Experience Platform では、一般的な顧客体験の使用例に対して、いくつかのコアポリシーがあります。これらのポリシーは、「 **[!UICONTROL ポリシー]** 」ワークスペースに移動し、「 **[!UICONTROL 参照]** 」タブを選択すると、UIで表示できます。 独自のカスタムポリシーの作成方法など、UIでのポリシーの操作に関する詳細な手順については [、](../../data-governance/policies/user-guide.md) ドキュメントの「 [!DNL Experience Platform] policiesユーザーガイド」を参照してください。

## データ使用コンプライアンスの実施 {#enforce-data-usage-compliance}

データにラベルが付けられ、使用ポリシーが定義されたら、データ使用に対するポリシーのコンプライアンスを適用できます。When activating audience segments to destinations in Real-time CDP, [!DNL Data Governance] automatically enforces usage policies should any violations occur.

次の図は、ポリシーの実施により、セグメントのアクティベーションのデータフローにどのように統合されるかを示しています。

<img src="assets/governance/enforcement-flow.png" width="650">

When a segment is first activated, [!DNL Policy Service] checks for policy violations based on the following factors:

* アクティブ化するセグメント内のフィールドおよびデータセットに適用される、データ使用ラベル。
* 宛先のマーケティングの目的。

>[!NOTE]
>
>データセット内の特定のフィールド（データセット全体ではなく）にのみ適用されたデータ使用量ラベルがある場合、アクティベーションに対するこれらのフィールドレベルラベルの適用は、次の条件でのみ発生します。
>* これらのフィールドは、セグメント定義で使用されます。
>* フィールドは、ターゲット先の投影属性として設定されます。


### データ系列 {#lineage}

リアルタイムCDPでは、ポリシーの適用にデータ系列が重要な役割を果たします。 一般的に、データ系列とは、一連のデータの接触チャネルを指し、時間の経過と共にその一連のデータに何が起こるか（またはデータの移動場所）を指します。

Linegageというコンテキストで [!DNL Data Governance]は、データ使用ラベルを、データを消費するデータセットから、リアルタイム顧客プロファイルや宛先などのダウンストリーム・サービスに伝播できます。 これにより、プラットフォームを通じたデータの遍歴のいくつかの重要なポイントでポリシーの評価と実施が可能になり、ポリシー違反が発生した理由に関するデータ・コンシューマへのコンテキストが提供されます。

リアルタイムCDPでは、ポリシーの適用は次の系統に関係しています。

1. データはリアルタイムCDPに取り込まれ、 **データセットに格納されます**。
1. 顧客プロファイルは、 **マージ・ポリシーに従ってデータ・フラグメントをマージすることによって、これらのデータセットから識別され、構築されます**。
1. プロファイルのグループは、共通の属性に基づいて **セグメント** に分けられます。
1. セグメントはダウンストリームの **宛先に対してアクティブ化されます**。

上のタイムラインの各ステージは、次の表に示すように、ポリシーの違反に貢献する可能性のあるエンティティを表します。

| データ系列のステージ | ポリシーの適用での役割 |
| --- | --- |
| データセット | データセットには、データセット全体または特定のフィールドをどの用途に使用できるかを定義する、データ使用量ラベル（データセットレベルまたはフィールドレベルで適用）が含まれます。 ポリシー違反は、特定のラベルを含むデータセットまたはフィールドをポリシーが制限する目的で使用した場合に発生します。 |
| マージポリシー | マージポリシーは、複数のデータセットからフラグメントを結合する場合に、データの優先順位付け方法を決定するためにプラットフォームで使用される規則です。 制限付きラベルを含むデータセットが宛先に対してアクティブ化されるようにマージポリシーが設定されている場合、ポリシー違反が発生します。 See the guide on [merge policies](../../profile/ui/merge-policies.md) for more information. |
| セグメント | セグメントルールは、顧客プロファイルから含める属性を定義します。 セグメント定義に含まれるフィールドに応じて、セグメントは、これらのフィールドに適用された使用ラベルを継承します。 ポリシー違反は、継承ラベルがターゲット先の適用可能なポリシーによって、そのマーケティングの使用例に基づいて制限されているセグメントをアクティブ化すると発生します。 |
| 宛先 | 宛先を設定する際に、マーケティングアクション（マーケティングの使用例とも呼ばれます）を定義できます。 この使用例は、データ使用ポリシーで定義されたマーケティングアクションに関連しています。 つまり、宛先に対して定義したマーケティングの使用例によって、その宛先に適用されるデータ使用ポリシーが決まります。 ポリシー違反は、使用ラベルがターゲット先の適用可能なポリシーによって制限されているセグメントをアクティブ化すると発生します。 |

ポリシー違反が発生した場合、UIに表示される結果のメッセージには、違反の貢献データ系列を調べ、問題の解決に役立つツールが用意されています。 詳しくは、次のセクションを参照してください。

### ポリシー違反メッセージ {#enforcement}

セグメントをアクティブ化（または[既にアクティブ化されたセグメントを編集](#policy-enforcement-for-activated-segments)）しようとするとポリシー違反が発生した場合、アクションは実行されず、1 つ以上のポリシーに違反したことを示すポップオーバーが表示されます。Once a violation has triggered, the **[!UICONTROL Save]** button is disabled for the entity you are modifying until the appropriate components are updated to comply with data usage policies.

ポリシー違反の詳細を表示するには、その違反の左列のポップオーバーでポリシー違反を選択します。

![](assets/governance/violation-policy-select.png)

違反メッセージには、ポリシーがチェック対象として設定されている条件、違反をトリガーした特定のアクション、問題の解決のリストなど、違反したポリシーの概要が表示されます。

![](assets/governance/violation-summary.png)

違反の概要の下にデータ系列グラフが表示され、ポリシー違反に関与したデータセット、マージポリシー、セグメントおよび宛先を視覚化できます。 現在変更中のエンティティがグラフ内でハイライト表示され、違反が発生する原因となっているフロー内のポイントを示します。 グラフ内でエンティティ名を選択して、対象のエンティティの詳細ページを開くことができます。

![](assets/governance/data-lineage.png)

「 **[!UICONTROL フィルタ]** 」アイコン(![](./assets/governance/filter.png))を使用して、表示エンティティをカテゴリでフィルタリングすることもできます。 データを表示するには、少なくとも2つのカテゴリを選択する必要があります。

![](assets/governance/lineage-filter.png)

リスト **[!UICONTROL 表示]** を選択し、データ系列をリストとして表示します。 ビジュアルグラフに戻すには、「 **[!UICONTROL パス表示]**」を選択します。

![](assets/governance/list-view.png)

### アクティブ化されたセグメントに対するポリシー施行 {#policy-enforcement-for-activated-segments}

ポリシーの施行は、アクティブ化された後も引き続きセグメントに適用され、ポリシー違反の原因となったセグメントや宛先に対する変更が制限されます。ポリシーの適用での [データ系列](#lineage) の動作が原因で、次の操作を実行すると違反が発生する可能性があります。

* データ使用ラベルの更新
* セグメントのデータセットの変更
* セグメント述語の変更
* 宛先設定の変更

上記のアクションのいずれかで違反がトリガーされると、そのアクションは保存されず、ポリシー違反のメッセージが表示され、データ使用ポリシーが変更されると、アクティブ化されたセグメントが引き続きそのポリシーを遵守するようにします。

## 次の手順

Now that you have been introduced to the key [!DNL Data Governance] features on Real-time CDP and how [!DNL Experience Platform] enables them, please continue to the [documentation for Data Governance on Adobe Experience Platform](../../data-governance/home.md). The documentation provides overviews of essential [!DNL Data Governance] concepts, as well as step-by-step workflows for managing data usage labels and policies.

次のビデオは、宛先でのマーケティングの使用例 [!DNL Data Governance] の使用、様々なシナリオでのワークフロー例を含む、Real-time CDPの概要を示しています。

>[!VIDEO](https://video.tv.adobe.com/v/33631?quality=12&learn=on)