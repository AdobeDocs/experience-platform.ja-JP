---
keywords: Experience Platform;home;popular topics; delete dataflows
description: Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、ソース・ワークスペースからデータ・フローを削除する手順を説明します。
solution: Experience Platform
title: データフローの削除
topic: overview
type: Tutorial
translation-type: tm+mt
source-git-commit: e327a3e195d97c0b547608f360c5b0b6a8aded61
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 9%

---


# データフローの削除

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、 **[!UICONTROL Sources]** Workspaceからデータフローを削除する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)] システム](../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 整理する際に使用される標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディタのチュートリアル](../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

## UIを使用したデータフローの削除

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左のナビゲーションバーで **[!UICONTROL 「ソース]** 」を選択して「 **[!UICONTROL ソース]** 」ワークスペースにアクセスします。 「 **[!UICONTROL カタログ]** 」画面には、アカウントおよびデータフローを作成できる様々なソースが表示されます。 各ソースには、関連付けられた既存のアカウントおよびデータフローの数が表示されます。

「 **[!UICONTROL Dataflows]** 」を選択して、 **[!UICONTROL Dataflows]** ページにアクセスします。

![dataset-flow-アクティビティ](../../images/tutorials/delete/dataflows.png)

既存のデータフローのリストが表示されます。 このページには、ソース、ユーザー名、実行ステータス、最終実行日など、既存のデータフローの並べ替え可能なリストが表示されます。 左上の **ファネルアイコン** を選択して並べ替えます。

![データフローリスト](../../images/tutorials/delete/dataflows-list.png)

並べ替えパネルは、画面の左側に表示され、使用可能なソースのリストが含まれます。
並べ替え機能を使用して、複数のソースを選択できます。

アクセスするソースを選択し、メイン・インタフェースのデータ・フローのリストから削除するデータ・フローを見つけます。 この例では、「source」が選択され、「dataflow name」が「 **[!DNL Azure Blob Storage]** Customerプロファイルdataflow ****」になっています。 並べ替えパネルから複数のソースを選択する場合、リストは作成日順に並べ替えられるので、最も新しく作成されたデータフローが最初に表示されます。

削除するデータフローを選択します。

![データフロー並べ替え](../../images/tutorials/delete/dataflows-sort.png)

[ **[!UICONTROL プロパティ]** ]パネルが画面の右側に表示され、選択したデータフローに関する情報と、[ **[!UICONTROL 編集]スケジュールのオプションが含まれます]**。

データ・フローを削除するには、「 **[!UICONTROL 削除]**」を選択します。

![データフロー並べ替え](../../images/tutorials/delete/dataflows-properties.png)

最後の確認ダイアログボックスが表示されます。「 **[!UICONTROL 削除]** 」を選択してプロセスを完了します。

![次を削除します。](../../images/tutorials/delete/delete.png)

しばらくすると、削除が成功したことを確認する緑の確認ボックスが画面の下部に表示されます。

![確認された](../../images/tutorials/delete/confirmed.png)

## 次の手順

このチュートリアルに従うと、 **[!UICONTROL Sources]** Workspaceを使用して既存のデータフローを削除できます。

APIを使用してプログラムでこれらの操作を実行する手順については、Flow Service APIを使用したデータフローの [!DNL Flow Service][削除に関するチュートリアルを参照してください](../../tutorials/api/delete-dataflows.md)