---
keywords: Experience Platform；ホーム；人気の高いトピック；データフローを削除
description: 「ソース」ワークスペースでは、エラーが含まれる、または古くなった既存のバッチおよびストリーミングデータフローを削除できます。
solution: Experience Platform
title: UI でのデータフローの削除
type: Tutorial
exl-id: aa224467-7733-40de-aab7-0ff1c557abf2
source-git-commit: 90eb6256179109ef7c445e2a5a8c159fb6cbfe28
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 19%

---

# UI でのデータフローの削除

この [!UICONTROL ソース] workspace では、エラーを含む、または古くなった既存のバッチおよびストリーミングのデータフローを削除できます。

このチュートリアルでは、 [!UICONTROL ソース] ワークスペース。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [ソース](../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
- [サンドボックス](../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## データフローの削除

内 [Experience PlatformUI](https://platform.adobe.com)を選択します。 **[!UICONTROL ソース]** 左側のナビゲーションから [!UICONTROL ソース] ワークスペースを選択し、 **[!UICONTROL データフロー]** を上部のヘッダーから削除します。

![カタログ](../../images/tutorials/delete/catalog.png)

この **[!UICONTROL データフロー]** ページが表示されます。 このページには、表示可能なデータフローのリストが表示されます。このリストには、ターゲットデータセット、ソース、アカウント名、作成日に関する情報が含まれます。

フィルターアイコン (![filter-icon](../../images/tutorials/delete/filter.png)) をクリックして、並べ替えパネルを起動します。

![データフロー](../../images/tutorials/delete/dataflows.png)

並べ替えパネルには、すべてのソースのリストが表示されます。 リストから複数のソースを選択して、選択した特定のソースに関連付けられた、フィルタリングされた選択のデータフローにアクセスできます。

既存のデータフローのリストを表示するソースを選択します。 削除するデータフローを特定したら、省略記号 (`...`) をクリックします。

![dataflows-filter](../../images/tutorials/delete/dataflows-filter.png)

ドロップダウンメニューが表示され、データフローのスケジュールを編集したり、データフローを無効にしたり、完全に削除したりするオプションが提供されます。

選択 **[!UICONTROL 削除]** をクリックして、データフローを削除します。

![削除](../../images/tutorials/delete/delete.png)

最終確認ダイアログボックスが表示されます。 選択 **[!UICONTROL 削除]** をクリックしてプロセスを完了します。

![confirm](../../images/tutorials/delete/confirm.png)

しばらくすると、削除が成功したことを確認する確認ボックスが画面の下部に表示されます。

![確認済み](../../images/tutorials/delete/confirmed.png)

## 次の手順

このチュートリアルでは、 [!UICONTROL ソース] ワークスペースを使用して、既存のデータフローを削除します。

に関するチュートリアルを参照してください。 [フローサービス API を使用したデータフローの削除](../../tutorials/api/delete-dataflows.md) を参照してください。
