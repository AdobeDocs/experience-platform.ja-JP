---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: アカウントとデータセットフローの監視
topic: overview
translation-type: tm+mt
source-git-commit: d3c725c4760acb3857a67d0d30b24732c963a030
workflow-type: tm+mt
source-wordcount: '466'
ht-degree: 1%

---


# アカウントとデータセットフローの監視

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、 *[!UICONTROL ソース]* ワークスペースから既存のアカウントとデータセットフローを表示する手順を説明します。

## はじめに

このチュートリアルでは、次のAdobe Experience Platformのコンポーネントについて十分に理解している必要があります。

- [Experience Data Model(XDM)System](../../../xdm/home.md): 顧客体験データを [!DNL Experience Platform] 整理するための標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成の主な原則とベストプラクティスが含まれます。
   - [スキーマエディタのチュートリアル](../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [リアルタイム顧客プロファイル](../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

## アカウントの監視

「 <a href="https://platform.adobe.com" target="_blank">Adobe Experience Platform</a> 」にログインし、左のナビゲーションバーで「 **[!UICONTROL ソース]** 」を選択して「 *[!UICONTROL ソース]* 」ワークスペースにアクセスします。 カ *[!UICONTROL タログ]* 画面には、様々なソースが表示され、これらのソースを使用してアカウントのデータセットフローを作成できます。 各ソースには、既存のアカウントの数と、それらに関連付けられたデータセットフローが表示されます。

上部のヘッダーから「 *[!UICONTROL アカウント]* 」を選択して、既存のアカウントを表示します。

![カタログ](../../images/tutorials/monitor/catalog.png)

[ *[!UICONTROL アカウント]* ]ページが表示されます。 このページには、アカウントのソース、ユーザー名、データセットフロー数、作成日など、表示可能なアカウントのリストが表示されます。

左上のアイコンを選択して、並べ替えウィンドウを起動します。

![アカウント](../../images/tutorials/monitor/accounts-list.png)

並べ替えパネルを使用すると、特定のソースのアカウントにアクセスできます。 操作するソースを選択し、右側のリストからアカウントを選択します。

![アカウントを選択](../../images/tutorials/monitor/accounts-sort.png)

ア *[!UICONTROL カウント]* ページでは、アクセスしたアカウントに関連付けられた既存のデータセットフローのリストを表示できます。 表示するデータセットフローを選択します。

![accounts-page](../../images/tutorials/monitor/dataset-flows.png)

[ *[!UICONTROL データセットフローアクティビティ]* ]画面が表示されます。 このページには、グラフの形式で消費されるメッセージの割合が表示されます。

![dataset-flow-アクティビティ](../../images/tutorials/monitor/dataset-flows-activity.png)

## データセットフローの監視

データセットフローは、 *[!UICONTROL アカウントを表示しなくても、]* カタログ *[!UICONTROL ページから直接アクセスできます]*。 上部のヘッダーから *[!UICONTROL データセットフロー]* （データセットフロー）を選択し、既存のデータセットフローのリストを表示します。

![データセットフロー](../../images/tutorials/monitor/dataset-flows-list.png)

アカウントと同様に、データセットフローのリストは左上の並べ替えアイコンを使用して並べ替えることができます。 表示するソースを選択し、右側のリストからデータセットフローを選択します。

![select-dataset-flows](../../images/tutorials/monitor/dataset-flows-sort.png)

[ *[!UICONTROL データセットフローアクティビティ]* ]画面が表示されます。 このページには、グラフの形式で消費されるメッセージの割合が表示されます。

![dataset-flow-アクティビティ](../../images/tutorials/monitor/dataset-flows-activity.png)

データセットの監視と取り込みの詳細については、ストリーミングデータフローの [監視に関するチュートリアルを参照してください](../../../ingestion/quality/monitor-data-flows.md)。

## 次の手順

このチュートリアルに従うと、 *[!UICONTROL Sources]* ワークスペースから既存のアカウントおよびデータセットフローに正常にアクセスできます。 受信データは、やなどのダウンストリーム [!DNL Platform] サービスで使用でき [!DNL Real-time Customer Profile] るようになり [!DNL Data Science Workspace]ました。 詳しくは、次のドキュメントを参照してください。

- [リアルタイム顧客プロファイルの概要](../../../profile/home.md)
- [Data Science Workspaceの概要](../../../data-science-workspace/home.md)