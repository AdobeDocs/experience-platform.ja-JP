---
title: Adobe Analytics 拡張機能の共有モジュール
description: Adobe Experience Platform の Adobe Analytics タグ拡張機能で提供される共有ライブラリモジュールについて説明します。
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 95%

---

# Adobe Analytics 拡張機能の共有モジュール

>[!NOTE]
>
>Adobe Experience Platform Launchは、Adobe Experience Platformのデータ収集テクノロジーのスイートとしてリブランドされました。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

[Adobe Analytics 拡張機能](./overview.md)では、エクスペリエンスアプリケーションに統合できる 2 つの異なる[共有モジュール](../../../extension-dev/web/shared.md)が提供されます。これらのモジュールについては、以下の節で説明します。

## [!DNL get-tracker]

Adobe Analytics がビーコンを送信する前に、トラッカーオブジェクトを初期化する必要があります。[AppMeasurement](https://experienceleague.adobe.com/docs/analytics/implementation/js/overview.html?lang=ja) を読み込み、トラッカーオブジェクトを作成すると、初期化プロセスが開始します。

トラッカーオブジェクトが完全に初期化されたら、次のように `get-tracker` 共有モジュールを使用してアクセスできます。

```js
var getTracker = turbine.getSharedModule('adobe-analytics', 'get-tracker');

getTracker().then(function(tracker) {
  // Use tracker in some way
});
```

### Adobe Analytics がインストールされていることを確認する

Adobe Analytics がインストールされていない、または拡張機能と同じタグライブラリに含まれていない可能性があります。このため、コードでそれを確認して、適切に処理することを強くお勧めします。以下の JavaScript は実装例です。

```js
var getTracker = turbine.getSharedModule('adobe-analytics', 'get-tracker');

if (getTracker) {
  getTracker().then(function(tracker) {
    // Use tracker in some way
  });
} else {
  turbine.logger.error("The Adobe Analytics extension is required for Extension XYZ to function properly.");
}
```

`getTracker` が `undefined` の場合、Adobe Analytics 拡張機能はタグライブラリに存在しません。ログに記録されたメッセージをカスタマイズして、Adobe Analytics がインストールされていない場合に失われる可能性がある機能を正確に示すことができます。


## [!DNL augment-tracker]

トラッカーオブジェクト初期化したら、プロセスの次の手順は強化です。この手順では、Adobe Analytics 拡張機能の設定から変数が適用される前、またはビーコンが送信される前に、拡張機能でトラッカーに必要なものをすべて追加することができます。

また、拡張機能では、サーバーからのデータや JavaScript の取得など、独自の非同期タスクを実行している間、トラッカーの初期化プロセスを一時停止することができます。

以下のように指定することで、`augment-tracker` モジュールを実装できます。

```js
var augmentTracker = turbine.getSharedModule('adobe-analytics', 'augment-tracker');

augmentTracker(function(tracker) {
  // Augment tracker in some way
});
```

`augmentTracker()` に渡された関数は、トラッカー初期化プロセスの拡張段階に達するとすぐに呼び出されます。

トラッカーを拡張する前に、拡張機能で非同期タスクを完了する必要がある場合は、関数から次のようにプロミスを返すことができます。

```js
var Promise = require('@adobe/reactor-promise');
var augmentTracker = turbine.getSharedModule('adobe-analytics', 'augment-tracker');

augmentTracker(function(tracker) {
  return new Promise(function(resolve) {
    // Augment the tracker object, then call resolve()
  });
});
```

プロミスを返すことで、拡張機能は Adobe Analytics に対して、プロミスが解決されるまでトラッカーの初期化プロセスを一時停止するように指示します。

>[!WARNING]
>
>トラッカーの初期化プロセスを一時停止する場合は、ビーコンの送信が遅れ、その結果データが収集されない可能性があるので注意が必要です（例えば、ビーコンの送信前にユーザーがページを離れた場合など）。
