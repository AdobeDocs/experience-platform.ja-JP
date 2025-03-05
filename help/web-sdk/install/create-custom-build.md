---
title: NPM パッケージを使用したカスタム web SDK ビルドの作成
description: 必要なモジュールのみを含んだカスタム web SDK ビルドを作成します。
source-git-commit: 0f77023b07102ac2bc812034afacb1522ef209e5
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 6%

---


# カスタム Web SDK ビルドの作成

Experience Platform web SDK ライブラリには、パーソナライズ機能、ID、リンクトラッキングなど、様々な機能に対応する複数のモジュールが含まれています。 ユースケースによっては、ライブラリ全体ではなく、特定の機能のみが必要になる場合があります。 カスタム web SDK ビルドを作成すると、必要なモジュールのみを選択でき、ライブラリのサイズを減らし、パフォーマンスを向上させることができます。

## ユースケース {#use-case}

カスタム web SDK ビルドを作成すると、ライブラリのサイズを減らし、パフォーマンスを向上させることができます。 次に例を示します。

### Media Analytics の削除 {#media-analytics-removal}

Web サイトにメディアコンテンツがない場合は、[!DNL Media Analytics] モジュールと [!DNL Streaming Media] モジュールをビルドから除外できます。 これにより、web SDKのビルドサイズを最大 50% 削減し、読み込み速度を向上させることができます。

### Personalizationの削除 {#personalization}

ユーザー指標のみを収集し、パーソナライゼーションにAdobe TargetやJourney Optimizerを使用する予定がない場合は、[!DNL Personalization] モジュールを除外できます。 これにより、ライブラリのサイズが小さくなりますが、必要な指標を収集できるようになります。

## 前提条件 {#prerequisites}

カスタム Web SDK ビルドを作成するには、Web SDK NPM パッケージが必要です。 [Node.js](https://nodejs.org/en/download/package-manager/all) がマシンにインストールされていることを確認します。 詳しくは、[NPM パッケージを使用して web SDKをインストールする ](npm.md) 方法に関するドキュメントを参照してください。

## コンポーネントと依存関係 {#components-dependencies}

カスタム Web SDK ビルドを作成する前に、使用する Web SDK コンポーネントおよびコマンドを定義します。 一部のコマンドは、ビルドに含まれている特定のモジュールによって異なります。

次の表に、Web SDK モジュールとモジュールに含まれるコマンドとの関係を示します。

| モジュールの依存関係 | 設定パラメーター | コマンド | サイズカテゴリ |
|---------|----------|---------|---------|
| アクティビティコレクター | [`clickCollectionEnabled`](../commands/configure/clickcollectionenabled.md) | なし | メディア |
| オーディエンス | なし | なし | 小 |
| コンテキスト | [`context`](../commands/configure/context.md) | なし | 小 |
| ルールエンジン | `personalizationStorageEnabled` | | <ul><li>`evaluateRulesets`</li><li>[`subscribeRulesetItems`](../commands/subscriberulesetitems.md)</li></ul> | メディア |
| イベントの結合 | なし | `createEventMergeId` | 小 |
| Media Analytics Bridge | なし | [`getMediaAnalyticsTracker`](../commands/getmediaanalyticstracker.md) | 大 |
| パーソナライズ機能 | <ul><li>[`prehidingStyle`](../commands/configure/prehidingstyle.md)</li><li>[`targetMigrationEnabled`](../commands/configure/targetmigrationenabled.md)</li><li>[`autoCollectPropositionInteractions`](../commands/configure/autocollectpropositioninteractions.md)</li></ul> | なし | 大 |
| 同意 | [`defaultConsent`](../commands/configure/defaultconsent.md) | [`setConsent`](../commands/setconsent.md) | 小 |
| ストリーミングメディア | [`streamingMedia`](../commands/configure/streamingmedia.md) | <ul><li>[`createMediaSession`](../commands/createmediasession.md)</li><li>[`sendMediaEvent`](../commands/sendmediaevent.md)</li></ul> | 大 |

## NPM パッケージを使用したカスタム web SDK ビルドの作成 {#create-custom-build}

1. ターミナルを開き、`npx @adobe/alloy` を実行します。 カスタムビルドに含める web SDK コンポーネントを選択するように求められます。

   ![ カスタムビルドモジュールの選択を示すターミナルの画像 ](../assets/custom-build/npx.png)

   矢印キーを使用して、モジュールリスト内を上下に移動します。

   * **スペース** を押して、選択したモジュールを有効または無効にします。
   * `A` を押して、すべてのモジュールを有効または無効にします。
   * `I` を押して、選択を反転します。
   * `Enter` を押して選択を確認し、次の手順に進みます。

1. カスタムビルドに含めるモジュールを選択したら、カスタム Web SDK ライブラリビルドの縮小バージョンまたは縮小されていないバージョンを保存するかどうかを選択できます。 目的のオプションを選択し、`Enter` キーを押します。

   ![ カスタムビルドの縮小選択を表示しているターミナルの画像 ](../assets/custom-build/minify.png)

1. 次に、ローカルマシン上のどこにビルドを保存するかを尋ねられます。 `Enter` キーを押して、事前に選択された場所を確認するか、新しい場所を入力します。

   ![ カスタムビルドの保存オプションを表示しているターミナルの画像 ](../assets/custom-build/save.png)

1. 場所を確認すると、カスタムビルドが生成されて保存されます。

   ![ カスタムビルドが保存された場所を示すターミナルの画像 ](../assets/custom-build/saved.png)

