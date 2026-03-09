---
title: Experience Platformでの AI アシスタント（従来）へのアクセス
description: Experience Cloud UI で AI アシスタントにアクセスする方法を説明します。
exl-id: c4cdff25-512c-4b4c-be91-ad9360067a0a
source-git-commit: 077c42f2190316a00168bbeca685c08677c2b13a
workflow-type: tm+mt
source-wordcount: '876'
ht-degree: 0%

---

# Experience Platformでの AI アシスタント（従来）へのアクセス

>[!IMPORTANT]
>
>このドキュメントは、AI アシスタント（レガシー）に当てはまります。 AI アシスタント（次世代）について詳しくは、[Experience Cloudの AI](https://experienceleague.adobe.com/ja/docs/experience-cloud-ai/experience-cloud-ai/ai-assistant/ai-assistant-ui) ドキュメントの [AI アシスタント UI ガイド &#x200B;](https://experienceleague.adobe.com/ja/docs/experience-cloud-ai/experience-cloud-ai/home) を参照してください。

AI Assistant （レガシー）と AI Assistant （次世代）の比較については、次の表を参照してください。

| 機能領域 | AI アシスタント（レガシー） | AI アシスタント（次世代） |
| --- | --- | --- |
| ユーザーエクスペリエンス | AI アシスタント（レガシー）は、右側のパネルでのみ使用できます。 | AI アシスタント（次世代）は、右パネルと没入型フルスクリーンエクスペリエンスの両方で利用できます。 |
| 業務の範囲 | 製品知識と運用インサイトの両方に AI アシスタント（レガシー）を使用できます。 | AI Assistant （Next-Gen）を使用して、製品の知識、運用のインサイト、高度なエージェンシースキル、複数のステップのタスク実行を行うことができます。 |
| Platform アーキテクチャ | AI アシスタント（従来の）は、Agent Orchestrator スタックに基づいて構築されていません。 | AI アシスタント（次世代）は [Adobe Experience Platform Agent Orchestrator](https://experienceleague.adobe.com/ja/docs/experience-cloud-ai/experience-cloud-ai/agents/agent-orchestrator) を活用し、拡張性と機能間の高度な調整を可能にします。 |
| アプリケーション範囲 | AI アシスタント（レガシー）は、アプリケーション固有の実装です。 | AI アシスタント（次世代）を使用すると、すべてのAdobe Experience Cloud アプリケーションで AI アシスタントを統一することができます。 |
| アクセスと権限モデル | 個々の製品境界に合わせた、アプリケーションスコープのアクセスモデル。 | すべてのユーザーが AI Assistant （次世代）および関連するExperience Platform エージェントにアクセスできます。 **注意**: <ul><li>**Adobe Experience Manager**：管理者から、[Adobe Admin Console](https://helpx.adobe.com/jp/enterprise/using/admin-console.html) 経由で AI アシスタント（次世代）にアクセスする権限を付与されている必要があります。</li><li>**Customer Journey Analytics**：管理者から、[Customer Journey Analytics アクセス制御 &#x200B;](https://experienceleague.adobe.com/ja/docs/analytics-platform/using/technotes/access-control?lang=en) を通じて AI アシスタントにアクセスする権限を付与してもらう必要があります。 これにより、製品に関する知識やデータインサイトを得ることができます。 |

Adobe Experience Cloudの複数のアプリケーションから AI アシスタント（従来）にアクセスできます。

>[!NOTE]
>
>AI アシスタント（従来）へのアクセス権を取得するには、最初に追加の法律条項に同意する必要があることを示すポップアップメッセージが権限 UI で表示された場合は、Adobe アカウントチームに連絡して、これらの条項に関するガイダンスを求めてください。

## 基本を学ぶ {#get-started}

AI アシスタント（レガシー）にアクセスするには、2 つの前提条件の手順を完了する必要があります。

1. 組織はまず法的条件に同意する必要があります。 詳しくは、Adobe アカウントチームにお問い合わせください。
2. 管理者から、AI アシスタント（レガシー）にアクセスするための十分な権限を付与してもらう必要があります。

これら 2 つの前提条件の手順がいずれも完了していない場合は、Experience Platform UI で AI アシスタント（従来）のチャットアイコンを選択すると、次のメッセージが表示されます。

>[!BEGINTABS]

>[!TAB  組織が AI アシスタントを使用できない（レガシー） ]

AI アシスタント（レガシー）を使用する法的資格を持たない組織を使用している場合は、次のメッセージが表示されます。 このシナリオでは、Adobe アカウントチームに連絡してアクセス権限を解決する必要があります。

![&#x200B; 組織が AI アシスタント（レガシー）を使用できない場合にExperience Platform UI に表示されるポップアップメッセージ &#x200B;](./images/access/modal-one.png)

>[!TAB  適切な権限がありません ]

AI アシスタント（レガシー）を使用する法的資格を持ち、引き続きこの機能にアクセスできない場合は、Experience Platform UI に次のメッセージが表示されます。 このシナリオは、機能にアクセスするための十分な権限を持っていないことを意味します。管理者に連絡して権限の問題を解決する必要があります。

![AI アシスタント（レガシー）に必要な権限がない場合にExperience Platform UI に表示されるポップアップメッセージ。](./images/access/modal-two.png)

>[!ENDTABS]

## AI アシスタントへのアクセス権の取得（レガシー） {#get-access-to-ai-assistant}

AI アシスタント（レガシー）へのアクセスは、次のパラメーターで制御されます。

* **アプリケーションへのアクセス：** AI アシスタント（従来）には、Adobe Experience Platform、Adobe Real-Time CDP、Adobe Journey Optimizer、[Customer Journey Analyticsでアクセスできます &#x200B;](https://experienceleague.adobe.com/ja/docs/analytics-platform/using/ai-assistant)。
<!-- * **Contractual access:** Your company must agree to certain [!DNL GenAI]-related legal terms before your organization can use AI Assistant (Legacy). Contact your organization's administrator or your Adobe Account Team if you are not able to access AI Assistant (Legacy).  -->
* **権限：** [&#x200B; 権限 UI](../access-control/abac/ui/permissions.md) を使用して、組織の AI アシスタント（レガシー）へのアクセスを許可または取り消します。 AI アシスタント（レガシー）を使用するには、特定のユーザーが **AI アシスタントを有効にする** および **オペレーショナルインサイトを表示** 権限でプロビジョニングされた役割に属する必要があります。
   * 管理者は、特定の役割に **AI アシスタントを有効にする** を追加し、その役割にユーザーを追加して、組織の AI アシスタント（レガシー）にアクセスできるようにすることができます。 **注意**：この権限は、当該ユーザーが AI アシスタント（レガシー）にアクセスできるようにするもので、他のユーザーに AI アシスタント（レガシー）へのアクセスを付与するための管理権限を付与するものではありません。
   * 管理者は、**運用インサイトを表示** を特定の役割に追加し、その役割にユーザーを追加して、AI アシスタント（レガシー）の運用インサイト機能を使用できます。

[&#x200B; 権限 UI](../access-control/abac/ui/roles.md) を使用して、Experience PlatformおよびJourney Optimizerで AI アシスタント（従来）を使用するための権限を付与します。 Customer Journey Analyticsで AI アシスタント（従来）にアクセスする方法について説明します。 [Customer Journey Analytics](https://experienceleague.adobe.com/ja/docs/analytics-platform/using/ai-assistant) のドキュメントを参照してください。

![AI アシスタントを有効にする（レガシー）権限と特定の役割に含まれるオペレーショナルインサイトを表示権限の権限 UI ページ。](./images/access/access-permissions.png)

必要な権限を取得したら、使用しているアプリケーションの上部ヘッダーにある「AI アシスタント（レガシー）」アイコンを選択して、AI アシスタント（レガシー）にアクセスできます。

![&#x200B; 初めてのユーザーエクスペリエンスを備えた AI アシスタント（レガシー）。](./images/access/access-home.png)

組織とユーザーに AI アシスタント（レガシー）へのアクセスを設定する方法については、次のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/3475920/?captions=jpn&learn=on)

## 次の手順

AI アシスタント（レガシー）に完全にアクセスできるようになったら、ワークフローでの機能の使用に進むことができます。詳しくは、[AI アシスタント（レガシー） UI ガイド &#x200B;](./ui-guide.md) を参照してください。
