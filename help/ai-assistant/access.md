---
title: Experience Platformでの AI アシスタントへのアクセス
description: Experience CloudUI で AI アシスタントにアクセスする方法を説明します。
source-git-commit: a1092e21940c5e4ba9b598bc51ba1243b57a0051
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 0%

---

# Experience Platformでの AI アシスタントへのアクセス

Adobe Experience Cloudの複数のアプリケーションから AI アシスタントにアクセスできます。

AI アシスタントへのアクセスは、次のパラメータで制御されます。

* **アプリケーションにアクセスします。** AI アシスタントには、Adobe Experience Platform、Adobe Real-Time CDP、Adobe Journey Optimizerおよび [Customer Journey Analytics](https://experienceleague.adobe.com/en/docs/analytics-platform/using/ai-assistant).
* **契約上のアクセス：** 会社は以下に同意する必要があります [!DNL GenAI]組織が AI アシスタントを使用する前に、に関連する法律条項を追加します。 AI アシスタントにアクセスできない場合は、組織の管理者またはAdobeアカウントチームにお問い合わせください。
* **権限：** の使用 [権限 UI](../access-control/abac/ui/permissions.md) 組織内の AI アシスタントへのアクセスを許可または取り消すには、次の手順を実行します。 AI アシスタントを使用するには、特定のユーザーがプロビジョニングされた役割に属している必要があります **AI アシスタントを有効にする** および **運用インサイトの表示** 権限。
   * 管理者は、を追加できます **AI アシスタントを有効にする** 特定の役割にユーザーを追加して、その役割が組織の AI アシスタントにアクセスできるようにします。
   * 管理者は、を追加できます **運用インサイトの表示** 特定の役割にユーザーを追加して、その役割で AI アシスタントの操作インサイト機能を使用できるようにします。 運用インサイトは現在ベータ版です。

![特定の役割に含まれる AI アシスタントを有効にする権限およびオペレーショナルインサイトを表示権限を持つ権限 UI ページ。](./images/permissions.png)

必要な権限を取得したら、使用しているアプリケーションの上部のヘッダーにある「AI アシスタント」アイコンを選択して、AI アシスタントにアクセスできます。

![初めてのユーザーエクスペリエンスを実現する AI アシスタント。](./images/ai-assistant.png)

## 次の手順

AI アシスタントに完全にアクセスできるようになったら、ワークフロー中にこの機能の使用に進むことができます。詳細は、 [AI アシスタント UI ガイド](./ui-guide.md) を参照してください。