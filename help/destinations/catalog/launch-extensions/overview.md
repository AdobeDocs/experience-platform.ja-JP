---
keywords: タグ拡張機能；タグ拡張；Launch の宛先；platform tag extensions;platform tag extension;platform launchの宛先
title: Adobe Experience Platformのタグ拡張
description: Adobe Experience Platformは、次世代のタグ管理機能をAdobeから提供します。 Platform は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。
exl-id: 54fca635-0e37-460e-abb3-5da294d4e0cf
source-git-commit: 272cf2906b44ccfeca041d9620ac0780e24ad1ae
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 35%

---

# Adobe Experience Platformのタグ拡張

Adobe Experience Platformは、次世代のタグ管理機能をAdobeから提供します。 Platform は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のタグをデプロイおよび管理するためのシンプルな手段を提供します。 タグは、Adobe Experience Cloudのお客様に、付属の付加価値機能として提供されます。

タグの概要については、以下のリソースを参照してください。

- [タグの概要](../../../tags/home.md)
- [クイックスタートガイド](../../../tags/quick-start/quick-start.md)

## Platform インターフェイスでタグの拡張を見つける方法 {#how-to-find-extensions-in-interface}

Platform インターフェイスで拡張機能を探すには、**[!UICONTROL 宛先]** / **[!UICONTROL カタログ]** を参照し、**[!UICONTROL タイプ]** フィルターで **[!UICONTROL 拡張機能]** を選択します。

![インターフェイスの「拡張機能」フィルター](../../assets/catalog/launch-extensions/filter.png)

## タグ拡張の仕組み {#how-extensions-work}

拡張機能は、イベントの生データを複数のタイプの宛先に転送します。 この拡張機能は、**イベント転送**&#x200B;タイプの宛先であると考えることができます。これは、宛先プラットフォームと単純に統合された機能で、イベントの生データを転送するだけです。例としては、[Gainsight パーソナライズ機能拡張機能](../personalization/gainsight.md)や [Confirmit Voice of the Customer 拡張機能](../voice/confirmit-digital-feedback.md)などがあります。

**Adobe Experience Platformのプロファイル/セ** グメントの書き出し先は、イベントデータを取り込み、他のデータソースと組み合わせ、セグメント化を適用し、セグメントと絞り込まれたプロファイルを宛先に書き出します。例としては、[Amazon S3 クラウドストレージの宛先](../cloud-storage/amazon-s3.md)や [Google Display &amp; Video 360 の広告の宛先](../advertising/google-dv360.md)などがあります。

![他の宛先とのタグ拡張](../../assets/common/launch-and-other-destinations.png)

## タグ拡張機能を使用するメリット {#extensions-benefits}

既存のプラットフォームのお客様は、Platform のタグ機能を無料でExperience Cloudできます。 このシステムは、使いやすい拡張機能を使用して、Web サイトにタグを簡単に導入できます。この拡張機能は、インストール、設定、更新、削除できます。 タグを使用すると、Web サイトに小さな足跡が残り、ページをすばやく読み込むことができます。

セグメントをアクティブ化して拡張機能にタグを付けることはできませんが、特定の状況でのみイベントデータを転送するルールを設定できます。 この強力な機能により、すべてのインタラクションでイベントデータを送信するのではなく、特定の状況でのみイベントデータを転送できます。詳しくは、[ タグのドキュメント ](../../../tags/ui/managing-resources/rules.md) のルールに関するページを参照してください。

##  の拡張機能の使用事例 {#extensions-use-cases}

拡張機能を使用すると、様々な顧客の使用例に対応できます。  の拡張機能の使用事例を以下に示します。

- Facebook ピクセル拡張機能を使用して、web サイトまたはネイティブのアプリケーションデータを Facebook に送信できます。Facebook ピクセルでは、訪問者がサイトまたはアプリケーションのどの部分にアクセスしたかを把握したり、その情報を Facebook に転送したりすることができます。また、Facebook を介して、訪問者を再ターゲティングすることができます。
- web サイトやアプリケーションのイベントデータを Google Analytics に転送してそのデータを分析し、それに基づいて決定を下すことができます。
- 設定したルールに従って、ユーザーがページとどのようにやり取りしているかに基づいて、適切なタイミングでクライアント側の Chatbox アプリをオンにすることができます。

## 拡張機能のカテゴリ {#extension-categories}

拡張機能は、Platform で次のカテゴリに分類できます。

- [Advertising Cloud](../advertising/overview.md)
- [Analytics](../analytics/overview.md)
- [データ管理プラットフォーム](../data-management/overview.md)
- [電子メールマーケティングの宛先](../email-marketing/overview.md)
- [パーソナライゼーション](../personalization/overview.md)
- [調査](../survey/overview.md)
- [顧客の声](../voice/overview.md)
