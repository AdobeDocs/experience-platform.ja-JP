---
keywords: 指標の概要；rtcdp 指標の概要
title: Real-Time Customer Data Platformのホームページとダッシュボード
description: Adobe Real-Time CDP の様々なダッシュボード、ホームページおよび初回ユーザーエクスペリエンスについて説明します。
feature: Dashboards, Get Started
exl-id: ced5b69c-5bb5-4e06-9cb4-938e36e6e5cc
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '925'
ht-degree: 12%

---

# [!DNL Real-Time Customer Data Platform] ホームページ

Adobe Real-Time Customer Data Platform（Real-Time CDP）のホームページは、Real-Time CDPにログインした後に表示される最初のページです。

Real-Time CDP ホームページには、いくつかの異なる機能にすばやくアクセスできる「はじめに」ウィジェットと、組織内のデータに関する最新の情報を表示する「指標」セクションが含まれています。

このドキュメントでは、Real-Time CDP ホームページと指標ダッシュボードの概要を説明します。

![Experience Platform UI ホームページ &#x200B;](assets/platform-home/home.png)

## はじめにウィジェット

[!UICONTROL &#x200B; リアルタイム顧客プロファイルの概要 &#x200B;] ウィジェットは次の 4 つのセクションに分かれています。

* **Experience Platformへのデータの取り込み**：このウィジェットは、ソースカタログに移動します。 ソースカタログを使用してソースを選択し、データをExperience Platformに取り込みます。 **[ソースを設定]** を選択して、ソースカタログに移動します。 詳しくは、[ソースの概要](../sources/home.md)を参照してください。
* **モデルデータ構造**：このウィジェットは、スキーマの概要に移動します。 スキーマの概要を使用すると、既存のスキーマを参照したり、データの構造を説明するブループリントを作成したりできます。 「**[!UICONTROL スキーマを作成]**」を選択して、スキーマ作成インターフェイスに移動します。 詳しくは、[&#x200B; スキーマの概要 &#x200B;](../xdm/home.md) を参照してください。
* **オーディエンスの作成**：このウィジェットは、UI のセグメントビルダーに移動します。 セグメントビルダーを使用してプロファイルデータ要素を操作し、セグメント定義の条件を定義します。 **[!UICONTROL オーディエンスを作成]** を選択して、セグメントビルダーに移動します。 詳しくは、[&#x200B; セグメント化サービスの概要 &#x200B;](../segmentation/home.md) を参照してください。
* **宛先へのデータ送信**：このウィジェットは、宛先カタログに移動します。 宛先カタログを使用して、オーディエンスに接続して送信できる宛先を選択します。 **[!UICONTROL 宛先を設定]** を選択して、宛先カタログに移動します。 詳しくは、[宛先の概要](../destinations/home.md)を参照してください。

![&#x200B; 「はじめに」ウィジェットを表示するExperience Platform UI ホームページ &#x200B;](assets/platform-home/getting-started-widget.png)

## 指標ダッシュボード {#metrics-dashboard}

>[!CONTEXTUALHELP]
>id="platform_home_metrics_totalProfiles"
>title="合計プロファイル数"
>abstract="組織が Experience Platform 内に持つプロファイルの合計数。この数は、組織の結合ポリシーに基づいており、プロファイルフラグメントは含まれません。プロファイルの数は 24 時間ごとに更新されます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/ui/user-guide.html?lang=ja#profile-count" text="詳しくは、ドキュメントを参照してください"

指標ダッシュボードには、Experience Platform データに関する最新の情報が表示されます。 ダッシュボードは 2 つのセクションに分かれています。

### リーダーボード

リーダーボードには、組織内のスキーマ、データセット、プロファイルおよびオーディエンスの現在の合計数のほか、最新の更新日が表示されます。

![Experience Platform UI ホームページのリーダーボードセクション。](assets/platform-home/leaderboard.png)

* **合計スキーマ数**: **合計スキーマ数** カウンターには、システム内のスキーマ数が表示されます。 このカウンターは、スキーマが作成されると更新されます。 詳しくは、[&#x200B; スキーマの概要 &#x200B;](../xdm/home.md) を参照してください。
* **合計データセット数**:**合計データセット数** カウンターは、システム内のデータセット数とExperience Platform内のデータ量を示します。 このカウンターは、データセットが作成されると更新されます。 データセットについて詳しくは、[&#x200B; データセットの概要 &#x200B;](../catalog/datasets/overview.md) を参照してください。
* **合計プロファイル数**:**プロファイル数** は、Experience Platform内の組織のプロファイルの合計数を示します。 プロファイルフラグメントは含まれません。これは、アドレス可能な合計オーディエンスです。 このカウントでは、リアルタイム顧客プロファイルの結合ポリシー設定で設定されたデフォルトの [&#x200B; 結合ポリシー &#x200B;](profile/merge-policies.md) を使用します。 プロファイルの数は 24 時間ごとに 1 回更新されます。 **[!UICONTROL プロファイル]** を選択して、プロファイルの概要ページに移動し、すべてのプロファイル指標を表示します。 プロファイルについて詳しくは、[&#x200B; リアルタイム顧客プロファイルの概要 &#x200B;](../profile/home.md) を参照してください。
* **合計オーディエンス数**: **合計オーディエンス数** カウンターは、組織で作成されたオーディエンスの合計数を示します。 この番号は、新しいオーディエンスが作成されると更新されます。 オーディエンスについて詳しくは、[&#x200B; セグメント化サービスの概要 &#x200B;](../segmentation/home.md) を参照してください。

### 最近のアイテム

最近の項目には、組織の最新の変更が一覧表示されます。 以下の例では、最新の変更がデータセット、ソース、オーディエンスおよび宛先に関係しています。

![Experience Platform UI ホームページの「最近の項目」セクション &#x200B;](assets/platform-home/recent-items.png)

* **最新のデータセット**:**[!UICONTROL 最新のデータセット]** カードには、組織内で作成された最新の 5 つのデータセットが表示されます。 このリストは、新しいデータセットが作成されると更新されます。 データセットを選択してその項目の詳細を表示するか、データセットのリストで **[!UICONTROL すべて表示]** を選択します。 ここから、特定のソースを選択して詳細を確認できます。 データセットの詳細については、「[データセットの概要](../catalog/datasets/overview.md)」を参照してください。
* **最新のソース**: **[!UICONTROL 最新のソース]** 指標カードには、組織内で作成された最新の 5 つのソースが表示されます。 このリストは、新しいソースが作成されると更新されます。 ソースを選択してその項目の詳細を表示するか、ソースのリストの **[!UICONTROL すべて表示]** を選択します。 ここから、特定のソースを選択して詳細を確認できます。 ソースについて詳しくは、「[ソースの概要](../sources/home.md)」を参照してください。
* **最近のオーディエンス**: **[!UICONTROL 最近のオーディエンス]** 指標カードには、組織内で作成された最新の 5 つのオーディエンスが表示されます。 このリストは、新しいオーディエンスが作成されると更新されます。 オーディエンスを選択してその項目の詳細を表示するか、オーディエンスのリストの **[!UICONTROL すべて表示]** を選択します。 オーディエンスについて詳しくは、[&#x200B; セグメント化サービスの概要 &#x200B;](../segmentation/home.md) を参照してください。
* **最近の宛先**: **[!UICONTROL 最近の宛先]** 指標カードには、組織内で作成された最新の 5 つの宛先が表示されます。 このリストは、新しい宛先が作成されると更新されます。 宛先を選択してその項目の詳細を表示するか、宛先のリストで **[!UICONTROL すべて表示]** を選択します。 詳しくは、[宛先の概要](../destinations/home.md)を参照してください。

## リソース

最後に、リソース ウィジェットには、参照可能な追加のドキュメントリソースが用意されています。 これには、以下が含まれます。

![Experience Platform UI ホームページの「リソース」セクション &#x200B;](assets/platform-home/resources.png)

* [スキーマ](../xdm/schema/composition.md)
* [ソースを接続](../sources/home.md)
* [リアルタイム顧客プロファイルの入力方法](../profile/home.md)
* [宛先への接続](../destinations/home.md)
* [アクセスの管理](../access-control/abac/overview.md)

<!-- ### Successful profile records

In the leaderboard **[!UICONTROL Successful profile records]** shows the total number of records that have been successfully processed into the profile.

There is also a metric card that shows the percentage of successful records. Select **[!UICONTROL View datasets]** to see more details about the profile records. Hover over the colored area of the graph to see additional details:

![image](assets/home-profilerecords-details.PNG)

The number of successful profile records is updated hourly. 

For more information about profiles, see [A unified view of your customer in Real-Time CDP](profile/profile-overview.md).

### Total profile records

The **[!UICONTROL Total profile records]** metric card shows the total number of data records enabled to feed into the profiles, and the percentage that are successful, updated once per day. This does not include all data in the data lake, because some data might not be enabled to feed into the profiles.

 Hover over the colored area of the graph to see additional details about the successful profiles:

![image](assets/home-profile-details.PNG)

Select **[!UICONTROL View profiles]** to see more details about the profile records.

For more information about profiles, see [A unified view of your customer in Real-Time CDP](profile/profile-overview.md).

For more information about viewing a specific profile, see [Profile viewer](profile/profile-viewer.md).

### Failed profile records

In the leaderboard, **[!UICONTROL Failed profile records]** counts the number of records that failed to process into the profile.

The **[!UICONTROL Failed profile records]** metric card shows this count, and includes a graphical representation that helps you see how failures have trended during the time shown below the graphic. This chart is updated hourly. Select **[!UICONTROL View datasets]** to see more details about the profile records.

The number of failed profile records is updated hourly. -->
