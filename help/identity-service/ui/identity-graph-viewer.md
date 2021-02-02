---
keywords: Experience Platform；ホーム；人気のあるトピック；IDグラフビューア；IDグラフビューア；IDグラフビューア；ID名前空間;ID名前空間;ID;IDサービス；IDサービス
solution: Experience Platform
title: Adobe Experience Platform ID サービス
topic: tutorial
description: アイデンティティグラフは、特定の顧客の異なるアイデンティティ間の関係を示すマップです。異なるチャネル間での顧客のブランドとの相互作用を視覚的に示します。
translation-type: tm+mt
source-git-commit: 7c9c81492df9333945ac62602f10b6097296d62b
workflow-type: tm+mt
source-wordcount: '905'
ht-degree: 1%

---


# （ベータ版）IDグラフビューア

>[!NOTE]
>
>IDグラフビューアは現在ベータ版です。 機能は変更される場合があります。

アイデンティティグラフは、特定の顧客の異なるアイデンティティ間の関係を示すマップです。異なるチャネル間での顧客のブランドとの相互作用を視覚的に示します。 すべての顧客IDグラフは、顧客のアクティビティに応じて、ほぼリアルタイムでAdobe Experience Platform・アイデンティティ・サービスによって管理および更新されます。

プラットフォームユーザーインターフェイスのIDグラフビューアを使用すると、顧客IDが繋ぎ合わされているのを視覚化し、より深く理解できます。 ビューアを使用すると、グラフの様々な部分をドラッグ&amp;操作でき、複雑な同一性の関係を調べたり、デバッグをより効率的に行ったり、情報の利用方法に関する透明性を高めることができます。

## はじめに

IDグラフビューアを使用するには、関連する様々なAdobe Experience Platformのサービスについて理解する必要があります。 IDグラフビューアの操作を開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Identity Service]](../home.md):デバイスとシステム間でIDをブリッジ化することで、個々の顧客とその行動をより良く表示できます。

### 用語

- **ID（ノード）:ID** またはノードは、エンティティに固有のデータ（通常は個人）です。IDは、名前空間とIDの値で構成されます。
- **リンク（エッジ）:** リンクまたはエッジは、ID間の接続を表します。
- **グラフ（クラスター）：グラフ** またはクラスターは、個人を表すIDとリンクのグループです。

## IDグラフビューアにアクセスする

IDグラフビューアをUIで使用するには、左側のナビゲーションで「**[!UICONTROL ID]**」を選択し、「**[!UICONTROL IDグラフ]**」タブを選択します。 **[!UICONTROL ID名前空間]**&#x200B;画面で、**[!UICONTROL ID名前空間]**&#x200B;を選択アイコンをクリックし、使用する名前空間を検索します。

![名前空間画面](../images/identity-graph-viewer/identity-namespace.png)

「**[!UICONTROL ID名前空間を選択]**」パネルが表示されます。 この画面には、名前空間の&#x200B;**[!UICONTROL 表示名]**、**[!UICONTROL ID記号]**、**[!UICONTROL 所有者]**、**[!UICONTROL 最終更新日]**、**[!UICONTROL 説明]**&#x200B;に関する情報など、組織が利用できる名前空間のリストが含まれます。 有効なID値が接続されている限り、指定された任意の名前空間を使用できます。

使用する名前空間を選択し、「****」をクリックして次に進みます。

![select-identity-名前空間](../images/identity-graph-viewer/select-identity-namespace.png)

名前空間を選択したら、「**[!UICONTROL ID値]**」テキストボックスに特定の顧客に対応する値を入力し、「**[!UICONTROL 表示]**」を選択します。

![add-identity-value](../images/identity-graph-viewer/identity-value-filled.png)

IDグラフビューアが表示されます。 画面の左側には、選択した名前空間にリンクされているすべてのIDと入力したID値が表示されるIDグラフが表示されます。 各IDノードは、名前空間と対応するID値で構成されます。 任意のIDを選択したままにして、グラフをドラッグ&amp;操作できます。 IDの上にマウスポインターを置いて、ID値に関する情報を表示することもできます。 グラフの出力は、画面の中央に表示されるリストとしても表示されます。

>[!IMPORTANT]
>
>IDグラフを生成するには、2つ以上のリンクされたIDと、有効な名前空間とIDのペアが必要です。 グラフビューアで表示できるIDの最大数は150です。 詳しくは、下の[付録](#appendix)を参照してください。

![同一性グラフ](../images/identity-graph-viewer/graph-viewer.png)

**[!UICONTROL ID]**&#x200B;テーブルで強調表示された行を更新し、右側のレールに表示される情報を更新するIDを選択します。IDには、**[!UICONTROL 値]**、**[!UICONTROL バッチID]**&#x200B;および&#x200B;**[!UICONTROL 最終更新]**&#x200B;日が含まれます。

![select-identity](../images/identity-graph-viewer/select-identity.png)

グラフ内でフィルターを適用し、**[!UICONTROL ID]**&#x200B;テーブルの上部にある並べ替えオプションを使用して、特定の名前空間を分離できます。 ドロップダウンメニューで、ハイライトする名前空間を選択します。

![名前空間別フィルター](../images/identity-graph-viewer/filter-namespace.png)

グラフビューアに戻り、選択した名前空間がハイライト表示されます。 また、フィルターオプションは、選択した名前空間の情報のみを返すように&#x200B;**[!UICONTROL ID]**&#x200B;テーブルを更新します。

![フィルタ](../images/identity-graph-viewer/filtered.png)

グラフビューアボックスの右上には、倍率に関するオプションが表示されます。 **(+)**&#x200B;アイコンを選択してグラフにズームインするか、**(-)**&#x200B;アイコンを選択してズームアウトします。

![zoom](../images/identity-graph-viewer/zoom.png)

バッチの詳細を表示するには、ヘッダーから&#x200B;**[!UICONTROL データソース]**&#x200B;を選択します。 **[!UICONTROL データソース]**&#x200B;テーブルには、グラフに関連付けられた&#x200B;**[!UICONTROL バッチID]**&#x200B;のリストと、**[!UICONTROL リンクされたID]**、ソーススキーマ、取り込み日が表示されます。

![データソース](../images/identity-graph-viewer/data-source-table.png)

アイデンティティ・グラフ内の任意のリンクを選択して、そのリンクに貢献したすべてのソース・バッチを表示できます。

![選択リンク](../images/identity-graph-viewer/select-edge.png)

または、1つのバッチを選択して、このバッチが寄与したすべてのリンクを表示することもできます。

![選択リンク](../images/identity-graph-viewer/select-batch.png)

IDグラフは、IDグラフビューアからもアクセスできます。

![大群の](../images/identity-graph-viewer/large-cluster.png)

## 付録

次の節では、IDグラフビューアの操作に関する追加情報を示します。

### エラーメッセージについて

IDグラフビューアにアクセスするとエラーが発生する場合があります。 IDグラフビューアを操作する場合の前提条件と制限のリストを次に示します。

- 選択した名前空間にID値が存在する必要があります。
- IDグラフビューアを生成するには、少なくとも2つのIDがリンクされている必要があります。
- IDグラフビューアは、IDの上限を150個にする必要があります。

![error-screen](../images/identity-graph-viewer/error-screen.png)

## 次の手順

このドキュメントを読むことで、Platform UIで顧客のIDグラフを表示する方法を学びました。 プラットフォームのIDの詳細については、[IDサービスの概要](../home.md)を参照してください。