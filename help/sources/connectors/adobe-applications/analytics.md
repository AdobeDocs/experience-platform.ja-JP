---
keywords: Experience Platform；ホーム；人気の高いトピック；Analytics Data Connector;analytics;Analytics
solution: Experience Platform
title: レポートスイートデータ用Adobe Analyticsソースコネクタ
topic: 概要
description: このドキュメントでは、 Analytics の概要と、Analytics データの使用例を説明します。
translation-type: tm+mt
source-git-commit: 126b3d1cf6d47da73c6ab045825424cf6f99e5ac
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 41%

---


# レポートスイートデータ用Adobe Analyticsコネクタ

Adobe Experience Platform を使用すると、Analytics Data Connector（ADC）を介して Adobe Analytics データを取り込むことができます。ADCは、[!DNL Analytics]が収集したデータをリアルタイムで[!DNL Platform]にストリーミングし、SCDS形式の[!DNL Analytics]データを[!DNL Experience Data Model] (XDM)フィールドに変換して[!DNL Platform]が使用します。

このドキュメントでは、[!DNL Analytics]の概要と[!DNL Analytics]データの使用例を説明します。

## Adobe Analytics と Analytics データ

[!DNL Analytics] は、顧客に関する詳細情報と顧客が Web プロパティとどのようにやり取りするかを学び、デジタルマーケティングの支出が効果的である場所を確認し、改善点を特定する強力なエンジンです。[!DNL Analytics] は1年に数兆件ものwebトランザクションを処理し、ADCを使用すると、この豊富な行動データを簡単にタップして数分 [!DNL Real-time Customer Profile] でデータを拡充できます。

![](./images/analytics-data-experience-platform.png)

[!DNL Analytics]は、高いレベルで、世界中の様々なデジタルチャネルや複数のデータセンターからデータを収集します。 データが収集されると、訪問者 ID、セグメント化および変換アーキテクチャ（VISTA）のルールと処理ルールが適用され、受信データが形成されます。生データがこの軽量処理を経た後、[!DNL Real-time Customer Profile]は消費の準備ができていると見なします。 前述と並行するプロセスでは、同じ処理済みデータがマイクロバッチ処理され、プラットフォームデータセットに取り込まれて[!DNL Data Science Workspace]、[!DNL Query Service]、および他のデータ検出アプリケーションで使用されます。

処理ルールの詳細については、[処理ルールの概要](https://docs.adobe.com/content/help/ja-JP/analytics/admin/admin-tools/processing-rules/processing-rules.html)を参照してください。

## エクスペリエンスデータモデル（XDM）

XDMは、[!DNL Experience Platform]上のサービスとの通信に使用するアプリケーションの共通の構造と定義を提供する、公式に文書化された仕様です。

XDM 標準規格に準拠することで、データを統一的に取り込むことができ、データの配信と情報の収集が容易になります。

XDM について詳しくは、「[XDM システムの概要](../../../xdm/home.md)」を参照してください。

## Adobe Analytics から XDM へのフィールドのマッピング方法

[!DNL Platform]ユーザーインターフェイスを使用して[!DNL Analytics]データを[!DNL Experience Platform]に取り込むためのソース接続が確立されると、データフィールドは自動的にマッピングされ、数分以内に[!DNL Real-time Customer Profile]に取り込まれます。 [!DNL Platform] UIを使用して[!DNL Analytics]とのソース接続を作成する手順については、[Analytics data connector tutorial](../../tutorials/ui/create/adobe-applications/analytics.md)を参照してください。

[!DNL Analytics]と[!DNL Experience Platform]の間に発生するフィールドマッピングの詳細については、『[Adobe Analyticsフィールドマッピング](./mapping/analytics.md)』ガイドを参照してください。

## Platform の Analytics データで予想される遅延はどのくらいですか。

| Analytics データ | 予想される遅延 |
| -------------- | ---------------- |
| [!DNL Real-time Customer Profile]への新しいデータ（A4T ****&#x200B;が有効になっていません） | &lt; 2 分 |
| [!DNL Real-time Customer Profile]への新しいデータ（A4T **は**&#x200B;有効） | &lt; 15 分 |
| データレイクの新しいデータ | &lt; 90 分 |
| データのバックフィル（13 か月分のデータまたは 100 億イベントのデータのいずれか低い方） | &lt; 4 週間 |

>[!NOTE]
>
> 遅延は、顧客の構成、データ量、消費者のアプリケーションによって異なります。例えば、Analytics の実装が `A4T` で設定されている場合、パイプラインの遅延が 5 ～ 10 分に増えます。

## Analyticsデータのプライマリ識別子

Analyticsデータコネクターのすべてのヒットには、ECIDとAIDのどちらが存在するかに応じたプライマリ識別子が含まれます。 ECIDがある場合、そのECIDを主識別子として指定します。 AIDがある場合、AIDはプライマリとして指定されます。