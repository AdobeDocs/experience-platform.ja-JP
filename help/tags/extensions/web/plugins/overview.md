---
title: Common Analytics 拡張機能の概要
description: Adobe Experience Platform の Common Analytics タグ拡張機能について説明します。
exl-id: 9eeb4589-df90-4356-b927-b2c29c32370b
source-git-commit: 0c2ee3bbb4d85bd755b4847a509fc7bd50ba67bc
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 100%

---

# Common Analytics Plugins 拡張機能の概要

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

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

このアクションでは、実装に含める各プラグインを選択し、変更を保存できます。実装時に使用する数だけ、または必要な数だけ選択します。各プラグインの使用方法に関するドキュメントへのリンクと簡単な説明が、Analytics [プラグインの概要](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/impl-plugins.html?lang=ja)に記載されています。

### プラグインの初期化

これらのアクションは、個別に使用する特定のプラグインを初期化します。プロパティで使用するすべてのプラグインを初期化するには、対応するアクションをルールに追加し、ルールを保存します。この方法で拡張を設定すると、通常よりも手間がかかりますが、コードの効率は向上します。そのため、アドビではこの方法を推奨しています。

## 共通の Analytics プラグイン拡張データ要素

このセクションでは、共通 Analytics プラグイン拡張機能で使用できるデータ要素について説明します。

### getGeoCoordinates

Adobe Experience Platform のネイティブのデータ収集 UI を利用して、getGeoCoordinates プラグインをセットアップおよび設定できます。

### getNewRepeat

ネイティブのデータ収集 UI を利用して、getNewRepeat プラグインをセットアップおよび設定できます。

### getPageName

ネイティブのデータ収集 UI を利用して、getPageName プラグインをセットアップおよび設定できます。

### getResponsiveLayout

ネイティブのデータ収集 UI を利用して、getResponsiveLayout プラグインをセットアップおよび設定できます。

### getTimeParting

ネイティブのデータ収集 UI を利用して、getTimeParting プラグインをセットアップおよび設定できます。

### getTimeSinceLastVisit

ネイティブのデータ収集 UI を利用して、getTimeSinceLastVisit プラグインをセットアップおよび設定できます。

### getVisitDuration

ネイティブのデータ収集 UI を利用して、getVisitDuration プラグインをセットアップおよび設定できます。

### getVisitNum

ネイティブのデータ収集 UI を利用して、getVisitNum プラグインをセットアップおよび設定できます。
