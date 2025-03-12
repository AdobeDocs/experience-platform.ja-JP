---
title: Experience Platformの AI アシスタントへのアクセス
description: Experience Cloud UI で AI アシスタントにアクセスする方法を説明します。
exl-id: c4cdff25-512c-4b4c-be91-ad9360067a0a
source-git-commit: 9010f430f432c5ead708c1cb01803abd7ffd86b3
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 0%

---

# Experience Platformの AI アシスタントへのアクセス

Adobe Experience Cloudの複数のアプリケーションから AI アシスタントにアクセスできます。

>[!IMPORTANT]
>
>AI アシスタントへのアクセス権を取得するには、最初に追加の法的条項に同意する必要があることを示すポップアップメッセージが権限 UI で表示された場合は、Adobe アカウントチームに連絡して、これらの条項に関するガイダンスを求めます。

## 基本を学ぶ {#get-started}

AI アシスタントにアクセスするには、2 つの前提条件の手順を完了する必要があります。

1. 組織はまず法的条件に同意する必要があります。 詳しくは、Adobe アカウントチームにお問い合わせください。
2. 管理者から、AI アシスタントにアクセスするための十分な権限を付与してもらう必要があります。

これら 2 つの前提条件の手順がいずれも完了していない場合は、Experience Platform UI で AI アシスタントのチャットアイコンを選択すると、次のメッセージが表示されます。

>[!BEGINTABS]

>[!TAB  組織が AI アシスタントを使用できない ]

法的に AI アシスタントを使用する資格がない組織を使用している場合は、次のメッセージが表示されます。 このシナリオでは、Adobe アカウントチームに連絡してアクセス権限を解決する必要があります。

![ 組織が AI アシスタントを使用できない場合にExperience Platform UI に表示されるポップアップメッセージ ](./images/access/modal-one.png)

>[!TAB  適切な権限がありません ]

AI アシスタントを使用する法的資格を持つ組織で、引き続きこの機能にアクセスできない場合は、Experience Platform UI に次のメッセージが表示されます。 このシナリオは、機能にアクセスするための十分な権限を持っていないことを意味します。管理者に連絡して権限の問題を解決する必要があります。

![AI アシスタントに必要な権限がない場合にExperience Platform UI に表示されるポップアップメッセージ ](./images/access/modal-two.png)

>[!ENDTABS]

## AI アシスタントにアクセスする

AI アシスタントへのアクセスは、次のパラメータで制御されます。

* **アプリケーションへのアクセス：** AI アシスタントには、Adobe Experience Platform、Adobe Real-Time CDP、Adobe Journey Optimizer、[Customer Journey Analyticsでアクセスできます ](https://experienceleague.adobe.com/en/docs/analytics-platform/using/ai-assistant)。
<!-- * **Contractual access:** Your company must agree to certain [!DNL GenAI]-related legal terms before your organization can use AI Assistant. Contact your organization's administrator or your Adobe Account Team if you are not able to access AI Assistant.  -->
* **権限：** [ 権限 UI](../access-control/abac/ui/permissions.md) を使用して、組織の AI アシスタントへのアクセスを許可または取り消します。 AI アシスタントを使用するには、特定のユーザーが **AI アシスタントを有効にする** および **運用上のインサイトを表示** 権限でプロビジョニングされた役割に属している必要があります。
   * 管理者は、特定の役割に **AI アシスタントを有効にする** を追加し、その役割にユーザーを追加して、組織の AI アシスタントにアクセスできるようにすることができます。 **注意**：この権限は、当該ユーザーが AI アシスタントにアクセスできるようにするもので、他のユーザーに AI アシスタントへのアクセスを許可するための管理権限を付与するものではありません。
   * 管理者は、特定の役割に **View Operational Insights** を追加し、その役割にユーザーを追加して、AI Assistant の Operational Insights 機能を使用できるようにします。 運用インサイトは現在ベータ版です。

[ 権限 UI](../access-control/abac/ui/roles.md) を使用して、Experience PlatformおよびJourney Optimizerで AI アシスタントを使用するための権限を付与します。 Customer Journey Analyticsで AI アシスタントにアクセスする方法について説明します。 [Customer Journey Analytics](https://experienceleague.adobe.com/en/docs/analytics-platform/using/ai-assistant) のドキュメントを参照してください。

![AI アシスタントを有効にする権限とオペレーショナルインサイトを表示権限が特定の役割に含まれている権限 UI ページ。](./images/access/access-permissions.png)

必要な権限を取得したら、使用しているアプリケーションの上部のヘッダーにある「AI アシスタント」アイコンを選択して、AI アシスタントにアクセスできます。

![ 初めてのユーザーエクスペリエンスを備えた AI アシスタント。](./images/access/access-home.png)

組織とユーザーに AI アシスタントへのアクセスを設定する方法については、次のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/3436470/?learn=on)

## 次の手順

AI アシスタントに完全にアクセスできるようになったら、ワークフローでの機能の使用に進むことができます。詳しくは、[AI アシスタント UI ガイド ](./ui-guide.md) を参照してください。
