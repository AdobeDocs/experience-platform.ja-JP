---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Analyticsデータコネクタ
topic: overview
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 3%

---


# Analyticsデータコネクタ

Adobe Experience Platformを使用すると、Analyticsデータコネクタ(ADC)を通じてアドビのAnalyticsデータを取り込むことができます。 ADCは、アドビAnalyticsが収集したデータをリアルタイムでPlatformにストリーミングし、SCDS形式のAnalyticsデータをエクスペリエンスデータモデル(XDM)フィールドに変換して、Platformが使用するようにします。

このドキュメントでは、アドビのAnalyticsの概要とAnalyticsデータの使用例を説明します。

## アドビのAnalyticsとAnalyticsのデータ

アドビのAnalyticsは、顧客に関する詳細情報、Webプロパティとのやり取り、デジタルマーケティング投資効果の見極め、改善された分野の特定を支援する強力なエンジンです。 アドビのAnalyticsは1年に数兆件ものWebトランザクションを処理しています。ADCを使用すると、この豊富な行動データを簡単にタップし、数分でリアルタイムの顧客プロファイルを強化できます。

![](./images/analytics-data-experience-platform.png)

アドビのAnalyticsは、様々なデジタルチャネルや世界中の複数のデータセンターからデータを高いレベルで収集しています。 データが収集されると、訪問者ID、セグメント化および変換アーキテクチャ(VISTA)ルールおよび処理ルールが適用され、入力データが形成されます。 生データは、この軽量な処理を経た後、リアルタイムのお客様プロファイルによって消費に対する準備ができていると見なされます。 前述と並行するプロセスでは、同じ処理済みデータがマイクロバッチ処理され、Data Science Workspace、クエリサービス、および他のデータ検出アプリケーションで使用するためにPlatformデータセットに取り込まれます。

処理ルールについて詳しくは、 [処理ルールの概要](https://docs.adobe.com/content/help/ja-JP/analytics/admin/admin-tools/processing-rules/processing-rules.html) を参照してください。

## エクスペリエンスデータモデル(XDM)

XDMは、Adobe Experience Platform上のサービスとの通信に使用するアプリケーションの共通の構造と定義を提供する、公式に文書化された仕様です。

XDM標準に準拠することで、データを均一に組み込むことができ、データの配信や情報の収集が容易になります。

XDMの詳細については、 [XDMシステムの概要を参照してください](../../../xdm/home.md)。

## フィールドはAdobeAnalyticsからXDMにどのようにマッピングされますか。

Platformユーザーインターフェイスを使用してAnalyticsデータをExperience Platformに移行するためのソース接続が確立されると、データフィールドは自動的にマッピングされ、数分以内にリアルタイムカスタマープロファイルに取り込まれます。 PlatformUIを使用してAdobeAnalyticsとのソース接続を作成する手順については、 [Analyticsdata connectorチュートリアルを参照してください](../../tutorials/ui/create/adobe-applications/analytics.md)。

AnalyticsとExperience Platformの間で発生するフィールドマッピングについて詳しくは、 [AdobeAnalyticsフィールドマッピング](./mapping/analytics.md) ガイドを参照してください。

## Platformに関するAnalyticsデータの予想待ち時間は何ですか？

| Analytics データ | 予想される遅延 |
| -------------- | ---------------- |
| リアルタイム顧客プロファイルへの新しいデータ(A4T **は有効になっていません** ) | &lt; 2 分 |
| リアルタイム顧客プロファイルへの新しいデータ(A4T **が有効** ) | &lt; 15 分 |
| Data Lakeへの新しいデータ | &lt; 45 分 |
| バックフィルデータ(13か月分のデータまたは100億イベントのデータのいずれか小さい方) | &lt; 4 週間 |

>[!NOTE]
>
>待ち時間は、顧客の構成、データ量、コンシューマーアプリケーションによって異なります。 例えば、Analyticsの導入でパイプラインへ `A4T` の待ち時間が設定されている場合、5 ～ 10分に増えます。