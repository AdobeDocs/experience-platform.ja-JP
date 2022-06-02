---
keywords: Experience Platform；ホーム；人気のトピック；セグメント化；セグメント化；セグメント化サービス；ユーザーガイド；ui ガイド；セグメント化 ui ガイド；セグメントビルダー；適合済み；既存；既存；
solution: Experience Platform
title: セグメント化サービス UI ガイド
topic-legacy: ui guide
description: Adobe Experience Platform Segmentation Service は、セグメント定義を作成および管理するためのユーザーインターフェイスを提供します。
exl-id: 0a2e8d82-281a-4c67-b25b-08b7a1466300
source-git-commit: 71741a18c99a003e6401bc324822d50a266350b3
workflow-type: tm+mt
source-wordcount: '1746'
ht-degree: 20%

---

# セグメント化サービス UI ガイド

[!DNL Adobe Experience Platform Segmentation Service] は、セグメント定義を作成および管理するためのユーザーインターフェイスを提供します。

## はじめに

セグメントの定義を使用するには、 [!DNL Experience Platform] セグメント化に関連するサービス。 このユーザガイドを読む前に、次のサービスのドキュメントを確認してください。

- [[!DNL Segmentation Service]](../home.md): [!DNL Segmentation Service] では、 [!DNL Experience Platform] 小さなグループに分類された個人（顧客、見込み客、ユーザー、組織など）に関連する
- [[!DNL Real-time Customer Profile]](../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
- [[!DNL Adobe Experience Platform Identity Service]](../../identity-service/home.md):に取り込まれる様々なデータソースの ID を関連付けることで、顧客プロファイルの作成を可能にします。 [!DNL Platform].
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：[!DNL Platform] がカスタマーエクスペリエンスのデータの整理に使用する、標準化されたフレームワーク。セグメント化を最適に利用するには、 [データモデリングのベストプラクティス](../../xdm/schema/best-practices.md).

また、このドキュメントを通して使用される次の 2 つの重要用語を知り、その違いを理解することも重要です。
- **セグメント定義**：ターゲットオーディエンスの重要な特徴やビヘイビアーの説明に使用されるルールセットです。
- **オーディエンス**：セグメント定義の条件を満たす一連のプロファイルです。

## 概要

Experience PlatformUI で、 **[!UICONTROL セグメント]** 左側のナビゲーションで、 **[!UICONTROL 概要]** タブに [!UICONTROL セグメント] ダッシュボード。

>[!NOTE]
>
>組織が Platform を初めて使用し、アクティブなプロファイルデータセットや結合ポリシーがまだ作成されていない場合、 [!UICONTROL セグメント] ダッシュボードが表示されません。 代わりに、 [!UICONTROL 概要] 「 」タブには、セグメントの使用を開始するのに役立つリンクとドキュメントが表示されます。

### [!UICONTROL セグメントダッシュボード] {#segments-dashboard}

この **[!UICONTROL セグメント]** ダッシュボードでは、組織のセグメントデータに関連する主要指標の概要を説明します。

詳しくは、 [セグメントダッシュボードガイド](../../dashboards/guides/segments.md).

![](../../dashboards/images/segments/dashboard-overview.png)

## 参照 {#browse}

>[!CONTEXTUALHELP]
>id="platform_segments_browse_churncolumnname"
>title="チャーン"
>abstract="チャーンは、セグメントジョブが最後に実行された時点と比較して、セグメント定義内で変更されているプロファイルの割合を表します。"

>[!CONTEXTUALHELP]
>id="platform_segments_browse_evaluationmethodcolumnname"
>title="評価方法"
>abstract="セグメントの評価方法には、バッチ、ストリーミング、エッジが含まれます。"

を選択します。 **[!UICONTROL 参照]** タブをクリックして、IMS 組織のすべてのセグメント定義のリストを表示します。

![](../images/ui/overview/segment-browse-all.png)

このビューには、セグメント定義に関する情報（分類、チャーン、プロファイル数、評価方法、作成日、最終変更日を含む）が表示されます。

分類には、次の各ステータスに属するプロファイルの割合を示す棒グラフが表示されます。 [!UICONTROL 実現済み], [!UICONTROL 既存]、および [!UICONTROL 終了]. また、 [!UICONTROL 参照] タブは、セグメントのステータスの最も正確な分類です。 この数が [!UICONTROL 概要] 」タブでは、 [!UICONTROL 参照] 」タブを正しい情報源として追加します。 [!UICONTROL 概要] タブ番号は 1 日に 1 回だけ更新されます。

![](../images/ui/overview/segment-browse-breakdown.png)

| ステータス | 説明 |
| ------ | ----------- |
| 実現済み | セグメント内の新しいプロファイル。 |
| 既存 | セグメント内に残っている既存のプロファイル。 |
| 終了 | セグメントから離脱する既存のプロファイル。 |

チャーンは、セグメントジョブが最後に実行された時点と比較して、セグメント定義内で変更されているプロファイルの割合を表し、プロファイル数は、セグメントに適合するプロファイルの合計数を表します。

評価方法は、ストリーミング、バッチ、エッジのいずれかです。 ストリーミングセグメントは、データがシステムに入力されるたびに評価されます。バッチセグメントは、設定されたスケジュールに従って評価されます。Edge セグメントはリアルタイムに評価され、同じページや次のページのパーソナライゼーションの使用例に使用できます。

![](../images/ui/overview/segment-browse-segments.png)

ページの上部には、すべてのセグメントをスケジュールに追加したり、新しいセグメントを作成したりするオプションがあります。

切り替え **[!UICONTROL すべてのセグメントをスケジュールに追加]** はスケジュールされたセグメント化を有効にします。 スケジュールに沿ったセグメント化について詳しくは、 [このユーザーガイドのスケジュールされたセグメント化に関する節](#scheduled-segmentation).

選択 **[!UICONTROL セグメントを作成]** がセグメントビルダーに移動します。 セグメントの作成の詳細については、 [ユーザーガイドでのセグメントの作成](#create-segment).

![](../images/ui/overview/segment-browse-top.png)

右側のサイドバーには、IMS 組織内のすべてのセグメントに関する情報が含まれ、セグメントの合計数、最終評価日、次の評価日のほか、評価方法別のセグメントの分類が表示されます。

![](../images/ui/overview/segment-browse-segment-info.png)

セグメント定義の行を選択すると、セグメントの編集または削除、宛先へのセグメントのアクティブ化、セグメントの対象オーディエンス、合計オーディエンスサイズに加え、セグメント名、説明、評価方法、作成日、最終変更日が表示されます。

>[!NOTE]
>
> 以下をおこないます。 **not** を使用すれば、宛先のアクティベーションで使用されているセグメントを削除できます。

![](../images/ui/overview/segment-browse-details.png)

## セグメント定義の詳細 {#segment-details}

特定のセグメント定義の詳細を表示するには、 **[!UICONTROL 参照]** タブをクリックします。

セグメントの詳細ページが表示されます。 上部には、セグメント定義の概要、対象オーディエンスのサイズに関する情報、およびセグメントがアクティブ化される宛先があります。

![](../images/ui/overview/segment-details-summary.png)

### セグメントの概要

この **[!UICONTROL セグメントの概要]** 「 」セクションには、ID、名前、説明、属性の詳細などの情報が表示されます。

また、宛先に対してセグメントをアクティブ化するか、セグメントを編集するかのどちらかのオプションが提供されます。 選択 **[!UICONTROL 宛先に対して有効化]** が宛先に対してセグメントをアクティブ化します。 宛先へのセグメントのアクティブ化について詳しくは、 [アクティベーションの概要](../../destinations/ui/activation-overview.md).

![](../images/ui/overview/segment-details-activate.png)

選択 **[!UICONTROL セグメントを編集]** 君を連れて行く [!DNL Segment Builder]. 詳しくは、 [!DNL Segment Builder] ワークスペース、お読みください [[!DNL Segment Builder] ユーザーガイド](./segment-builder.md).

![](../images/ui/overview/segment-details-edit-segment.png)

### セグメント内の合計オーディエンス

この **[!UICONTROL セグメント内の合計オーディエンス]** 「 」セクションには、セグメントに適合するプロファイルの合計数が表示されます。

見積もりは、その日のサンプルデータのサンプルサイズを使用して生成されます。 プロファイルストアのエンティティ数が 100 万個未満の場合は、データセット全体が使用されます。100 万個から 2,000 万個のエンティティがある場合は、100 万個のエンティティが使用されます。2,000 万個を超えるエンティティがある場合は、合計エンティティ数の 5％が使用されます。セグメントの推定サイズを生成する方法について詳しくは、セグメントの作成に関するチュートリアルの[予測値の生成に関する節](../tutorials/create-a-segment.md#estimate-and-preview-an-audience)を参照してください。

### アクティブ化された宛先

この **[!UICONTROL アクティブ化された宛先]** 「 」セクションには、このセグメントがアクティブ化される宛先が表示されます。

>[!NOTE]
>
> 宛先は、 [!DNL Real-time Customer Data Platform]を使用して外部プラットフォームにデータを書き出すことができます。 宛先について詳しくは、 [宛先の概要](../../destinations/home.md). 宛先に対してセグメントをアクティブ化する方法については、 [アクティベーションの概要](../../destinations/ui/activation-overview.md).

### プロファイルサンプル

の下には、セグメントに適合するプロファイルのサンプリングがあり、 [!DNL Profile] ID、名、姓および個人の電子メール。

データサンプリングがトリガーされる方法は、取り込み方法によって異なります。

バッチ取得の場合、プロファイルストアは 15 分ごとに自動的にスキャンされ、最後のサンプリングジョブの実行後に新しいバッチが正常に取得されたかどうかが確認されます。 その場合、プロファイルストアはその後スキャンされ、レコード数に少なくとも 5%の変更があったかどうかが確認されます。 これらの条件が満たされると、新しいサンプリングジョブがトリガーされます。

ストリーミング取り込みの場合、プロファイルストアは 1 時間ごとに自動的にスキャンされ、レコード数に 5%以上の変更があったかどうかが確認されます。 この条件が満たされると、新しいサンプリングジョブがトリガーされます。

スキャンのサンプルサイズは、プロファイルストア内のエンティティの総数によって異なります。 これらのサンプルサイズを次の表に示します。

| プロファイルストア内のエンティティ数 | サンプルサイズ |
| ------------------------- | ----------- |
| 100 万未満 | フルデータセット |
| 100 万～2000 万 | 100 万 |
| 2000 万以上 | 全体の 5% |

各 [!DNL Profile] は、 [!DNL Profile] ID。 プロファイルの詳細について詳しくは、 [[!DNL Real-time Customer Profile] ユーザーガイド](../../profile/ui/user-guide.md#profile-detail).

![](../images/ui/overview/segment-details-profiles.png)

## セグメントの作成 {#create-segment}

選択 **[!UICONTROL セグメントを作成]** 右上隅には、 [!DNL Segment Builder] ワークスペース（セグメント定義の作成を開始できます）。

![](../images/ui/overview/segment-browse-create.png)

### [!DNL Segment Builder] workspace

[!DNL Segment Builder] は、操作できる豊富なワークスペースを提供します。 [!DNL Profile] データ要素。 ワークスペースには、ルールを作成および編集するための直感的なコントロールがあります。例えば、データプロパティを表示する際に使用するドラッグ＆ドロップタイルなどです。

詳しくは、 [!DNL Segment Builder] ワークスペース、お読みください [[!DNL Segment Builder] ユーザーガイド](./segment-builder.md).

![](../images/ui/overview/segment-builder.png)

## スケジュールされたセグメント化 {#scheduled-segmentation}

セグメント定義を作成したら、オンデマンドで、またはスケジュールに沿って（継続的に）セグメント定義を評価することができます。評価手段移動 [!DNL Real-time Customer Profile] 対応するオーディエンスを生成するために、セグメント定義を介したデータ。 作成したオーディエンスは保存され、保存されて、 [!DNL Experience Platform] API

オンデマンド評価では、API を使用して評価を実行し、必要に応じてオーディエンスを作成します。一方、スケジュールに沿った評価（「スケジュールに沿ったセグメント化」とも言います）では、特定の時間（最大 1 日に 1 回）にセグメント定義を評価する反復スケジュールを作成できます。

### スケジュールに沿ったセグメント化を有効にする {#enable-scheduled-segmentation}

セグメント定義でスケジュールに沿った評価を有効にするには、UI または API を使用します。UI で、に戻ります。 **[!UICONTROL 参照]** タブ内 **[!UICONTROL セグメント]** と切り替え **[!UICONTROL すべてのセグメントをスケジュールに追加]**. これで、すべてのセグメントが組織で設定したスケジュールに沿って評価されます。

>[!NOTE]
>
>の結合ポリシーが最大 5 個あるサンドボックスに対して、スケジュールされた評価を有効にすることができます。 [!DNL XDM Individual Profile]. 組織に対する結合ポリシーが 6 つ以上ある場合 [!DNL XDM Individual Profile] 単一のサンドボックス環境内では、スケジュールされた評価を使用できません。

現在、スケジュールを作成するには API を使用する必要があります。API を使用してスケジュールを作成、編集および操作する手順について詳しくは、セグメント結果の評価とアクセスに関するチュートリアル（特に、[API を使用した、スケジュールに沿った評価](../tutorials/evaluate-a-segment.md#scheduled-evaluation)に関する節）を参照してください。

![](../images/ui/overview/segment-browse-scheduled.png)

## ストリーミングセグメント化 {#streaming-segmentation}

ストリーミングセグメント化は、でセグメント化を実行する機能です。 [!DNL Platform] ほぼリアルタイムで、データの充実性に重点を置きながら。 ストリーミングセグメント化を使用すると、データがに到着するとセグメント認定がおこなわれるようになります [!DNL Platform]を使用することで、セグメント化ジョブのスケジュールおよび実行の必要性を軽減できます。

ストリーミングによるセグメント化について詳しくは、 [ストリーミングセグメント化ユーザガイド](./streaming-segmentation.md).

>[!NOTE]
>
>ストリーミングセグメント化を機能させるには、組織でスケジュールに沿ったセグメント化を有効にする必要があります。 スケジュールに沿ったセグメント化を有効にする方法について詳しくは、 [このユーザガイドの「ストリーミングのセグメント化」の節](#scheduled-segmentation).

## エッジのセグメント化 {#edge-segmentation}

エッジのセグメント化は、Platform 内のセグメントを即座にエッジ上で評価する機能で、同じページや次のページのパーソナライゼーションの使用例を可能にします。

エッジのセグメント化について詳しくは、 [エッジセグメント化 UI ガイド](./edge-segmentation.md)

## ポリシー違反

>[!NOTE]
>
>ポリシー違反は、宛先に割り当てられたセグメントを作成する場合にのみ適用されます。

セグメントの作成が完了すると、そのセグメントがAdobe Experience Platformデータガバナンスによって分析され、セグメント内にポリシー違反がないことを確認します。 詳しくは、 [データガバナンスの概要](../../data-governance/home.md) を参照してください。

![](../images/ui/overview/segment-dule-policy-violations.png)

## 次の手順と追加のリソース {#next-steps}

この [!DNL Segmentation Service] UI には、マーケティング可能なオーディエンスを次の場所から分離できる豊富なワークフローが用意されています。 [!DNL Real-time Customer Profile] データ。

詳しくは、以下を参照してください。 [!DNL Segmentation Service]を参照してください。ドキュメントを引き続きお読みください。 を使用する方法を学ぶには、以下を実行します。 [!DNL Segmentation Service] API( [[!DNL Segmentation Service] 開発者ガイド](../api/overview.md).
