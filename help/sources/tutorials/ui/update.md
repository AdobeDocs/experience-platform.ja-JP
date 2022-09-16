---
keywords: Experience Platform；ホーム；人気の高いトピック；アカウントの更新
description: 場合によっては、既存のソースアカウントの詳細を更新する必要が生じることがあります。 「ソース」ワークスペースでは、既存のバッチ接続またはストリーミング接続（名前、説明、資格情報など）の詳細を追加、編集および削除できます。
solution: Experience Platform
title: UI でのソース接続アカウントの詳細の更新
topic-legacy: overview
type: Tutorial
exl-id: de264bd4-fe3d-4622-9f24-f1612d8334c9
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 16%

---

# UI でのアカウント詳細の更新

場合によっては、既存のソースアカウントの詳細を更新する必要が生じることがあります。 この [!UICONTROL ソース] workspace では、既存のバッチ接続またはストリーミング接続（名前、説明、資格情報など）の詳細を追加、編集および削除できます。

このチュートリアルでは、既存のアカウントの詳細と資格情報を [!UICONTROL ソース] ワークスペース。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [ソース](../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
- [サンドボックス](../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## アカウントの更新

にログインします。 [Experience PlatformUI](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションから [!UICONTROL ソース] ワークスペース。 選択 **[!UICONTROL アカウント]** 上部のヘッダーから既存のアカウントを表示します。

![カタログ](../../images/tutorials/update/catalog.png)

この **[!UICONTROL アカウント]** ページが表示されます。 このページには、ソース、ユーザー名、データフロー数、作成日に関する情報を含む、表示可能なアカウントのリストが表示されます。

フィルターアイコンを選択します。 ![フィルター](../../images/tutorials/update/filter.png) をクリックして、並べ替えパネルを起動します。

![accounts-list](../../images/tutorials/update/accounts-list.png)

並べ替えパネルには、すべてのソースのリストが表示されます。 リストから複数のソースを選択して、様々なソースに関連付けられたフィルター済みのアカウントの選択にアクセスできます。

既存のアカウントのリストを表示するソースを選択します。 更新するアカウントを特定したら、省略記号 (`...`) をクリックします。

![accounts-sort](../../images/tutorials/update/accounts-sort.png)

ドロップダウンメニューが表示され、次のオプションが表示されます。 **[!UICONTROL データを追加]**, **[!UICONTROL 詳細を編集]**、および **[!UICONTROL 削除]**. 選択 **[!UICONTROL 詳細を編集]** をクリックして、アカウントを更新します。

![更新](../../images/tutorials/update/update.png)

この **[!UICONTROL アカウントの詳細を編集]** ダイアログボックスで、アカウントの名前、説明、および認証資格情報を更新できます。 目的の情報を更新したら、「 **[!UICONTROL 保存]**.

![edit-account-details](../../images/tutorials/update/edit-account-details.png)

しばらくすると、画面の下部に、更新が成功したことを確認する確認ボックスが表示されます。

![update-confirmed](../../images/tutorials/update/update-confirmed.png)

## 次の手順

このチュートリアルでは、 [!UICONTROL ソース] ワークスペースを使用して、既存のソースアカウントの情報を更新します。

を使用してこれらの操作をプログラムで実行する手順については、 [!DNL Flow Service] API( [フローサービス API を使用した接続情報の更新](../../tutorials/api/update.md).
