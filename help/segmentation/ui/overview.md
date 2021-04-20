---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント化；セグメント化；セグメント化サービス；ユーザーガイド；uiガイド；セグメント化uiガイド；セグメントビルダー；実現；既存；終了；
solution: Experience Platform
title: Segmentation Service UIガイド
topic: ui guide
description: Adobe Experience Platformセグメントサービスは、セグメント定義を作成および管理するためのユーザーインターフェイスを提供します。
translation-type: tm+mt
source-git-commit: 1634466d3a1d8eadc4c98bb93214d8772b6a47a3
workflow-type: tm+mt
source-wordcount: '1514'
ht-degree: 23%

---


# Segmentation Service UIガイド

[!DNL Adobe Experience Platform Segmentation Service] には、セグメント定義を作成および管理するためのユーザーインターフェイスが用意されています。

## はじめに

セグメントの定義を使用するには、セグメント化に関連する様々な[!DNL Experience Platform]サービスについて理解しておく必要があります。 このユーザガイドを読む前に、次のサービスのドキュメントを確認してください。

- [[!DNL Segmentation Service]](../home.md): [!DNL Segmentation Service] 個人(顧客、見込み客、ユーザー、組織など) [!DNL Experience Platform] に関連付けて保存されているデータを、より小さなグループに分割できます。
- [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
- [[!DNL Adobe Experience Platform Identity Service]](../../identity-service/home.md):取り込まれる異なるデータソースのIDをブリッジ化することで、顧客プロファイルを作成でき [!DNL Platform]ます。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Platform] に使用される標準化されたフレームワーク。

また、このドキュメントを通して使用される次の 2 つの重要用語を知り、その違いを理解することも重要です。
- **セグメント定義**：ターゲットオーディエンスの重要な特徴やビヘイビアーの説明に使用されるルールセットです。
- **オーディエンス**：セグメント定義の条件を満たす一連のプロファイルです。

## 概要

