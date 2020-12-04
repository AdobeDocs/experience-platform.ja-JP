---
keywords: Experience Platform;home;popular topics;identity graph viewer;Identity graph viewer;graph viewer;Graph viewer;identity namespace;Identity namespace;identity;Identity;Identity service;identity service
solution: Experience Platform
title: Adobe Experience Platform ID サービス
topic: tutorial
description: アイデンティティグラフは、特定の顧客の異なるアイデンティティ間の関係を示すマップです。異なるチャネル間での顧客のブランドとの相互作用を視覚的に示します。
translation-type: tm+mt
source-git-commit: ef1025dfacc91b13c064db99e6304f2c09abb3d9
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

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

- **ID（ノード）:** IDまたはノードは、エンティティに固有のデータ（通常は人）です。 IDは、名前空間とIDの値で構成されます。
- **リンク（エッジ）:** リンクまたはエッジは、ID間の接続を表します。
- **グラフ（クラスター）:** グラフまたはクラスターは、個人を表すIDとリンクのグループです。

## IDグラフビューアにアクセスする

IDグラフビューアをUIで使用するには、左側のナビゲーションで「 **[!UICONTROL ID]** 」を選択し、「 **[!UICONTROL IDグラフ]** 」タブを選択します。 「 **[!UICONTROL ID名前空間]** 」画面で、「ID名前空間を **[!UICONTROL 選択]** 」アイコンをクリックして、使用する名前空間を検索します。

![名前空間画面](../images/identity-graph-viewer/identity-namespace.png)

「ID **[!UICONTROL 名前空間を選択]** 」パネルが表示されます。 この画面には、名前空間の **[!UICONTROL 表示名]**、IDシンボル所有者、 **[!UICONTROL 所有者、最終更新日、更新日、名前空間]**&#x200B;説明など、組織が使用できるのリストが含まれ ************&#x200B;ます。 有効なID値が接続されている限り、指定された任意の名前空間を使用できます。

使用する名前空間を選択し、「 **[!UICONTROL 選択]** 」をクリックして続行します。

![select-identity-名前空間](../images/identity-graph-viewer/select-identity-namespace.png)

名前空間を選択したら、「 **[!UICONTROL ID値]** 」テキストボックスに特定の顧客に対応する値を入力し、「 **[!UICONTROL 表示」を選択します]**。

![add-identity-value](../images/identity-graph-viewer/identity-value-filled.png)

IDグラフビューアが表示されます。 画面の左側には、選択した名前空間にリンクされているすべてのIDと入力したID値が表示されるIDグラフが表示されます。 各IDノードは、名前空間と対応するID値で構成されます。 任意のIDを選択したままにして、グラフをドラッグ&amp;操作できます。 IDの上にマウスポインターを置いて、ID値に関する情報を表示することもできます。 グラフの出力は、画面の中央に表示されるリストとしても表示されます。

>[!IMPORTANT]
>
>IDグラフを生成するには、2つ以上のリンクされたIDと、有効な名前空間とIDのペアが必要です。 グラフビューアで表示できるIDの最大数は400です。 See the [appendix](#appendix) section below for more information.

![同一性グラフ](../images/identity-graph-viewer/graph-viewer.png)

「 **[!UICONTROL ID]** 」テーブルでハイライト表示された行を更新し、右側に表示される情報を更新するIDを選択します。この情報には、IDの **[!UICONTROL 値]**、バッチID **[!UICONTROL 、]****** 最終更新日が含まれます。

![select-identity](../images/identity-graph-viewer/select-identity.png)

グラフ内でフィルタリングを行い、 **[!UICONTROL Identities]** テーブルの上部にある並べ替えオプションを使用して特定の名前空間を分離できます。 ドロップダウンメニューで、ハイライトする名前空間を選択します。

![名前空間別フィルター](../images/identity-graph-viewer/filter-namespace.png)

グラフビューアに戻り、選択した名前空間がハイライト表示されます。 また、フィルターオプションは、選択した名前空間の **[!UICONTROL 情報のみを返すように「]** ID」テーブルを更新します。

![フィルタ](../images/identity-graph-viewer/filtered.png)

グラフビューアボックスの右上には、倍率に関するオプションが表示されます。 グラフにズームインするには **(+)** アイコンを、ズームアウトするには **(-)** アイコンを選択します。

![zoom](../images/identity-graph-viewer/zoom.png)

バッチの詳細を表示するには、ヘッダーから **[!UICONTROL データソース]** を選択します。 「 **[!UICONTROL データソース]** 」( **[!UICONTROL Data source]** )テーブルには、グラフに関連付けられた **[!UICONTROL バッチIDのリスト、リンクされたID]**、ソーススキーマ、取り込み日が表示されます。

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
- IDグラフビューアは、IDの上限を400個にする必要があります。
- IDグラフビューアは、現在、実稼動用サンドボックス以外ではアクセスできません。
- IDグラフビューアは、現在、バッチで取り込んだデータのみをサポートし、ストリーミングソースを使用して取り込んだデータは表示されません。

![error-screen](../images/identity-graph-viewer/error-screen.png)

## 次の手順

このドキュメントを読むことで、Platform UIで顧客のIDグラフを表示する方法を学びました。 プラットフォームのIDの詳細については、「 [IDサービスの概要」を参照してください。](../home.md)