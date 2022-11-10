---
description: Adobe Experience Platform Debugger での「ソリューション」タブの使用
keywords: debugger;experience Platform Debugger 拡張機能；chrome；拡張機能；概要；クリア；リクエスト；ソリューション；ソリューション；情報；analytics;target;audience manager;media manager;amo;id サービス
seo-description: Using the Solution tabs in Adobe Experience Platform Debugger
seo-title: Solution Tabs in Adobe Experience Platform Debugger
title: ソリューションタブ
uuid: 5e999ef2-6399-4ab5-a841-3a839d081728
exl-id: 2cb49f78-4a4b-4410-8a4b-6f9009c51d58
source-git-commit: 5d66fd826da33ec4c60ccfb20ccec40b265edcbb
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 29%

---

# ソリューション

Adobe Experience Platform Debugger は、 **ソリューション** をクリックします。 特定のAdobe Experience Cloudテクノロジーの結果を確認するソリューションを選択します。

![Debugger UI に表示される使用可能なソリューションのリスト](../images/solutions/overview/left-nav.png)

## Adobe Experience Platform Web SDK {#aep}

Adobe Experience Platform Web SDK 画面には、Adobe Experience Platform Web SDK に関する情報が表示されます。選択 **[!UICONTROL 設定]** コンソールログのオン/オフを切り替えます。

## [!UICONTROL Analytics] {#section-f71dfcc22bb44c86bec328491606a482}

この [!UICONTROL Analytics] 「 」タブに、 [Adobe Analytics実装](https://experienceleague.adobe.com/docs/analytics/implementation/home.html?lang=ja).

## [!UICONTROL ターゲット] {#section-988873ba5ede4317953193bd7ac5474c}

以下を使用： [!UICONTROL ターゲット] 表示する画面 [Adobe Target](https://docs.adobe.com/content/help/ja-JP/experience-cloud/user-guides/home.translate.html) リクエストまたは [mboxTrace](https://experienceleague.adobe.com/docs/target/using/activities/troubleshoot-activities/content-trouble.html#section_256FCF7C14BB435BA2C68049EF0BA99E) 応答の詳細。

詳しくは、 [Target 実装でのデバッガーの使用](./target.md) を参照してください。

## [!UICONTROL Audience Manager] {#section-1d4484f8b46f457f859ba88039a9a585}

以下を使用： [[!UICONTROL Audience Manager]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/aam-home.html?lang=ja) タブに詳細を表示 [イベント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-event-calls/dcs-event-calls.html) Adobe Audience Manager 展開する組織を選択し、情報を表示します。

## [!UICONTROL Adobe Experience Platformタグ] {#section-ee80a9c509f2462c89c1e5bd8d05d7c8}

以下を使用： [!UICONTROL Adobe Experience Platformタグ] セクションで、タグリクエストを表示できます。 また、 **[!UICONTROL 設定]** 設定する [埋め込みコード](../../tags/ui/publishing/environments.md#embed-code). Experience Platform Debugger 内で埋め込みコードを編集、置換、または追加できます。ログインしている場合は、ドロップダウンを使用して別のプロパティを選択できます。

## [!UICONTROL Experience Cloud ID] {#section-a96c32f8e63a4991abb296f6e8ea01cf}

以下を使用： [!UICONTROL Experience CloudID] タブで表示 [Experience CloudID サービス](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja) リクエスト。

## [!UICONTROL Dynamic Tag Management]

以前にExperience Platformに古いバージョンのタグを実装している場合 ( [!DNL Dynamic Tag Management (DTM)]) を参照する場合は、このタブを使用して埋め込みコードを設定し、ネットワークリクエストの詳細を表示できます。
