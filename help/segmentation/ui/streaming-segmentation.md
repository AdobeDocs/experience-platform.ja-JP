---
keywords: Experience Platform;home;popular topics;streaming segmentation;Segmentation;Segmentation Service;segmentation service;ui guide;
solution: Experience Platform
title: ストリーミングセグメント化
topic: ui guide
description: Adobe Experience Platformでのセグメント化のストリーミングにより、データの豊富性に重点を置き、ほぼリアルタイムでセグメント化を行うことができます。 ストリーミングセグメント化では、セグメント化ジョブのスケジュールや実行の必要性が軽減され、データがプラットフォームに到着するとセグメントの認定が行われるようになりました。 この機能を使用すると、ほとんどのセグメントルールを、データがプラットフォームに渡される際に評価できるようになりました。つまり、セグメントのメンバーシップは、スケジュール済みのセグメント化ジョブを実行せずに最新の状態に維持されます。
translation-type: tm+mt
source-git-commit: 17ef6c1c6ce58db2b65f1769edf719b98d260fc6
workflow-type: tm+mt
source-wordcount: '805'
ht-degree: 1%

---


# ストリーミングセグメント化

>[!NOTE]
>
>次のドキュメントでは、UIを使用したストリーミングセグメントの使用方法を説明します。 APIを使用したストリーミングセグメントの使用について詳しくは、『 [ストリーミングセグメントAPI』ガイドを参照してください](../api/streaming-segmentation.md)。

セグメント化のストリーミング [!DNL Adobe Experience Platform] により、お客様はデータの豊富性に重点を置きながら、ほぼリアルタイムでセグメント化を行うことができます。 ストリーミングセグメント化では、セグメント化ジョブのスケジュールや実行の必要性を軽減し、データのストリーミング到着時にセグメントの認定が行わ [!DNL Platform]れるようになりました。 With this capability, most segment rules can now be evaluated as the data is passed into [!DNL Platform], meaning segment membership will be kept up-to-date without running scheduled segmentation jobs.

>[!NOTE]
>
>ストリーミングセグメントは、プラットフォームにストリーミングされるデータを評価する場合にのみ使用できます。 つまり、バッチ取り込みによって取り込まれたデータは、ストリーミングセグメント化によって評価されず、バッチ評価をトリガする必要があります。

## ストリーミングセグメントクエリタイプ

