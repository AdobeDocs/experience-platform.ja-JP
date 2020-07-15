---
title: Launch のクイックスタート
seo-title: Adobe Experience Platform Web SDK：Launch のクイックスタート
description: Experience Platform Web SDK 拡張機能を使用してデータを収集するためのクイックスタートガイド
seo-description: Experience Platform Web SDK 拡張機能を使用してデータを収集するためのクイックスタートガイド
translation-type: tm+mt
source-git-commit: 9d58693646f472e84f04a64c4ad66f61dc5d3eba
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 23%

---


# ようこそ

このガイドでは、Adobe LaunchでのAdobe Experience PlatformWeb SDKのセットアップ方法に関する様々な手順を順を追って説明します。 この機能を使用するには、権限が必要で、許可リスト上にある必要があります。 待機中のリストに移動したい場合は、CSMに連絡してください。 また、この機能を使用するには、次の作業が必要です。

- [ファーストパーティドメイン（CNAME）](https://docs.adobe.com/content/help/ja-JP/core-services/interface/ec-cookies/cookies-first-party.html)が有効になっている。アドビAnalyticsのCNAMEを既にお持ちの場合は、そのCNAMEを使用する必要があります。 開発環境でのテストはCNAMEなしでは機能しますが、実稼働環境に移行する前にCNAMEが必要になります
- 訪問者 ID サービスの最新バージョンを使用している

## 設定IDの作成

Adobe Launchの [エッジ設定ツールを使用して、設定IDを作成できます](../fundamentals/edge-configuration.md) 。 これにより、Edge Networkで様々なソリューションにデータを送信できるようになります。 各オプションの検索方法について詳しくは、「 [Edge Configuration Tool](../fundamentals/edge-configuration.md) 」ページを参照してください。

>[!NOTE]
>
>組織がこの機能を使用する許可リスト上に存在する必要があります。 許可リストに追加するには、CSMに問い合わせてください。

## スキーマの準備

Experience Platformエッジネットワークは、データをXDMとして受け取ります。 XDMは、スキーマを定義できるデータ形式です。 スキーマは、Edge Networkでデータの形式設定方法を定義します。 データを送信するには、スキーマを定義する必要があります。 次の項目を必ず実行してください。

1. [スキーマの作成](../../xdm/tutorials/create-schema-ui.md)
2. AEP Web SDK ExperienceEvent Mixin追加を作成したスキーマにミックスインします。
3. 作成したスキーマからデータセットを作成します。

次のビデオでは、Web SDKデータ用のスキーマ、データセット、ストリーミングソースコネクタの作成をサポートします。

>[!VIDEO](https://video.tv.adobe.com/v/35395?quality=12&learn=on)

## Adobe LaunchでのSDKのインストール

Log in to Adobe Launch and install the `AEP Web SDK` extension. SDK のインストールの一環として、拡張機能を設定するよう求めるプロンプトが表示されます。上記で要求した構成IDを入力します。 拡張機能によって、組織の ID が自動的に入力されます。

様々な設定オプションについて詳しくは、[SDK の設定](../fundamentals/configuring-the-sdk.md)を参照してください。

## スキーマに基づくデータ要素の作成

Adobe Launchで、拡張機能をAEP Web SDKに変更し、タイプをXDMオブジェクトに設定して、スキーマを参照するデータ要素を作成します。 これによりスキーマが読み込まれ、データ要素をスキーマの別の部分にマップできます。

![開始時の日付要素](../../assets/edge_data_element.png)

## イベントの送信

拡張機能のインストール後、開始は、AEP Web SDK拡張機能から「sendEvent」アクションをルールに追加してイベントを送信します。 作成したデータ要素をXDMデータとしてイベントに追加してください。 ページが読み込まれるたびに、少なくとも1つのイベントを送信することをお勧めします。

イベントの追跡方法について詳しくは、[イベントのトラッキング](../fundamentals/tracking-events.md)を参照してください。

## 次の手順

データのフローが完了したら、次の操作を実行できます。

- [スキーマの構築](https://docs.adobe.com/content/help/ja-JP/experience-platform/xdm/schema/composition.html)
- [デバッグについて](../fundamentals/debugging.md)
- エクスペリエンスを [パーソナライズする方法を説明します。](../fundamentals/rendering-personalization-content.md)
- 複数のソリューションにデータを送信する方法について説明します。
   - [Adobe Analytics](../solution-specific/analytics/analytics-overview.md)
   - [Adobe Audience Manager](../solution-specific/audience-manager/audience-manager-overview.md)
   - [Adobe Target](../solution-specific/target/target-overview.md)
