---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント化；セグメント化；セグメントビルダー；セグメントビルダー
solution: Experience Platform
title: リファクタリングされたセグメント化の時間制限UIガイド
topic-legacy: ui guide
description: セグメントビルダーのワークスペースには、プロファイルのデータ要素を操作できる豊富な機能があります。ワークスペースには、ルールを作成および編集するための直感的なコントロールがあります。例えば、データプロパティを表示する際に使用するドラッグ＆ドロップタイルなどです。
exl-id: 3a352d46-829f-4a58-b676-73c3147f792c
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 13%

---

# 時間制約リファクタリング

Adobe Experience Platformの2020年10月のリリースでは、Adobe Experience Platformセグメントサービスのパフォーマンスが変更され、ORおよびAND論理演算子の使用に新しい制限が加えられました。 これらの変更は、セグメントビルダーのUIを使用して新しく作成または編集されたセグメントに影響します。 このガイドでは、これらの変更を軽減する方法を説明します。

2020年10月のリリースより前のリリースでは、ルールレベル、グループレベル、イベントレベルの時間制限はすべて、重複して同じタイムスタンプを参照していました。 時間制限の使用を明確にするため、ルールレベルとグループレベルの時間制限は削除されました。 この変更に対応するために、すべての時間制限をイベントレベルの時間制限として書き換える必要があります。

以前は、個々のイベントに複数の時間制約ルールを割り当てることができました。

![](../images/ui/segment-refactoring/former-time-constraint.png)

このセグメントには、ルールレベルに2つの制約があります。1つは「[!UICONTROL 今日]」に、もう1つは「[!UICONTROL 昨日]」に使用します。

前のセグメントは、次のセグメントと同じです。イベントレベルの時間制約はどちらも、AND演算子を使用して結び付けられています。 最初のイベントレベルの時間制約は、名前が「Training」で今日発生しているクリックイベントを参照し、2番目のイベントレベルの時間制約は、名前が「Pets」で昨日発生したクリックイベントを参照します。

![](../images/ui/segment-refactoring/time-constraint-1.png) ![](../images/ui/segment-refactoring/time-constraint-2.png)

この時間制限のリファクタリングは、OR演算子を使用して接続される時間制限にも影響します。
