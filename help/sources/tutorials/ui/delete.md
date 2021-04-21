---
keywords: Experience Platform；ホーム；人気の高いトピック；データフローの削除
description: ソース・ワークスペースでは、エラーを含む、または古くなった既存のバッチおよびストリーミング・データ・フローを削除できます。
solution: Experience Platform
title: UI内のデータフローの削除
topic-legacy: overview
type: Tutorial
exl-id: aa224467-7733-40de-aab7-0ff1c557abf2
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 5%

---

# UIのデータフローの削除

[!UICONTROL ソース]ワークスペースでは、エラーを含む、または古くなった既存のバッチおよびストリーミングデータフローを削除できます。

このチュートリアルでは、[!UICONTROL ソース]ワークスペースを使用してデータフローを削除する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [ソース](../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
- [サンドボックス](../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

## データフローの削除

[Experience PlatformUI](https://platform.adobe.com)で、左側のナビゲーションから「**[!UICONTROL ソース]**」を選択して[!UICONTROL ソース]ワークスペースにアクセスし、上部のヘッダーから「**[!UICONTROL データフロー]**」を選択します。

![カタログ](../../images/tutorials/delete/catalog.png)

**[!UICONTROL Dataflows]**&#x200B;ページが表示されます。 このページには、ターゲットデータセット、ソース、アカウント名、作成日など、表示可能なデータフローのリストが表示されます。

左上のフィルターアイコン(![filter-icon](../../images/tutorials/delete/filter.png))を選択して、並べ替えパネルを起動します。

![データフロー](../../images/tutorials/delete/dataflows.png)

並べ替えパネルには、すべてのソースのリストが表示されます。 リストから複数のソースを選択して、選択した特定のソースに関連付けられたデータフローのフィルタ選択にアクセスできます。

既存のデータフローのリストを表示するために使用するソースを選択します。 削除するデータフローを特定したら、データフロー名の横の三点リーダー(`...`)を選択します。

![dataflows-filter](../../images/tutorials/delete/dataflows-filter.png)

ドロップダウンメニューが表示され、データフローのスケジュールを編集したり、データフローを無効にしたり、完全に削除したりするオプションが表示されます。

データフローを削除するには、**[!UICONTROL 削除]**&#x200B;を選択します。

![次を削除します。](../../images/tutorials/delete/delete.png)

最終確認ダイアログボックスが表示されます。 「**[!UICONTROL 削除]**」を選択して、プロセスを完了します。

![confirm](../../images/tutorials/delete/confirm.png)

しばらくすると、削除が成功したことを確認する確認ボックスが画面の下部に表示されます。

![確認された](../../images/tutorials/delete/confirmed.png)

## 次の手順

このチュートリアルに従うと、[!UICONTROL ソース]ワークスペースを使用して既存のデータフローを削除できます。

API呼び出しを使用してプログラム的にこれらの操作を実行する手順については、Flow Service API](../../tutorials/api/delete-dataflows.md)を使用したデータフローの削除に関するチュートリアルを参照してください。[
