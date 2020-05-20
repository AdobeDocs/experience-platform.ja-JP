---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: アカウントとデータセットフローの監視
topic: overview
translation-type: tm+mt
source-git-commit: fc0a406bdea7b31e046d02427805a9deba557e93
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 1%

---


# アカウントとデータセットフローの監視

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、 *ソース* ワークスペースから既存のアカウントとデータセットフローを表示する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

- [Experience Data Model(XDM)System](../../../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
   - [スキーマ構成の基本](../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成における主な原則とベストプラクティスが含まれます。
   - [スキーマエディタのチュートリアル](../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [リアルタイム顧客プロファイル](../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

## アカウントの監視

<a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし、左のナビゲーションバーで「</a> Sources **** 」を選択して *Sources* ワークスペースにアクセスします。 カ *タログ* 画面には、様々なソースが表示され、これらのソースを使用してアカウントのデータセットフローを作成できます。 各ソースには、既存のアカウントの数と、それらに関連付けられたデータセットフローが表示されます。

上部のヘッダーから「 *アカウント* 」を選択して、既存のアカウントを表示します。

![カタログ](../../images/tutorials/monitor/catalog.png)

[ *アカウント* ]ページが表示されます。 このページには、アカウントのソース、ユーザー名、データセットフロー数、作成日など、表示可能なアカウントのリストが表示されます。

左上のアイコンを選択して、並べ替えウィンドウを起動します。

![アカウント](../../images/tutorials/monitor/accounts-list.png)

並べ替えパネルを使用すると、特定のソースのアカウントにアクセスできます。 操作するソースを選択し、右側のリストからアカウントを選択します。

![アカウントを選択](../../images/tutorials/monitor/accounts-sort.png)

ア *カウント* ページでは、アクセスしたアカウントに関連付けられた既存のデータセットフローのリストを表示できます。 表示するデータセットフローを選択します。

![accounts-page](../../images/tutorials/monitor/dataset-flows.png)

[ *データセットフローアクティビティ* ]画面が表示されます。 このページには、グラフの形式で消費されるメッセージの割合が表示されます。

![dataset-flow-アクティビティ](../../images/tutorials/monitor/dataset-flows-activity.png)

## データセットフローの監視

データセットフローは、 *アカウントを表示しなくても、* カタログ *ページから直接アクセスできます*。 上部のヘッダーから *データセットフロー* （データセットフロー）を選択し、既存のデータセットフローのリストを表示します。

![データセットフロー](../../images/tutorials/monitor/dataset-flows-list.png)

アカウントと同様に、データセットフローのリストは左上の並べ替えアイコンを使用して並べ替えることができます。 表示するソースを選択し、右側のリストからデータセットフローを選択します。

![select-dataset-flows](../../images/tutorials/monitor/dataset-flows-sort.png)

[ *データセットフローアクティビティ* ]画面が表示されます。 このページには、グラフの形式で消費されるメッセージの割合が表示されます。

![dataset-flow-アクティビティ](../../images/tutorials/monitor/dataset-flows-activity.png)

データセットの監視と取り込みの詳細については、ストリーミングデータフローの [監視に関するチュートリアルを参照してください](../../../ingestion/quality/monitor-data-flows.md)。

## 次の手順

このチュートリアルに従うと、 *Sources* ワークスペースから既存のアカウントおよびデータセットフローに正常にアクセスできます。 受信データは、リアルタイム顧客プロファイルやデータサイエンスワークスペースなどのダウンストリームプラットフォームサービスで使用できるようになりました。 詳しくは、次のドキュメントを参照してください。

- [リアルタイム顧客プロファイルの概要](../../../profile/home.md)
- [Data Science Workspaceの概要](../../../data-science-workspace/home.md)