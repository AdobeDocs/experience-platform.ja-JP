---
title: Common Analytics 拡張機能の概要
description: Adobe Experience Platform の Common Analytics タグ拡張機能について説明します。
exl-id: 9eeb4589-df90-4356-b927-b2c29c32370b
source-git-commit: 44e2b8241a8c348d155df3061d398c4fa43adcea
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 85%

---

# Common Analytics Plugins 拡張機能の概要

このリファレンスは、Common Analytics Plugins 拡張機能の設定に関する情報、およびこの拡張機能を使用して[!DNL Adobe Analytics] 拡張機能を強化するときに使用できるオプションに関する情報です。

## Common Analytics Plugins 拡張機能の設定

この節では、Common Analytics Plugins 拡張機能を設定する際に使用できるオプションについて説明します。

>[!IMPORTANT]
>
>Common Analytics Plugins 拡張機能は、[!DNL Adobe Analytics] 拡張機能を強化します。[!DNL Adobe Analytics] 拡張機能を使用するには、プロパティに拡張機能をインストールする必要があります。さらに、トラッカーを [!DNL Adobe Analytics] 拡張機能内でグローバルにアクセスできるようにする必要があります。

拡張機能レベルでは、追加の設定は必要ありません。

## [!DNL Adobe Analytics] 拡張機能へのプラグインの追加

この拡張機能で提供されるプラグインを使用するには、まず、独自のルールで使用するプラグインを初期化する必要があります。

1. 新しいルールを作成します。
1. コアの追加 - ライブラリ読み込み（ページ上部）イベント
1. 以下のいずれかの初期化方法を使用します。

## Common Analytics Plugins 拡張機能アクションタイプ

この節では、Common Analytics Plugins 拡張機能で使用できるアクションタイプについて説明します。

Common Analytics Plugins 拡張機能は、次のアクションを提供します。

* [初期設定](#initialize)
* [プラグインの初期化](#initialize-plugin)

### 初期設定

>[!IMPORTANT]
>
>このアクションの実装は簡単ですが、プラグインの重み付けが高まるので、アドビコンサルティングではこのアクションの使用をお勧めしません。

このアクションでは、実装に含める各プラグインを選択し、変更を保存できます。実装時に使用する数を選択します。

### プラグインの初期化

これらのアクションは、個別に使用する特定のプラグインを初期化します。プロパティで使用するすべてのプラグインを初期化するには、対応するアクションをルールに追加し、ルールを保存します。この方法で拡張を設定すると、通常よりも手間がかかりますが、コードの効率は向上します。そのため、アドビではこの方法を推奨しています。

## 共通の Analytics プラグイン拡張データ要素

Common Analytics Plugins 拡張機能では、タグ機能を利用して Analytics で対応するプラグインをセットアップおよび設定できる、次のデータ要素を使用できます。

* `getGeoCoordinates`
* `getNewRepeat`
* `getPageName`
* `getResponsiveLayout`
* `getTimeParting`
* `getTimeSinceLastVisit`
* `getVisitDuration`
* `getVisitNum`

>[!NOTE]
>
>上記のプラグインについて詳しくは、[Analytics ドキュメント ](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/impl-plugins.html?lang=ja) を参照してください。
