---
keywords: Experience Platform；ホーム；人気のトピック；アカウントの削除
description: Adobe Experience PlatformのSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、ソース ワークスペースからアカウントを削除する手順を説明します。
solution: Experience Platform
title: UI でのSource接続アカウントの削除
type: Tutorial
exl-id: 7cb65d17-d99d-46ff-b28f-7469d0b57d07
source-git-commit: 90eb6256179109ef7c445e2a5a8c159fb6cbfe28
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 19%

---

# ソース接続アカウントの削除

Adobe Experience PlatformのSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、**[!UICONTROL ソース]** ワークスペースからアカウントを削除する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)]  システム](../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-Time Customer Profile]](../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

## UI を使用したアカウントの削除

>[!TIP]
>
>ソースアカウントを削除する前に、まず、ソースアカウントに関連付けられている既存のデータフローを削除する必要があります。既存のデータフローを削除するには、[UI でのソースデータフローの削除 &#x200B;](./delete.md) のチュートリアルを参照してください。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから **[!UICONTROL ソース]** を選択して **[!UICONTROL ソース]** ワークスペースにアクセスします。 **[!UICONTROL カタログ]** 画面には、アカウントやデータフローを作成できる様々なソースが表示されます。 各ソースには、関連付けられている既存のアカウントとデータフローの数が表示されます。

「**[!UICONTROL アカウント]**」を選択して、「**[!UICONTROL アカウント]**」ページにアクセスします。

![catalog-accounts](../../images/tutorials/delete-accounts/catalog.png)

既存のアカウントのリストが表示されます。 このページには、ソース、ユーザー名、関連データフロー、作成日など、既存のアカウントに関する並べ替え可能な情報のリストが表示されます。 左上の **ファネルアイコン** を選択して、並べ替えます。

![dataflows-list](../../images/tutorials/delete-accounts/accounts.png)

使用可能なソースのリストを含む並べ替えパネルが画面の左側に表示されます。 並べ替え関数を使用して、複数のソースを選択できます。

アクセスするソースを選択し、メインインターフェイスのアカウントのリストから削除するアカウントを見つけます。 この例では、選択したソースは **[!DNL Azure Blob Storage]** で、アカウント名は **[!UICONTROL blobTestConnector]** です。 並べ替えパネルで複数のソースを選択すると、リストが作成日順に並べ替えられるので、最近作成したアカウントが最初に表示されます。

削除するアカウントを選択します。

![dataflows-sort](../../images/tutorials/delete-accounts/sort.png)

**[!UICONTROL プロパティ]** パネルが画面の右側に表示され、選択したアカウントに関する情報が含まれます。

削除するアカウント名の横にある省略記号（`...`）を選択します。 ポップアップパネルが表示され、**[!UICONTROL データを追加]**、**[!UICONTROL 詳細を編集]**、および **[!UICONTROL 削除]** するオプションが提供されます。 「**[!UICONTROL 削除]**」を選択して、アカウントを削除します。

![dataflows-sort](../../images/tutorials/delete-accounts/delete.png)

最終確認ダイアログボックスが表示されるので、「**[!UICONTROL 削除]**」を選択してプロセスを完了します。

![削除](../../images/tutorials/delete-accounts/confirm.png)

## 次の手順

このチュートリアルでは、既存のアカウントを削除するために **[!UICONTROL ソース]** ワークスペースを正常に使用しました。

[!DNL Flow Service] API を使用してプログラムでこれらの操作を実行する手順については、[Flow Service API を使用した接続の削除 &#x200B;](../../tutorials/api/delete.md) に関するチュートリアルを参照してください。
