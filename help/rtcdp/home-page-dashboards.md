---
keywords: 指標の概要； rtcdp 指標の概要
title: Real-time Customer Data Platformのホームページとダッシュボード
description: Adobe Experience Platform のダッシュボード、ホームページ、初回ユーザーエクスペリエンス
feature: Dashboards, Get Started
exl-id: ced5b69c-5bb5-4e06-9cb4-938e36e6e5cc
source-git-commit: d052f307d91890f89d6cb3f18525fe395c116f95
workflow-type: tm+mt
source-wordcount: '915'
ht-degree: 10%

---

# [!DNL Real-Time Customer Data Platform] ホームページ

Adobe Real-time Customer Data Platform(Real-Time CDP) ホームページは、Real-Time CDPにログインした後に表示される最初のページです。

Real-Time CDPのホームページには、様々な機能にすばやくアクセスできる「はじめに」ウィジェットと、組織内のデータに関する最新の情報を表示する指標セクションが含まれています。

このドキュメントでは、Real-Time CDPのホームページと指標ダッシュボードの概要を説明します。

![Platform UI のホームページ。](assets/platform-home/home.png)

## はじめにウィジェット

The [!UICONTROL リアルタイム顧客プロファイルの概要] ウィジェットは次の 4 つのセクションに分かれています。

* **データの Platform への取り込み**：このウィジェットはソースカタログに移動します。 ソースカタログを使用してソースを選択し、データをExperience Platformに取り込みます。 選択 **[ソースの設定]** をクリックして、ソースカタログに移動します。 詳しくは、 [ソースの概要](../sources/home.md).
* **モデルのデータ構造**：このウィジェットはスキーマの概要に移動します。 スキーマの概要を使用して、既存のスキーマを参照するか、データの構造を記述するブループリントを作成します。 選択 **[!UICONTROL スキーマを作成]** をクリックして、スキーマ作成インターフェイスに移動します。 詳しくは、 [スキーマの概要](../xdm/home.md).
* **オーディエンスの構築**：このウィジェットは UI のセグメントビルダーに移動します。 セグメントビルダーを使用して、プロファイルデータ要素を操作し、セグメント定義の条件を定義します。 選択 **[!UICONTROL オーディエンスを作成]** をクリックして、セグメントビルダーに移動します。 詳しくは、 [セグメント化サービスの概要](../segmentation/home.md).
* **宛先へのデータ送信**：このウィジェットは宛先カタログに移動します。 宛先カタログを使用して、接続してセグメントを送信できる宛先を選択します。 選択 **[!UICONTROL 宛先の設定]** をクリックして、宛先カタログに移動します。 詳しくは、 [宛先の概要](../destinations/home.md).

![はじめにウィジェットを表示する Platform UI ホームページ](assets/platform-home/getting-started-widget.png)

## 指標ダッシュボード {#metrics-dashboard}

>[!CONTEXTUALHELP]
>id="platform_home_metrics_totalProfiles"
>title="合計プロファイル数"
>abstract="組織が Experience Platform 内に持つプロファイルの合計数。この数は、組織の結合ポリシーに基づいており、プロファイルフラグメントは含まれません。プロファイルの数は 24 時間ごとに更新されます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/profile/ui/user-guide.html?lang=ja#profile-count" text="詳しくは、ドキュメントを参照してください"

指標ダッシュボードには、指標データに関する最新の情報がExperience Platformされます。 ダッシュボードは次の 2 つのセクションに分かれています。

### リーダーボード

リーダーボードには、組織内の現在のスキーマ、データセット、プロファイル、セグメントの合計数と最新の更新日が表示されます。

![Platform UI ホームページのリーダーボードの節。](assets/platform-home/leaderboard.png)

* **合計スキーマ数**: **合計スキーマ数** カウンターは、システム内のスキーマの数を表示します。 このカウンターは、スキーマが作成されると更新されます。 詳しくは、 [スキーマの概要](../xdm/home.md).
* **合計データセット数**: **合計データセット数** カウンターは、システム内のデータセットの数とExperience Platform内のデータの量を示します。 このカウンターは、データセットの作成時に更新されます。 データセットの詳細については、 [データセットの概要](../catalog/datasets/overview.md).
* **合計プロファイル数**: **プロファイル** 「カウント」には、組織がExperience Platform内に持つプロファイルの合計数が表示されます。 プロファイルフラグメントは含まれません。これは、アドレス可能なオーディエンスの合計です。 このカウントでは、 [結合ポリシー](profile/merge-policies.md) リアルタイム顧客プロファイルの結合ポリシー設定で設定されたとおりです。 プロファイルの数は 24 時間に 1 度更新されます。 選択 **[!UICONTROL プロファイル]** をクリックして、プロファイルの概要ページに移動し、すべてのプロファイル指標を表示します。 プロファイルの詳細については、 [リアルタイム顧客プロファイルの概要](../profile/home.md).
* **合計オーディエンス数**: **合計オーディエンス数** カウンターは、組織に対して作成されたオーディエンスの合計数を表示します。 この数は、新しいオーディエンスが作成されると更新されます。 オーディエンスの詳細については、 [セグメント化サービスの概要](../segmentation/home.md).

### 最近の項目

最近使用した項目には、組織内の最新の変更が一覧表示されます。 次の例では、データセット、ソース、セグメント、宛先に関する最新の変更です。

![Platform UI ホームページの最近の項目の節。](assets/platform-home/recent-items.png)

* **最近のデータセット**: **[!UICONTROL 最近のデータセット]** カードには、組織内で作成された最新の 5 つのデータセットが表示されます。 このリストは、新しいデータセットが作成されると更新されます。 データセットを選択してその項目の詳細を表示するか、「 」を選択します。 **[!UICONTROL すべて表示]** データセットのリスト用。 ここから、特定のソースを選択して詳細を表示できます。 データセットの詳細については、「[データセットの概要](../catalog/datasets/overview.md)」を参照してください。
* **最近のソース**: **[!UICONTROL 最近のソース]** 指標カードには、組織内で作成された最新の 5 つのソースが表示されます。 このリストは、新しいソースが作成されると更新されます。 ソースを選択してその項目の詳細を表示するか、「 」を選択します。 **[!UICONTROL すべて表示]** を参照してください。 ここから、特定のソースを選択して詳細を表示できます。 ソースについて詳しくは、「[ソースの概要](../sources/home.md)」を参照してください。
* **最近のセグメント**: **[!UICONTROL 最近のセグメント]** 指標カードには、組織内で作成された最新の 5 つのセグメントが表示されます。 このリストは、新しいセグメントが作成されると更新されます。 セグメントを選択してその項目の詳細を表示するか、「 」を選択します。 **[!UICONTROL すべて表示]** ：セグメントのリスト。 セグメントについて詳しくは、「[セグメント化サービスの概要](../segmentation/home.md)」を参照してください。
* **最近の宛先**: **[!UICONTROL 最近の宛先]** 指標カードには、組織内で作成された最新の 5 つの宛先が表示されます。 このリストは、新しい宛先が作成されると更新されます。 宛先を選択してその項目の詳細を表示するか、「 」を選択します。 **[!UICONTROL すべて表示]** ：宛先のリスト。 詳しくは、 [宛先の概要](../destinations/home.md).

## リソース

最後に、リソースウィジェットには、参照可能な追加のドキュメントリソースが表示されます。 これには、以下が含まれます。

![Platform UI ホームページのリソース節。](assets/platform-home/resources.png)

* [スキーマ](../xdm/schema/composition.md)
* [ソースの接続](../sources/home.md)
* [リアルタイム顧客プロファイルを生成する方法](../profile/home.md)
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
