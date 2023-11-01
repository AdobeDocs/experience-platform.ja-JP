---
title: Adobe Experience Platform Web SDK を使用するための前提条件
description: Adobe Experience Platform Web SDK を使用するための前提条件について説明します。
keywords: ファーストパーティドメイン；CNAME；スキーマ；スキーマの作成；launch;aep web sdk 拡張機能；拡張機能；設定 id；設定ツール；データ要素；データ要素の作成；XDM オブジェクト；sendEvent；イベントの送信；
exl-id: 98ae69db-bc87-4ea3-b101-664ac53e7ae0
source-git-commit: e300e57df998836a8c388511b446e90499185705
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 4%

---

# Adobe Experience Platform Web SDK を使用するための前提条件

Adobe Experience Platform Web SDK を使用するには、まず以下をおこなう必要があります。

- 組織内のユーザーに対して適切な権限を設定します。 すべてのExperience Cloudのお客様がデータ収集ツールにアクセスできるので、組織の各ユーザーに必要なのは、スキーマ、ID、データストリームに対する権限だけです。 これらの権限の設定方法の詳細については、 [データ収集権限の管理](https://experienceleague.adobe.com/docs/experience-platform/collection/permissions.html?lang=ja).
- ファーストパーティドメイン (CNAME) を有効にすることをお勧めします。 既にAdobe Analyticsの CNAME がある場合は、その CNAME を使用する必要があります。 開発でのテストは CNAME なしで動作しますが、実稼動環境に移行する前にAdobeにテストを用意することをお勧めします。 CNAME 実装は cookie の有効期間に関してメリットを提供しませんが、特定の広告ブロッカーや一般的でないブラウザーが SDK リクエストをブロックするのを防ぐ可能性があります。 このような場合、CNAME を使用すると、これらのツールを使用するユーザーのデータ収集が中断されるのを防ぐことができます。

>[!IMPORTANT]
>
>**11/10/20以降、ファーストパーティ CNAME 実装では、すべての Safari ブラウザーとモバイルiOSデバイスで 7 日間の有効期限が設定されます。**

- Web サイトが既に [Experience CloudID サービス](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html?lang=ja) ( 訪問者 API またはAdobe Experience Platform LaunchのExperience CloudID サービス拡張機能を通じて )Web サイトで、Adobe Experience Platform Web SDK への移行中に引き続き使用する場合は、訪問者 API の最新バージョンまたはExperience CloudID サービス拡張機能を使用する必要があります。 詳しくは、 [ID の移行](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html#identity) を参照してください。
