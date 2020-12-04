---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート 2019 年 9 月 10 日
doc-type: release notes
last-update: September 13, 2019
author: ens28527
translation-type: tm+mt
source-git-commit: 1b398e479137a12bcfc3208d37472aae3d6721e1
workflow-type: tm+mt
source-wordcount: '532'
ht-degree: 46%

---


# Adobe Experience Platform リリースノート

**リリース日：2019 年 10 月 10 日**

Adobe Experience Platform の既存の機能のアップデート：

* [[!DNL Data Ingestion]](#ingestion)
* [[!DNL Data Science Workspace]](#dsw)
* [[!DNL Query Service]](#query)

## [!DNL Data Ingestion] {#ingestion}

Adobe Experience Platform には、あらゆる種類のデータやデータのレイテンシを取り込むための豊富な機能が用意されています。Adobe Experience Platform [!DNL Data Ingestion] provides multiple alternatives for ingesting data including Batch APIs, Streaming APIs, native Adobe connectors, data integration partners, or the Adobe Experience Platform UI.

**新機能**

| 機能 | 説明 |
| ----------- | ---------- |
| ストリーミング取得用の新しいドメイン | `dcs.data.adobe.net` ドメインは、新しい共通データ収集ドメイン `dcs.adobedc.net` に移動しました。ユーザーは、Adobe Experience Platform の改訂されたストリーミング取得ドキュメントに従って、実装を更新する必要があります。Adobe Experience Platform のストリーミング取得に関連するすべてのドキュメントは、新しいドメインを使用するように更新されています。 |

詳しくは、[データ取得ドキュメント](../../ingestion/home.md)を参照してください。

## [!DNL Data Science Workspace] {#dsw}

Adobe Experience Platform [!DNL Data Science Workspace] is a fully managed service within [!DNL Experience Platform] that enables data scientists to seamlessly generate insights from data and content across Adobe solutions and third-party systems by building and operationalizing Machine Learning Models. [!DNL Data Science Workspace] は、XDMデータの調査と準備を含む、エンドツーエンドのデータ科学ライフサイクルと緊密に統合され、さらにモデルの開発と運用を強化して、機械学習インサイトを自動的に強化 [!DNL Platform][!DNL Real-time Customer Profile] します。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| UI を介したサービスのスケジュール | Integrated with [!DNL Platform] Orchestration Service to automate Model training and scoring with user-defined schedules using the UI. |
| [!DNL Service Gallery] | Browse, monitor, and access machine learning Services with the ability to schedule automated training and scoring jobs, all within the redesigned [!DNL Service Gallery]. |
| [!DNL JupyterLab] 5.0.0 | [!DNL JupyterLab] UIの改善。 |

**既知の問題**

* There is currently no accessible way in the [!DNL Service Gallery] to delete an existing Service. 当面は、[Senesi の機械学習 API リファレンスを](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml)参照して、API 呼び出しを通じて既存のサービスを削除してください。
* The [!DNL Service Gallery] does not have pagination support to filter a Service&#39;s training and scoring runs.
* When configuring scheduled training or scoring runs through the [!DNL Service Gallery], setting the frequency to hourly prevents the schedule from being applied.

詳しくは、「[Data Science Workspace の概要](../../data-science-workspace/home.md)」を参照してください。

## [!DNL Query Service] {#query}

[!DNL Query Service] は、Adobe Experience Platformの標準的なSQLからクエリデータへの使用を可能にし、様々な分析およびデータ管理の使用例をサポートします。 It is a serverless tool that allows you to join datasets from the [!DNL Data Lake] and capture the query results as a new dataset for use in reporting, [!DNL Data Science Workspace], or for ingestion into [!DNL Real-time Customer Profile].

You can use [!DNL Query Service] to build data analysis ecosystems, creating a picture of customers across their various interaction channels. これらのチャネルには、POS（販売時点管理システム）、web、モバイル、CRM システムなどが含まれます。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| 改善点 [!DNL Query Editor] | 保存関数が追加され、クエリを保存して後で使用できるようになりました。Added a &quot;Browse&quot; tab to the [!DNL Query Service] user interface on Adobe Experience Platform that shows queries saved by users in your organization. 表示中のクエリに関する有用なメタデータを表示する「クエリの詳細」パネルが実装されました。 |
| 新しい属性関数 | Adobe-defined functions in [!DNL Query Service] to query for channel attribution with expiration parameters. |
| SQL 構文の強化 | iLike 構文のサポート。 |
| 定義済みの XDM スキーマを使用してデータセットを生成 | Create Table as Select（CTAS）クエリの新しい句が追加され、ターゲットスキーマを指定できるようになりました。 |

詳しくは、[クエリサービスのドキュメント](../../query-service/home.md)を参照してください。