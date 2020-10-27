---
keywords: Experience Platform;home;popular topics; delete accounts
description: Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、ソースワークスペースからアカウントを削除する手順を説明します。
solution: Experience Platform
title: アカウントの削除
topic: overview
type: Tutorial
translation-type: tm+mt
source-git-commit: e0168c58e3279f27d45d10b79c130935e46afd3d
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 9%

---


# アカウントの削除

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、 **[!UICONTROL Sources]** ワークスペースからアカウントを削除する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model] (XDM)システム](../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 整理する際に使用される標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディタのチュートリアル](../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

## UIを使用したアカウントの削除

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左のナビゲーションバーで **[!UICONTROL 「ソース]** 」を選択して「 **[!UICONTROL ソース]** 」ワークスペースにアクセスします。 「 **[!UICONTROL カタログ]** 」画面には、アカウントおよびデータフローを作成できる様々なソースが表示されます。 各ソースには、関連付けられた既存のアカウントおよびデータフローの数が表示されます。

「 **[!UICONTROL アカウント]** 」を選択して、「 **[!UICONTROL アカウント]** 」ページにアクセスします。

![カタログ勘定](../../images/tutorials/delete-accounts/catalog.png)

既存のアカウントのリストが表示されます。 このページには、ソース、ユーザー名、関連するデータ・フロー、作成日など、既存のアカウントに関するソート可能なリストが表示されます。 左上の **ファネルアイコン** を選択して並べ替えます。

![データフローリスト](../../images/tutorials/delete-accounts/accounts.png)

並べ替えパネルは、画面の左側に表示され、使用可能なソースのリストが含まれます。 並べ替え機能を使用して、複数のソースを選択できます。

アクセスするソースを選択し、メインインターフェイスでアカウントのリストから削除するアカウントを見つけます。 この例では、ソースが「」 **[!DNL Azure Blob Storage]** で、アカウント名が「 **[!UICONTROL blobTestConnector]**」です。 並べ替えパネルから複数のソースを選択する場合は、作成日順にリストが並べ替えられるので、最も新しく作成されたアカウントが最初に表示されます。

削除するアカウントを選択します。

![データフロー並べ替え](../../images/tutorials/delete-accounts/sort.png)

画面の右側に **[!UICONTROL プロパティ]** パネルが表示され、選択したアカウントに関する情報が表示されます。

削除するアカウント名の横にある三点リーダー(`...`)を選択します。 ポップアップパネルが表示され、 **[!UICONTROL データ]**、 **[!UICONTROL 詳細の編集]****[!UICONTROL 、]**&#x200B;削除の各オプションが表示されます。 「 **[!UICONTROL 削除]** 」を選択して、アカウントを削除します。

![データフロー並べ替え](../../images/tutorials/delete-accounts/delete.png)

最後の確認ダイアログボックスが表示されます。「 **[!UICONTROL 削除]** 」を選択してプロセスを完了します。

![次を削除します。](../../images/tutorials/delete-accounts/confirm.png)

## 次の手順

このチュートリアルに従うと、 **[!UICONTROL ソース]** ・ワークスペースを使用して既存のアカウントを削除できます。

APIを使用してプログラムでこれらの操作を実行する手順については、Flow Service APIを使用した [!DNL Flow Service] 接続の [削除に関するチュートリアルを参照してください](../../tutorials/api/delete.md)