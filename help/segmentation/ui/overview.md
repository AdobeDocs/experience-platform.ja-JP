---
keywords: Experience Platform;home;popular topics;Segmentation Service;segmentation;segmentation service;user guide;ui guide;segmentation ui guide;segment builder;Segment builder;realized;existing;exiting;
solution: Experience Platform
title: Segmentation Serviceユーザーガイド
topic: ui guide
description: Adobe Experience Platformセグメントサービスは、セグメント定義を作成および管理するためのユーザーインターフェイスを提供します。
translation-type: tm+mt
source-git-commit: 3e83215cc24b32b7fe9486c6faf455f247b6c922
workflow-type: tm+mt
source-wordcount: '1449'
ht-degree: 24%

---


# Segmentation Service UIガイド

[!DNL Adobe Experience Platform Segmentation Service] には、セグメント定義を作成および管理するためのユーザーインターフェイスが用意されています。

## はじめに

Working with segment definitions requires an understanding of the various [!DNL Experience Platform] services involved with segmentation. このユーザガイドを読む前に、次のサービスのドキュメントを確認してください。

- [[!DNL Segmentation Service]](../home.md): [!DNL Segmentation Service] 個人(顧客、見込み客、ユーザー、組織など) [!DNL Experience Platform] に関連付けて保存されているデータを、より小さなグループに分割できます。
- [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
- [[!DNL Adobe Experience Platform Identity Service]](../../identity-service/home.md):取り込まれる異なるデータソースのIDをブリッジ化して、顧客プロファイルを作成でき [!DNL Platform]ます。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):顧客体験データを [!DNL Platform] 整理する際に使用される標準化されたフレームワーク。

また、このドキュメントを通して使用される次の 2 つの重要用語を知り、その違いを理解することも重要です。
- **セグメント定義**：ターゲットオーディエンスの重要な特徴やビヘイビアーの説明に使用されるルールセットです。
- **オーディエンス**：セグメント定義の条件を満たす一連のプロファイルです。

## 概要

