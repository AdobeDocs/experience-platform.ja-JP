---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Analyticsデータコネクタ
topic: overview
translation-type: tm+mt
source-git-commit: 14cd3d17c7d9ba602d02925abddec9e0b246a8c8

---


# Analytics Data Connector

Adobe Experience Platformでは、Analytics Data Connector(ADC)を使用してAdobe Analyticsデータを取り込むことができます。 ADCは、Adobe Analyticsが収集したデータをリアルタイムでプラットフォームにストリーミングし、SCDS形式のAnalyticsデータをエクスペリエンスデータモデル(XDM)フィールドに変換して、プラットフォームで使用します。

このドキュメントでは、Adobe Analyticsの概要と、Analyticsデータの使用例を説明します。

## Adobe AnalyticsとAnalyticsのデータ

Adobe Analyticsは、顧客に関する詳細情報、Webプロパティとのやり取り、デジタルマーケティング投資効果の見極め、改善された分野の特定を支援する強力なエンジンです。 Adobe Analyticsでは1年に数兆件ものWebトランザクションを処理しており、ADCを使用すると、この豊富な行動データを簡単にタップして、数分でリアルタイム顧客プロファイルを強化できます。

![](./images/analytics-data-experience-platform.png)

Adobe Analyticsは、様々なデジタルチャネルや、世界中の複数のデータセンターからデータを高度に収集します。 データが収集されると、訪問者ID、セグメント化および変換アーキテクチャ(VISTA)ルールおよび処理ルールが適用され、入力データが形成されます。 生データは、この軽量な処理を経た後、リアルタイムのお客様プロファイルによって消費に対する準備ができていると見なされます。 前述と並行するプロセスでは、同じ処理済みデータがマイクロバッチ処理され、Data Science Workspace、クエリサービス、およびその他のデータ検出アプリケーションで使用するためにプラットフォームデータセットに取り込まれます。

処理ルールについて詳しくは、 [処理ルールの概要](https://docs.adobe.com/content/help/ja-JP/analytics/admin/admin-tools/processing-rules/processing-rules.html) を参照してください。

## エクスペリエンスデータモデル(XDM)

XDMは、Adobe Experience Platformのサービスとの通信に使用するアプリケーションの共通の構造と定義を提供する、公式に文書化された仕様です。

XDM標準に準拠することで、データを均一に組み込むことができ、データの配信や情報の収集が容易になります。

XDMの詳細については、 [XDMシステムの概要を参照してください](../../../xdm/home.md)。

## Adobe AnalyticsからXDMへのフィールドのマッピング方法

プラットフォームユーザーインターフェイスを使用してAnalyticsデータをエクスペリエンスプラットフォームに取り込むためのソース接続が確立されると、データフィールドは自動的にマッピングされ、数分以内にリアルタイム顧客プロファイルに取り込まれます。 プラットフォームUIを使用してAdobe Analyticsとのソース接続を作成する手順については、 [Analytics data connectorsのチュートリアルを参照してください](../../tutorials/ui/create/adobe-applications/analytics.md)。

AnalyticsとExperience Platformの間で発生するフィールドマッピングについて詳しくは、 [Adobe Analyticsフィールドマッピング](./mapping/analytics.md) ガイドを参照してください。

## プラットフォームでのAnalyticsデータの予想される待ち時間は何ですか。

| Analytics データ | 予想される遅延 |
| -------------- | ---------------- |
| リアルタイム顧客プロファイルへの新しいデータ(A4T **は有効になっていません** ) | &lt; 2 分 |
| リアルタイム顧客プロファイルへの新しいデータ(A4T **が有効** ) | &lt; 15 分 |
| Data Lakeへの新しいデータ | &lt; 45 分 |
| バックフィルデータ(13か月分のデータまたは100億イベントのデータのいずれか小さい方) | &lt; 4週間 |

>[!NOTE] 待ち時間は、顧客の構成、データ量、コンシューマーアプリケーションによって異なります。 例えば、Analyticsの導入でパイプラインへの待ち時間 `A4T` が設定されている場合、5 ～ 10分に増えます。