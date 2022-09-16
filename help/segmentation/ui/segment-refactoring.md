---
keywords: Experience Platform；ホーム；人気の高いトピック；セグメント化；セグメント化；セグメントビルダー；セグメントビルダー
solution: Experience Platform
title: リファクタリングされたセグメント化の時間制約 UI ガイド
topic-legacy: ui guide
description: セグメントビルダーのワークスペースには、プロファイルのデータ要素を操作できる豊富な機能があります。ワークスペースには、ルールを作成および編集するための直感的なコントロールがあります。例えば、データプロパティを表示する際に使用するドラッグ＆ドロップタイルなどです。
exl-id: 3a352d46-829f-4a58-b676-73c3147f792c
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 13%

---

# 時間制約のリファクタリング

Adobe Experience Platformの 2020 年 10 月リリースでは、Adobe Experience Platform Segmentation Service のパフォーマンスが変更され、OR および AND 論理演算子の使用に新しい制限が追加されました。 これらの変更は、セグメントビルダー UI を使用して新しく作成または編集されたセグメントに影響します。 このガイドでは、これらの変更を軽減する方法について説明します。

2020 年 10 月のリリースより前は、すべてのルールレベル、グループレベル、イベントレベルの時間制約が、同じタイムスタンプを冗長的に参照していました。 時間制限の使用を明確にするために、ルールレベルとグループレベルの時間の制約が削除されました。 この変更に対応するために、すべての時間制約をイベントレベルの時間制約として書き換える必要があります。

以前は、個々のイベントに複数の時間制約ルールを付加していました。

![](../images/ui/segment-refactoring/former-time-constraint.png)

ご覧のように、このセグメントにはルールレベルで 2 つの制約があります。1 は&quot;[!UICONTROL 今日]」、もう 1 つは&quot;[!UICONTROL 昨日]&quot;.

前のセグメントは次のセグメントと同じです。イベントレベルの時間制約の両方が、AND 演算子を使用して接続されています。 最初のイベントレベルの時間制約は、名前が「トレーニング」に等しく今日に発生するクリックイベントを参照し、2 番目のイベントレベルの時間制約は、名前が「ペット」に等しく昨日に発生したクリックイベントを参照します。

![](../images/ui/segment-refactoring/time-constraint-1.png) ![](../images/ui/segment-refactoring/time-constraint-2.png)

この時間制約のリファクタリングは、OR 演算子を使用して接続される時間制約にも影響します。
