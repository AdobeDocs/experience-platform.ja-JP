---
keywords: Experience Platform;home;popular topics;query service;Query service;query
solution: Experience Platform
title: Adobe Experience Platform クエリサービス
topic: overview
description: このドキュメントでは、Experience Platform 内でのクエリサービスの役割を概説します。
translation-type: tm+mt
source-git-commit: 23516c66a67ae5663dcf90a40ccba98bfd266ab0
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 26%

---


# [!DNL Query Service]概要

Adobe Experience Platform は様々なソースからデータを取得します。マーケターにとっての主要な課題は、このデータの意味を理解して、顧客に関するインサイトを得ることです。Adobe Experience Platform [!DNL Query Service] facilitates that by allowing you to use standard SQL to query data in [!DNL Platform]. Using [!DNL Query Service], you can join any dataset in the [!DNL Data Lake] and capture the query results as a new dataset for use in reporting, machine learning, or for ingestion into [!DNL Real-time Customer Profile]. This document provides an overview of the role of [!DNL Query Service] within [!DNL Experience Platform].

[!DNL Query Service] ブランドがオンラインからオフラインへの顧客の遍歴を結びつけ、オムニチャネルアトリビューションを理解できるようになります。 次のビデオでは、エクスペリエンスビジネスが主要な使用例 [!DNL Query Service] にどのように対処し、どのように [!DNL Query Service] 機能するかを示します。

>[!VIDEO](https://video.tv.adobe.com/v/29795?quality=12&learn=on)

## 使用 [!DNL Query Service]

[!DNL Query Service] には、データをより良く分析するためのSQLクエリを作成できるユーザーインターフェイスとRESTful APIが用意されています。 ユーザーインターフェイスを使用すると、クエリの記述と実行、以前に実行されたクエリの表示、IMS 組織内のユーザーによって保存されたクエリへのアクセスをおこなうことができます。ユーザーインターフェイスは、より広範なデータセットに対してクエリを実行する前に、クエリをテストするためのサンドボックスとして使用されます。More information on using the interactive service within [!DNL Platform] can be found in the [Query Service user interface guide](ui/overview.md). RESTful API も同様のエクスペリエンスを提供し、クエリの記述と実行、将来の使用と繰り返しに備えたクエリのスケジュール、記述するクエリのテンプレートの作成をプログラムで行えるようにします。More information on using the [!DNL Query Service] API can be found in the [Query Service developer guide](api/getting-started.md).

## [!DNL Query Service] および [!DNL Experience Platform] サービス

[!DNL Query Service] が相互に作用し、複数のサー [!DNL Experience Platform] ビスと組み合わせて使用できます。 In order to make the most out of [!DNL Query Service's] capabilities, it is recommended that you become familiar with these services and how they interact with [!DNL Query Service].

### [!DNL Data Science Workspace]

Adobe Experience Platform [!DNL Data Science Workspace] uses machine learning and artificial intelligence to gain insights from data stored within [!DNL Experience Platform]. [!DNL Data Science Workspace] を使用すると、データサイエンティストは、顧客とそのアクティビティに関するレコードと時系列データに基づいてレシピを作成できるため、購入傾向やユーザーが評価して使用する可能性の高い推奨オファーなどの予測が容易になります。You can use SQL within [!DNL Data Science Workspace] by integrating [!DNL Query Service] into [!DNL JupyterLab], allowing you to explore, transform, and analyze Adobe Analytics data. との対話方法の詳細については、 [!DNL Data Science Workspace] 概要をお読みください。詳しく [!DNL Data Science Workspace]は、 [!DNL Query Service] 統合ガイドを参照し [!DNL Data Science Workspace] てくだ [!DNL Query Service]さい。

### [!DNL Segmentation Service]

Adobe Experience Platform [!DNL Segmentation Service] allows users to divide their customers into smaller groups that share similar traits. These segments can subsequently be evaluated to provide better analysis on your [!DNL Real-time Customer Profile] data. [!DNL Query Service] を使用して、内のこのセグメントデータに対してクエリを実行することで、この分析を提供でき [!DNL Data Lake]ます。 Please read the [!DNL Segmentation Service] overview for more information about segmentation, and the [!DNL Profile Query Language] (PQL) guide for more information on how to analyze segments.

### Looker BIの使用例

Adobe Experience Platformでは、行動、CRM、POS（販売時点情報）データを含むすべての保存済みデータセットを取り込み、保存、構造化、取り込みできます。 を使用 [!DNL Experience Platform's Query Service]すると、これらのデータセットにクエリを付け、ビジネスに関する特定の質問に答え、開始がインパクトなインサイトを生み出すことができます。 次のビデオでは、を使用してビジネスインテリジェンス(BI)ツールでダッシュボードを作成する価値を示し [!DNL Query Service]ます。

>[!VIDEO](https://video.tv.adobe.com/v/28981?quality=12&learn=on)

## 次の手順とその他のリソース

By reading this document, you have been introduced to [!DNL Query Service] and how it functions within the greater scope of [!DNL Experience Platform]. For more information on interacting with various endpoints within the [!DNL Query Service] API, please read the [Query Service developer guide](api/getting-started.md). For more information on using the interactive service within [!DNL Platform], please read the [Query Service user interface guide](ui/overview.md). For a comprehensive list on connecting external clients with [!DNL Query Service], please read the [Query Service clients overview](clients/overview.md).

クエリを実行する準備を整えるために、次のビデオをご覧ください。 このビデオでは、クエリエディタインターフェイス、PSQLクライアント、ビジネスインテリジェンス(BI)ソリューション、およびHTTP APIでのクエリの実行に関するヒントとベストプラクティスを紹介します。

>[!VIDEO](https://video.tv.adobe.com/v/29811?quality=12&learn=on)
