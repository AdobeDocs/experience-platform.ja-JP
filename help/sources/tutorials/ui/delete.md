---
keywords: Experience Platform；ホーム；人気のトピック；データフローの削除
description: ソースワークスペースを使用すると、エラーが含まれていたり古くなったりした既存のバッチおよびストリーミングのデータフローを削除できます。
solution: Experience Platform
title: UI でのデータフローの削除
type: Tutorial
exl-id: aa224467-7733-40de-aab7-0ff1c557abf2
source-git-commit: c2832821ea6f9f630e480c6412ca07af788efd66
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 18%

---

# UI でのデータフローの削除

[!UICONTROL  ソース ] ワークスペースを使用すると、エラーが含まれていたり古くなったりした既存のバッチおよびストリーミングデータフローを削除できます。

このチュートリアルでは、[!UICONTROL  ソース ] ワークスペースを使用してデータフローを削除する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [ソース](../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
- [サンドボックス](../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## データフローの削除

[Experience PlatformUI](https://platform.adobe.com) で、左側のナビゲーションから **[!UICONTROL Sources]** を選択して [!UICONTROL Sources] ワークスペースにアクセスし、上部のヘッダーから **[!UICONTROL Dataflows]** を選択します。

![カタログ](../../images/tutorials/delete/catalog.png)

**[!UICONTROL データフロー]** ページが表示されます。 このページには、ターゲットデータセット、ソース、アカウント名、作成日に関する情報を含め、表示可能なデータフローがリストされます。

左上のフィルターアイコン（![filter-icon](/help/images/icons/filter.png)）を選択して、並べ替えパネルを開きます。

![ データフロー ](../../images/tutorials/delete/dataflows.png)

並べ替えパネルには、すべてのソースのリストが表示されます。 リストから複数のソースを選択して、選択した特定のソースに関連付けられたフィルター適用済みのデータフローの選択にアクセスできます。

操作するソースを選択して、既存のデータフローのリストを表示します。 削除するデータフローを特定したら、データフロー名の横にある省略記号（`...`）を選択します。

![dataflows-filter](../../images/tutorials/delete/dataflows-filter.png)

ドロップダウンメニューが表示され、データフローのスケジュールを編集、データフローを無効にする、または完全に削除するためのオプションが提供されます。

データフローを削除する場合は、「**[!UICONTROL 削除]**」を選択します。

![削除](../../images/tutorials/delete/delete.png)

最終確認ダイアログボックスが表示されます。 「**[!UICONTROL 削除]**」を選択してプロセスを完了します。

![ 確認 ](../../images/tutorials/delete/confirm.png)

しばらくすると、画面の下部に確認ボックスが表示され、削除が成功したことを確認します。

![ 確認 ](../../images/tutorials/delete/confirmed.png)

## 次の手順

このチュートリアルでは、[!UICONTROL Sources] ワークスペースを使用して既存のデータフローを正常に削除しました。

API 呼び出しを使用してプログラムでこれらの操作を実行する手順については、[Flow Service API を使用したデータフローの削除 ](../../tutorials/api/delete-dataflows.md) に関するチュートリアルを参照してください。
