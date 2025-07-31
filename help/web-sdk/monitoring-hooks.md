---
title: Adobe Experience Platform Web SDKのフックの監視
description: Adobe Experience Platform Web SDKが提供するモニタリングフックを使用して、実装をデバッグし、Web SDK ログを取得する方法について説明します。
exl-id: 56633311-2f89-4024-8524-57d45c7d38f7
source-git-commit: 35429ec2dffacb9c0f2c60b608561988ea487606
workflow-type: tm+mt
source-wordcount: '1244'
ht-degree: 6%

---

# Web SDKのフックの監視

Adobe Experience Platform Web SDKには、様々なシステムイベントを監視するために使用できるモニタリングフックが含まれています。 これらのツールは、独自のデバッグツールを開発したり、web SDKのログを取得したりするのに役立ちます。

[ デバッグ ](commands/configure/debugenabled.md) を有効にしているかどうかに関係なく、Web SDKはモニタリング機能をトリガーします。

## `onInstanceCreated` {#onInstanceCreated}

このコールバック関数は、新しい web SDK インスタンスが正常に作成されたときにトリガーされます。 関数のパラメーターについて詳しくは、以下のサンプルを参照してください。

```js
onInstanceCreated(data) {
    // data.instanceName
    // data.instance
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|----------|
| `data.instanceName` | 文字列 | Web SDK インスタンスが格納されているグローバル変数の名前。 |
| `data.instance` | 関数 | Web SDK コマンドを呼び出すために使用されるインスタンス関数。 |

## `onInstanceConfigured` {#onInstanceConfigured}

このコールバック関数は、[`configure`](commands/configure/overview.md) コマンドが正常に解決されたときに web SDKによってトリガーされます。 関数のパラメーターについて詳しくは、以下のサンプルを参照してください。

```js
 onInstanceConfigured(data) {
     // data.instanceName
     // data.config
 }
