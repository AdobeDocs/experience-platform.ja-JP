---
keywords: 指標の概要；rtcdp 指標の概要
title: Real-time Customer Data Platformのホームページとダッシュボード
description: Adobe Real-Time CDP の様々なダッシュボード、ホームページおよび初回ユーザーエクスペリエンスについて説明します。
feature: Dashboards, Get Started
exl-id: ced5b69c-5bb5-4e06-9cb4-938e36e6e5cc
source-git-commit: 2704184446f7945c744e7e2d2a8c3cda3fc12527
workflow-type: tm+mt
source-wordcount: '919'
ht-degree: 9%

---

# [!DNL Real-Time Customer Data Platform] ホームページ

Adobe Real-time Customer Data Platform（Real-Time CDP）のホームページは、Real-Time CDPにログインした後に表示される最初のページです。

Real-Time CDP ホームページには、いくつかの異なる機能にすばやくアクセスできる「はじめに」ウィジェットと、組織内のデータに関する最新の情報を表示する「指標」セクションが含まれています。

このドキュメントでは、Real-Time CDP ホームページと指標ダッシュボードの概要を説明します。

![Platform UI ホームページ。](assets/platform-home/home.png)

## はじめにウィジェット

この [!UICONTROL リアルタイム顧客プロファイルの概要] ウィジェットは次の 4 つのセクションに分かれています。

* **Platform へのデータの取り込み**：このウィジェットを使用すると、ソースカタログに移動できます。 ソースカタログを使用してソースを選択し、データをExperience Platformに取り込みます。 を選択 **[ソースを設定]** をクリックして、ソースカタログに移動します。 詳しくは、 [ソースの概要](../sources/home.md).
* **モデルデータ構造**：このウィジェットは、スキーマの概要に移動します。 スキーマの概要を使用すると、既存のスキーマを参照したり、データの構造を説明するブループリントを作成したりできます。 を選択 **[!UICONTROL スキーマを作成]** に移動して、スキーマ作成インターフェイスにアクセスします。 詳しくは、 [スキーマの概要](../xdm/home.md).
* **オーディエンスの構築**：このウィジェットは、UI のセグメントビルダーに移動します。 セグメントビルダーを使用してプロファイルデータ要素を操作し、セグメント定義の条件を定義します。 を選択 **[!UICONTROL オーディエンスを作成]** をクリックして、セグメントビルダーに移動します。 詳しくは、 [セグメント化サービスの概要](../segmentation/home.md).
* **宛先へのデータの送信**：このウィジェットは、宛先カタログを表示します。 宛先カタログを使用して、オーディエンスに接続して送信できる宛先を選択します。 を選択 **[!UICONTROL 宛先の設定]** 宛先カタログに移動します。 詳しくは、 [宛先の概要](../destinations/home.md).

![「はじめに」ウィジェットを表示する Platform UI ホームページ](assets/platform-home/getting-started-widget.png)

## 指標ダッシュボード {#metrics-dashboard}

>[!CONTEXTUALHELP]
>id="platform_home_metrics_totalProfiles"
>title="合計プロファイル数"
>abstract="組織が Experience Platform 内に持つプロファイルの合計数。この数は、組織の結合ポリシーに基づいており、プロファイルフラグメントは含まれません。プロファイルの数は 24 時間ごとに更新されます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/ui/user-guide.html?lang=ja#profile-count" text="詳しくは、ドキュメントを参照してください"

指標ダッシュボードには、Experience Platformデータに関する最新の情報が表示されます。 ダッシュボードは 2 つのセクションに分かれています。

### リーダーボード

リーダーボードには、組織内のスキーマ、データセット、プロファイルおよびオーディエンスの現在の合計数のほか、最新の更新日が表示されます。

![Platform UI ホームページのリーダーボードセクション。](assets/platform-home/leaderboard.png)

