---
title: Audience ManagerからReal-Time CDPへの変化
description: Audience ManagerからReal-Time CDPへの移行を計画する前に考慮すべき事項を理解します。
source-git-commit: 147e95cce203933d591fc807d9d20bcbc06e68e3
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 0%

---


# Audience ManagerからReal-Time CDPへの変化

組織がAdobe Real-Time CDPの使用に進化するにつれ、以下の考慮事項を検討して、データを準備し、2 つのテクノロジー間の重要な違いを認識します。 この記事は、開業医向けの内容です。

![Audience ManagerからReal-Time CDPへの展開図](/help/rtcdp/assets/aam-to-rtcdp-evolution.png)

## 1.Audience Manager内のデータ・アーキテクチャを検討する

Audience ManagerからReal-Time CDPへの移行を考えると、今は、 [Audience Managerセグメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/segments/segments-purpose.html?lang=en) そして何を決めるか [シグナル](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/data-explorer/data-explorer-understanding-signals.html?lang=en), [特性](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/traits/trait-details-page.html?lang=en)、および [ルール](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/segments/segment-builder.html?lang=en#segment-builder-section) これらのセグメントを構成するのがです。

さらに、現在Audience Managerで使用しているデータソースについて考えてみましょう。

Adobeでは、次のようにセグメントを分類することをお勧めします。

* セグメントの一部として、 [[!UICONTROL Audience Managerソースコネクタ]](/help/sources/connectors/adobe-applications/audience-manager.md)にはデータの依存関係がないので、宛先やアクティブ化の課題はなく、セグメント化ルールはリアルタイム CDP を使用して作成できます。 [セグメントビルダー](/help/segmentation/ui/segment-builder.md) 後で。
* サポート可能なルールを持つが、Real-Time CDPで使用できないデータを含むセグメント。
* リアルタイム CDP で作成できず、機能が欠落しているセグメント。

>[!TIP]
>
>Adobe Real-Time CDPオファー [3 種類のセグメント評価](/help/segmentation/home.md#evaluate-segments): [!UICONTROL バッチ], [!UICONTROL ストリーミング]、および [!UICONTROL Edge]. Audience Managerでリアルタイムセグメントを使用するお客様は、Real-Time CDPでのストリーミングセグメント 500 件という現在の制限により、制限される場合があります。 詳細を表示 [セグメント化ガードレール](/help/profile/guardrails.md).

## 2.を介して送信するのに重要なセグメント [!UICONTROL Audience Managerソースコネクタ]?

評価基準に基づき、データに依存しないセグメント、宛先やアクティブ化の課題がないセグメント、セグメント化ルールは、Real-Time CDPのデータ収集 ( [Adobe Experience Platform Web SDK](/help/edge/web-sdk-faq.md) は、後日、Audience Managerソースコネクタを通じて送信する必要があります。

## 3. [!UICONTROL Experience Cloudオーディエンス] 宛先にデータを取り込みますか？

ルールを持ち、Real-Time CDPでサポートできるが、Audience Managerにアクティベーションの依存関係を持つセグメントは、 [Experience Cloudオーディエンス](/help/destinations/catalog/adobe/experience-cloud-audiences.md) 宛先カード。

## 4.今日、Real-Time CDPへの移行を開始できるAudience Managerの宛先はどれですか？

Adobeでは、Audience Managerでアクティブ化されたセグメントを [ユーザーベースの宛先](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/destinations/people-based/people-based-destinations-overview.html?lang=en) 経由でReal-Time CDPに押し込まれる [!UICONTROL Audience Managerソースコネクタ]をクリックし、Real-Time CDPからアクティベートします。

Audience Manager — で使用可能なすべてのユーザーベースの宛先 [Facebook](/help/destinations/catalog/social/facebook.md), [[!UICONTROL Google Customer Match]](/help/destinations/catalog/advertising/google-customer-match.md), [linkedIn](/help/destinations/catalog/social/linkedin.md)  — は、Real-Time CDPでも使用できます。

その他のファーストパーティデータおよびメディア戦略パートナー ( [Pinterest](/help/destinations/catalog/advertising/pinterest.md), [Snapchat](/help/destinations/catalog/advertising/snap-inc.md), [TikTok](/help/destinations/catalog/social/tiktok.md), [Amazon Ads](/help/destinations/catalog/advertising/amazon-ads.md)、および [[!UICONTROL トレードデスク]](/help/destinations/catalog/advertising/tradedesk.md) が使用可能です。

Real-Time CDPは現在、 [カタログ](/help/destinations/catalog/overview.md)（そのうち 20 件以上が、ファーストパーティオーディエンスのマッチングをサポートする広告またはソーシャルの宛先）。

## 次の手順

このページを読んだ後、Audience ManagerからReal-Time CDPへの移行を開始する際に、最初に検討すべき点がいくつかあります。