>[!NOTE]
>
>ストリーミングセグメントを機能させるには、組織でスケジュール済みのセグメント化を有効にする必要があります。 For details on enabling scheduled segmentation, please refer to [the streaming segmentation section in the Segmentation user guide](./overview.md#scheduled-segmentation).

次のいずれかの条件を満たす場合、クエリはストリーミングセグメント化によって自動的に評価されます。

| クエリタイプ | 詳細 | 例 |
| ---------- | ------- | ------- |
| 受信ヒット | 時間制限のない、単一の着信イベントを参照するセグメント定義。 | ![](../images/ui/streaming-segmentation/incoming-hit.png) |
| 相対時間枠内での着信ヒット | 過去7日間に発生した単一のイベントを参照す **るセグメント定義**。 | ![](../images/ui/streaming-segmentation/relative-hit-success.png) |
| プロファイルのみ | プロファイル属性のみを参照するセグメント定義。 |  |
| プロファイルを参照する着信ヒット | 時間制限のない、1つの着信イベント、および1つ以上のプロファイル属性を参照するセグメント定義。 | ![](../images/ui/streaming-segmentation/profile-hit.png) |
| 相対的な時間枠内のプロファイルを参照する着信ヒット | 過去7日間の、1つの着信イベントと1つ以上のプロファイル属性を参照す **るセグメント定義**。 | ![](../images/ui/streaming-segmentation/profile-relative-success.png) |
| プロファイルを参照する複数のイベント | 過去24時間以内に複数のイベントを参照するセグメント定義 **には** 、1つ以上のプロファイル属性が含まれます。 | ![](../images/ui/streaming-segmentation/event-history-success.png) |

次の節では、ストリーミングセグメントに対して **有効にしないリストセグメント定義の例を示します** 。

| クエリタイプ | 詳細 | 例 |
| ---------- | ------- | ------- |
| 相対時間枠内での着信ヒット | セグメント定義が、 **過去7日間** 以内でない着信イベントを参照する場合 ****。 例えば、 **過去2週間以内の場合**。 | ![](../images/ui/streaming-segmentation/relative-hit-failure.png) |
| 相対的なウィンドウ内のプロファイルを参照する着信ヒット | 次のオプションは、ストリーミングセグメントをサポートし **ません** 。<ul><li>過去7日間 **以** 内の着信イベント ****。</li><li>セグメントまたは特性を含む [!DNL Adobe Audience Manager (AAM)] セグメント定義。</li></ul> | ![](../images/ui/streaming-segmentation/profile-relative-failure.png) |
| プロファイルを参照する複数のイベント | 次のオプションは、ストリーミングセグメントをサポートし **ません** 。<ul><li>過去24時間以内に **発生しないイベント******。</li><li>Adobe Audience Manager(AAM)のセグメントまたは特性を含むセグメント定義。</li></ul> | ![](../images/ui/streaming-segmentation/event-history-failure.png) |
| マルチエンティティクエリ | マルチエンティティクエリは、全体として、ストリーミングセグメントでは **サポートされていません** 。 |  |

さらに、ストリーミングセグメント化を行う際には、次のようなガイドラインが適用されます。

| クエリタイプ | ガイドライン |
| ---------- | -------- |
| 単一イベントクエリ | ルックバックウィンドウは **7日間に制限されています**。 |
| イベント履歴のあるクエリ | <ul><li>ルックバックウィンドウは **1日に制限されます**。</li><li>イベント間に厳密な時間順序条件 **が存在する** 。</li><li>イベント間の単純な時間順（前後）のみが許可されます。</li><li>個々のイベント **を無効にすることはできません** 。 ただし、クエリ全体を無効にす **ることはできます** 。</li></ul> |

## ストリーミングセグメントの詳細

ストリーミングが有効なセグメントを作成したら、そのセグメントの詳細を表示できます。

![](../images/ui/streaming-segmentation/monitoring-streaming-segment.png)

特に、条件を満たした **[!UICONTROL オーディエンスの合計サイズ]** の詳細が表示されます。 「 **[!UICONTROL 条件付き]** オーディエンスの合計サイズ」には、最後に完了したセグメントジョブの実行からの条件付きオーディエンスの合計数が表示されます。 過去24時間以内にセグメントジョブが完了しなかった場合、オーディエンス数は予測から取得されます。

下に示す折れ線グラフは、過去24時間に修飾および欠格されたセグメントの数を示しています。 ドロップダウンは、過去24時間、先週または過去30日間を表示するように調整できます。

![](../images/ui/streaming-segmentation/monitoring-streaming-segment-graph.png)

最後のセグメント評価に関する追加情報は、情報バブルを選択して確認できます。

![](../images/ui/streaming-segmentation/info-bubble.png)

セグメント定義の詳細については、セグメント定義の詳細に関する前の節を参照して [ください](#segment-details)。

## ストリーミングセグメントのビデオデモ

次のビデオでは、ストリーミングのセグメント化について理解しておくことを目的としています。 ここには、顧客体験の例を示し、その後インター [!DNL Platform] フェイスの主な機能を簡単に紹介します。

>[!VIDEO](https://video.tv.adobe.com/v/36184?quality=12&learn=on)

## 次の手順

このユーザガイドでは、ストリーミングが有効なセグメント定義がAdobe Experience Platformでどのように機能するか、およびストリーミングが有効なセグメントを監視する方法について説明します。

Adobe Experience Platformのユーザインターフェイスの使用方法の詳細については、『 [セグメント化ユーザガイド](./overview.md)』を参照してください。