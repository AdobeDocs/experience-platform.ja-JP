---
title: Advertising ポッドの詳細コレクションのデータタイプ
description: Advertising ポッドの詳細コレクション Experience Data Model （XDM）データタイプについて説明します。
exl-id: 401c393f-aeda-4ecd-89f4-458833190ced
source-git-commit: 9350cfc299c20bd63a2a559c177b3af02739e5b9
workflow-type: tm+mt
source-wordcount: '160'
ht-degree: 8%

---

# [!UICONTROL Advertising ポッドの詳細 ] コレクション データ型

[!UICONTROL Advertising ポッドの詳細 ] コレクションは、標準の Experience Data Model （XDM）データタイプです。 通常、コンテンツブレーク中に連続して再生される広告のシーケンスまたはグループを定義します。 [!UICONTROL Advertising ポッドの詳細 ] コレクション データタイプを使用すると、広告ブレーク ID、広告ブレークのわかりやすい名前、ブレーク内の広告のインデックス、コンテンツのタイムライン内の広告ブレークのオフセット （秒）などの詳細をキャプチャできます。

![Advertising Pod Details Information Collection データタイプの図。](../images/data-types/advertising-pod-details-collection.png)

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|-----------------------------------------|-----------------|-----------|----------|---------------------------------------------------------|
| [!UICONTROL  ポッド位置の広告 ] | `index` | 整数 | ○ | 親広告の改ページ内の広告のインデックスが開始されます。 |
| [!UICONTROL  ポッドのわかりやすい名前 ] | `friendlyName` | 文字列 | × | 広告ブレークのわかりやすい名前。 |
| [!UICONTROL  ポッド オフセット ] | `offset` | 整数 | ○ | コンテンツ内の広告ブレークのオフセット （秒単位）。 |

{style="table-layout:auto"}
