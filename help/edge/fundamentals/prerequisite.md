---
title: Adobe Experience Platform Web SDK を使用するための前提条件
description: Adobe Experience Platform Web SDK を使用するための前提条件について説明します。
keywords: ファーストパーティドメイン；CNAME；スキーマ；スキーマの作成；launch;aep web sdk 拡張機能；拡張機能；設定 id；設定ツール；データ要素；データ要素の作成；XDM オブジェクト；sendEvent；イベントの送信；
exl-id: 98ae69db-bc87-4ea3-b101-664ac53e7ae0
source-git-commit: a9b63d2ad2c1adbd647c0c3a43331cddffa8a04e
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 2%

---

# Adobe Experience Platform Web SDK を使用するための前提条件

Adobe Experience Platform Web SDK を使用するには、まず以下をおこなう必要があります。

- この機能のプロビジョニングをおこないます。 アクセスを希望される場合は、次の情報を入力してください [フォーム](https://adobe.ly/websdkaccess) およびAdobeは、（必要に応じて）Datastreams とAdobe Experience Platformへのアクセス権をユーザーに提供します。 Adobeは、SDK での使用に必要なアクセス権を、追加料金なしで限定的に提供することに注意してください。
- ファーストパーティドメイン (CNAME) を有効にすることをお勧めします。 既にAdobe Analyticsの CNAME がある場合は、その CNAME を使用する必要があります。 開発でのテストは CNAME なしで動作しますが、実稼動環境に移行する前にAdobeにテストを用意することをお勧めします。 CNAME 実装は cookie の有効期間に関してメリットを提供しませんが、特定の広告ブロッカーや一般的でないブラウザーが SDK リクエストをブロックするのを防ぐ可能性があります。 このような場合、CNAME を使用すると、これらのツールを使用するユーザーのデータ収集が中断されるのを防ぐことができます。

>[!IMPORTANT]
>
>**11/10/20以降、ファーストパーティ CNAME 実装では、すべての Safari ブラウザーとモバイルiOSデバイスで 7 日間の有効期限が設定されます。**

- Web サイトが既に [Experience CloudID サービス](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html) ( 訪問者 API またはAdobe Experience Platform LaunchのExperience CloudID サービス拡張機能を通じて )Web サイトで、Adobe Experience Platform Web SDK への移行中に引き続き使用する場合は、訪問者 API の最新バージョンまたはExperience CloudID サービス拡張機能を使用する必要があります。 詳しくは、 [ID の移行](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html?lang=en#identity) を参照してください。

## Adobe Experience Platform Web SDK の権限の管理

Adobe Experience Platformの使用を開始するには、権限が必要です [権限](https://experienceleague.adobe.com/docs/experience-platform/access-control/home.html?lang=ja) を使用して、スキーマを作成し、id を管理します。 必要な最小限の権限は、「データモデリングと ID」カテゴリにあります。

![](../images/AEP-permission-categories.png)

データモデリングカテゴリ内で、ユーザーに「スキーマの管理」および「スキーマの表示」権限を付与します。

![](../images/data-modeling-permissions.png)

Identity Managementカテゴリ内で、ユーザーに ID 名前空間の管理権限と ID 名前空間の表示権限を付与します。

![](../images/identity-management-permissions.png)
