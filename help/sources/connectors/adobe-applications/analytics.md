---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Analyticsデータコネクタ
topic: overview
translation-type: tm+mt
source-git-commit: a1161630c8edae107b784f32ee20af225f9f8c46

---


# Analytics Data Connector

Adobe Experience Platformを使用すると、Analytics Data Connector(ADC)を介してAdobe Analyticsデータを取り込むことができます。 ADCは、Adobe Analyticsが収集したデータをリアルタイムでプラットフォームにストリーミングし、SCDS形式のAnalyticsデータをエクスペリエンスデータモデル(XDM)フィールドに変換して、プラットフォーム別に消費します。

このドキュメントでは、Adobe Analyticsの概要と、Analyticsデータの使用例を説明します。

## Adobe AnalyticsとAnalyticsのデータ

Adobe Analyticsは、顧客に関する詳細情報、Webプロパティとのやり取り、デジタルマーケティングの効果の見極め、改善の余地の特定を支援する強力なエンジンです。 Adobe Analyticsは1年に数兆件ものWebトランザクションを処理し、ADCを使用すると、この豊富な行動データを簡単にタップして、数分でリアルタイム顧客プロファイルを強化できます。

![](./images/analytics-data-experience-platform.png)

Adobe Analyticsは、様々なデジタルチャネルや世界中の複数のデータセンターからデータを高度に収集します。 データが収集されると、訪問者ID、セグメント化および変換アーキテクチャ(VISTA)のルールと処理ルールが適用され、受信データが形成されます。 生データは、この軽量な処理を経た後、リアルタイムのお客様のプロファイルで使用可能と見なされます。 前述と並行するプロセスでは、同じ処理済みデータがマイクロバッチされ、Data Science Workspace、クエリサービス、および他のデータ検出アプリケーションでの使用のためにPlatformデータセットに取り込まれます。

VISTAと処理ルールの詳細については、次のドキュメントを参照してください。
* [VISTAルールの概要](https://marketing.adobe.com/resources/help/ja_JP/reference/VISTA.html)
* [処理ルールの概要](https://docs.adobe.com/content/help/ja-JP/analytics/admin/admin-tools/processing-rules/processing-rules.html)

## エクスペリエンスデータモデル(XDM)

XDMは、Adobe Experience Platform上のサービスとの通信に使用するアプリケーションの共通の構造と定義を提供する、公開された仕様です。

XDM標準に準拠することで、データを統一的に組み込むことができ、データの配信と情報の収集が容易になります。

XDMについて詳しくは、 [XDM Systemの概要を参照してください](../../../xdm/home.md)。

## Adobe AnalyticsからXDMへのフィールドのマッピング方法を教えてください。

プラットフォームユーザーインターフェイスを使用してAnalyticsデータをエクスペリエンスプラットフォームに取り込むためのソース接続が確立されると、データフィールドは自動的にマッピングされ、数分以内にリアルタイム顧客プロファイルに取り込まれます。 プラットフォームUIを使用してAdobe Analyticsとのソース接続を作成する手順については、 [Analytics data connectorsのチュートリアルを参照してください](../../tutorials/ui/create/adobe-applications/analytics.md)。

AnalyticsとExperience Platformの間で発生するフィールドマッピングについて詳しくは、 [Adobe Analyticsフィールドマッピングガイドを参照してください](./analytics-mapping.md) 。

## プラットフォーム上のAnalyticsデータの予想される遅延は何ですか。

| Analytics データ | 予想される遅延 |
| -------------- | ---------------- |
| リアルタイム顧客プロファイルへの新しいデータ(A4T **は** 無効) | &lt; 2 分 |
| リアルタイム顧客プロファイルへの新しいデータ(A4T **が** 有効) | &lt; 15 分 |
| Data Lakeの新しいデータ | &lt; 45 分 |
| データのバックフィル(13か月分のデータまたは100億イベントのデータのいずれか低い方) | &lt; 4週間 |

>[!NOTE] 待ち時間は、顧客の構成、データ量、消費者のアプリケーションによって異なります。 例えば、Analyticsの実装でPipelineへの待ち `A4T` 時間が5 ～ 10分に増えます。