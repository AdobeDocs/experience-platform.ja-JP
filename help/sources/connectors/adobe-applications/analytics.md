---
keywords: Experience Platform；ホーム；人気のあるトピック；Analyticsソースコネクタ；analytics;Analytics
solution: Experience Platform
title: レポートスイートデータ用のAdobe Analytics Source Connector
topic-legacy: overview
description: このドキュメントでは、 Analytics の概要と、Analytics データの使用例を説明します。
exl-id: c4887784-be12-40d4-83bf-94b31eccdc2e
source-git-commit: 9defe1c3087c2f1284ceedede9d274a51cf97b96
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 27%

---

# レポートスイートデータ用のAdobe Analyticsソースコネクタ

Adobe Experience Platformでは、Adobe AnalyticsデータをAnalyticsソースコネクタを通じて取り込むことができます。 [!DNL Analytics]ソースコネクタは、[!DNL Analytics]によって収集されたデータをリアルタイムでPlatformにストリーミングし、SCDS形式の[!DNL Analytics]データを[!DNL Experience Data Model](XDM)フィールドに変換して、Platformで使用します。

このドキュメントでは、[!DNL Analytics]の概要と[!DNL Analytics]データの使用例を説明します。

## Adobe Analytics と Analytics データ

[!DNL Analytics] は、顧客に関する詳細情報、顧客がWebプロパティとどのようにやり取りするか、デジタルマーケティングの支出が効果的かを確認し、改善点を特定するのに役立つ強力なエンジンです。[!DNL Analytics] は1年に数兆件ものWebトランザクションを処理し、ソースコネクタを使用する [!DNL Analytics] と、この豊富な行動データを簡単にタップして、数分でをエンリッチメ [!DNL Real-time Customer Profile] ントできます。

![](./images/analytics-data-experience-platform.png)

[!DNL Analytics]は、大まかに、様々なデジタルチャネルや世界中の複数のデータセンターからデータを収集します。 データが収集されると、訪問者ID、セグメント化および変換アーキテクチャ(VISTA)ルール、処理ルールが適用され、受信データが形成されます。 生データは、この軽量な処理を経た後、[!DNL Real-time Customer Profile]によって消費可能と見なされます。 前述と並行するプロセスでは、同じ処理済みデータがマイクロバッチされ、[!DNL Data Science Workspace]、[!DNL Query Service]および他のデータ検出アプリケーションでの使用のためにPlatformデータセットに取り込まれます。

処理ルールについて詳しくは、[処理ルールの概要](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/processing-rules/processing-rules.html)を参照してください。

## エクスペリエンスデータモデル（XDM）

XDM は公に文書化された仕様で、 Experience Platform 上のサービスとの通信に使用するアプリケーションの共通の構造と定義を提供します。

XDM 標準規格に準拠することで、データを統一的に取り込むことができ、データの配信と情報の収集が容易になります。

XDM について詳しくは、「[XDM システムの概要](../../../xdm/home.md)」を参照してください。

## Adobe Analytics から XDM へのフィールドのマッピング方法

Platformユーザーインターフェイスを使用して[!DNL Analytics]データをExperience Platformに取り込むためのソース接続が確立されると、データフィールドは自動的にマッピングされ、数分以内に[!DNL Real-time Customer Profile]に取り込まれます。 Platform UIを使用して[!DNL Analytics]とのソース接続を作成する手順については、[Analyticsソースコネクタのチュートリアル](../../tutorials/ui/create/adobe-applications/analytics.md)を参照してください。

[!DNL Analytics]とExperience Platformの間で発生するフィールドマッピングについて詳しくは、『[Adobe Analyticsフィールドマッピング](./mapping/analytics.md)』ガイドを参照してください。

## Platform の Analytics データで予想される遅延はどのくらいですか。

次の表に、Platform上のAnalyticsデータで予想される遅延を示します。  遅延は、顧客の構成、データ量、消費者のアプリケーションによって異なります。例えば、Analytics の実装が `A4T` で設定されている場合、パイプラインの遅延が 5 ～ 10 分に増えます。

| Analytics データ | 予想される遅延 |
| -------------- | ---------------- |
| [!DNL Real-time Customer Profile]への新しいデータ（A4T **not**&#x200B;有効） | &lt; 2 分 |
| [!DNL Real-time Customer Profile]への新しいデータ(A4T **is** enabled) | &lt; 15 分 |
| データレイクの新しいデータ | &lt; 90 分 |
| データのバックフィル（13 か月分のデータまたは 100 億イベントのデータのいずれか低い方） | &lt; 4 週間 |

>[!NOTE]
>
>Analyticsバックフィルデータは[!DNL Profile]に取り込まれないので、ライセンスプロファイルには反映されません。

## [!DNL Analytics]データのプライマリ識別子

[!DNL Analytics]ソースコネクタのすべてのヒットには、ECIDとAAIDのどちらが存在するかに応じたプライマリ識別子が含まれます。 ECIDがある場合、そのECIDが主識別子として指定されます。 AAIDがある場合、AAIDがプライマリとして指定されます。
