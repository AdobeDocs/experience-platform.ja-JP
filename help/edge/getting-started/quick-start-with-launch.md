---
title: Launch のクイックスタート
seo-title: Adobe Experience Platform Web SDK：Launch のクイックスタート
description: Experience Platform Web SDK 拡張機能を使用してデータを収集するためのクイックスタートガイド
seo-description: Experience Platform Web SDK 拡張機能を使用してデータを収集するためのクイックスタートガイド
translation-type: tm+mt
source-git-commit: 2ccb2c17590780f7f1bd5e553164209763ab9e24
workflow-type: tm+mt
source-wordcount: '539'
ht-degree: 31%

---


# ようこそ

このガイドでは、Adobe Experience Platform Web SDKの起動でのセットアップ方法を説明します。 この機能を使用するには、ホワイトリストに登録する必要があります。 待機中のリストに移動する場合は、CSMに連絡してください。

- [ファーストパーティドメイン（CNAME）](https://docs.adobe.com/content/help/ja-JP/core-services/interface/ec-cookies/cookies-first-party.html)が有効になっている。既に Analytics 用 CNAME をお持ちの場合は、その CNAME を使用する必要があります。開発環境でのテストはCNAMEなしでは機能しますが、実稼働環境に移行する前にCNAMEが必要になります
- Adobe Experience Platform Data Platformの権利を付与されます。 プラットフォームを購入していない場合は、Experience Platform Data Services Foundationをご利用いただき、SDKでの利用を制限し、無償で提供します。
- 訪問者 ID サービスの最新バージョンを使用している

## 設定IDの作成

起動時に [エッジ設定ツールを使用して、設定IDを作成できます](../fundamentals/edge-configuration.md) 。 これにより、Edge Networkで様々なソリューションにデータを送信できるようになります。 各オプションの検索方法について詳しくは、「 [Edge Configuration Tool](../fundamentals/edge-configuration.md) 」ページを参照してください。

>[!NOTE]
>
>この機能を使用するには、組織がホワイトリストに登録されている必要があります。 最終的なホワイトリスト登録のために、CSMにリストに登録するようにお問い合わせください。

## スキーマの準備

Experience Platform Edge Networkは、データをXDMとして受け取ります。 XDMは、スキーマを定義できるデータ形式です。 スキーマは、Edge Networkでデータの形式設定方法を定義します。 データを送信するには、スキーマを定義する必要があります。

- [スキーマの作成](../../xdm/tutorials/create-schema-ui.md)
- 作成したスキーマに Adobe Experience Platform Web SDK mixin を追加します。

## Launch での SDK のインストール

Launch にログインし、`AEP Web SDK` 拡張機能をインストールします。SDK のインストールの一環として、拡張機能を設定するよう求めるプロンプトが表示されます。上記で要求した設定 ID を入力します。拡張機能によって、組織の ID が自動的に入力されます。

様々な設定オプションについて詳しくは、[SDK の設定](../fundamentals/configuring-the-sdk.md)を参照してください。

## スキーマに基づくデータ要素の作成

起動時に、拡張機能をAEP Web SDKに変更し、種類をXDMオブジェクトに設定して、スキーマを参照するデータ要素を作成します。 これによりスキーマが読み込まれ、データ要素をスキーマの別の部分にマップできます。

![開始時の日付要素](../../assets/edge_data_element.png)

## イベントの送信

拡張機能のインストール後、開始は、AEP Web SDK拡張機能から「sendEvent」アクションをルールに追加してイベントを送信します。 作成したデータ要素をXDMデータとしてイベントに追加してください。 ページが読み込まれるたびに、少なくとも1つのイベントを送信することをお勧めします。

イベントの追跡方法について詳しくは、[イベントのトラッキング](../fundamentals/tracking-events.md)を参照してください。

## 次の手順

データのフローが完了したら、次の操作を実行できます。

- [スキーマの構築](https://docs.adobe.com/content/help/en/experience-platform/xdm/schema/composition.html)
- [デバッグについて](../fundamentals/debugging.md)
- エクスペリエンスを [パーソナライズする方法を説明します。](../fundamentals/rendering-personalization-content.md)
- 複数のソリューションにデータを送信する方法について説明します。
   - [Adobe Analytics](../solution-specific/analytics/analytics-overview.md)
   - [Adobe Audience Manager](../solution-specific/audience-manager/audience-manager-overview.md)
   - [Adobe Target](../solution-specific/target/target-overview.md)
