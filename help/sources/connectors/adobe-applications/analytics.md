---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Analytics コネクタ
topic: overview
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 97%

---


# Analytics コネクタ

Adobe Experience Platform を使用すると、Analytics コネクタ（ADC）を介して Adobe Analytics データを取得することができます。ADC は、Adobe Analytics が収集したデータをリアルタイムで Platform にストリーミングし、Platform で消費するために SCDS 形式の Analytics データをエクスペリエンスデータモデル（XDM）フィールドに変換します。

このドキュメントでは、Adobe Analytics の概要と、Analytics データの使用例を説明します。

## Adobe Analytics と Analytics データ

Adobe Analytics は、顧客に関する詳細情報と顧客が Web プロパティとどのようにやり取りするかを学び、デジタルマーケティングの支出が効果的である場所を確認し、改善点を特定する強力なエンジンです。Adobe Analytics は 1 年に数兆件もの Web トランザクションを処理します。ADC を使用すると、この豊富な行動データを簡単に利用して、リアルタイムの顧客プロファイルをほんの数分で充実させることができます。

![](./images/analytics-data-experience-platform.png)

高レベルでは、Adobe Analytics は、様々なデジタルチャネルや世界中の複数のデータセンターからデータを収集します。データが収集されると、訪問者 ID、セグメント化および変換アーキテクチャ（VISTA）のルールと処理ルールが適用され、受信データが形成されます。生データは、この軽量な処理を経た後、リアルタイム顧客プロファイルで使用可能と見なされます。前述と並行するプロセスでは、同じ処理済みデータがマイクロバッチされ、Data Science Workspace、クエリサービス、および他のデータ検出アプリケーションでの使用のために Platform データセットに取得されます。

処理ルールについて詳しくは、 [処理ルールの概要](https://docs.adobe.com/content/help/ja-JP/analytics/admin/admin-tools/processing-rules/processing-rules.html) を参照してください。

## エクスペリエンスデータモデル（XDM）

XDM は公に文書化された仕様で、Adobe Experience Platform 上のサービスとの通信に使用するアプリケーションの共通の構造と定義を提供します。

XDM 標準規格に準拠することで、データを統一的に取り込むことができ、データの配信と情報の収集が容易になります。

XDM について詳しくは、「[XDM システムの概要](../../../xdm/home.md)」を参照してください。

## Adobe Analytics から XDM へのフィールドのマッピング方法

Platform ユーザーインターフェイスを使用して Analytics データを Experience Platform に取得するためのソース接続が確立されると、データフィールドは自動的にマッピングされ、数分以内にリアルタイム顧客プロファイルに取得されます。Platform UI を使用して Adobe Analytics とのソース接続を作成する手順については、『[Analytics コネクタチュートリアル](../../tutorials/ui/create/adobe-applications/analytics.md)』を参照してください。

Analytics と Experience Platform の間で発生するフィールドマッピングについて詳しくは、『[Adobe Analytics フィールドマッピングガイド](./mapping/analytics.md)』を参照してください。

## Platform の Analytics データで予想される遅延はどのくらいですか。

| Analytics データ | 予想される遅延 |
| -------------- | ---------------- |
| リアルタイム顧客プロファイルへの新しいデータ（A4T **無効**） | &lt; 2 分 |
| リアルタイム顧客プロファイルへの新しいデータ（A4T **有効**） | &lt; 15 分 |
| データレイクの新しいデータ | &lt; 45 分 |
| データのバックフィル（13 か月分のデータまたは 100 億イベントのデータのいずれか低い方） | &lt; 4 週間 |

>[!NOTE]
>
> 遅延は、顧客の構成、データ量、消費者のアプリケーションによって異なります。例えば、Analytics の実装が `A4T` で設定されている場合、パイプラインの遅延が 5 ～ 10 分に増えます。