```

| パラメーター | タイプ | 説明 |
|---------|----------|----------|
| `data.instanceName` | 文字列 | Web SDK インスタンスが格納されているグローバル変数の名前。 |
| `data.config` | オブジェクト | Web SDK インスタンスに使用した設定を含むオブジェクト。 これらは、すべてのデフォルト値が追加された状態で、[`configure`](commands/configure/overview.md) コマンドに渡されるオプションです。 |

## `onBeforeCommand` {#onBeforeCommand}

このコールバック関数は、他のコマンドが実行される前に web SDKによってトリガーされます。 この関数を使用すると、特定のコマンドの設定オプションを取得できます。 関数のパラメーターについて詳しくは、以下のサンプルを参照してください。

```js
onBeforeCommand(data) {
    // data.instanceName
    // data.commandName
    // data.options
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|----------|
| `data.instanceName` | 文字列 | Web SDK インスタンスが格納されているグローバル変数の名前。 |
| `data.commandName` | 文字列 | この関数を実行する前の Web SDK コマンドの名前です。 |
| `data.options` | オブジェクト | Web SDK コマンドに渡されるオプションを含むオブジェクト。 |

## `onCommandResolved` {#onCommandResolved}

このコールバック関数は、コマンドのプロミスを解決するときにトリガーされます。 この関数を使用して、コマンド オプションと結果を確認できます。 関数のパラメーターについて詳しくは、以下のサンプルを参照してください。

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
| `data.instanceName` | 文字列 | Web SDK インスタンスが格納されているグローバル変数の名前。 |
| `data.commandName` | 文字列 | 実行した Web SDK コマンドの名前。 |
| `data.options` | オブジェクト | Web SDK コマンドに渡されるオプションを含むオブジェクト。 |
| `data.result` | オブジェクト | Web SDK コマンドの結果を含むオブジェクト。 |

## `onCommandRejected` {#onCommandRejected}

このコールバック関数は、コマンドプロミスが拒否される前にトリガーされ、コマンドが拒否された理由に関する情報が含まれます。 関数のパラメーターについて詳しくは、以下のサンプルを参照してください。

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
| `data.instanceName` | 文字列 | Web SDK インスタンスが格納されているグローバル変数の名前。 |
| `data.commandName` | 文字列 | 実行した Web SDK コマンドの名前。 |
| `data.options` | オブジェクト | Web SDK コマンドに渡されるオプションを含むオブジェクト。 |
| `data.error` | オブジェクト | ブラウザーのネットワーク呼び出しから返されたエラーメッセージ（ほとんどの場合は `fetch`）と、コマンドが拒否された理由を含むオブジェクト。 |

## `onBeforeNetworkRequest` {#onBeforeNetworkRequest}

このコールバック関数は、ネットワークリクエストが実行される前にトリガーされます。 関数のパラメーターについて詳しくは、以下のサンプルを参照してください。

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
| `data.instanceName` | 文字列 | Web SDK インスタンスが格納されているグローバル変数の名前。 |
| `data.requestId` | 文字列 | デバッグを有効にするために Web SDKで生成される `requestId`。 |
| `data.url` | 文字列 | リクエストされた URL。 |
| `data.payload` | オブジェクト | `POST` メソッドを介して、JSON 形式に変換され、リクエストの本文で送信されるネットワークリクエストペイロードオブジェクト。 |

## `onNetworkResponse` {#onNetworkResponse}

このコールバック関数は、ブラウザーが応答を受信したときにトリガーされます。 関数のパラメーターについて詳しくは、以下のサンプルを参照してください。

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
| `data.instanceName` | 文字列 | Web SDK インスタンスが格納されているグローバル変数の名前。 |
| `data.requestId` | 文字列 | デバッグを有効にするために Web SDKで生成される `requestId`。 |
| `data.url` | 文字列 | リクエストされた URL。 |
| `data.payload` | オブジェクト | JSON 形式に変換され、`POST` メソッドを介してリクエストの本文で送信されるペイロードオブジェクト。 |
| `data.body` | 文字列 | 文字列形式の応答本文。 |
| `data.parsedBody` | オブジェクト | 解析された応答本文を含むオブジェクト。 応答本文の解析中にエラーが発生した場合、このパラメーターは未定義です。 |
| `data.status` | 文字列 | 整数フォーマットの応答コード。 |
| `data.retriesAttempted` | 整数 | リクエストの送信時に試行された再試行の回数。 ゼロは、リクエストが最初の試行で成功したことを意味します。 |

## `onNetworkError` {#onNetworkError}

このコールバック関数は、ネットワークリクエストが失敗したときにトリガーされます。 関数のパラメーターについて詳しくは、以下のサンプルを参照してください。

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
| `data.instanceName` | 文字列 | Web SDK インスタンスが格納されているグローバル変数の名前。 |
| `data.requestId` | 文字列 | デバッグを有効にするために Web SDKで生成される `requestId`。 |
| `data.url` | 文字列 | リクエストされた URL。 |
| `data.payload` | オブジェクト | JSON 形式に変換され、`POST` メソッドを介してリクエストの本文で送信されるペイロードオブジェクト。 |
| `data.error` | オブジェクト | ブラウザーのネットワーク呼び出しから返されたエラーメッセージ（ほとんどの場合は `fetch`）と、コマンドが拒否された理由を含むオブジェクト。 |

## `onBeforeLog` {#onBeforeLog}

このコールバック関数は、Web SDKがコンソールに何かを記録する前にトリガーされます。 関数のパラメーターについて詳しくは、以下のサンプルを参照してください。

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
| `data.instanceName` | 文字列 | Web SDK インスタンスが格納されているグローバル変数の名前。 |
| `data.componentName` | 文字列 | ログメッセージを生成したコンポーネントの名前。 |
| `data.level` | 文字列 | ログレベル。 サポートされるレベル：`log`、`info`、`warn`、`error`。 |
| `data.arguments` | 文字列配列 | ログメッセージの引数。 |


## `onContentRendering` {#onContentRendering}

このコールバック関数は、レンダリングの様々な段階で、`personalization` コンポーネントによってトリガーされます。 ペイロードは、`status` パラメーターによって異なる場合があります。 関数のパラメーターについて詳しくは、以下のサンプルを参照してください。

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
| `data.instanceName` | 文字列 | Web SDK インスタンスが格納されているグローバル変数の名前。 |
| `data.componentName` | 文字列 | ログメッセージを生成したコンポーネントの名前。 |
| `data.payload` | オブジェクト | JSON 形式に変換され、`POST` メソッドを介してリクエストの本文で送信されるペイロードオブジェクト。 |
| `data.status` | 文字列 | `personalization` コンポーネントは、レンダリングのステータスを Web SDKに通知します。  サポートされている値： <ul><li>`rendering-started`: Web SDKが提案をレンダリングしようとしていることを示します。 Web SDKが決定範囲またはビューのレンダリングを開始する前に、`data` オブジェクトで `personalization` コンポーネントによってレンダリングされようとしている提案と、範囲名を確認できます。</li><li>`no-offers`：リクエストされたパラメーターのペイロードを受信しなかったことを示します。</li> <li>`rendering-failed`:Web SDKが提案のレンダリングに失敗したことを示します。</li><li>`rendering-succeeded`：決定範囲のレンダリングが完了したことを示します。</li> <li>`rendering-redirect`: Web SDKがリダイレクトの提案をレンダリングすることを示します。</li></ul> |

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
| `data.instanceName` | 文字列 | Web SDK インスタンスが格納されているグローバル変数の名前。 |
| `data.componentName` | 文字列 | ログメッセージを生成したコンポーネントの名前。 |
| `data.status` | 文字列 | `personalization` コンポーネントは、レンダリングのステータスを Web SDKに通知します。 サポートされている値： <ul><li>`hide-containers`</li><li>`show-containers`</ul> |

## NPM パッケージの使用時に監視フックを指定する方法 {#specify-monitoring-npm}

[NPM パッケージ ](install/npm.md) を通じて Web SDKを使用している場合は、以下に示すように、`createInstance` 関数でモニタリングフックを指定できます。

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

Web SDKは、`__alloyMonitors` というグローバル変数でオブジェクトの配列を検索します。

すべての Web SDK イベントをキャプチャするには、Web SDK コードがページに読み込まれる前に、モニタリングフックを定義する必要があります。 各モニタリング手法は、Web SDK イベントをキャプチャします。

モニタリングフック *後* を定義しても、Web SDK コードはページに読み込まれますが、ページの読み込み前にトリガーされたフックは *取得されません*。

モニタリングフックオブジェクトを定義する場合は、特別なロジックを定義するメソッドを定義するだけで済みます。
例えば、`onContentRendering` のみを重視する場合は、そのメソッドのみを定義できます。 すべてのモニタリングフックを一度に使用する必要はありません。

複数のモニタリングフックオブジェクトを定義できます。 指定されたメソッドを持つすべてのオブジェクトは、対応するイベントがトリガーされたときに呼び出されます。

>[!TIP]
>
>すべてのモニタリングフックが実装されているページの例を以下に示します。

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
