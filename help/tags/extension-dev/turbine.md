---
title: turbine 自由変数
description: Adobe Experience Platform タグのランタイムに固有の情報やユーティリティを提供する自由変数である turbine オブジェクトについて説明します。
exl-id: 1664ab2e-8704-4a56-8b6b-acb71534084e
source-git-commit: 814f853d16219021d9151458d93fc5bdc6c860fb
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 89%

---

# turbine 自由変数

>[!NOTE]
>
>Adobe Experience Platform Launchは、Adobe Experience Platformのデータ収集テクノロジーのスイートとしてリブランドされました。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../term-updates.md)を参照してください。

`turbine` オブジェクトは、拡張機能のライブラリモジュールの範囲内の「自由変数」です。Adobe Experience Platform タグのランタイムに固有の情報とユーティリティを提供し、`require()` を使用しなくてもライブラリモジュールでいつでも利用できます。

## `buildInfo`

```js
console.log(turbine.buildInfo.turbineBuildDate);
```

`turbine.buildInfo` は、現在のタグのランタイムライブラリに関するビルド情報を含むオブジェクトです。

```js
{
    turbineVersion: "14.0.0",
    turbineBuildDate: "2016-07-01T18:10:34Z",
    buildDate: "2016-03-30T16:27:10Z"
}
```

| プロパティ | 説明 |
| --- | --- |
| `turbineVersion` | 現在のライブラリ内で使用されている [Turbine](https://www.npmjs.com/package/@adobe/reactor-turbine) バージョン。 |
| `turbineBuildDate` | コンテナ内で使用されている [Turbine](https://www.npmjs.com/package/@adobe/reactor-turbine) のバージョンが作成された日付（ISO 8601 形式）。 |
| `buildDate` | 現在のライブラリが構築された日付（ISO 8601 形式）。 |


## `environment`

```js
console.log(turbine.environment.stage);
```

`turbine.environment` は、ライブラリがデプロイされている環境に関する情報を含むオブジェクトです。

```js
{
    id: "ENbe322acb4fc64dfdb603254ffe98b5d3",
    stage: "development"
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | 環境のID。 |
| `stage` | このライブラリが構築された環境。指定できる値は`development`、`staging`、`production`です。 |


## `debugEnabled`

タグデバッグが現在有効になっているかどうかを示すboolean値。

メッセージをログに記録するだけの場合、これを使用する必要はありません。代わりに、常に `turbine.logger` を使用してメッセージをログに記録し、タグのデバッグが有効になっている場合にのみメッセージがコンソールに出力されるようにします。

### `getDataElementValue`

```js
console.log(turbine.getDataElementValue(dataElementName));
```

データ要素の値が返されます。

### `getExtensionSettings` {#get-extension-settings}

```js
var extensionSettings = turbine.getExtensionSettings();
```

[拡張機能の設定](./configuration.md)ビューから最後に保存された設定オブジェクトが返されます。

返される設定オブジェクト内の値は、データ要素から取得した値である可能性があることに注意してください。このため、データ要素の値が変更されている場合、異なる時間に `getExtensionSettings()` を呼び出すと、違う結果が返されることがあります。最新の値を取得するには、できるだけ長い間待ってから、`getExtensionSettings()` を呼び出してください。

### `getHostedLibFileUrl` {#get-hosted-lib-file}

```js
var loadScript = require('@adobe/reactor-load-script');
loadScript(turbine.getHostedLibFileUrl('AppMeasurement.js')).then(function() {
  // Do something ...
})
```

[hostedLibFiles](./manifest.md) プロパティは、タグのランタイムライブラリとともに様々なファイルをホストするために、拡張機能マニフェスト内で定義できます。このモジュールによって、指定されたライブラリファイルがホストされている URL が返されます。

### `getSharedModule` {#shared}

```js
var mcidInstance = turbine.getSharedModule('adobe-mcid', 'mcid-instance');
```

別の拡張機能から共有されているモジュールを取得します。一致するモジュールが見つからない場合は、`undefined` が返されます。共有モジュールの詳細については、[共有モジュールの実装](./web/shared.md)に関するページを参照してください。

### `logger`

```js
turbine.logger.error('Error!');
```

ログユーティリティは、コンソールにメッセージを記録するために使用します。ユーザーがデバッグを有効にしている場合、メッセージはコンソールのみに表示されます。デバッグを有効にするには、[Adobe Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj?src=propaganda)を使用することをお勧めします。 別の方法として、ユーザーはブラウザー開発者コンソール内で次のコマンド `_satellite.setDebug(true)` を実行できます。ロガーには次のメソッドがあります。

* `logger.log(message: string)`：コンソールにメッセージを記録します。
* `logger.info(message: string)`：コンソールに情報メッセージを記録します。
* `logger.warn(message: string)`：コンソールに警告メッセージを記録します。
* `logger.error(message: string)`：コンソールにエラーメッセージを記録します。
* `logger.debug(message: string)`：デバッグメッセージをコンソールに記録します。（ブラウザーコンソール内で`verbose` ログが有効になっている場合にのみ表示されます）

### `onDebugChanged`

コールバック関数を `turbine.onDebugChanged` に渡すと、タグは、デバッグが切り替えられるたびにコールバックを呼び出します。タグ は、デバッグが有効な場合は true、デバッグが無効な場合は false のブール値をコールバック関数に渡します。

メッセージをログに記録するだけの場合、これを使用する必要はありません。代わりに、常に `turbine.logger` を使ってメッセージをログに記録すると、タグデバッグが有効な場合にメッセージがコンソールにのみ出力されます。

### `propertySettings` {#property-settings}

```js
console.log(turbine.propertySettings.domains);
```

現在のタグのランタイムライブラリのプロパティに対してユーザーが定義する、次の設定を含むオブジェクト：

* `propertySettings.domains: Array<String>`

   プロパティの対象となるドメインの配列です。

* `propertySettings.undefinedVarsReturnEmpty: boolean`

   拡張機能の開発者は、この設定を気に掛ける必要はありません。
