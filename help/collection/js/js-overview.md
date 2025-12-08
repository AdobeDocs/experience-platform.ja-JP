---
title: Web SDK JavaScript ライブラリの概要
description: JavaScriptを使用してAdobe Experience Platform Edge Networkにデータを送信します。
exl-id: 1348144a-7d25-4c27-bc40-3daee2f043a6
source-git-commit: 7f932e9868e84cf8abdaa6cf0b2da5bac837234d
workflow-type: tm+mt
source-wordcount: '440'
ht-degree: 3%

---

# Web SDK JavaScript ライブラリの概要

**Adobe Experience Platform Web SDK** は、Adobe Experience Platform Edge Networkにデータを送信できるクライアントサイド JavaScript ライブラリです。 このガイドには、中心概念、インストール、設定、コマンドなど、web SDK JavaScript ライブラリ（`alloy.js`）の実装パスが記載されています。 データ収集 UI の web SDK タグ拡張機能については、[web SDK タグ拡張機能 ](/help/tags/extensions/client/web-sdk/overview.md) を参照してください。

Web SDKは、ソリューションに依存しない方法（XDM）でExperience Platform Edge Networkにデータを送信し、その後、データをソリューション固有の形式と宛先にマッピングし、リアルタイムで送信します。

## Experience PlatformEdge Network {#edge-network}

Adobe Experience Platform Edge Networkは、すべてのアドレス可能なチャネルにわたって、低遅延のデータ収集、プラグ可能なコンピューティング、迅速なデータアクティブ化を提供します。 Web、モバイルおよびサーバーサイドチャネル用の統合SDKを 1 つ提供し、データを共通のAdobe ドメイン（`adobedc.net`）に送信して、データとエクスペリエンスの配信に使用する 1 つのペイロードを受け取ります。

サーバーサイドでは、統合エッジゲートウェイと共通のプラットフォームサービスフレームワークにより、新しい機能のデプロイを簡素化できると同時に、次のメリットがあります。

* お客様の価値創出までの時間の短縮
* 「ポイント」統合の必要性を排除
* 古いライブラリと比較してパフォーマンスを向上させる。
* 運用コストの削減
* イノベーションのスピードの向上
* Adobeのお客様に持続的な競争上の優位性を提供する。

統合されたエッジシステムにより、あらゆるチャネルをまたいで、広告、マーケティングおよびパーソナライゼーションキャンペーンを管理できます。 総所有コストを削減し、様々なデータタイプをサポートするので、複数のExperience Cloud製品で使用するデータモデルをマッピングできます。

>[!VIDEO](https://video.tv.adobe.com/v/34141?quality=12&learn=on)

## Web SDK に置き換わるライブラリ {#sdks}

Web SDKは、既存のライブラリの機能を統合するために一から構築されたオープンソースライブラリです。 タグの実行順序、バージョンの不整合、依存関係の管理の問題に対処し、多くのExperience Cloud製品を実装する手段を提供します。 Web SDKは、次のサービスのデータ収集に代わるものです。

* Adobe Experience Platform訪問者 ID サービス （`Visitor.js`）
* Adobe Analytics（`AppMeasurement.js`）
* Adobe Target（`AT.js`）
* Adobe Audience Manager（`DIL.js`）
* Adobe Media Analytics
* Adobe Advertising

また、Adobe ソリューションへの HTTP リクエストを効率化する新しいエンドポイントも導入されます。 以前は、各データ収集ライブラリに対して複数の呼び出しが必要でした。 1 回の呼び出しで ID を取得し、[!DNL Target] エクスペリエンスを取得し、[!DNL Audience Manager] にデータを送信し、Adobe Experience Platformにデータを渡すことができるようになりました。

## 既存のライブラリから Web SDK への移行 {#migrating-to-web-sdk}

Adobeは、合理化されたアップグレードパスを提供し、既存のライブラリから web SDKへの移行を簡素化します。 一度にサイト全体を移行しなくても、web サイトの各ページを個別に移行できます。 一部のページでは Web SDKを使用できますが、既存のライブラリは別のページで使用することができ、段階的に移行することができます。
