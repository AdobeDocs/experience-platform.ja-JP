---
keywords: Experience Platform；ホーム；人気の高いトピック；アカウントの更新
description: 状況によっては、既存のソースアカウントの詳細を更新する必要が生じる場合があります。 「ソース」ワークスペースでは、既存のバッチ接続またはストリーミング接続の詳細（名前、説明、資格情報など）を追加、編集および削除できます。
solution: Experience Platform
title: UI でのソース接続アカウントの詳細の更新
topic-legacy: overview
type: Tutorial
exl-id: de264bd4-fe3d-4622-9f24-f1612d8334c9
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 9%

---

# UI でのアカウントの詳細の更新

状況によっては、既存のソースアカウントの詳細を更新する必要が生じる場合があります。 [!UICONTROL Sources] ワークスペースでは、既存のバッチ接続またはストリーミング接続の詳細（名前、説明、資格情報など）を追加、編集および削除できます。

このチュートリアルでは、既存のアカウントの詳細と資格情報を [!UICONTROL Sources] ワークスペースから更新する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [ソース](../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Platform サービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
- [サンドボックス](../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

## アカウントの更新

[Experience PlatformUI](https://platform.adobe.com) にログインし、左のナビゲーションから「**[!UICONTROL ソース]**」を選択して、「[!UICONTROL  ソース ]」ワークスペースにアクセスします。 上部のヘッダーから「**[!UICONTROL アカウント]**」を選択して、既存のアカウントを表示します。

![カタログ](../../images/tutorials/update/catalog.png)

「**[!UICONTROL アカウント]**」ページが表示されます。 このページには、ソース、ユーザー名、データフロー数、作成日に関する情報を含む、表示可能なアカウントのリストが表示されます。

左上のフィルターアイコン ![ フィルター ](../../images/tutorials/update/filter.png) を選択して、並べ替えパネルを起動します。

![accounts-list](../../images/tutorials/update/accounts-list.png)

並べ替えパネルには、すべてのソースのリストが表示されます。 リストから複数のソースを選択して、様々なソースに関連付けられた、フィルタされた一連のアカウントにアクセスできます。

既存のアカウントのリストを表示するには、使用するソースを選択します。 更新するアカウントを特定したら、アカウント名の横にある省略記号 (`...`) を選択します。

![accounts-sort](../../images/tutorials/update/accounts-sort.png)

ドロップダウンメニューが表示され、「**[!UICONTROL データを追加]**」、「**[!UICONTROL 詳細を編集]**」、「**[!UICONTROL 削除]**」のオプションが表示されます。 メニューから「**[!UICONTROL 詳細を編集]**」を選択して、アカウントを更新します。

![更新](../../images/tutorials/update/update.png)

**[!UICONTROL アカウントの詳細を編集]** ダイアログボックスでは、アカウントの名前、説明、認証資格情報を更新できます。 目的の情報を更新したら、「**[!UICONTROL 保存]**」を選択します。

![edit-account-details](../../images/tutorials/update/edit-account-details.png)

しばらくすると、更新が正常に完了したことを確認する確認ボックスが画面の下部に表示されます。

![update-confirmed](../../images/tutorials/update/update-confirmed.png)

## 次の手順

このチュートリアルでは、[!UICONTROL Sources] ワークスペースを使用して、既存のソースアカウントの情報を更新しました。

[!DNL Flow Service] API を使用してこれらの操作をプログラムで実行する手順については、[ フローサービス API](../../tutorials/api/update.md) を使用した接続情報の更新に関するチュートリアルを参照してください。
