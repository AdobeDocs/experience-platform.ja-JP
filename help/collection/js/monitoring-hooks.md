---
title: Adobe Experience Platform Web SDKのモニタリングフック
description: Adobe Experience Platform Web SDKが提供するモニタリングフックを使用して、実装をデバッグし、Web SDK ログを取得する方法を説明します。
exl-id: 56633311-2f89-4024-8524-57d45c7d38f7
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '1244'
ht-degree: 6%

---

# Web SDKのモニタリングフック

Adobe Experience Platform Web SDKには、様々なシステムイベントのモニタリングに使用できるモニタリングフックが含まれています。 これらのツールは、独自のデバッグツールを開発したり、Web SDKのログを取得したりするのに便利です。

Web SDKは、[ デバッグ ](commands/configure/debugenabled.md)を有効にしているかどうかに関係なく、監視機能をトリガーします。

## `onInstanceCreated` {#onInstanceCreated}

このコールバック関数は、新しいWeb SDK インスタンスを正常に作成したときにトリガーされます。 関数パラメーターについて詳しくは、以下のサンプルを参照してください。

```js
onInstanceCreated(data) {
    // data.instanceName
    // data.instance
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|----------|
| `data.instanceName` | 文字列 | Web SDK インスタンスが保存されるグローバル変数の名前。 |
| `data.instance` | 関数 | Web SDK コマンドの呼び出しに使用されるインスタンス関数。 |

## `onInstanceConfigured` {#onInstanceConfigured}

このコールバック関数は、[`configure`](commands/configure/overview.md) コマンドが正常に解決されたときにWeb SDKによってトリガーされます。 関数パラメーターについて詳しくは、以下のサンプルを参照してください。

```js
 onInstanceConfigured(data) {
     // data.instanceName
     // data.config
 }
