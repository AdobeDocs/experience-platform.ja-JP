---
title: Experience Platformでの AI アシスタントへのアクセス
description: Experience CloudUI で AI アシスタントにアクセスする方法を説明します。
source-git-commit: b51291e6c3663c6d6e6d416f0d2c37563c852155
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---

# Experience Platformでの AI アシスタントへのアクセス

Adobe Experience Cloudの複数のアプリケーションから AI アシスタントにアクセスできます。

>[!IMPORTANT]
>
>AI アシスタントにアクセスするために、追加の法律条項に同意する必要があることを知らせるポップアップ メッセージが権限 UI に表示された場合は、Adobeアカウントチームに連絡して、これらの条項に関するガイダンスを求めてください。

AI アシスタントへのアクセスは、次のパラメータで制御されます。

* **アプリケーションにアクセスします。** AI アシスタントには、Adobe Experience Platform、Adobe Real-Time CDP、Adobe Journey Optimizerおよび [Customer Journey Analytics](https://experienceleague.adobe.com/en/docs/analytics-platform/using/ai-assistant).
<!-- * **Contractual access:** Your company must agree to certain [!DNL GenAI]-related legal terms before your organization can use AI Assistant. Contact your organization's administrator or your Adobe Account Team if you are not able to access AI Assistant.  -->
* **権限：** の使用 [権限 UI](../access-control/abac/ui/permissions.md) 組織内の AI アシスタントへのアクセスを許可または取り消すには、次の手順を実行します。 AI アシスタントを使用するには、特定のユーザーがプロビジョニングされた役割に属している必要があります **AI アシスタントを有効にする** および **運用インサイトの表示** 権限。
   * 管理者は、を追加できます **AI アシスタントを有効にする** 特定の役割にユーザーを追加して、その役割が組織の AI アシスタントにアクセスできるようにします。
   * 管理者は、を追加できます **運用インサイトの表示** 特定の役割にユーザーを追加して、その役割で AI アシスタントの操作インサイト機能を使用できるようにします。 運用インサイトは現在ベータ版です。

![特定の役割に含まれる AI アシスタントを有効にする権限およびオペレーショナルインサイトを表示権限を持つ権限 UI ページ。](./images/permissions.png)

権限 UI を使用して、Experience PlatformおよびJourney Optimizerで AI アシスタントを使用する権限を付与します。 Customer Journey Analyticsで AI アシスタントにアクセスする方法について説明します。 のドキュメントを参照してください。 [Customer Journey Analytics](https://experienceleague.adobe.com/en/docs/analytics-platform/using/ai-assistant).

必要な権限を取得したら、使用しているアプリケーションの上部のヘッダーにある「AI アシスタント」アイコンを選択して、AI アシスタントにアクセスできます。

![初めてのユーザーエクスペリエンスを実現する AI アシスタント。](./images/ai-assistant.png)

## 次の手順

AI アシスタントに完全にアクセスできるようになったら、ワークフロー中にこの機能の使用に進むことができます。詳細は、 [AI アシスタント UI ガイド](./ui-guide.md) を参照してください。