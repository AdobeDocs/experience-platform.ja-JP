---
title: リアルタイム顧客データプラットフォームのホームページとダッシュボード
seo-title: リアルタイム顧客データプラットフォームのホームページとダッシュボード
description: Adobe Experience Platform のダッシュボード、ホームページ、初回ユーザーエクスペリエンス
seo-description: Adobe Experience Platform のダッシュボード、ホームページ、初回ユーザーエクスペリエンス
translation-type: tm+mt
source-git-commit: 6c8d0757d7e7568b1976823d9c52221374e67cbb

---


# リアルタイム顧客データプラットフォーム指標の概要

Real-time CDP にログインすると、指標ダッシュボードを含むアドビリアルタイム顧客データプラットフォーム（Real-time CDP）ホームページが表示されます。

ホームページは、指標カードが表示される場所の 1 つに過ぎません。Real-time CDP は、ユーザーの経験を通じて指標カードを提供します。これらの指標は、システム内のデータ、プロファイル、セグメントオーディエンスに関する情報を提供します。

![画像](assets/home2.jpg)

Real-time CDP にログインしたときにシステムにデータがない場合、ホームページのダッシュボードは表示されません。この場合、ホームページでは初めて使用するユーザーのための学習教材を提供します。データが収集されると（つまり、<!--sources-->データセット、プロファイル、セグメント、宛先が作成され、データがシステムにフローされると）、ダッシュボードは自動的に更新され、そのデータに関する情報が表示されます <!-- in metric cards-->。

## ホームページのダッシュボードビュー

<!--The dashboard shows information in several areas. Each category of information displays for the time range shown beneath the data.-->

ダッシュボードは次のように分かれています<!-- two areas.-->。

* **リーダーボード**&#x200B;は、ダッシュボードの上部に表示されます。リーダーボードには、システム内のデータセット、プロファイル、セグメント、宛先の数が表示されます。

   ![画像](assets/home-leaderboard2.jpg)

<!-- * **Metric cards** display beneath the leaderboard. Metric cards show additional information, such as percentages or trends. Metric cards appear as data is collected.
    ![image](assets/home-metrics.jpg)
Some information is shown in different ways on both the leaderboard and metric cards. -->
* **最近の項目**&#x200B;には、システムに追加された最新の 5 つのデータセット、ソース、セグメント、宛先が表示されます。

   ![画像](assets/home-recent.jpg)

追加の指標（プロファイルやセグメントなど）は、リアルタイム顧客データプラットフォームの他の部分で使用できます。

### データセット

The **[!UICONTROL Datasets]** counter shows the number of datasets in the system and the amount of data in Platform. このカウンターは、データセットの作成時に更新されます。

データセットについて詳しくは、[Adobe Experience Platform へのデータの取り込み](https://www.adobe.io/apis/experienceplatform/home/tutorials/alltutorials.html#!api-specification/markdown/narrative/tutorials/data_ingestion_tutorial/data_ingestion_tutorial.md)を参照してください。

### プロファイル

The **[!UICONTROL Profiles]** count shows the total number of people with profiles in the Real-time Customer Profile. プロファイルフラグメントは含まれません。これは、アドレス可能なオーディエンスの合計です。

このカウントでは、「統合プロファイル」の[結合ポリシー](profile/merge-policies.md)構成で設定された既定の結合ポリシーを使用します。

プロファイルの数は 24 時間ごとに更新されます。

プロファイルの詳細については、「[Real-time CDP での顧客の統合ビュー](profile/profile-overview.md)」を参照してください。

### セグメント

**[!UICONTROL Segments]** 組織に対して作成されたセグメントの合計数が表示されます。 この数は、新しいセグメントが作成されると更新されます。

セグメントについて詳しくは、「[セグメント化サービスの概要](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/segmentation/segmentation-overview.md)」を参照してください。

### 宛先

**[!UICONTROL Destinations]** 組織に対して作成された宛先の合計数が表示されます。 この数は、新しい宛先が作成されると更新されます。

宛先について詳しくは、「[宛先の概要](destinations/destinations-overview.md)」を参照してください。

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

The **[!UICONTROL Recent datasets]** card shows the five most recent datasets created within the organization. このリストは、新しいデータセットが作成されると更新されます。

Click a dataset to view the details for that item, or **[!UICONTROL View all]** to see the list of datasets. ここからは、特定のソースをクリックして詳細を表示できます。

データセットについて詳しくは、[Adobe Experience Platform へのデータの取り込み](https://www.adobe.io/apis/experienceplatform/home/tutorials/alltutorials.html#!api-specification/markdown/narrative/tutorials/data_ingestion_tutorial/data_ingestion_tutorial.md)を参照してください。

### 最近のソース

The **[!UICONTROL Recent sources]** metric card shows the five most recent sources created within the organization. このリストは、新しいソースが作成されると更新されます。

Click a source to view the details for that item, or **[!UICONTROL View all]** to see the list of sources. ここからは、特定のソースをクリックして詳細を表示できます。

ソースについて詳しくは、「[ソースの概要](sources/sources-overview.md)」を参照してください。

### 最近のセグメント

The **[!UICONTROL Recent segments]** metric card shows the five most recent segments created within the organization. このリストは、新しいセグメントが作成されると更新されます。

Click a segment to view the details for that item, or **[!UICONTROL View all]** to see information about more segments.

セグメントについて詳しくは、「[セグメント化サービスの概要](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/segmentation/segmentation-overview.md)」を参照してください。

### 最近の宛先

The **[!UICONTROL Recent destinations]** metric card shows the five most recent destinations created within the organization. このリストは、新しい宛先が作成されると更新されます。

Click a destination to view the details for that item, or **[!UICONTROL View all]** to see information about more destinations.

宛先について詳しくは、「[宛先の概要](destinations/destinations-overview.md)」を参照してください。