* **合計スキーマ数**：です **合計スキーマ数** カウンター：システム内のスキーマの数を表示します。 このカウンターは、スキーマが作成されると更新されます。 詳しくは、 [スキーマの概要](../xdm/home.md).
* **合計データセット数**：です **合計データセット数** カウンターは、システム内のデータセット数と、Experience Platform内のデータ量を示します。 このカウンターは、データセットが作成されると更新されます。 データセットについて詳しくは、 [データセットの概要](../catalog/datasets/overview.md).
* **合計プロファイル数**：です **プロファイル** カウントは、Experience Platform内の組織のプロファイルの合計数を表示します。 プロファイルフラグメントは含まれません。これは、アドレス可能な合計オーディエンスです。 このカウントはデフォルトを使用します [結合ポリシー](profile/merge-policies.md) リアルタイム顧客プロファイルの結合ポリシー設定で設定されるとおりに使用します。 プロファイルの数は 24 時間ごとに 1 回更新されます。 を選択 **[!UICONTROL プロファイル]** プロファイルの概要ページに移動し、すべてのプロファイル指標を表示します。 プロファイルについて詳しくは、を参照してください。 [リアルタイム顧客プロファイルの概要](../profile/home.md).
* **合計オーディエンス数**：です **合計オーディエンス数** カウンターは、組織に対して作成されたオーディエンスの合計数を表示します。 この番号は、新しいオーディエンスが作成されると更新されます。 オーディエンスについて詳しくは、 [セグメント化サービスの概要](../segmentation/home.md).

### 最近のアイテム

最近の項目には、組織の最新の変更が一覧表示されます。 以下の例では、最新の変更がデータセット、ソース、オーディエンスおよび宛先に関係しています。

![Platform UI ホームページの「最近の項目」セクション。](assets/platform-home/recent-items.png)

* **最近のデータセット**：です **[!UICONTROL 最近のデータセット]** カードには、組織内で作成された最新の 5 つのデータセットが表示されます。 このリストは、新しいデータセットが作成されると更新されます。 データセットを選択してその項目の詳細を表示するか、を選択します **[!UICONTROL すべて表示]** データセットのリスト用。 ここから、特定のソースを選択して詳細を確認できます。 データセットの詳細については、「[データセットの概要](../catalog/datasets/overview.md)」を参照してください。
* **最近のソース**：です **[!UICONTROL 最近のソース]** 指標カードには、組織内で作成された最新の 5 つのソースが表示されます。 このリストは、新しいソースが作成されると更新されます。 ソースを選択してその項目の詳細を表示するか、を選択します **[!UICONTROL すべて表示]** ソースのリスト用。 ここから、特定のソースを選択して詳細を確認できます。 ソースについて詳しくは、「[ソースの概要](../sources/home.md)」を参照してください。
* **最近のオーディエンス**：です **[!UICONTROL 最近のオーディエンス]** 指標カードには、組織内で作成された最新の 5 つのオーディエンスが表示されます。 このリストは、新しいオーディエンスが作成されると更新されます。 オーディエンスを選択してその項目の詳細を表示するか、を選択します **[!UICONTROL すべて表示]** オーディエンスのリスト用。 オーディエンスについて詳しくは、を参照してください。 [セグメント化サービスの概要](../segmentation/home.md).
* **最近の宛先**：です **[!UICONTROL 最近の宛先]** 指標カードには、組織内で作成された最新の 5 つの宛先が表示されます。 このリストは、新しい宛先が作成されると更新されます。 宛先を選択してその項目の詳細を表示するか、を選択します **[!UICONTROL すべて表示]** 宛先のリスト 詳しくは、 [宛先の概要](../destinations/home.md).

## リソース

最後に、リソース ウィジェットには、参照可能な追加のドキュメントリソースが用意されています。 これには、以下が含まれます。

![Platform UI ホームページの「リソース」セクション。](assets/platform-home/resources.png)

* [スキーマ](../xdm/schema/composition.md)
* [ソースを接続](../sources/home.md)
* [リアルタイム顧客プロファイルの入力方法](../profile/home.md)
* [宛先の接続](../destinations/home.md)
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
