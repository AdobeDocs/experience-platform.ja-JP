---
keywords: Experience Platform；ホーム；人気の高いトピック；アカウントを削除
description: Adobe Experience Platform のソースコネクタには、外部ソースの データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、「ソース」ワークスペースからアカウントを削除する手順を説明します。
solution: Experience Platform
title: UI でのソース接続アカウントの削除
topic-legacy: overview
type: Tutorial
exl-id: 7cb65d17-d99d-46ff-b28f-7469d0b57d07
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '487'
ht-degree: 27%

---

# ソース接続アカウントを削除

Adobe Experience Platform のソースコネクタには、外部ソースの データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、 **[!UICONTROL ソース]** ワークスペース。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)]  システム](../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-Time Customer Profile]](../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

## UI を使用したアカウントの削除

>[!TIP]
>
>ソースアカウントを削除する前に、まず、ソースアカウントに関連付けられている既存のデータフローを削除する必要があります。既存のデータフローを削除するには、 [UI でのソースデータフローの削除](./delete.md).

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから **[!UICONTROL ソース]** ワークスペース。 この **[!UICONTROL カタログ]** 画面には、アカウントおよびデータフローを作成できる様々なソースが表示されます。 各ソースには、関連付けられている既存のアカウントとデータフローの数が表示されます。

選択 **[!UICONTROL アカウント]** にアクセスするには **[!UICONTROL アカウント]** ページ。

![catalog-accounts](../../images/tutorials/delete-accounts/catalog.png)

既存のアカウントのリストが表示されます。 このページには、ソース、ユーザー名、関連するデータフロー、作成日など、既存のアカウントに関する並べ替え可能な情報のリストが表示されます。 を選択します。 **ファネルアイコン** を左上に配置して並べ替えます。

![dataflows-list](../../images/tutorials/delete-accounts/accounts.png)

並べ替えパネルは、使用可能なソースのリストを含む画面の左側に表示されます。 並べ替え機能を使用して、複数のソースを選択できます。

アクセスするソースを選択し、削除するアカウントをメインインターフェイスのアカウントのリストから探します。 この例では、選択したソースは **[!DNL Azure Blob Storage]** アカウント名は **[!UICONTROL blobTestConnector]**. 並べ替えパネルから複数のソースを選択する場合、リストは作成日順に並べ替えられるので、最近作成されたアカウントが最初に表示されます。

削除するアカウントを選択します。

![dataflows-sort](../../images/tutorials/delete-accounts/sort.png)

この **[!UICONTROL プロパティ]** パネルが画面の右側に表示され、選択したアカウントに関する情報が含まれます。

省略記号 (`...`) をクリックします。 ポップアップパネルが表示され、次のオプションが表示されます。 **[!UICONTROL データを追加]**, **[!UICONTROL 詳細を編集]**、および **[!UICONTROL 削除]**. 選択 **[!UICONTROL 削除]** をクリックしてアカウントを削除します。

![dataflows-sort](../../images/tutorials/delete-accounts/delete.png)

最終確認ダイアログボックスが表示されます。次の項目を選択します。 **[!UICONTROL 削除]** をクリックしてプロセスを完了します。

![削除](../../images/tutorials/delete-accounts/confirm.png)

## 次の手順

このチュートリアルでは、 **[!UICONTROL ソース]** ワークスペースを使用して、既存のアカウントを削除します。

を使用してこれらの操作をプログラムで実行する手順については、 [!DNL Flow Service] API( [フローサービス API を使用した接続の削除](../../tutorials/api/delete.md)
