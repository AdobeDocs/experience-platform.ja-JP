---
title: Launch のクイックスタート
seo-title: Adobe Experience Platform Web SDK：Launch のクイックスタート
description: Experience Platform Web SDK 拡張機能を使用してデータを収集するためのクイックスタートガイド
seo-description: Experience Platform Web SDK 拡張機能を使用してデータを収集するためのクイックスタートガイド
keywords: 1st-party domain;CNAME;schema;create schema;launch;aep web sdk extension;extension;configuration id;configuration tool;data element;create data element;XDM Object;sendEvent;send Event;
translation-type: tm+mt
source-git-commit: 8c256b010d5540ea0872fa7e660f71f2903bfb04
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 28%

---


# ようこそ

このガイドでは、開始時にAdobe Experience PlatformWeb SDKを設定する様々な方法の手順を説明します。 この機能を使用するには、ホワイトリストに登録する必要があります。 待機中のリストに移動する場合は、CSMに連絡してください。

- [ファーストパーティドメイン（CNAME）](https://docs.adobe.com/content/help/ja-JP/core-services/interface/ec-cookies/cookies-first-party.html)が有効になっている。既に Analytics 用 CNAME をお持ちの場合は、その CNAME を使用する必要があります。開発でのテストはCNAMEを使用しなくても機能しますが、実稼働環境に移行する前に必要になります。
- Adobe Experience Platform　を使用する資格がある. プラットフォームを購入していない場合、Adobeは、限られた方法でPlatform Data Services Foundationを提供し、SDKを使用して無償で利用できます。
- 訪問者 ID サービスの最新バージョンを使用している.

## スキーマの準備

Experience Platformエッジネットワークは、データをXDMとして受け取ります。 XDMは、スキーマを定義できるデータ形式です。 スキーマは、Edge Networkでデータの形式設定方法を定義します。 データを送信するには、スキーマを定義する必要があります。

1. [スキーマ](../../xdm/tutorials/create-schema-ui.md)
2. 作追加成したスキーマに対するAEP Mixin。 [!DNL Web SDK ExperienceEvent]
3. 作成したスキーマからデータセットを作成します。

次のビデオでは、データ用のスキーマ、データセット、ストリーミングソースコネクタの作成をサポートします。 [!DNL Web SDK]


>[!VIDEO](https://video.tv.adobe.com/v/35395?quality=12&learn=on)

Launch にログインし、`AEP Web SDK` 拡張機能をインストールします。SDKをインストールすると、拡張機能を設定するように求められます。 上記で要求した設定 ID を入力します。拡張機能によって、組織の ID が自動的に入力されます。


様々な設定オプションについて詳しくは、[SDK の設定](../fundamentals/configuring-the-sdk.md)を参照してください。

## 設定IDの作成

Launchの [エッジ設定ツールを使用して、設定IDを作成できます](../fundamentals/edge-configuration.md) 。 これにより、Edge Networkで様々なソリューションにデータを送信できるようになります。 各オプションの検索方法について詳しくは、「 [Edge Configuration Tool](../fundamentals/edge-configuration.md) 」ページを参照してください。

>[!NOTE]
>
>この機能を使用するには、組織がホワイトリストに登録されている必要があります。 最終的なホワイトリスト登録のために、CSMにリストに登録するようにお問い合わせください。

## スキーマに基づいたデータ要素の作成

「起動」で、拡張機能をAEP Web SDKに変更し、タイプをに設定して、スキーマを参照するデータ要素を作成し `XDM Object`ます。 これにより、スキーマが読み込まれ、データ要素をスキーマの様々な部分にマップできます。

![開始時の日付要素](../../assets/edge_data_element.png)

## イベントの送信

After the extension is installed, start sending events by adding a `sendEvent` action from the AEP Web SDK extension to a rule. 先追加ほど作成したデータ要素をXDMデータとしてイベントに送信します。 Adobeでは、ページが読み込まれるたびに少なくとも1つのイベントを送信することをお勧めします。

イベントの追跡方法について詳しくは、[イベントのトラッキング](../fundamentals/tracking-events.md)を参照してください。

## 次の手順

データのフローを設定したら、次の操作を実行できます。

- [スキーマの構築](https://docs.adobe.com/content/help/ja-JP/experience-platform/xdm/schema/composition.html)
- [デバッグについて](../fundamentals/debugging.md)
- エクスペリエンスを [パーソナライズする方法を説明します。](../fundamentals/rendering-personalization-content.md)
- 複数のソリューションにデータを送信する方法について説明します。
   - [Adobe Analytics](../solution-specific/analytics/analytics-overview.md)
   - [Adobe Audience Manager](../solution-specific/audience-manager/audience-manager-overview.md)
   - [Adobe Target](../solution-specific/target/target-overview.md)