```

| パラメーター | タイプ | 説明 |
|---------|----------|----------|
| `data.instanceName` | 文字列 | Web SDK インスタンスが保存されるグローバル変数の名前。 |
| `data.config` | オブジェクト | Web SDK インスタンスに使用した設定を含むオブジェクト。 これらは、すべてのデフォルト値が追加された[`configure`](commands/configure/overview.md) コマンドに渡されるオプションです。 |

## `onBeforeCommand` {#onBeforeCommand}

このコールバック関数は、他のコマンドが実行される前にWeb SDKによってトリガーされます。 この関数を使用して、特定のコマンドの設定オプションを取得できます。 関数パラメーターについて詳しくは、以下のサンプルを参照してください。

```js
onBeforeCommand(data) {
    // data.instanceName
    // data.commandName
    // data.options
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|----------|
| `data.instanceName` | 文字列 | Web SDK インスタンスが保存されるグローバル変数の名前。 |
| `data.commandName` | 文字列 | この関数が実行される前のWeb SDK コマンドの名前。 |
| `data.options` | オブジェクト | Web SDK コマンドに渡されるオプションを含むオブジェクト。 |

## `onCommandResolved` {#onCommandResolved}

このコールバック関数は、コマンド プロミスを解決するときにトリガーされます。 この関数を使用して、コマンドのオプションと結果を確認できます。 関数パラメーターについて詳しくは、以下のサンプルを参照してください。

```js
onCommandResolved(data) {
    // data.instanceName
    // data.commandName
    // data.options
    // data.result
},
```

| パラメーター | タイプ | 説明 |
|---------|----------|----------|
| `data.instanceName` | 文字列 | Web SDK インスタンスが保存されるグローバル変数の名前。 |
| `data.commandName` | 文字列 | 実行されたWeb SDK コマンドの名前。 |
| `data.options` | オブジェクト | Web SDK コマンドに渡されるオプションを含むオブジェクト。 |
| `data.result` | オブジェクト | Web SDK コマンドの結果を含むオブジェクト。 |

## `onCommandRejected` {#onCommandRejected}

このコールバック関数は、コマンドプロミスが拒否される前にトリガーされ、コマンドが拒否された理由に関する情報が含まれます。 関数パラメーターについて詳しくは、以下のサンプルを参照してください。

```js
onCommandRejected(data) {
    // data.instanceName
    // data.commandName
    // data.options
    // data.error
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|----------|
| `data.instanceName` | 文字列 | Web SDK インスタンスが保存されるグローバル変数の名前。 |
| `data.commandName` | 文字列 | 実行されたWeb SDK コマンドの名前。 |
| `data.options` | オブジェクト | Web SDK コマンドに渡されるオプションを含むオブジェクト。 |
| `data.error` | オブジェクト | ブラウザーのネットワーク呼び出しから返されたエラーメッセージを含むオブジェクト（ほとんどの場合`fetch`）。コマンドが拒否された理由も含まれます。 |

## `onBeforeNetworkRequest` {#onBeforeNetworkRequest}

このコールバック関数は、ネットワークリクエストが実行される前にトリガーされます。 関数パラメーターについて詳しくは、以下のサンプルを参照してください。

```js
onBeforeNetworkRequest(data) {
    // data.instanceName
    // data.requestId
    // data.url
    // data.payload
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|----------|
| `data.instanceName` | 文字列 | Web SDK インスタンスが保存されるグローバル変数の名前。 |
| `data.requestId` | 文字列 | デバッグを有効にするためにWeb SDKによって生成された`requestId`。 |
| `data.url` | 文字列 | リクエストされたURL。 |
| `data.payload` | オブジェクト | JSON形式に変換され、`POST` メソッドを介してリクエストの本文で送信されるネットワーク要求ペイロード オブジェクト。 |

## `onNetworkResponse` {#onNetworkResponse}

このコールバック関数は、ブラウザーが応答を受信したときにトリガーされます。 関数パラメーターについて詳しくは、以下のサンプルを参照してください。

```js
onNetworkResponse(data) {
    // data.instanceName
    // data.requestId
    // data.url
    // data.payload
    // data.body
    // data.parsedBody
    // data.status
    // data.retriesAttempted
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|----------|
| `data.instanceName` | 文字列 | Web SDK インスタンスが保存されるグローバル変数の名前。 |
| `data.requestId` | 文字列 | デバッグを有効にするためにWeb SDKによって生成された`requestId`。 |
| `data.url` | 文字列 | リクエストされたURL。 |
| `data.payload` | オブジェクト | JSON形式に変換され、`POST` メソッドを介してリクエストの本文で送信されるペイロードオブジェクト。 |
| `data.body` | 文字列 | 文字列形式の応答本文。 |
| `data.parsedBody` | オブジェクト | 解析された応答ボディを含むオブジェクト。 応答本文の解析中にエラーが発生した場合、このパラメーターは未定義です。 |
| `data.status` | 文字列 | 応答コードを整数形式で指定します。 |
| `data.retriesAttempted` | 整数 | リクエストの送信時に試行された再試行回数。 ゼロは、最初の試行でリクエストが成功したことを意味します。 |

## `onNetworkError` {#onNetworkError}

このコールバック関数は、ネットワークリクエストが失敗したときにトリガーされます。 関数パラメーターについて詳しくは、以下のサンプルを参照してください。

```js
onNetworkError(data) {
    // data.instanceName
    // data.requestId
    // data.url
    // data.payload
    // data.error
},
```

| パラメーター | タイプ | 説明 |
|---------|----------|----------|
| `data.instanceName` | 文字列 | Web SDK インスタンスが保存されるグローバル変数の名前。 |
| `data.requestId` | 文字列 | デバッグを有効にするためにWeb SDKによって生成された`requestId`。 |
| `data.url` | 文字列 | リクエストされたURL。 |
| `data.payload` | オブジェクト | JSON形式に変換され、`POST` メソッドを介してリクエストの本文で送信されるペイロードオブジェクト。 |
| `data.error` | オブジェクト | ブラウザーのネットワーク呼び出しから返されたエラーメッセージを含むオブジェクト（ほとんどの場合`fetch`）。コマンドが拒否された理由も含まれます。 |

## `onBeforeLog` {#onBeforeLog}

このコールバック関数は、Web SDKがコンソールに何かを記録する前にトリガーされます。 関数パラメーターについて詳しくは、以下のサンプルを参照してください。

```js
onBeforeLog(data) {
    // data.instanceName
    // data.componentName
    // data.level
    // data.arguments
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|----------|
| `data.instanceName` | 文字列 | Web SDK インスタンスが保存されるグローバル変数の名前。 |
| `data.componentName` | 文字列 | ログメッセージを生成したコンポーネントの名前。 |
| `data.level` | 文字列 | ログレベル。 サポートされているレベル：`log`、`info`、`warn`、`error`。 |
| `data.arguments` | 文字列配列 | ログメッセージの引数。 |


## `onContentRendering` {#onContentRendering}

このコールバック関数は、レンダリングのさまざまな段階で`personalization` コンポーネントによってトリガーされます。 ペイロードは、`status` パラメーターによって異なる場合があります。 関数パラメーターについて詳しくは、以下のサンプルを参照してください。

```js
 onContentRendering(data) {
     // data.instanceName
     // data.componentName
     // data.payload
     // data.status
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|----------|
| `data.instanceName` | 文字列 | Web SDK インスタンスが保存されるグローバル変数の名前。 |
| `data.componentName` | 文字列 | ログメッセージを生成したコンポーネントの名前。 |
| `data.payload` | オブジェクト | JSON形式に変換され、`POST` メソッドを介してリクエストの本文で送信されるペイロードオブジェクト。 |
| `data.status` | 文字列 | `personalization` コンポーネントは、レンダリングのステータスをWeb SDKに通知します。  サポートされている値： <ul><li>`rendering-started`: Web SDKが提案をレンダリングしようとしていることを示します。 Web SDKが決定範囲またはビューのレンダリングを開始する前に、`data` オブジェクトで、`personalization` コンポーネントとスコープ名によってレンダリングされる提案を確認できます。</li><li>`no-offers`：要求されたパラメーターに対してペイロードが受信されなかったことを示します。</li> <li>`rendering-failed`: Web SDKが提案のレンダリングに失敗したことを示します。</li><li>`rendering-succeeded`：決定範囲のレンダリングが完了したことを示します。</li> <li>`rendering-redirect`: Web SDKがリダイレクト提案をレンダリングすることを示します。</li></ul> |

## `onContentHiding` {#onContentHiding}

このコールバック関数は、事前非表示スタイルが適用または削除されたときにトリガーされます。

```js
onContentHiding(data) {
    // data.instanceName
    // data.componentName
    // data.status
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|----------|
| `data.instanceName` | 文字列 | Web SDK インスタンスが保存されるグローバル変数の名前。 |
| `data.componentName` | 文字列 | ログメッセージを生成したコンポーネントの名前。 |
| `data.status` | 文字列 | `personalization` コンポーネントは、レンダリングのステータスをWeb SDKに通知します。 サポートされている値： <ul><li>`hide-containers`</li><li>`show-containers`</ul> |

## NPM パッケージを使用する際の監視フックの指定方法 {#specify-monitoring-npm}

[NPM パッケージ ](install/npm.md)を使用してWeb SDKを使用している場合は、次に示すように、`createInstance`関数でモニタリングフックを指定できます。

```js
var monitor = {
  onBeforeCommand(data) {
    console.log(data);
  },
  ...
};
var alloyLibrary = require("@adobe/alloy");
var alloy = alloyLibrary.createInstance({ name: "alloy", monitors: [monitor] });
alloy("config", { ... });
alloy("sendEvent", { ... });
```

## 例 {#example}

Web SDKは、`__alloyMonitors`というグローバル変数内のオブジェクトの配列を検索します。

すべてのWeb SDK イベントをキャプチャするには、Web SDK コードがページに読み込まれる前に、モニタリングフックを定義する必要があります。 各モニタリング手法は、Web SDK イベントをキャプチャします。

Web SDK コードの読み込み後&#x200B;*以降*&#x200B;の監視フックを定義できますが、ページの読み込み前にトリガーされたフックは&#x200B;*ではなく* キャプチャされます。

監視フックオブジェクトを定義する場合は、特殊なロジックを定義するメソッドのみを定義する必要があります。
例えば、`onContentRendering`だけを気にする場合は、そのメソッドを定義するだけです。 すべてのモニタリングフックを一度に使用する必要はありません。

複数の監視フックオブジェクトを定義できます。 指定されたメソッドを持つすべてのオブジェクトは、対応するイベントがトリガーされたときに呼び出されます。

>[!TIP]
>
>すべての監視フックが実装されたサンプルページを以下に示します。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
  <script>
    window.__alloyMonitors = window.__alloyMonitors || [];
    window.__alloyMonitors.push({
      onInstanceCreated(data) {
        // data.instanceName
        // data.instance
      },
      onInstanceConfigured(data) {
        // data.instanceName
        // data.config
      },
      onBeforeCommand(data) {
        // data.instanceName
        // data.commandName
        // data.options
      },
      // Added in version 2.4.0
      onCommandResolved(data) {
        // data.instanceName
        // data.commandName
        // data.options
        // data.result
      },
      // Added in version 2.4.0
      onCommandRejected(data) {
        // data.instanceName
        // data.commandName
        // data.options
        // data.error
      },
      onBeforeNetworkRequest(data) {
        // data.instanceName
        // data.requestId
        // data.url
        // data.payload
      },
      onNetworkResponse(data) {
        // data.instanceName
        // data.requestId
        // data.url
        // data.payload
        // data.body
        // data.parsedBody
        // data.status
        // data.retriesAttempted
      },
      onNetworkError(data) {
        // data.instanceName
        // data.requestId
        // data.url
        // data.payload
        // data.error
      },
      onBeforeLog(data) {
        // data.instanceName
        // data.componentName
        // data.level
        // data.arguments
      }
      onContentRendering(data) {
        // data.instanceName
        // data.componentName
        // data.payload
        // data.status
      },
      onContentHiding(data) {
        // data.instanceName
        // data.componentName
        // data.status
      }
    });
  </script>
  <script>
      !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
      []).push(o),n[o]=function(){var u=arguments;return new Promise(
      function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
      (window,["alloy"]);
    </script>
    <script src="alloy.js" async></script>
</head>
<body>
  <h1>Monitor Test</h1>
</body>
</html>
```
