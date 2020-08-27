---
keywords: Experience Platform;home;popular topics;data access;python sdk;spark sdk;data access api
solution: Experience Platform
title: データアクセスの概要
topic: overview
description: Data Accessは、Experience Platform内で取り込まれたデータセットの検出性とアクセス性に重点を置いたツールをユーザーに提供することで、Adobe Experience Platformをサポートします。
translation-type: tm+mt
source-git-commit: 75e1d3c9912e54e925032bbc2ae4e984948a30eb
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 19%

---


# [!DNL Data Access] 概要

[!DNL Data Access] で取り込まれたデータセットの検出性とアクセス性に重点を置いたツールを提供することで、Adobe Experience Platformをサポート [!DNL Experience Platform]します。

![Experience Platform でのデータアクセス](images/Data_Access_Experience_Platform.png)

## [!DNL Data Access] API

APIを使用してと接続する方法について詳し [!DNL Data Access] くは、 [!DNL Platform] データアクセス開発ガイドを参照してください [](api.md)。

## [!DNL Python] SDK

SDKを使用して、データセットを使用して読み取りと書き込みを行うことができ [!DNL Python] ます。 SDKに関する詳細は、 [!DNL Python] Python SDKのチュートリアル [](./tutorials/python-sdk.md)を参照してください。

[!DNL Data Science Workspace] は、ノートブックとレシピ内で [!DNL Python] SDKを使用します。 詳細については、「 [!DNL Data Science Workspace]Data Science Workspaceの概要」を読んで開始してください [](../data-science-workspace/home.md)。

## [!DNL Spark] SDK

SDKを使用して、データセットを使用して読み取りと書き込みを行うことができ [!DNL Spark] ます。 SDKに関する詳細は、「 [!DNL Spark] Spark SDK [](./tutorials/spark-sdk.md)」チュートリアルを参照してください。

[!DNL Data Science Workspace] は、ノートブックとレシピ内で [!DNL Spark] SDKを使用します。 詳細については、「 [!DNL Data Science Workspace]Data Science Workspaceの概要」を読んで開始してください [](../data-science-workspace/home.md)。

## データ取得イベントへのサブスクライブ

[!DNL Platform] は、 [Adobeデベロッパーコンソールを通じて特定の高価値イベントを購読用に使用できるようにします](https://www.adobe.com/go/devs_console_ui)。 例えば、データ取り込みイベントに登録して、遅延や障害の可能性についての通知を受け取ることができます。詳しくは、データインジェスト通知の [サブスクライブに関するチュートリアル](../ingestion/quality/subscribe-events.md) を参照してください。