[[!DNL Experience Platform] UI](http://platform.adobe.com/)：左側のナビゲーションで「 **[!UICONTROL セグメント]** 」を選択し、「 **[!UICONTROL 概要]** 」タブを開きます。 このタブには、セグメントの理解と操作の開始に役立つドキュメントとビデオへのリンクが含まれています。

![](../images/ui/overview/segment-overview.png)

## 参照

「 **[!UICONTROL 参照]** 」タブを選択して、IMS組織のすべてのセグメント定義のリストを表示します。

![](../images/ui/overview/segment-browse-all.png)

この表示リストでは、分類、切り捨て、プロファイル数、評価方法、作成日、最終変更日など、セグメント定義に関する情報を説明します。

内訳には、次の各ステータスに属するプロファイルの割合を示す棒グラフが表示されます。 [!UICONTROL 入力]、 [!UICONTROL 実現]、 [!UICONTROL 終了]。

![](../images/ui/overview/segment-browse-breakdown.png)

| ステータス | 説明 |
| ------ | ----------- |
| 入力済み | セグメント内の新しいプロファイル。 |
| 実現 | セグメント内に残った既存のプロファイル。 |
| 終了 | セグメントから離れる既存のプロファイル。 |

チャーンは、セグメント定義内でセグメントジョブを最後に実行した時と比較して変化するプロファイルの割合を表し、プロファイル数は、そのセグメントに該当するプロファイルの合計数を表します。

評価方法は、ストリーミングまたはバッチのいずれかです。ストリーミングセグメントは、データがシステムに入力されるたびに評価されます。バッチセグメントは、設定されたスケジュールに従って評価されます。

![](../images/ui/overview/segment-browse-segments.png)

ページの上部には、すべてのセグメントをスケジュールに追加したり、新しいセグメントを作成したりするためのオプションがあります。

す **[!UICONTROL べてのセグメントをスケジュールに切り替えると]** 、スケジュール済みのセグメント化が有効になります。 スケジュール済みセグメントの詳細については、このユーザガイドの「 [スケジュール済みセグメント」の節を参照してください](#scheduled-segmentation)。

「セグメント **[!UICONTROL を作成]** 」を選択すると、セグメントビルダーに移動します。 セグメントの作成について詳しくは、ユーザーガイドのセグメントの [作成に関する節を参照してください](#create-segment)。

![](../images/ui/overview/segment-browse-top.png)

右側のサイドバーには、IMS組織内のすべてのセグメントに関する情報が含まれており、セグメントの合計数、最終評価日、次の評価日、およびセグメントの評価方法別の分類が表示されます。

![](../images/ui/overview/segment-browse-segment-info.png)

セグメント定義の行を選択すると、セグメントの編集または削除、セグメントの修飾オーディエンス、合計オーディエンスサイズ、セグメント名、説明、評価方法、作成日、最終変更日など、セグメント定義の概要が表示されます。

![](../images/ui/overview/segment-browse-details.png)

## セグメント定義の詳細 {#segment-details}

特定のセグメント定義の詳細を表示するには、「 **[!UICONTROL 参照]** 」タブでセグメントの名前を選択します。

セグメントの詳細ページが表示されます。 上部には、セグメント定義の概要、資格を満たしたオーディエンスサイズに関する情報、およびセグメントがアクティブ化される宛先が表示されます。

![](../images/ui/overview/segment-details-summary.png)

### セグメントサマリ

「 **[!UICONTROL セグメントサマリ]** 」セクションには、ID、名前、説明、属性の詳細などの情報が表示されます。

また、セグメントを編集するオプションも与えられます。 「 **[!UICONTROL セグメントを編集]** 」を選択すると [!DNL Segment Builder]、が表示されます。 Workspaceの使用に関する詳細は、 [!DNL Segment Builder] ユーザーガイドを参照してください [[!DNL Segment Builder] ](./segment-builder.md)。

### セグメント内の合計オーディエンス数

[セグメント内の **[!UICONTROL 合計オーディエンス]** ]セクションには、そのセグメントに該当するプロファイルの合計数が表示されます。

予測は、その日のサンプルデータのサンプルサイズを使用して生成されます。 プロファイルストアのエンティティ数が 100 万個未満の場合は、データセット全体が使用されます。100 万個から 2,000 万個のエンティティがある場合は、100 万個のエンティティが使用されます。2,000 万個を超えるエンティティがある場合は、合計エンティティ数の 5％が使用されます。セグメントの推定サイズを生成する方法について詳しくは、セグメントの作成に関するチュートリアルの[予測値の生成に関する節](../tutorials/create-a-segment.md#estimate-and-preview-an-audience)を参照してください。

### アクティブ化された宛先

「 **[!UICONTROL アクティブ化された宛先]** 」セクションには、このセグメントがアクティブ化される宛先が表示されます。

>[!NOTE]
>
> 宛先は、で使用できる機能で [!DNL Real-time Customer Data Platform]、データを外部プラットフォームに書き出すことができます。 For more information on destinations, please read the [destinations overview](../../destinations/home.md). 宛先へのセグメントのアクティブ化方法を学ぶには、宛先へのセグメントのアクティブ化に関する [ガイドをお読みください](../../destinations/ui/activate-destinations.md)。

### プロファイルサンプル

Understandは、セグメントに適合するプロファイルのサンプリングです。 [!DNL Profile] ID、名、姓、個人用電子メールなどの詳細情報が含まれます。

データサンプリングがトリガされる方法は、取り込み方法によって異なります。

バッチ取り込みの場合、プロファイルストアは15分ごとに自動的にスキャンされ、最後のサンプリングジョブが実行されてから新しいバッチが正常に取り込まれたかどうかを確認します。 その場合、プロファイルストアがスキャンされ、レコード数に少なくとも5%の変更があったかどうかが確認されます。 これらの条件が満たされると、新しいサンプリングジョブがトリガされます。

ストリーミング取り込みの場合、プロファイルストアは1時間ごとに自動的にスキャンされ、レコード数に少なくとも5%の変更があったかどうかが確認されます。 この条件が満たされると、新しいサンプリングジョブがトリガされます。

スキャンのサンプルサイズは、プロファイルストアのエンティティの全体数によって異なります。 これらのサンプルサイズを次の表に示します。

| プロファイルストア内のエンティティ数 | サンプルサイズ |
| ------------------------- | ----------- |
| 100 万未満 | フルデータセット |
| 100 万～2000 万 | 100 万 |
| 2000 万以上 | 全体の 5% |

各IDに関する詳細な情報 [!DNL Profile] は、 [!DNL Profile] IDを選択すると確認できます。 プロファイルの詳細については、 [[!DNL Real-time Customer Profile] ユーザーガイドを参照してください](../../profile/ui/user-guide.md#profile-detail)。

![](../images/ui/overview/segment-details-profiles.png)

## セグメントの作成 {#create-segment}

Selecting **[!UICONTROL Create segment]** in the top-right corner opens the [!DNL Segment Builder] workspace, where you can begin creating a segment definition.

![](../images/ui/overview/segment-browse-create.png)

### [!DNL Segment Builder] ワークスペース

[!DNL Segment Builder] は、データ要素を操作できるリッチワークスペースを提供し [!DNL Profile] ます。 ワークスペースには、ルールを作成および編集するための直感的なコントロールがあります。例えば、データプロパティを表示する際に使用するドラッグ＆ドロップタイルなどです。

Workspaceの使用に関する詳細は、 [!DNL Segment Builder] ユーザーガイドを参照してください [[!DNL Segment Builder] ](./segment-builder.md)。

![](../images/ui/overview/segment-builder.png)

## スケジュールされたセグメント化 {#scheduled-segmentation}

セグメント定義を作成したら、オンデマンドで、またはスケジュールに沿って（継続的に）セグメント定義を評価することができます。Evaluation means moving [!DNL Real-time Customer Profile] data through segment definitions in order to produce corresponding audiences. Once created, the audiences are saved and stored so that they can be exported using [!DNL Experience Platform] APIs.

オンデマンド評価では、API を使用して評価を実行し、必要に応じてオーディエンスを作成します。一方、スケジュールに沿った評価（「スケジュールに沿ったセグメント化」とも言います）では、特定の時間（最大 1 日に 1 回）にセグメント定義を評価する反復スケジュールを作成できます。

### スケジュールに沿ったセグメント化を有効にする {#enable-scheduled-segmentation}

セグメント定義でスケジュールに沿った評価を有効にするには、UI または API を使用します。In the UI, return to the **[!UICONTROL Browse]** tab within **[!UICONTROL Segments]** and toggle on **[!UICONTROL Add all segments to schedule]**. これで、すべてのセグメントが組織で設定したスケジュールに沿って評価されます。

>[!NOTE]
>
>Scheduled evaluation can be enabled for sandboxes with a maximum of five (5) merge policies for [!DNL XDM Individual Profile]. If your organization has more than five merge policies for [!DNL XDM Individual Profile] within a single sandbox environment, you will not be able to use scheduled evaluation.

現在、スケジュールを作成するには API を使用する必要があります。API を使用してスケジュールを作成、編集および操作する手順について詳しくは、セグメント結果の評価とアクセスに関するチュートリアル（特に、[API を使用した、スケジュールに沿った評価](../tutorials/evaluate-a-segment.md#scheduled-evaluation)に関する節）を参照してください。

![](../images/ui/overview/segment-browse-scheduled.png)

## ストリーミングセグメント化 {#streaming-segmentation}

ストリーミングセグメント化は、データの豊富さに重点を置きながら、ほぼリアルタイム [!DNL Platform] でセグメント化を行う機能です。 ストリーミングセグメント化では、データが到着する際にセグメントの認定が行われ、セグメント化ジョブのスケジュール [!DNL Platform]や実行の必要性が軽減されました。

ストリーミングセグメントの詳細については、『 [ストリーミングセグメントユーザガイド](./streaming-segmentation.md)』を参照してください。

>[!NOTE]
>
>ストリーミングセグメントを機能させるには、組織でスケジュール済みのセグメント化を有効にする必要があります。 For details on enabling scheduled segmentation, please refer to [the streaming segmentation section in this user guide](#scheduled-segmentation).

## ポリシー違反

>[!NOTE]
>
>ポリシー違反は、宛先に割り当てられたセグメントを作成している場合にのみ適用されます。

セグメントの作成が完了すると、そのセグメントがAdobe Experience Platformデータガバナンスによって分析され、そのセグメント内にポリシー違反がないことを確認します。 See the [[!DNL Data Governance] overview](../../data-governance/home.md) for more information.

![](../images/ui/overview/segment-dule-policy-violations.png)

## 次の手順とその他のリソース {#next-steps}

UIは、マーケティング可能なオーディエンスをデータから分離できる豊富なワークフローを提供 [!DNL Segmentation Service][!DNL Real-time Customer Profile] します。

詳細については、引き続きドキュメント [!DNL Segmentation Service]を読んでください。 この [!DNL Segmentation Service] APIの使用方法を学ぶには、 [[!DNL Segmentation Service] 開発者ガイドを読んでください](../api/overview.md)。
