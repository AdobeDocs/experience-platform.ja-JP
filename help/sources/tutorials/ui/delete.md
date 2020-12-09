---
keywords: Experience Platform;home;popular topics; delete dataflows
description: ソース・ワークスペースでは、エラーを含む、または古くなった既存のバッチおよびストリーミング・データ・フローを削除できます。
solution: Experience Platform
title: データフローの削除
topic: overview
type: Tutorial
translation-type: tm+mt
source-git-commit: 7cb5862112c80e386e697aa2bd503abe49f11a3f
workflow-type: tm+mt
source-wordcount: '358'
ht-degree: 6%

---


# UIのデータフローの削除

「 [!UICONTROL Sources] 」ワークスペースでは、エラーを含む、または古くなった既存のバッチおよびストリーミングデータフローを削除できます。

このチュートリアルでは、 [!UICONTROL ソース] ・ワークスペースを使用してデータ・フローを削除する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [ソース](../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
- [サンドボックス](../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

## データフローの削除

「 [Experience PlatformUI](https://platform.adobe.com)」で、左側のナビゲーションから「 **[!UICONTROL Sources]** 」を選択して [!UICONTROL Sources] ワークスペースにアクセスし、トップ・ヘッダーから「Dataflows」 **** を選択します。

![カタログ](../../images/tutorials/delete/catalog.png)

The **[!UICONTROL Dataflows]** page appears. このページには、ターゲットデータセット、ソース、アカウント名、作成日など、表示可能なデータフローのリストが表示されます。

左上のフィルターアイコン(![フィルターアイコン](../../images/tutorials/delete/filter.png))を選択して、並べ替えパネルを起動します。

![データフロー](../../images/tutorials/delete/dataflows.png)

並べ替えパネルには、すべてのソースのリストが表示されます。 リストから複数のソースを選択して、選択した特定のソースに関連付けられたデータフローのフィルタ選択にアクセスできます。

既存のデータフローのリストを表示するために使用するソースを選択します。 削除するデータフローを識別したら、データフロー名の横にある「三点リーダー」(`...`)を選択します。

![dataflows-filter](../../images/tutorials/delete/dataflows-filter.png)

ドロップダウンメニューが表示され、データフローのスケジュールを編集したり、データフローを無効にしたり、完全に削除したりするオプションが表示されます。

データフローを削除するには、 **[!UICONTROL 「Delete]** 」を選択します。

![次を削除します。](../../images/tutorials/delete/delete.png)

最終確認ダイアログボックスが表示されます。 「 **[!UICONTROL Delete]** 」を選択してプロセスを完了します。

![confirm](../../images/tutorials/delete/confirm.png)

しばらくすると、削除が成功したことを確認する確認ボックスが画面の下部に表示されます。

![確認された](../../images/tutorials/delete/confirmed.png)

## 次の手順

このチュートリアルに従うと、 [!UICONTROL Sources] Workspaceを使用して既存のデータフローを削除できます。

API呼び出しを使用してプログラム的にこれらの操作を実行する手順については、Flow Service APIを使用したデータフローの [](../../tutorials/api/delete-dataflows.md) 削除に関するチュートリアルを参照してください。