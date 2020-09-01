---
keywords: Experience Platform;home;popular topics;Analytics Data Connector;analytics;Analytics
solution: Experience Platform
title: Analytics コネクタ
topic: overview
description: このドキュメントでは、 Analytics の概要と、Analytics データの使用例を説明します。
translation-type: tm+mt
source-git-commit: 6934bfeee84f542558894bbd4ba5759891cd17f3
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 43%

---


# Analytics コネクタ

Adobe Experience Platform を使用すると、Analytics コネクタ（ADC）を介して Adobe Analytics データを取得することができます。ADCは、によって収集されたデータ [!DNL Analytics] をリアルタイム [!DNL Platform] でストリーミングし、SCDS形式の [!DNL Analytics] データを [!DNL Experience Data Model] (XDM)フィールドに変換して、別々に消費し [!DNL Platform]ます。

This document provides an overview of [!DNL Analytics] and describes the use-cases for [!DNL Analytics] data.

## Adobe Analytics と Analytics データ

[!DNL Analytics] は、顧客に関する詳細情報と顧客が Web プロパティとどのようにやり取りするかを学び、デジタルマーケティングの支出が効果的である場所を確認し、改善点を特定する強力なエンジンです。[!DNL Analytics] は1年に数兆件ものwebトランザクションを処理します。ADCを使用すると、この豊富な行動データを簡単にタップして数分でデータ [!DNL Real-time Customer Profile] を豊富にすることができます。

![](./images/analytics-data-experience-platform.png)

At a high level, [!DNL Analytics] collects data from various digital channels and multiple data centers around the world. データが収集されると、訪問者 ID、セグメント化および変換アーキテクチャ（VISTA）のルールと処理ルールが適用され、受信データが形成されます。After raw data has gone through this lightweight processing, it is then considered ready for consumption by [!DNL Real-time Customer Profile]. In a process parallel to the aforementioned, the same processed data is micro-batched and ingested into Platform datasets for consumption by [!DNL Data Science Workspace], [!DNL Query Service], and other data-discovery applications.

処理ルールについて詳しくは、 [処理ルールの概要](https://docs.adobe.com/content/help/ja-JP/analytics/admin/admin-tools/processing-rules/processing-rules.html) を参照してください。

## エクスペリエンスデータモデル（XDM）

XDM is a publicly documented specification that provides common structures and definitions for an application to use to communicate with services on [!DNL Experience Platform].

XDM 標準規格に準拠することで、データを統一的に取り込むことができ、データの配信と情報の収集が容易になります。

XDM について詳しくは、「[XDM システムの概要](../../../xdm/home.md)」を参照してください。

## Adobe Analytics から XDM へのフィールドのマッピング方法

When a source connection is established for bringing [!DNL Analytics] data into [!DNL Experience Platform] using the [!DNL Platform] user interface, data fields are automatically mapped and ingested into [!DNL Real-time Customer Profile] within minutes. For instructions on creating a source connection with [!DNL Analytics] using the [!DNL Platform] UI, see the [Analytics data connector tutorial](../../tutorials/ui/create/adobe-applications/analytics.md).

For detailed information on the field mapping that occurs between [!DNL Analytics] and [!DNL Experience Platform], please visit the [Adobe Analytics field mapping](./mapping/analytics.md) guide.

## Platform の Analytics データで予想される遅延はどのくらいですか。

| Analytics データ | 予想される遅延 |
| -------------- | ---------------- |
| New data to [!DNL Real-time Customer Profile] (A4T **not** enabled) | &lt; 2 分 |
| 新しいデータ [!DNL Real-time Customer Profile] の送信先(A4T **が有効** ) | &lt; 15 分 |
| データレイクの新しいデータ | &lt; 45 分 |
| データのバックフィル（13 か月分のデータまたは 100 億イベントのデータのいずれか低い方） | &lt; 4 週間 |

>[!NOTE]
>
> 遅延は、顧客の構成、データ量、消費者のアプリケーションによって異なります。例えば、Analytics の実装が `A4T` で設定されている場合、パイプラインの遅延が 5 ～ 10 分に増えます。

## Analyticsデータのプライマリ識別子

Analyticsデータコネクターのすべてのヒットには、ECIDとAIDのどちらが存在するかに応じたプライマリ識別子が含まれます。 ECIDがある場合、そのECIDを主識別子として指定します。 AIDがある場合、AIDはプライマリとして指定されます。