---
keywords: Experience Platform；ホーム；人気のあるトピック；アカウントの削除
description: Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに従って取り込む機能を提供します。 このチュートリアルでは、「ソース」ワークスペースからアカウントを削除する手順を説明します。
solution: Experience Platform
title: UI でのソース接続アカウントの削除
topic-legacy: overview
type: Tutorial
exl-id: 7cb65d17-d99d-46ff-b28f-7469d0b57d07
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '455'
ht-degree: 12%

---

# ソース接続アカウントの削除

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに従って取り込む機能を提供します。 このチュートリアルでは、**[!UICONTROL Sources]** ワークスペースからアカウントを削除する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)] システム](../../../xdm/home.md):顧客体験データを整理する際に使用す [!DNL Experience Platform] る標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../xdm/tutorials/create-schema-ui.md):スキーマエディターの UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

## UI を使用したアカウントの削除

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから「**[!UICONTROL ソース]**」を選択して、「**[!UICONTROL ソース]**」ワークスペースにアクセスします。 「**[!UICONTROL カタログ]**」画面には、アカウントとデータフローを作成できる様々なソースが表示されます。 各ソースには、関連付けられている既存のアカウントとデータフローの数が表示されます。

**[!UICONTROL アカウント]** を選択して、**[!UICONTROL アカウント]** ページにアクセスします。

![カタログアカウント](../../images/tutorials/delete-accounts/catalog.png)

既存のアカウントのリストが表示されます。 このページには、ソース、ユーザー名、関連するデータフロー、作成日など、既存のアカウントに関する並べ替え可能な情報のリストが表示されます。 左上の **ファネルアイコン** を選択して並べ替えます。

![dataflows-list](../../images/tutorials/delete-accounts/accounts.png)

並べ替えパネルは、使用可能なソースのリストを含む画面の左側に表示されます。 並べ替え機能を使用して、複数のソースを選択できます。

アクセスするソースを選択し、削除するアカウントをメインインターフェイスのアカウントのリストから探します。 この例では、選択するソースは **[!DNL Azure Blob Storage]** で、アカウント名は **[!UICONTROL blobTestConnector]** です。 並べ替えパネルから複数のソースを選択する場合、リストは作成日順で並べ替えられるので、最も最近作成されたアカウントが最初に表示されます。

削除するアカウントを選択します。

![dataflows-sort](../../images/tutorials/delete-accounts/sort.png)

画面の右側に **[!UICONTROL プロパティ]** パネルが表示され、選択したアカウントに関する情報が表示されます。

削除するアカウント名の横にある省略記号 (`...`) を選択します。 ポップアップパネルが表示され、「**[!UICONTROL データを追加]**」、「**[!UICONTROL 詳細を編集]**」、「**[!UICONTROL 削除]**」の各オプションが表示されます。 **[!UICONTROL 削除]** を選択して、アカウントを削除します。

![dataflows-sort](../../images/tutorials/delete-accounts/delete.png)

最後の確認ダイアログボックスが表示されます。「**[!UICONTROL 削除]**」を選択して、プロセスを完了します。

![次を削除します。](../../images/tutorials/delete-accounts/confirm.png)

## 次の手順

このチュートリアルでは、**[!UICONTROL ソース]** ワークスペースを使用して既存のアカウントを削除しました。

[!DNL Flow Service] API を使用してこれらの操作をプログラムで実行する手順については、[ フローサービス API](../../tutorials/api/delete.md) を使用した接続の削除に関するチュートリアルを参照してください。
