---
title: Audience Manager から Real-Time CDP への進化
description: Audience Manager から Real-Time CDP への移行を計画する前の考慮事項について説明します。
exl-id: 83ab9a5d-9abc-4072-b449-e2a9ecd48639
source-git-commit: ba5a539603da656117c95d19c9e989ef0e252f82
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 90%

---

# Audience Manager から Real-Time CDP への進化

組織が Adobe Real-Time CDP を使用するように進化するにつれて、データを準備し、2 つのテクノロジー間の重要な違いを認識するために次の考慮事項を検討します。この記事は、実務担当者オーディエンスを対象としています。

![Audience Manager から Real-Time CDP への進化を示す図](/help/rtcdp/assets/aam-to-rtcdp-evolution.png)

## 1. Audience Manager 内のデータアーキテクチャの検討

Audience Manager から Real-Time CDP への進化を検討する場合、今が [Audience Manager のセグメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/segments/segments-purpose.html?lang=ja)を分析し、これらのセグメントを構成する[シグナル](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/data-explorer/data-explorer-understanding-signals.html?lang=ja)、[特性](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/traits/trait-details-page.html?lang=ja)、[ルール](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/segments/segment-builder.html?lang=ja#segment-builder-section)が何であるかを判断する重要な時期です。

さらに、Audience Manager で現在使用しているデータソースについても考慮します。

アドビでは、セグメントを次のように分類することをお勧めします。

* セグメントの一部として、 [[!UICONTROL Audience Managerソースコネクタ]](/help/sources/connectors/adobe-applications/audience-manager.md)データに依存しないので、宛先やアクティブ化に関する課題はなく、セグメント化ルールはReal-Time CDPを使用して作成できます [セグメントビルダー](/help/segmentation/ui/segment-builder.md) 後で。
* サポートできるルールがあるセグメント：Real-Time CDP で使用できないデータが含まれている場合があります。
* Real-Time CDPで作成できず、機能が欠落しているセグメント。

>[!TIP]
>
>Adobe Real-Time CDP では、[!UICONTROL バッチ]、[!UICONTROL ストリーミング]、[!UICONTROL エッジ]の [3 種類のセグメント評価](/help/segmentation/home.md#evaluate-segments)が提供されます。Audience Manager でリアルタイムセグメントを使用するお客様は、Real-Time CDP での 500 個のストリーミングセグメントという現在の制限により、制限される場合があります。詳しくは、[セグメント化ガードレール](/help/profile/guardrails.md)を参照してください。

## 2. [!UICONTROL Audience Manager ソースコネクタ]経由で送信するために重要なセグメント

評価基準に基づいて、データの依存関係がなく、宛先やアクティブ化の課題がなく、後日 [Adobe Experience Platform Web SDK](/help/edge/web-sdk-faq.md) などの Real-Time CDP データ収集を通じて作成できるセグメントは、Audience Manager ソースコネクタ経由で送信する必要があります。

## 3. [!UICONTROL Experience Cloud Audiences] の宛先を使用してデータを Audience Manager に戻す

Real-Time CDP でサポートできるルールはあるが、Audience Manager へのアクティブ化依存関係があるセグメントは、[Experience Cloud Audiences](/help/destinations/catalog/adobe/experience-cloud-audiences.md) 宛先カード経由で Audience Manager に送信できます。

## 4. Real-Time CDP への移行を開始できる、現在 Audience Manager に設定されている宛先

アドビでは、Audience Manager で[人物ベースの宛先](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/destinations/people-based/people-based-destinations-overview.html?lang=ja)にアクティブ化されたセグメントを、[!UICONTROL Audience Manager ソースコネクタ]経由で Real-Time CDP にプッシュし、Real-Time CDP を通じてアクティブ化することを強くお勧めします。

Audience Manager で使用できるすべての人物ベースの宛先（[Facebook](/help/destinations/catalog/social/facebook.md)、[[!UICONTROL Google Customer Match]](/help/destinations/catalog/advertising/google-customer-match.md)、[LinkedIn](/help/destinations/catalog/social/linkedin.md)）は、Real-Time CDP でも使用できます。

[Pinterest](/help/destinations/catalog/advertising/pinterest.md)、[Snapchat](/help/destinations/catalog/advertising/snap-inc.md)、[TikTok](/help/destinations/catalog/social/tiktok.md)、[Amazon Ads](/help/destinations/catalog/advertising/amazon-ads.md)、[[!UICONTROL The Trade Desk]](/help/destinations/catalog/advertising/tradedesk.md) などの追加のファーストパーティデータおよびメディア戦略パートナーも使用できます。

Real-Time CDP は現在、[カタログ](/help/destinations/catalog/overview.md)内で 60 を超える宛先をネイティブにサポートしており、そのうち 20 を超える宛先は、ファーストパーティオーディエンスマッチングをサポートする広告宛先またはソーシャル宛先です。

## 次の手順

このページを参照すると、Audience Manager から Real-Time CDP への進化を開始する際に、最初に検討すべき一連の考慮事項が得られます。
