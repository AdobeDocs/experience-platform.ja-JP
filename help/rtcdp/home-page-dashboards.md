---
keywords: 指標の概要；rtcdp 指標の概要
title: リアルタイム顧客データプラットフォームのホームページとダッシュボード
seo-title: Real-time Customer Data Platform Home Page and Dashboards
description: Adobe Experience Platform のダッシュボード、ホームページ、初回ユーザーエクスペリエンス
seo-description: Dashboards, Home Page, and First-Time User Experience of Adobe Experience Platform
exl-id: ced5b69c-5bb5-4e06-9cb4-938e36e6e5cc
source-git-commit: 0085306a2f5172eb19590cc12bc9645278bd2b42
workflow-type: tm+mt
source-wordcount: '619'
ht-degree: 74%

---

# [!DNL Real-time Customer Data Platform] ホームページとダッシュボード

リアルタイム CDP にログインすると、指標ダッシュボードを含むReal-time Customer Data Platform（リアルタイム CDP）のホームページが表示されます。

ホームページは、指標カードが表示される場所の 1 つに過ぎません。リアルタイム CDP は、ユーザーの経験を通じて指標カードを提供します。これらの指標は、システム内のデータ、プロファイル、セグメントオーディエンスに関する情報を提供します。

![画像](assets/home.png)

リアルタイム CDP にログインしたときにシステムにデータがない場合、ホームページのダッシュボードは表示されません。この場合、ホームページでは初めて使用するユーザーのための学習教材を提供します。データが収集されると（つまり、<!--sources-->データセット、プロファイル、セグメント、宛先が作成され、データがシステムにフローされると）、ダッシュボードは自動的に更新され、そのデータに関する情報が表示されます <!-- in metric cards-->。

## ホームページのダッシュボードビュー

<!--The dashboard shows information in several areas. Each category of information displays for the time range shown beneath the data.-->

ダッシュボードは次のように分かれています<!-- two areas.-->。

* **リーダーボード**&#x200B;は、ダッシュボードの上部に表示されます。リーダーボードには、システム内のデータセット、プロファイル、セグメント、宛先の数が表示されます。

   ![画像](assets/leaderboard.png)

<!-- * **Metric cards** display beneath the leaderboard. Metric cards show additional information, such as percentages or trends. Metric cards appear as data is collected.
    ![image](assets/home-metrics.jpg)
Some information is shown in different ways on both the leaderboard and metric cards. -->
* **最近の項目**&#x200B;には、システムに追加された最新の 5 つのデータセット、ソース、セグメント、宛先が表示されます。

   ![画像](assets/recent.png)

追加の指標（プロファイルやセグメントなど）は、リアルタイム顧客データプラットフォームの他の部分で使用できます。

### データセット

**[!UICONTROL Datasets]** カウンターは、システム内のデータセットの数と [!DNL Platform] 内のデータの量を示します。 このカウンターは、データセットの作成時に更新されます。

データセットの詳細については、「[データセットの概要](../catalog/datasets/overview.md)」を参照してください。

### プロファイル

**[!UICONTROL プロファイル]** の数は、[!DNL Real-time Customer Profile] 内のプロファイルを持つ人の合計数を示します。 プロファイルフラグメントは含まれません。これは、アドレス可能なオーディエンスの合計です。

このカウントでは、「統合プロファイル」の[結合ポリシー](profile/merge-policies.md)構成で設定された既定の結合ポリシーを使用します。

プロファイルの数は 24 時間ごとに更新されます。

プロファイルの詳細については、「[リアルタイム CDP での顧客の統合ビュー](profile/profile-overview.md)」を参照してください。

### セグメント

**[!UICONTROL セグメント]**：組織に対して作成されたセグメントの合計数が表示されます。この数は、新しいセグメントが作成されると更新されます。

セグメントについて詳しくは、「[セグメント化サービスの概要](segmentation/segmentation-overview.md)」を参照してください。

### 宛先

**[!UICONTROL 宛先]**&#x200B;は、組織に対して作成された宛先の合計数を表示します。この数は、新しい宛先が作成されると更新されます。

宛先について詳しくは、「[宛先の概要](destinations/overview.md)」を参照してください。

<!-- ### Successful profile records

In the leaderboard **[!UICONTROL Successful profile records]** shows the total number of records that have been successfully processed into the profile.

There is also a metric card that shows the percentage of successful records. Select **[!UICONTROL View datasets]** to see more details about the profile records. Hover over the colored area of the graph to see additional details:

![image](assets/home-profilerecords-details.PNG)

The number of successful profile records is updated hourly. 

For more information about profiles, see [A unified view of your customer in Real-time CDP](profile/profile-overview.md).

### Total profile records

The **[!UICONTROL Total profile records]** metric card shows the total number of data records enabled to feed into the profiles, and the percentage that are successful, updated once per day. This does not include all data in the data lake, because some data might not be enabled to feed into the profiles.

 Hover over the colored area of the graph to see additional details about the successful profiles:

![image](assets/home-profile-details.PNG)

Select **[!UICONTROL View profiles]** to see more details about the profile records.

For more information about profiles, see [A unified view of your customer in Real-time CDP](profile/profile-overview.md).

For more information about viewing a specific profile, see [Profile viewer](profile/profile-viewer.md).

### Failed profile records

In the leaderboard, **[!UICONTROL Failed profile records]** counts the number of records that failed to process into the profile.

The **[!UICONTROL Failed profile records]** metric card shows this count, and includes a graphical representation that helps you see how failures have trended during the time shown below the graphic. This chart is updated hourly. Select **[!UICONTROL View datasets]** to see more details about the profile records.

The number of failed profile records is updated hourly. -->

### 最近のデータセット

**[!UICONTROL 最近のデータセット]**&#x200B;カードには、組織内で作成された最新の 5 つのデータセットが表示されます。このリストは、新しいデータセットが作成されると更新されます。

データセットを選択してその項目の詳細を表示するか、**[!UICONTROL すべての]** を表示してデータセットのリストを表示します。 ここから、特定のソースを選択して詳細を表示できます。

データセットの詳細については、「[データセットの概要](../catalog/datasets/overview.md)」を参照してください。

### 最近のソース

「**[!UICONTROL 最近のソース]**」指標カードには、組織内で作成された最新の 5 つのソースが表示されます。このリストは、新しいソースが作成されると更新されます。

ソースを選択してその項目の詳細を表示するか、**[!UICONTROL すべて表示]** を選択してソースのリストを表示します。 ここから、特定のソースを選択して詳細を表示できます。

ソースについて詳しくは、「[ソースの概要](sources/sources-overview.md)」を参照してください。

### 最近のセグメント

「**[!UICONTROL 最近のセグメント]**」指標カードには、組織内で作成された最新の 5 つのセグメントが表示されます。このリストは、新しいセグメントが作成されると更新されます。

セグメントを選択してその項目の詳細を表示するか、**[!UICONTROL すべて表示]** を選択してその他のセグメントに関する情報を表示します。

セグメントについて詳しくは、「[セグメント化サービスの概要](segmentation/segmentation-overview.md)」を参照してください。

### 最近の宛先

「**[!UICONTROL 最近の宛先]**」指標カードには、組織内で作成された最新の 5 つの宛先が表示されます。このリストは、新しい宛先が作成されると更新されます。

宛先を選択してその項目の詳細を表示するか、**[!UICONTROL すべて表示]** を選択してその他の宛先に関する情報を表示します。

宛先について詳しくは、「[宛先の概要](destinations/overview.md)」を参照してください。
