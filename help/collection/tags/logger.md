---
title: logger
description: デバッグ時にブラウザーコンソールに情報を出力します。
source-git-commit: 6f8bdfd09023ea48962a40a9539afe017bc108cc
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 1%

---

# `logger`

`_satellite.logger` オブジェクトには、[&#x200B; デバッグ &#x200B;](../use-cases/debugging.md) が有効な場合にブラウザーコンソールに診断メッセージまたは情報メッセージを出力できるメソッドが含まれています。 デバッグが有効になっていない場合、すべての `logger` メソッド呼び出しは何も行いません。

これらの手法を使用すると、開発者、テクニカルマーケターおよびテスターは、タグプロパティ内のトリガーとタイミングを簡単に確認できます。 これらのコンソールメッセージはデバッグが有効な場合にのみ表示されるので、サイト訪問者のブラウザーコンソールに影響を与えることなく、`logger` のメッセージをデプロイメントから実稼動環境に残すことができます。

```ts
readonly _satellite.logger: {
  debug(...args: unknown[]): void;
  log(...args: unknown[]): void;
  info(...args: unknown[]): void;
  warn(...args: unknown[]): void;
  error(...args: unknown[]): void;
}
```

>[!TIP]
>
>`_satellite.notify()` で使用されていたタグオブジェクトの以前のバージョン。 `notify()` 関数は非推奨（廃止予定）となり、`_satellite.logger` に置き換わりました。

## メソッド

デバッグが有効な場合、すべての `_satellite.logger` メソッドは対応するJavaScript `console.*` メソッドにパスします。 ほとんどの `console` 引数またはオブジェクトは、`_satellite.logger` を使用してサポートされています。

| メソッド | 転送先 | 推奨される使用方法 |
|---|---|---|
| `_satellite.logger.debug()` | `console.debug()` | 詳細診断。一部のブラウザーでは、詳細なログを必要とする場合があります。 |
| `_satellite.logger.log()` | `console.log()` | 一般的なメッセージ。 |
| `_satellite.logger.info()` | `console.info()` | 情報イベントの概要。 |
| `_satellite.logger.warn()` | `console.warn()` | 回復可能な問題。 コンソールエントリは黄色でハイライト表示されます。 |
| `_satellite.logger.error()` | `console.error()` | 失敗。 コンソールエントリは赤くハイライト表示されます。 Adobeでは、スタックに `error` オブジェクトを使用することをお勧めします。 |

```js
// First enable debugging mode
_satellite.setDebug(true);

// Logs a debug message
_satellite.logger.debug('Verbose diagnostic event');

// Logs a generic message
_satellite.logger.log('Example');

// Logs an informational message with mixed arguments
_satellite.logger.info('Rule triggered', 42, { ruleId: 'R123' }, ['a', 'b']);

// Logs a warning message
_satellite.logger.warn('Data element does not contain a value');

// Logs an error message with stack
_satellite.logger.error(new Error('Required extension not found'));
```

## コンソール出力

ライブラリは、すべてのコンソール出力メッセージで次の情報を追加します。

* **🚀**: タグ実装から発生するコンソールメッセージを簡単に検出できます。
* **\[ 生成元\]**: ログの生成元のルール、アクション、拡張子、またはデータ要素名。 実装の外部（ブラウザーコンソール経由など）でロガーメソッドを呼び出す場合は、`[Custom Script]` が使用されます。
* **メッセージ出力**: メソッドを呼び出す際に含まれるメッセージ出力。

>[!NOTE]
>
>ロガーに `%c` プレフィックスが適用されているので、`%s`、`%d`、`🚀 [Origin]` などのブラウザーフォーマットトークンは適用されません。
