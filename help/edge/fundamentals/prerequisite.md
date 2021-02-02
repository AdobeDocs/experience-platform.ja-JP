---
title: Adobe Experience PlatformWeb SDKを使用するための前提条件
seo-title: Adobe Experience PlatformWeb SDKを使用するための前提条件
description: Adobe Experience PlatformWeb SDKを使用するための前提条件
seo-description: Adobe Experience PlatformWeb SDKを使用するための前提条件
keywords: ファーストパーティドメイン；CNAME;スキーマ;スキーマの作成；起動；aep web sdk extension；拡張；設定id；設定ツール；データ要素の作成；データ要素の作成；XDMオブジェクト；sendEvent；送信イベント;
translation-type: tm+mt
source-git-commit: a19b4384e2ea64eb9ab5f0f5443fd329ec44a2a0
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 4%

---


# Adobe Experience PlatformWeb SDKを使用するための前提条件

プラットフォームWeb SDKを使用するには、まず次の操作を行う必要があります。

- この機能用に組織をプロビジョニングします。 (プラットフォームWeb SDKへのアクセスは無料です。アクセスを取得したい場合は、Customer Success Manager(CSM)にお問い合わせください)。
- ファーストパーティドメイン（CNAME）が有効になっている。Adobe AnalyticsのCNAMEを既にお持ちの場合は、そのCNAMEを使用する必要があります。 開発でのテストはCNAMEを使用しなくても機能しますが、実稼働環境に移行する前に必要になります。

>[!IMPORTANT]
>
>**2011年11月10日現在、ファーストパーティCNAMEの実装では、すべてのSafariブラウザーとモバイルiOSデバイスで7日間の有効期限が切れています。**

- Adobe Experience Platform　を使用する資格がある. Adobe Experience Platformを購入していない場合、AdobeはSDKでの使用を制限した方法で、追加料金なしで必要なアクセスを提供します。
- Webサイトで、既にWebサイト上の[Experience CloudIDサービス](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html)を(訪問者APIまたはAdobe Experience Platform Launch内のExperience CloudIDサービス拡張を使用して)使用しており、Adobe Experience PlatformWeb SDKへの移行時に引き続き使用する場合は、最新バージョンの訪問者APIまたはExperience CloudIDサービス拡張を使用する必要があります。 詳しくは、[IDの移行](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html?lang=en#identity)を参照してください。
