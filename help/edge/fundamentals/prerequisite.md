---
title: Adobe Experience PlatformWeb SDKを使用するための前提条件
seo-title: Adobe Experience PlatformWeb SDKを使用するための前提条件
description: Adobe Experience PlatformWeb SDKを使用するための前提条件
seo-description: Adobe Experience PlatformWeb SDKを使用するための前提条件
keywords: 1st-party domain;CNAME;schema;create schema;launch;aep web sdk extension;extension;configuration id;configuration tool;data element;create data element;XDM Object;sendEvent;send Event;
translation-type: tm+mt
source-git-commit: 930e98502294cfd4ce86188246b6c33eb0744634
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 16%

---


# Adobe Experience PlatformWeb SDKを使用するための前提条件

このSDKを使用するには、まず次の操作を行う必要があります。

- この機能用に組織をプロビジョニングします。 (待機中のリストに移動したい場合は、認定ソフトウェアマネージャ(CSM)にお問い合わせください)。
- [ファーストパーティドメイン（CNAME）](https://docs.adobe.com/content/help/ja-JP/core-services/interface/ec-cookies/cookies-first-party.html)が有効になっている。Adobe AnalyticsのCNAMEを既にお持ちの場合は、そのCNAMEを使用する必要があります。 開発でのテストはCNAMEを使用しなくても機能しますが、実稼働環境に移行する前に必要になります。

>[!IMPORTANT]
>
>**2011年11月10日現在、ファーストパーティCNAMEの実装では、すべてのSafariブラウザーとモバイルiOSデバイスで7日間の有効期限が切れています。**

- Adobe Experience Platform　を使用する資格がある. Adobe Experience Platformを購入していない場合、Adobeは、SDKを使用した限定的な方法で、追加料金なしでExperience Platformデータサービス基金を提供します。
- 訪問者 ID サービスの最新バージョンを使用している.
