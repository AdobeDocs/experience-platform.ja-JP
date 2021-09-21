---
title: Adobe Experience Platform Web SDKを使用するための前提条件
description: Adobe Experience Platform Web SDKを使用するための前提条件について説明します。
keywords: ファーストパーティドメイン；CNAME；スキーマ；スキーマの作成；launch;aep web sdk拡張機能；拡張機能；設定id；設定ツール；データ要素；データ要素の作成；XDMオブジェクト；sendEvent；イベントの送信；
exl-id: 98ae69db-bc87-4ea3-b101-664ac53e7ae0
source-git-commit: 9d3965be1956de754f0d2a82178bf5dcd871e239
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 1%

---

# Adobe Experience Platform Web SDKを使用するための前提条件

Platform Web SDKを使用するには、まず以下をおこなう必要があります。

- この機能のプロビジョニングをおこないます。 (Platform Web SDKへのアクセスは無料です。アクセスするには、カスタマーサクセスマネージャー(CSM)にお問い合わせください)。
- ファーストパーティドメイン(CNAME)を有効にすることをお勧めします。 既にAdobe AnalyticsのCNAMEがある場合は、そのCNAMEを使用する必要があります。 開発でのテストはCNAMEなしで機能しますが、Adobeでは、実稼動に移行する前にテストをおこなうことをお勧めします。 CNAME実装は、cookieの有効期間に関してメリットを提供しませんが、特定の広告ブロッカーや一般的でないブラウザーがSDKリクエストをブロックするのを防ぐ可能性があります。 このような場合、CNAMEを使用すると、これらのツールを使用するユーザーのデータ収集が中断されるのを防ぐことができます。

>[!IMPORTANT]
>
>**11/10/20の時点で、ファーストパーティCNAME実装では、すべてのSafariブラウザーとモバイルiOSデバイスで7日間の有効期限が設定されます。**

- Adobe Experience Platform　を使用する資格がある. Adobe Experience Platformを購入していない場合、Adobeは、SDKでの使用に必要なアクセスを追加料金なしで提供します。
- Webサイトで、Adobe Experience Platform Launch内のExperience CloudAPIまたはExperience CloudIDサービス拡張機能を使用して、Webサイトで既に[訪問者IDサービス](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html)を使用している場合、Adobe Experience Platform Web SDKへの移行時に引き続き使用するには、訪問者APIの最新バージョンまたはExperience CloudIDサービス拡張機能を使用する必要があります。 詳しくは、[IDの移行](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html?lang=en#identity)を参照してください。

## Adobe Experience Platform Web SDKの権限の管理

Adobe Experience Platformを使用する場合は、特別な権限は必要ありませんが、Adobe Experience Platformでスキーマを作成する権限[](https://experienceleague.adobe.com/docs/experience-platform/access-control/home.html?lang=en)が必要です。 必要な最小限の権限は、「データモデリングとID」カテゴリにあります。

![](../images/AEP-permission-categories.png)

データモデリングカテゴリ内で、ユーザーに「スキーマの管理」および「スキーマの表示」権限を付与します。

![](../images/data-modeling-permissions.png)

Identity Managementカテゴリ内で、ユーザーにID名前空間の管理とID名前空間の表示の権限を付与します。

![](../images/identity-management-permissions.png)
