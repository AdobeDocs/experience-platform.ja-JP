---
title: エッジ拡張機能モジュールのコンテキスト
description: context オブジェクトと、エッジプロパティのタグ拡張機能でライブラリモジュールとのやり取りでオブジェクトが果たす役割について説明します。
exl-id: 04e4e369-687e-4b46-9d24-18a97a218555
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '747'
ht-degree: 100%

---

# エッジ拡張機能モジュールのコンテキスト

>[!NOTE]
>
> Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

エッジ拡張機能内のすべてのライブラリモジュールには、実行時に `context` オブジェクトが提供されます。 このドキュメントでは、`context` オブジェクトによって提供されるプロパティと、それらがライブラリモジュールで果たす役割について説明します。

## Adobe リクエストのコンテキスト（arc）

`arc` プロパティは、ルールをトリガーするイベントに関する情報を提供するオブジェクトです。以下の節では、このオブジェクトに含まれる様々なサブプロパティについて説明します。

### [!DNL event]

`event` オブジェクトは、ルールをトリガーしたイベントを表し、次の値を含みます。

```js
logger.log(context.arc.event);
```

| プロパティ | 説明 |
| --- | --- |
| `xdm` | イベントの XDM オブジェクト。 |
| `data` | カスタムデータレイヤー。 |

### [!DNL request]

`request` は Adobe Experience Platform Edge Network からのオブジェクトで、クライアントデバイスからのリクエストと混同されないように、少し変更されています。

```js
logger.log(context.arc.request)
```

`request` オブジェクトには、`body` と `head` の 2 つの最上位プロパティがあります。`body` プロパティには Experience Data Model（XDM）情報が含まれており、「**[!UICONTROL Launch]**」に移動して「**[!UICONTROL エッジトレース]**」タブを選択すると、Adobe Experience Platform Debugger で調べることができます。

### [!DNL ruleStash] {#rulestash}

`ruleStash` は、アクションモジュールからすべての結果を収集するオブジェクトです。

```js
logger.log(context.arc.ruleStash);
```

各拡張機能には独自の名前空間があります。例えば、拡張機能の名前が「`send-beacon` 」である場合、`send-beacon` アクションの結果はすべて `ruleStash['send-beacon']` 名前空間に保存されます。

名前空間は拡張機能ごとに一意で、先頭に「`undefined`」の値が付きます。

名前空間は、各アクションから返された結果で上書きされます。例えば、`transform` 拡張機能に `generate-fullname` と `generate-fulladdress` の 2 つのアクションが含まれているとします。これら 2 つのアクションはルールに追加されます。

`generate-fullname` アクションの結果が `Firstname Lastname` の場合、アクションの完了後、ルールが次のように表示されます。

```js
{
  transform: 'Firstname Lastname'
}
```

`generate-address` アクションの結果が `3900 Adobe Way` の場合、アクションの完了後、ルールが次のように表示されます。

```js
{
  transform: '3900 Adobe Way'
}
```

`generate-address` アクションによって新しい値で上書きされたので、「Firstname Lastname」はルール内に存在しなくなりました。

`ruleStash` で `transform` 名前空間内に両方のアクションの結果を格納する場合は、次の例のようなアクションモジュールを記述します。

```js
module.exports = (context) => {
  let transformRuleStash = context.arc.ruleStash.transform;

  if (!transformRuleStash) {
    transformRuleStash = {};
  }

  transformRuleStash.fullName = 'Firstname Lastname';

  return transformRuleStash;
}
```

このアクションを初めて実行する場合、`ruleStash` は `undefined` で始まり、空のオブジェクトとして初期化されます。次にアクションが実行されると、このアクションが以前呼び出されたときに返された `ruleStash` を受け取ります。オブジェクトを `ruleStash` として使用すると、他のアクションによって以前設定されたデータが拡張機能から失われることなく、新しいデータを追加できます。

>[!NOTE]
>
>この方法を使用する場合は、常に完全な拡張機能ルールを返すように注意してください。値のみを返すと、設定した他のプロパティが上書きされます。

## ユーティリティ

`utils` プロパティは、タグのランタイムに固有のユーティリティを提供するオブジェクトを表します。

### [!DNL logger]

`logger` ユーティリティを使用すると、[Adobe Experience Platform Debugger](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) を使用したデバッグセッション中に表示されるメッセージをログに記録できます。

```js
context.utils.logger.error('Error!');
```

ロガーには次のメソッドがあります。`message` はログに記録するメッセージです。

| メソッド | 説明 |
| --- | --- |
| `log(message)` | コンソールにメッセージを記録します。 |
| `info(message)` | コンソールに情報メッセージを記録します。 |
| `warn(message)` | コンソールに警告メッセージを記録します。 |
| `error(message)` | コンソールにエラーメッセージを記録します。 |
| `debug(message)` | コンソールにデバッグメッセージを記録します。これは、ブラウザーコンソール内で `verbose` ログが有効になっている場合にのみ表示されます。 |

### [!DNL fetch]

このユーティリティにより、[Fetch API](https://developer.mozilla.org/ja-JP/docs/Web/API/Fetch_API) が実装されます。この関数を使用して、サードパーティのエンドポイントにリクエストを送信できます。

```js
context.utils.fetch('http://example.com/movies.json')
  .then(response => response.json())
```

### [!DNL getBuildInfo]

このユーティリティは、現在のタグのランタイムライブラリのビルドに関する情報を含むオブジェクトを返します。

```js
logger.log(context.utils.getBuildInfo().turbineBuildDate);
```

オブジェクトには、次の値が含まれています。

| プロパティ | 説明 |
| --- | --- |
| `turbineVersion` | 現在のライブラリ内で使用されている [Turbine](https://www.npmjs.com/package/@adobe/reactor-turbine-edge) バージョン。 |
| `turbineBuildDate` | コンテナ内で使用されている [Turbine](https://www.npmjs.com/package/@adobe/reactor-turbine-edge) のバージョンが作成された日付（ISO 8601 形式）。 |
| `buildDate` | 現在のライブラリが構築された日付（ISO 8601 形式）。 |
| `environment` | このライブラリが構築された環境。`development`、`staging`、`production.` などの値が使用されます。 |

次の例は、`getBuildInfo` オブジェクトで返される値を示しています。

```js
{
  turbineVersion: "1.0.0",
  turbineBuildDate: "2016-07-01T18:10:34Z",
  buildDate: "2016-03-30T16:27:10Z",
  environment: "development"
}
```

### [!DNL getExtensionSettings]

このユーティリティは、 [拡張機能の設定](../configuration.md)ビューから最後に保存された `settings` オブジェクトを返します。

```js
logger.log(context.utils.getExtensionSettings());
```

### [!DNL getSettings]

このユーティリティは、対応するライブラリモジュールビューから最後に保存された `settings` オブジェクトを返します。

```js
logger.log(context.utils.getSettings());
```

### [!DNL getRule]

このユーティリティは、モジュールをトリガーするルールに関する情報を含むオブジェクトを返します。

```js
logger.log(context.utils.getRule());
```

このオブジェクトには次の値が含まれます。

| プロパティ | 説明 |
| --- | --- |
| `id` | ルール ID。 |
| `name` | ルール名。 |
