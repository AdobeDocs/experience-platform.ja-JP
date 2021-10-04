---
title: Adobe Experience Platform Web SDK を使用するための前提条件
description: Adobe Experience Platform Web SDK を使用するための前提条件について説明します。
keywords: ファーストパーティドメイン；CNAME；スキーマ；スキーマの作成；スキーマの起動；aep web sdk 拡張機能；拡張機能；設定 id；設定ツール；データ要素；データ要素の作成；XDM オブジェクト；sendEvent；イベントの送信；
exl-id: 98ae69db-bc87-4ea3-b101-664ac53e7ae0
source-git-commit: 9d3965be1956de754f0d2a82178bf5dcd871e239
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 3%

---

# Adobe Experience Platform Web SDK を使用するための前提条件

Platform Web SDK を使用するには、まず以下をおこなう必要があります。

- この機能のプロビジョニングをおこないます。 ( アクセスを希望される場合は、Platform Web SDK へのアクセスは無料です。カスタマーサクセスマネージャー (CSM) にお問い合わせください )。
- ファーストパーティドメイン (CNAME) を有効にすることをお勧めします。 既にAdobe Analyticsの CNAME がある場合は、その CNAME を使用する必要があります。 開発でのテストは CNAME を使用しないでも機能しますが、実稼動環境に移行する前に、Adobeでテストをおこなうことをお勧めします。 CNAME 実装は cookie の有効期間に関してメリットはありませんが、特定の広告ブロッカーや一般的でないブラウザーが SDK リクエストをブロックするのを防ぐ可能性があります。 このような場合、CNAME を使用すると、これらのツールを使用するユーザーのデータ収集が中断されなくなる可能性があります。

>[!IMPORTANT]
>
>**11/10/20の時点で、ファーストパーティ CNAME 実装は、すべての Safari ブラウザーとモバイル iOS デバイスで 7 日間の有効期限が設定されています。**

- Adobe Experience Platform　を使用する資格がある. Adobe Experience Platformを購入していない場合、Adobeは、SDK で限られた方法で使用するために必要なアクセス権を追加料金なしで提供します。
- Web サイトで、Adobe Experience Platform LaunchのExperience CloudAPI またはExperience CloudID サービス拡張機能を使用して、Web サイトで既に [ 訪問者 ID サービス ](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html) を使用していて、Adobe Experience Platform Web SDK への移行中に引き続き使用する場合は、訪問者 API の最新バージョンまたはExperience CloudID サービス拡張機能を使用する必要があります。 詳しくは、[ID の移行 ](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html?lang=en#identity) を参照してください。

## Adobe Experience Platform Web SDK の権限の管理

Adobe Experience Platformを使用する場合は特別な権限は必要ありませんが、Adobe Experience Platformでスキーマを作成するための権限 [](https://experienceleague.adobe.com/docs/experience-platform/access-control/home.html?lang=ja) が必要です。 必要な最小限の権限は、「データモデリングと ID」カテゴリにあります。

![](../images/AEP-permission-categories.png)

データモデリングカテゴリ内で、ユーザーに「スキーマの管理」および「スキーマの表示」権限を付与します。

![](../images/data-modeling-permissions.png)

Identity Managementカテゴリ内で、ユーザーに ID 名前空間の管理権限と ID 名前空間の表示権限を付与します。

![](../images/identity-management-permissions.png)
