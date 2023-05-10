---
description: Adobe Experience Platform Debugger での「ソリューション」タブの使用
keywords: デバッガー;experience Platform デバッガー拡張機能;chrome;拡張機能;概要;クリア;リクエスト;ソリューション;情報;analytics;target;audience manager;media manager;amo;id サービス
seo-description: Using the Solution tabs in Adobe Experience Platform Debugger
seo-title: Solution Tabs in Adobe Experience Platform Debugger
title: ソリューションタブ
uuid: 5e999ef2-6399-4ab5-a841-3a839d081728
exl-id: 2cb49f78-4a4b-4410-8a4b-6f9009c51d58
source-git-commit: 5d66fd826da33ec4c60ccfb20ccec40b265edcbb
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 100%

---

# ソリューション

Adobe Experience Platform デバッガーの左側のナビゲーションには、**ソリューション**&#x200B;のリストが表示されています。ソリューションを選択すると、特定の Adobe Experience Cloud テクノロジーの結果を確認できます。

![デバッガー UI に表示される使用可能なソリューションのリスト](../images/solutions/overview/left-nav.png)

## Adobe Experience Platform Web SDK {#aep}

Adobe Experience Platform Web SDK 画面には、Adobe Experience Platform Web SDK に関する情報が表示されます。「**[!UICONTROL 設定]**」を選択すると、コンソールログのオン／オフを切り替えることができます。

## [!UICONTROL Analytics] {#section-f71dfcc22bb44c86bec328491606a482}

「[!UICONTROL Analytics]」タブには、[Adobe Analytics 実装](https://experienceleague.adobe.com/docs/analytics/implementation/home.html?lang=ja)に関する情報が表示されます。

## [!UICONTROL Target] {#section-988873ba5ede4317953193bd7ac5474c}

[!UICONTROL Target] 画面には、[Adobe Target](https://experienceleague.adobe.com/docs/target/using/target-home.html?lang=ja) リクエストまたは [mboxTrace](https://experienceleague.adobe.com/docs/target/using/activities/troubleshoot-activities/content-trouble.html?lang=ja#section_256FCF7C14BB435BA2C68049EF0BA99E) 応答の詳細が表示されます。

詳しくは、[Target 実装でのデバッガーの使用](./target.md)のガイドを参照してください。

## [!UICONTROL Audience Manager] {#section-1d4484f8b46f457f859ba88039a9a585}

「[[!UICONTROL Audience Manager]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/aam-home.html?lang=ja)」タブには、Adobe Audience Manager での[イベント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-event-calls/dcs-event-calls.html?lang=ja)の詳細が表示されます。組織を選択して展開し、情報を表示します。

## [!UICONTROL Adobe Experience Platform タグ] {#section-ee80a9c509f2462c89c1e5bd8d05d7c8}

「[!UICONTROL Adobe Experience Platform タグ]」セクションには、タグリクエストが表示されます。「**[!UICONTROL 設定]**」を選択して[埋め込みコード](../../tags/ui/publishing/environments.md#embed-code)を設定することもできます。Experience Platform Debugger 内で埋め込みコードを編集、置換、または追加できます。ログインしている場合は、ドロップダウンを使用して別のプロパティを選択できます。

## [!UICONTROL Experience Cloud ID] {#section-a96c32f8e63a4991abb296f6e8ea01cf}

「[!UICONTROL Experience Cloud ID]」タブには、[Experience Cloud ID サービス](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja)リクエストが表示されます。

## [!UICONTROL Dynamic Tag Management]

以前に Experience Platform に古いバージョンのタグ（[!DNL Dynamic Tag Management (DTM)] と呼ばれます）を実装している場合は、このタブを使用して埋め込みコードを設定したり、ネットワークリクエストの詳細を確認したりできます。
