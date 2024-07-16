---
keywords: タグ拡張機能；タグ拡張機能；launch の宛先；Platform タグ拡張機能；Platform タグ拡張機能；platform launchの宛先
title: Adobe Experience Platformのタグ拡張機能
description: Adobe Experience Platformは、Adobeの次世代タグ管理機能を提供します。 Platform を使用すると、関連する顧客体験を強化するために必要なすべての分析、マーケティング、広告などのタグを、簡単にデプロイして管理できます。
exl-id: 54fca635-0e37-460e-abb3-5da294d4e0cf
source-git-commit: d6402f22ff50963b06c849cf31cc25267ba62bb1
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 22%

---

# Adobe Experience Platformのタグ拡張機能

Adobe Experience Platformは、Adobeの次世代タグ管理機能を提供します。 Platform を使用すると、関連する顧客体験を強化するために必要なすべての分析、マーケティング、広告などのタグを、簡単にデプロイして管理できます。 タグは、Adobe Experience Cloudのお客様に、付属の付加価値機能として提供されます。

タグの概要については、以下のリソースを参照してください。

- [タグの概要](../../../tags/home.md)
- [クイックスタートガイド](../../../tags/quick-start/quick-start.md)

## Platform インターフェイスでタグ拡張機能を見つける方法 {#how-to-find-extensions-in-interface}

Platform インターフェイスで拡張機能を見つけるには、**[!UICONTROL 宛先]**/**[!UICONTROL カタログ]** を参照し、**[!UICONTROL タイプ]** フィルターの **[!UICONTROL 拡張機能]** を選択します。

![インターフェイスの「拡張機能」フィルター](../../assets/catalog/launch-extensions/filter.png)

## タグ拡張機能の仕組み {#how-extensions-work}

[ タグ拡張機能 ](../../../tags/home.md#extensions) は、web サイトまたはモバイルアプリの機能を強化するコードのパッケージです。 これには、生のイベントデータを [Google Analyticsなどの宛先に送信することが含まれますが ](/help/destinations/catalog/analytics/google-universal-analytics.md) 他の機能を提供することもできます。

タグとイベント転送拡張機能を区別することが重要です。 Platform 宛先のユーザーインターフェイスに表示される拡張機能は、*タグ拡張機能* です。 [ タグとイベント転送の違い ](/help/tags/ui/event-forwarding/overview.md#differences-between-event-forwarding-and-tags) について詳しくは、イベント転送の概要を参照してください。



<!--

Extensions forward raw event data to several types of destinations. Think of extensions as an **Event Forwarding** type of destination. This is a simpler type of integration with destination platforms, which only forwards raw event data. Examples of those are the [Gainsight personalization extension](../personalization/gainsight.md) or the [Confirmit Voice of the Customer extension](../voice/confirmit-digital-feedback.md).

**Profile/Segment Export** destinations in Adobe Experience Platform capture event data, combine it with other data sources, apply segmentation, and export audiences and qualified profiles to destinations. Examples of those are the [Amazon S3 cloud storage destination](../cloud-storage/amazon-s3.md) or the [Google Display & Video 360 advertising destination](../advertising/google-dv360.md).

![Tag extensions compared to other destinations](../../assets/common/launch-and-other-destinations.png)

-->

## タグ拡張機能を使用するメリット {#extensions-benefits}

Platform のタグ機能は、既存のExperience Cloudのお客様には無料で利用できます。 このシステムは、インストール、設定、更新、削除が可能な使いやすい拡張機能を使用して、web サイトでのタグのデプロイメントを簡素化します。 タグを使用すると、web サイト上に小さな足跡が残り、ページの読み込みを迅速に維持できます。

オーディエンスをアクティブ化して拡張機能をタグ付けすることはできませんが、特定の状況でイベントデータのみを転送するようにルールを設定できます。 この強力な機能により、すべてのインタラクションでイベントデータを送信するのではなく、特定の状況でのみイベントデータを転送できます。詳しくは、[ タグドキュメント ](../../../tags/ui/managing-resources/rules.md) のルールについてを参照してください。

## 拡張機能の使用例 {#extensions-use-cases}

拡張機能を使用すると、様々な顧客のユースケースを満たすことができます。 拡張機能を使用するユースケースの例を次に示します。

- Facebook ピクセル拡張機能を使用して、web サイトまたはネイティブのアプリケーションデータを Facebook に送信できます。Facebook ピクセルでは、訪問者がサイトまたはアプリケーションのどの部分にアクセスしたかを把握したり、その情報を Facebook に転送したりすることができます。また、Facebook を介して、訪問者を再ターゲティングすることができます。
- web サイトやアプリケーションのイベントデータを Google Analytics に転送してそのデータを分析し、それに基づいて決定を下すことができます。
- 設定したルールに従って、ユーザーがページとやり取りする方法に基づいて、適切なタイミングでクライアントサイドのチャットボックスアプリをオンにできます。

## 拡張機能のカテゴリ {#extension-categories}

拡張機能は、Platform の次のカテゴリに該当する場合があります。

- [広告](../advertising/overview.md)
- [Analytics](../analytics/overview.md)
- [データ管理プラットフォーム](../data-management/overview.md)
- [メールマーケティングの宛先](../email-marketing/overview.md)
- [パーソナライズ機能](../personalization/overview.md)
- [調査](../survey/overview.md)
- [お客様の声](../voice/overview.md)
