---
keywords: Experience Platform;home;popular topics;data access;python sdk;spark sdk;data access api
solution: Experience Platform
title: データアクセスの概要
topic: overview
description: Data Accessは、Experience Platform内で取り込まれたデータセットの検出性とアクセス性に重点を置いたツールをユーザーに提供することで、Adobe Experience Platformをサポートします。
translation-type: tm+mt
source-git-commit: bececfde1df15fd8648d75b937da5e264d60b9a4
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 21%

---


# [!DNL Data Access]概要

[!DNL Data Access] で取り込まれたデータセットの検出性とアクセス性に重点を置いたツールを提供することで、Adobe Experience Platformをサポート [!DNL Experience Platform]します。

![Experience Platform でのデータアクセス](images/Data_Access_Experience_Platform.png)

## [!DNL Data Access] API

APIを使用してと接続する方法について詳し [!DNL Data Access] くは、 [!DNL Platform] データアクセス開発ガイドを参照してください [](api.md)。

## Data Science Workspaceのデータへのアクセス

Data Science Workspaceのレシピおよびモデル開発に [!DNL Python] おいて、およびを使用して、データセット [!DNL Spark] に対して読み取りと書き込みを行うことができます。 データへのアクセスについて詳しくは、 [Pythonデータアクセス](../data-science-workspace/authoring/python.md) 、または [Sparkデータアクセス](../data-science-workspace/authoring/spark.md) ドキュメントを参照してください。

詳しくは、 [!DNL Data Science Workspace]Data Science Workspaceの概要を読んで開始 [を参照してください](../data-science-workspace/home.md)。

## データ取得イベントへのサブスクライブ

[!DNL Platform] は、 [Adobeデベロッパーコンソールを通じて特定の高価値イベントを購読用に使用できるようにします](https://www.adobe.com/go/devs_console_ui)。 例えば、データ取り込みイベントに登録して、遅延や障害の可能性についての通知を受け取ることができます。詳しくは、データインジェスト通知の [サブスクライブに関するチュートリアル](../ingestion/quality/subscribe-events.md) を参照してください。