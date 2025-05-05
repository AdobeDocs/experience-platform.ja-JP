---
keywords: Experience Platform；ホーム；人気のトピック；アカウントの更新
description: 状況によっては、既存のソースアカウントの詳細を更新する必要が生じる場合があります。 ソースワークスペースを使用すると、既存のバッチまたはストリーミング接続（名前、説明、資格情報など）の詳細を追加、編集および削除できます。
solution: Experience Platform
title: UI でのSource接続アカウントの詳細の更新
type: Tutorial
exl-id: de264bd4-fe3d-4622-9f24-f1612d8334c9
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 5%

---

# UI でのアカウント詳細の更新

状況によっては、既存のソースアカウントの詳細を更新する必要が生じる場合があります。 [!UICONTROL &#x200B; ソース &#x200B;] ワークスペースを使用すると、既存のバッチまたはストリーミング接続（名前、説明、資格情報など）の詳細を追加、編集および削除できます。

このチュートリアルでは、[!UICONTROL &#x200B; ソース &#x200B;] ワークスペースから既存のアカウントの詳細と資格情報を更新する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [ ソース ](../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
- [ サンドボックス ](../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## アカウントの更新

[Experience Platform UI にログインし ](https://platform.adobe.com) 左側のナビゲーションから「**[!UICONTROL ソース]**」を選択して、「[!UICONTROL &#x200B; ソース &#x200B;] ワークスペースにアクセスします。 上部のヘッダーから **[!UICONTROL アカウント]** を選択して、既存のアカウントを表示します。

![カタログ](../../images/tutorials/update/catalog.png)

**[!UICONTROL アカウント]** ページが表示されます。 このページには、ソース、ユーザー名、データフロー数、作成日などの、表示可能なアカウントのリストが表示されます。

左上のフィルターアイコン ![ フィルター ](/help/images/icons/filter.png) を選択して、並べ替えパネルを開きます。

![accounts-list](../../images/tutorials/update/accounts-list.png)

並べ替えパネルには、すべてのソースのリストが表示されます。 リストから複数のソースを選択して、異なるソースに関連付けられたフィルター適用済みアカウントの選択にアクセスできます。

操作するソースを選択して、既存のアカウントのリストを表示します。 更新するアカウントを識別したら、アカウント名の横にある省略記号（`...`）を選択します。

![accounts-sort](../../images/tutorials/update/accounts-sort.png)

ドロップダウンメニューが表示され、**[!UICONTROL データを追加]**、**[!UICONTROL 詳細を編集]**、および **[!UICONTROL 削除]** するオプションが提供されます。 メニューから **[!UICONTROL 詳細を編集]** を選択して、アカウントを更新します。

![更新](../../images/tutorials/update/update.png)

**[!UICONTROL アカウントの詳細を編集]** ダイアログボックスでは、アカウントの名前、説明および認証資格情報を更新できます。 必要な情報を更新したら、「**[!UICONTROL 保存]**」を選択します。

![edit-account-details](../../images/tutorials/update/edit-account-details.png)

しばらくすると、更新が正常に完了したことを確認する確認ボックスが画面の下部に表示されます。

![update-confirmed](../../images/tutorials/update/update-confirmed.png)

## 次の手順

このチュートリアルでは、[!UICONTROL Sources] ワークスペースを正常に使用して、既存のソースアカウントの情報を更新しました。

[!DNL Flow Service] API を使用してプログラムでこれらの操作を実行する手順については、[Flow Service API を使用した接続情報の更新 ](../../tutorials/api/update.md) に関するチュートリアルを参照してください。
