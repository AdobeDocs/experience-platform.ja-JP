---
title: エッジのセグメント化の監視
description: モニタリングダッシュボードを使用して、エッジセグメント化のスループットを監視する方法を説明します。
exl-id: 7abba7e8-1f2d-4a21-a93f-8bda7aa4d849
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '532'
ht-degree: 6%

---

# エッジのセグメント化の監視

Adobe Experience Platform UIのモニタリングダッシュボードを使用すると、組織内のエッジセグメント化をリアルタイムでモニタリングできます。 この機能を使用すると、エッジデータのスループットの透明性を高めることができます。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ データストリーム ](../../datastreams/overview.md): データストリームを使用すると、Experience Platform Edge Networkをデータセットに接続できます。
* [ キャパシティ ](../../landing/license-usage-and-guardrails/capacity.md):Experience Platformでは、自社がガードレールのいずれかを超えているかどうかをキャパシティで確認し、これらの問題を解決する方法に関する情報を提供します。
* [Edge セグメンテーション ](../../segmentation/methods/edge-segmentation.md): Edge セグメンテーションは、エッジ [上の](../../landing/edge-and-hub-comparison.md)Adobe Experience Platformでセグメント定義を即座に評価し、同じページと次のページのパーソナライズのユースケースを有効にします。

## アクセス {#access}

エッジセグメント化スループットの監視ダッシュボードにアクセスするには、**[!UICONTROL Monitoring]** セクション内の&#x200B;**[!UICONTROL Data management]**&#x200B;を選択し、続いて&#x200B;**[!UICONTROL Edge]**&#x200B;を選択します。

![ モニターエッジのセグメント化ダッシュボードにアクセスする方法がハイライト表示されます。](/help/dataflows/assets/ui/monitor-edge/access.png)

監視ダッシュボードが表示されます。 エッジストリーミングスループットのモニタリング指標、エッジストリーミングスループットのレートを示すグラフ、データストリームビューが表示されます。 これらの指標は、サービス、エッジ、日付でフィルタリングできます。

![監視ダッシュボード内のフィルタリングオプションがハイライト表示されます。](/help/dataflows/assets/ui/monitor-edge/filtering.png)

>[!NOTE]
>
>**を選択すると、データストリームビュー**&#x200B;のみ[!UICONTROL Edge segmentation throughput]が表示されます。

サービスでフィルタリングする場合は、スループット情報を表示するサービスを選択できます。 これには、Edge セグメンテーション、Data Collection、Target、Adobe Journey Optimizer、Offer Decisioning、カスタムパーソナライズされた宛先、イベント転送、Adobe Analytics、Adobe Audience Managerなどのサービスが含まれます。

エッジでフィルタリングする場合は、情報を表示するエッジを選択できます。 サポートされるエッジには、米国東海岸、米国西海岸、ヨーロッパ、インド、シンガポール、オーストラリア、日本、スイスなどがあります。 一度に複数のエッジを選択して表示できます。

日付でフィルタリングする場合は、タイムスケールを選択してイベントをフィルタリングできます。 このタイムスケールは30日まで設定できます。 または、事前設定済みの時間スケール [!UICONTROL Last 6 hours]、[!UICONTROL Last 12 hours]、[!UICONTROL Last 24 hours]、[!UICONTROL Last 7 days]、[!UICONTROL Last 30 days]のいずれかを使用することもできます。

## エッジスループットの指標の監視

指標テーブルには、選択したサービスのエッジスループットに固有の情報が表示されます。 各列の詳細については、次の表を参照してください。

| 指標 | 説明 |
| ------ | ----------- |
| リクエストが受信されました | 時間枠内に選択したエッジが受信したリクエストの数。 |
| ピークスループット | 時間枠内で選択したエッジが受信したリクエストの最も高い割合。 |

{style="table-layout:auto"}

## エッジセグメント化スループットのモニタリンググラフ

監視グラフには、割り当てられた時間枠内で選択したエッジが受信した1秒あたりのレコードと、許可される最大容量が示されます。

![ エッジ セグメント化スループット グラフが表示されます。](/help/dataflows/assets/ui/monitor-edge/edge-segmentation-throughput.png)

## データストリームビュー

>[!NOTE]
>
>データストリームビューは、Edge セグメンテーションスループット用にフィルタリングする場合は&#x200B;**のみ**&#x200B;利用できます。

データストリームビューの節には、サンドボックスのエッジを通過した最新のデータストリームのリストが表示されます。

![ データストリーム ビューが表示され、リストされたデータストリームに関する情報が表示されます。](/help/dataflows/assets/ui/monitor-edge/datastream-view.png)

| フィールド | 説明 |
| ----- | ----------- |
| データストリーム名 | データストリームの名前。 |
| データセット | データストリームが属するデータセットの名前。 |
| サービスが有効 | データストリームが有効になっているサービスの名前。 |
| リクエスト | データストリームを通過したリクエストの数。 |
| ピークスループット | データストリームを通過したリクエストの最大割合。 |
