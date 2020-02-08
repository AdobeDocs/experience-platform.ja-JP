---
title: リアルタイム顧客データプラットフォームのホームページとダッシュボード
seo-title: リアルタイム顧客データプラットフォームのホームページとダッシュボード
description: Adobe Experience Platformのダッシュボード、ホームページ、初回ユーザーエクスペリエンス
seo-description: Adobe Experience Platformのダッシュボード、ホームページ、初回ユーザーエクスペリエンス
translation-type: tm+mt
source-git-commit: 6dde6c90fe9073c50c0a9d3dd43ef045730d1e13

---


# リアルタイム顧客データプラットフォーム指標の概要

指標ダッシュボードを含むAdobe Real-time Customer Data Platform(Real-time CDP)ホームページは、Real-time CDPにログインすると表示されます。

ホームページは、指標カードが表示される場所の1つに過ぎません。 リアルタイムCDPは、お客様の経験を通じて指標カードを提供します。 これらの指標は、システム内のデータ、プロファイルおよびセグメントオーディエンスに関する情報を提供します。

![image](assets/home2.jpg)

Real-time CDPにログインしたときにシステムにデータがない場合、ホームページのダッシュボードは表示されません。 この場合、ホームページは初めてユーザーが体験できるように学習教材を提供します。 データが収集されると（つまり、データセット、プロファイル、セグメント、宛先が作成され、データがシステムにフローされると）、ダッシュボードは自動的に更新され、そのデータに関する情報が表示されます <!--sources--><!-- in metric cards-->。

## ホームページのダッシュボードビュー

<!--The dashboard shows information in several areas. Each category of information displays for the time range shown beneath the data.-->

ダッシュボードは次のように分かれています<!-- two areas.-->。

* **リーダーボードは** 、ダッシュボードの上部に表示されます。 リーダーボードには、システム内のデータセット、プロファイル、セグメント、および宛先の数が表示されます。

   ![image](assets/home-leaderboard2.jpg)

<!-- * **Metric cards** display beneath the leaderboard. Metric cards show additional information, such as percentages or trends. Metric cards appear as data is collected.
    ![image](assets/home-metrics.jpg)
Some information is shown in different ways on both the leaderboard and metric cards. -->
* **最近の項目には** 、システムに追加された最新の5つのデータセット、ソース、セグメント、および宛先が表示されます。

   ![image](assets/home-recent.jpg)

追加の指標（プロファイルやセグメントなど）は、リアルタイム顧客データプラットフォームの他の部分で使用できます。

### データセット

Datasetsカ **[!UICONTROL ウンターは]** 、システム内のデータセットの数とプラットフォーム内のデータの量を示します。 このカウンターは、データセットの作成時に更新されます。

