---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データアクセスの概要
topic: overview
translation-type: tm+mt
source-git-commit: 49aa2e2664fe658d89b6279d1f869eb30c48ccad
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 3%

---


# データアクセスの概要

[!DNL Data Access] Experience Platform内で取り込まれたデータセットの検出性とアクセス性に重点を置いたツールを提供し、Adobe Experience Platformをサポートします。

![Experience Platformのデータアクセス](images/Data_Access_Experience_Platform.png)

## データアクセスAPI

APIを使用してと接続する方法について詳し [!DNL Data Access] くは、 [!DNL Platform] データアクセス開発ガイドを参照してください [](api.md)。

## Python SDK

SDKを使用して、データセットを使用して読み取りと書き込みを行うことができ [!DNL Python] ます。 SDKに関する詳細は、 [!DNL Python] Python SDKのチュートリアル [](./tutorials/python-sdk.md)を参照してください。

[!DNL Data Science Workspace] は、ノートブックとレシピ内でPython SDKを使用します。 詳細については、「 [!DNL Data Science Workspace]Data Science Workspaceの概要」を読んで開始してください [](../data-science-workspace/home.md)。

## Spark SDK

SDKを使用して、データセットを使用して読み取りと書き込みを行うことができ [!DNL Spark] ます。 SDKに関する詳細は、「 [!DNL Spark] Spark SDK [](./tutorials/spark-sdk.md)」チュートリアルを参照してください。

[!DNL Data Science Workspace] は、ノートブックとレシピ内で [!DNL Spark] SDKを使用します。 詳細については、「 [!DNL Data Science Workspace]Data Science Workspaceの概要」を読んで開始してください [](../data-science-workspace/home.md)。

## データ取り込みイベントのサブスクライブ

[!DNL Platform] は、 [Adobe Developer Consoleを通じて特定の高価値イベントを購読用に使用できるようにします](https://www.adobe.com/go/devs_console_ui)。 例えば、データ取り込みイベントを登録して、遅延や失敗の可能性を通知することができます。 詳しくは、データインジェスト通知の [サブスクライブに関するチュートリアル](../ingestion/quality/subscribe-events.md) を参照してください。