[[!DNL Experience Platform] UI](http://platform.adobe.com/)で、左側のナビゲーションの「**[!UICONTROL セグメント]**」を選択し、「**[!UICONTROL 概要]**」タブを開きます。 このタブには、セグメントの理解と操作の開始に役立つドキュメントとビデオへのリンクが含まれています。

![](../images/ui/overview/segment-overview.png)

## 参照

「**[!UICONTROL 参照]**」タブを選択して、IMS組織のすべてのセグメント定義のリストを表示します。

![](../images/ui/overview/segment-browse-all.png)

この表示リストでは、分類、切り捨て、プロファイル数、評価方法、作成日、最終変更日など、セグメント定義に関する情報を説明します。

内訳には、次の各ステータスに属するプロファイルの割合を示す棒グラフが表示されます。[!UICONTROL 入力]、[!UICONTROL 実現]、[!UICONTROL 終了]。

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

**[!UICONTROL す追加べてのセグメントをスケジュール]**&#x200B;に切り替えると、スケジュール済みのセグメント化が有効になります。 スケジュール済みセグメントの詳細については、このユーザーガイド](#scheduled-segmentation)の[スケジュール済みセグメントの節を参照してください。

「**[!UICONTROL セグメントを作成]**」を選択すると、セグメントビルダーに移動します。 セグメントの作成について詳しくは、ユーザーガイド](#create-segment)の[セグメントの作成の節を参照してください。

![](../images/ui/overview/segment-browse-top.png)

右側のサイドバーには、IMS組織内のすべてのセグメントに関する情報が含まれており、セグメントの合計数、最終評価日、次の評価日、およびセグメントの評価方法別の分類が表示されます。

![](../images/ui/overview/segment-browse-segment-info.png)

セグメント定義の行を選択すると、セグメントの編集または削除、セグメントの修飾オーディエンス、合計オーディエンスサイズ、セグメント名、説明、評価方法、作成日、最終変更日など、セグメント定義の概要が表示されます。

![](../images/ui/overview/segment-browse-details.png)

## セグメント定義の詳細{#segment-details}

特定のセグメント定義の詳細を表示するには、「**[!UICONTROL 参照]**」タブでセグメント名を選択します。

セグメントの詳細ページが表示されます。 上部には、セグメント定義の概要、資格を満たしたオーディエンスサイズに関する情報、およびセグメントがアクティブ化される宛先が表示されます。

![](../images/ui/overview/segment-details-summary.png)

### セグメントサマリ

**[!UICONTROL セグメントサマリ]**&#x200B;セクションには、属性のID、名前、説明、詳細などの情報が表示されます。

また、セグメントを編集するオプションも与えられます。 「**[!UICONTROL セグメントを編集]**」を選択すると、[!DNL Segment Builder]に移動します。 [!DNL Segment Builder]ワークスペースの使用に関する詳細は、[[!DNL Segment Builder] ユーザーガイド](./segment-builder.md)を参照してください。

### セグメント内の合計オーディエンス数

**[!UICONTROL セグメント]**&#x200B;の合計オーディエンス数セクションは、そのセグメントに該当するプロファイルの合計数を示します。

予測は、その日のサンプルデータのサンプルサイズを使用して生成されます。 プロファイルストアのエンティティ数が 100 万個未満の場合は、データセット全体が使用されます。100 万個から 2,000 万個のエンティティがある場合は、100 万個のエンティティが使用されます。2,000 万個を超えるエンティティがある場合は、合計エンティティ数の 5％が使用されます。セグメントの推定サイズを生成する方法について詳しくは、セグメントの作成に関するチュートリアルの[予測値の生成に関する節](../tutorials/create-a-segment.md#estimate-and-preview-an-audience)を参照してください。

### アクティブ化された宛先

**[!UICONTROL アクティブ化された宛先]**&#x200B;セクションには、このセグメントがアクティブ化される宛先が表示されます。

>[!NOTE]
>
> 宛先は[!DNL Real-time Customer Data Platform]で利用できる機能で、データを外部プラットフォームにエクスポートできます。 宛先の詳細については、[宛先の概要](../../destinations/home.md)を参照してください。 宛先へのセグメントのアクティブ化方法を学ぶには、宛先](../../destinations/ui/activate-destinations.md)へのセグメントのアクティブ化に関する[ガイドをお読みください。

### プロファイルサンプル

Understandeは、セグメントに該当するプロファイルのサンプリングで、[!DNL Profile] ID、名、姓、個人の電子メールなどの詳細情報を示しています。

データサンプリングがトリガされる方法は、取り込み方法によって異なります。

バッチ取り込みの場合、プロファイルストアは15分ごとに自動的にスキャンされ、最後のサンプリングジョブが実行されてから新しいバッチが正常に取り込まれたかどうかを確認します。 その場合、プロファイルストアがスキャンされ、レコード数に少なくとも5%の変更があったかどうかが確認されます。 これらの条件が満たされると、新しいサンプリングジョブがトリガされます。

ストリーミング取り込みの場合、プロファイルストアは1時間ごとに自動的にスキャンされ、レコード数に少なくとも5%の変更があったかどうかが確認されます。 この条件が満たされると、新しいサンプリングジョブがトリガされます。

スキャンのサンプルサイズは、プロファイルストアのエンティティの全体数によって異なります。 これらのサンプルサイズを次の表に示します。

| プロファイルストア内のエンティティ数 | サンプルサイズ |
| ------------------------- | ----------- |
| 100 万未満 | フルデータセット |
| 100 万～2000 万 | 100 万 |
| 2000 万以上 | 全体の 5% |

各[!DNL Profile]に関する詳細な情報は、[!DNL Profile] IDを選択すると確認できます。 プロファイルの詳細については、[[!DNL Real-time Customer Profile] ユーザーガイド](../../profile/ui/user-guide.md#profile-detail)を参照してください。

![](../images/ui/overview/segment-details-profiles.png)

## セグメントの作成 {#create-segment}

右上隅の「**[!UICONTROL セグメントを作成]**」を選択すると、[!DNL Segment Builder]ワークスペースが開き、セグメント定義の作成を開始できます。

![](../images/ui/overview/segment-browse-create.png)

### [!DNL Segment Builder] ワークスペース

[!DNL Segment Builder] は、 [!DNL Profile] データ要素を操作できるリッチワークスペースを提供します。ワークスペースには、ルールを作成および編集するための直感的なコントロールがあります。例えば、データプロパティを表示する際に使用するドラッグ＆ドロップタイルなどです。

[!DNL Segment Builder]ワークスペースの使用に関する詳細は、[[!DNL Segment Builder] ユーザーガイド](./segment-builder.md)を参照してください。

![](../images/ui/overview/segment-builder.png)

## スケジュールされたセグメント化 {#scheduled-segmentation}

セグメント定義を作成したら、オンデマンドで、またはスケジュールに沿って（継続的に）セグメント定義を評価することができます。評価手段は、[!DNL Real-time Customer Profile]データをセグメント定義を通じて移動し、対応するオーディエンスを生成する。 作成したオーディエンスは保存され、保存されます。保存されるので、[!DNL Experience Platform] APIを使用して書き出すことができます。

オンデマンド評価では、API を使用して評価を実行し、必要に応じてオーディエンスを作成します。一方、スケジュールに沿った評価（「スケジュールに沿ったセグメント化」とも言います）では、特定の時間（最大 1 日に 1 回）にセグメント定義を評価する反復スケジュールを作成できます。

### スケジュールに沿ったセグメント化を有効にする {#enable-scheduled-segmentation}

セグメント定義でスケジュールに沿った評価を有効にするには、UI または API を使用します。UIで、**[!UICONTROL セグメント]**&#x200B;内の&#x200B;**[!UICONTROL 参照]**&#x200B;タブに戻り、**[!UICONTROL すべてのセグメントで追加スケジュール]**&#x200B;を切り替えます。 これで、すべてのセグメントが組織で設定したスケジュールに沿って評価されます。

>[!NOTE]
>
>[!DNL XDM Individual Profile]に対して、最大5個のマージポリシーを持つサンドボックスに対して、スケジュールされた評価を有効にできます。 1つのサンドボックス環境内に[!DNL XDM Individual Profile]のマージポリシーが5つを超える場合、スケジュールされた評価を使用できません。

現在、スケジュールを作成するには API を使用する必要があります。API を使用してスケジュールを作成、編集および操作する手順について詳しくは、セグメント結果の評価とアクセスに関するチュートリアル（特に、[API を使用した、スケジュールに沿った評価](../tutorials/evaluate-a-segment.md#scheduled-evaluation)に関する節）を参照してください。

![](../images/ui/overview/segment-browse-scheduled.png)

## ストリーミングセグメント化 {#streaming-segmentation}

ストリーミングセグメント化は、[!DNL Platform]に対してほぼリアルタイムでセグメント化を行う機能で、データの豊富さに重点を置いています。 ストリーミングセグメント化では、セグメント化ジョブをスケジュールおよび実行する必要がなくなり、データが[!DNL Platform]に到着するとセグメントの認定が行われるようになりました。

ストリーミングセグメントについて詳しくは、[ストリーミングセグメントユーザーガイド](./streaming-segmentation.md)を参照してください。

>[!NOTE]
>
>ストリーミングセグメントを機能させるには、組織でスケジュール済みのセグメント化を有効にする必要があります。 スケジュールされたセグメント化を有効にする方法の詳細については、[このユーザーガイド](#scheduled-segmentation)のストリーミングセグメント化の節を参照してください。

## エッジセグメント{#edge-segmentation}

エッジセグメント化は、エッジ上でプラットフォーム内のセグメントを瞬時に評価する機能で、同じページや次のページのパーソナライズの使用例を実現します。

エッジのセグメント化について詳しくは、『[エッジセグメント化UIガイド](./edge-segmentation.md)』を参照してください。

## ポリシー違反

>[!NOTE]
>
>ポリシー違反は、宛先に割り当てられたセグメントを作成している場合にのみ適用されます。

セグメントの作成が完了すると、そのセグメントがAdobe Experience Platformデータガバナンスによって分析され、そのセグメント内にポリシー違反がないことを確認します。 詳しくは、[[!DNL Data Governance] 概要](../../data-governance/home.md)を参照してください。

![](../images/ui/overview/segment-dule-policy-violations.png)

## 次の手順とその他のリソース {#next-steps}

[!DNL Segmentation Service] UIは、[!DNL Real-time Customer Profile]データからマーケティング可能なオーディエンスを分離できる豊富なワークフローを提供します。

[!DNL Segmentation Service]の詳細については、ドキュメントを読んでください。 [!DNL Segmentation Service] APIの使い方を学ぶには、[[!DNL Segmentation Service] 開発者ガイド](../api/overview.md)を読んでください。
