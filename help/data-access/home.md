---
keywords: Experience Platform；ホーム；人気のあるトピック；データアクセス；Python sdk;Spark sdk;Data Access api
solution: Experience Platform
title: データアクセスの概要
topic: overview
description: Data Accessは、Experience Platform内で取り込まれたデータセットの検出性とアクセス性に重点を置いたツールをユーザーに提供することで、Adobe Experience Platformをサポートします。
translation-type: tm+mt
source-git-commit: bececfde1df15fd8648d75b937da5e264d60b9a4
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 20%

---


# [!DNL Data Access]概要

[!DNL Data Access] で取り込まれたデータセットの検出性とアクセス性に重点を置いたツールを提供することで、Adobe Experience Platformをサポート [!DNL Experience Platform]します。

![Experience Platform でのデータアクセス](images/Data_Access_Experience_Platform.png)

## [!DNL Data Access] API

[!DNL Data Access] APIを使用して[!DNL Platform]と接続する方法について詳しくは、[データアクセス開発ガイド](api.md)を参照してください。

## Data Science Workspaceのデータへのアクセス

Data Science Workspaceでのレシピおよびモデル開発に[!DNL Python]と[!DNL Spark]を使用して、データセットに対して読み取りと書き込みを行うことができます。 データへのアクセスについて詳しくは、[Python data access](../data-science-workspace/authoring/python.md)または[Spark data access](../data-science-workspace/authoring/spark.md)のドキュメントを参照してください。

[!DNL Data Science Workspace]の詳細については、[データサイエンスワークスペースの概要](../data-science-workspace/home.md)を読んで開始してください。

## データ取得イベントへのサブスクライブ

[!DNL Platform] は、 [Adobeデベロッパーコンソールを通じて特定の高価値イベントを購読用に使用できるようにします](https://www.adobe.com/go/devs_console_ui)。例えば、データ取り込みイベントに登録して、遅延や障害の可能性についての通知を受け取ることができます。詳しくは、[データインジェスト通知のサブスクライブ](../ingestion/quality/subscribe-events.md)のチュートリアルを参照してください。