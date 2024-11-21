---
title: Experience Platformでの AI アシスタントへのアクセス
description: Experience CloudUI で AI アシスタントにアクセスする方法を説明します。
exl-id: c4cdff25-512c-4b4c-be91-ad9360067a0a
source-git-commit: 8d69d7010442519ce02bb9a504d4228742b4f092
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# Experience Platformでの AI アシスタントへのアクセス

Adobe Experience Cloudの複数のアプリケーションから AI アシスタントにアクセスできます。

>[!IMPORTANT]
>
>AI アシスタントにアクセスするために、追加の法律条項に同意する必要があることを知らせるポップアップ メッセージが権限 UI に表示された場合は、Adobeアカウントチームに連絡して、これらの条項に関するガイダンスを求めてください。

AI アシスタントへのアクセスは、次のパラメータで制御されます。

* **アプリケーションへのアクセス：** AI アシスタントには、Adobe Experience Platform、Adobe Real-Time CDP、Adobe Journey Optimizer、[Customer Journey Analyticsからアクセスできます ](https://experienceleague.adobe.com/en/docs/analytics-platform/using/ai-assistant)。
<!-- * **Contractual access:** Your company must agree to certain [!DNL GenAI]-related legal terms before your organization can use AI Assistant. Contact your organization's administrator or your Adobe Account Team if you are not able to access AI Assistant.  -->
* **権限：** [ 権限 UI](../access-control/abac/ui/permissions.md) を使用して、組織の AI アシスタントへのアクセスを許可または取り消します。 AI アシスタントを使用するには、特定のユーザーが **AI アシスタントを有効にする** および **運用上のインサイトを表示** 権限でプロビジョニングされた役割に属している必要があります。
   * 管理者は、特定の役割に **AI アシスタントを有効にする** を追加し、その役割にユーザーを追加して、組織の AI アシスタントにアクセスできるようにすることができます。
   * 管理者は、特定の役割に **View Operational Insights** を追加し、その役割にユーザーを追加して、AI Assistant の Operational Insights 機能を使用できるようにします。 運用インサイトは現在ベータ版です。

![AI アシスタントを有効にする権限とオペレーショナルインサイトを表示権限が特定の役割に含まれている権限 UI ページ。](./images/permissions.png)

権限 UI を使用して、Experience PlatformおよびJourney Optimizerで AI アシスタントを使用する権限を付与します。 Customer Journey Analyticsで AI アシスタントにアクセスする方法について説明します。 [Customer Journey Analytics](https://experienceleague.adobe.com/en/docs/analytics-platform/using/ai-assistant) のドキュメントを参照してください。

必要な権限を取得したら、使用しているアプリケーションの上部のヘッダーにある「AI アシスタント」アイコンを選択して、AI アシスタントにアクセスできます。

![ 初めてのユーザーエクスペリエンスを備えた AI アシスタント。](./images/ai-assistant.png)

## AI アシスタントにアクセスする

組織とユーザーに AI アシスタントへのアクセスを設定する方法については、次のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/3436470/?learn=on)

## 次の手順

AI アシスタントに完全にアクセスできるようになったら、ワークフローでの機能の使用に進むことができます。詳しくは、[AI アシスタント UI ガイド ](./ui-guide.md) を参照してください。