データセットについて詳しくは、Adobe Experience Platformへの [データの取り込みを参照してください](https://www.adobe.io/apis/experienceplatform/home/tutorials/alltutorials.html#!api-specification/markdown/narrative/tutorials/data_ingestion_tutorial/data_ingestion_tutorial.md)。

### プロファイル

プロフ **[!UICONTROL ァイル数は]** 、リアルタイム顧客プロファイル内のプロファイルを持つ人の合計数を示します。 プロファイルフラグメントは含まれません。 これは、アドレス可能なオーディエンスの合計です。

このカウントでは、統合プロファイ [ルの結合ポリシー](profile/merge-policies.md) 構成で設定された既定の結合ポリシーを使用します。

プロファイルの数は24時間に1回更新されます。

プロファイルの詳細については、「 [A unified view of your customer in Real-time CDP](profile/profile-overview.md)」を参照してください。

### セグメント

**[!UICONTROL セグメント]** ：組織に対して作成されたセグメントの合計数が表示されます。 この番号は、新しいセグメントが作成されると更新されます。

セグメントについて詳しくは、 [Segmentation Serviceの概要を参照してください](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!end-user/markdown/segmentation_overview/segmentation.md)。

### 宛先

**[!UICONTROL 宛先は]** 、組織に対して作成された宛先の合計数を表示します。 この番号は、新しい宛先が作成されると更新されます。

宛先について詳しくは、宛先の概要を参 [照してください](destinations/destinations-overview.md)。

<!-- ### Successful profile records

In the leaderboard **[!UICONTROL Successful profile records]** shows the total number of records that have been successfully processed into the profile.

There is also a metric card that shows the percentage of successful records. Click **[!UICONTROL View datasets]** to see more details about the profile records. Hover over the colored area of the graph to see additional details:

![image](assets/home-profilerecords-details.PNG)

The number of successful profile records is updated hourly. 

For more information about profiles, see [A unified view of your customer in Real-time CDP](profile/profile-overview.md).

### Total profile records

The **[!UICONTROL Total profile records]** metric card shows the total number of data records enabled to feed into the profiles, and the percentage that are successful, updated once per day. This does not include all data in the data lake, because some data might not be enabled to feed into the profiles.

 Hover over the colored area of the graph to see additional details about the successful profiles:

![image](assets/home-profile-details.PNG)

Click **[!UICONTROL View profiles]** to see more details about the profile records.

For more information about profiles, see [A unified view of your customer in Real-time CDP](profile/profile-overview.md).

For more information about viewing a specific profile, see [Profile viewer](profile/profile-viewer.md).

### Failed profile records

In the leaderboard, **[!UICONTROL Failed profile records]** counts the number of records that failed to process into the profile.

The **[!UICONTROL Failed profile records]** metric card shows this count, and includes a graphical representation that helps you see how failures have trended during the time shown below the graphic. This chart is updated hourly. Click **[!UICONTROL View datasets]** to see more details about the profile records.

The number of failed profile records is updated hourly. -->

### 最近のデータセット

最近のデ **[!UICONTROL ータセット]** ・カードには、組織内で作成された最新の5つのデータセットが表示されます。 このリストは、新しいデータセットが作成されると更新されます。

データセットをクリックしてその項目の詳細を表示するか、「すべて表示」をク **[!UICONTROL リックして]** 「データセット」のリストを表示します。 ここから、特定のソースをクリックして詳細を表示できます。

データセットについて詳しくは、Adobe Experience Platformへの [データの取り込みを参照してください](https://www.adobe.io/apis/experienceplatform/home/tutorials/alltutorials.html#!api-specification/markdown/narrative/tutorials/data_ingestion_tutorial/data_ingestion_tutorial.md)。

### 最近のソース

「最近の **[!UICONTROL ソース]** 」指標カードには、組織内で作成された最新の5つのソースが表示されます。 このリストは、新しいソースが作成されると更新されます。

ソースをクリックしてその項目の詳細を表示するか、「すべて表示」 **[!UICONTROL をクリックして]** 「ソース」のリストを表示します。 ここから、特定のソースをクリックして詳細を表示できます。

ソースについて詳しくは、「ソースの概要」を参 [照してください](sources/sources-overview.md)。

### 最近のセグメント

「最近の **[!UICONTROL セグメント]** 」指標カードには、組織内で作成された最新の5つのセグメントが表示されます。 このリストは、新しいセグメントが作成されると更新されます。

セグメントをクリックしてその項目の詳細を表示するか、「すべて表示」をク **[!UICONTROL リックして]** 、その他のセグメントに関する情報を表示します。

セグメントについて詳しくは、 [Segmentation Serviceの概要を参照してください](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!end-user/markdown/segmentation_overview/segmentation.md)。

### 最近の宛先

「最近の **[!UICONTROL 宛先]** 」指標カードには、組織内で作成された最新の宛先が5つ表示されます。 このリストは、新しい宛先が作成されると更新されます。

宛先をクリックしてそのアイテムの詳細を表示するか、「すべて表示」を **[!UICONTROL クリックして]** 、その他の宛先に関する情報を表示します。

宛先について詳しくは、宛先の概要を参 [照してください](destinations/destinations-overview.md)。
