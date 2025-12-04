---
title: setDebug （）
description: ブラウザーコンソールを使用したサイトでのデバッグを有効にします。
source-git-commit: 6f8bdfd09023ea48962a40a9539afe017bc108cc
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 0%

---

# `setDebug()`

`_satellite.setDebug()` メソッドを使用すると、サイトで [&#x200B; デバッグ &#x200B;](../use-cases/debugging.md) を有効または無効にできます。 このメソッドは、ブラウザーコンソールでローカルに実行することを目的としています。Adobeでは、タグルール内でこのメソッドを呼び出さないよう推奨しています。

```ts
_satellite.setDebug(enabled: boolean): void
```

* **`_satellite.setDebug(true);`**: [&#x200B; デバッグ &#x200B;](../use-cases/debugging.md) を有効にします。 タグライブラリで生成されたメッセージ（[`_satellite.logger`](logger.md) を含む）は、ブラウザーコンソールに表示されます。
* **`_satellite.setDebug(false);`**: デバッグを無効にします。 タグライブラリで生成されたコンソールメッセージが表示されない。

```js
// This console message does not appear because debug mode is not yet enabled
_satellite.logger.log('Example message');

// Enable debug mode
_satellite.setDebug(true);

// This console message appears in the console
_satellite.logger.info('Debug messages are now visible');

// Disable debug mode
_satellite.setDebug(false);

// This console message does not appear because debug mode is no longer enabled
_satellite.logger.warn('Incorrect rule trigger detected');
```

>[!NOTE]
>
>JavaScriptのネイティブ `console.log()` で記述されたメッセージは、開発ツールを開いているユーザーには常に表示されます。 Adobeでは、可能な限り [`_satellite.logger`](logger.md) を使用することをお勧めします。

## デバッグモードの永続性

このメソッドを呼び出す場合、デバッグモードでは次のスコープを使用します。

* **ブラウザーセッション**：ページ読み込み全体でデバッグモードが保持されます。 ブラウザーセッションを終了するか、明示的に無効にするまで、有効のままです。
* **接触チャネルスコープ**：デバッグモードは、同じサイト（ドメイン + ポート + プロトコル）に対してのみ保持されます。
* **ブラウザープロファイルごと**：デバッグモードはクロスプロファイルではありません。

他の形式の [&#x200B; デバッグ &#x200B;](../use-cases/debugging.md) は、ブラウザーコンソールメソッドを使用するデバッグモードに影響を与え、上書きする場合があります。
