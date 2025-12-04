---
title: getVar （）
description: データ要素の値、または setVar （）を使用して設定された値を返します。
source-git-commit: 434d6913ea391b127b4b52c8494730c496bbcfe2
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 2%

---

# `getVar()`

`_satellite.getVar()` メソッドは、[Data 要素 &#x200B;](/help/tags/ui/managing-resources/data-elements.md) の現在の値、または [`_satellite.setVar()`](setvar.md) を使用して設定された値を返します。 データ要素と `setVar()` 値が同じ名前を共有する場合、データ要素が優先されます。 まだ存在しない文字列識別子を呼び出すと、メソッドは `undefined` を返します。 評価は同期されます。

```js
_satellite.getVar(name: string, event?: unknown) => unknown
```

戻り値とタイプは、参照するデータ要素タイプによって異なります。 例えば、文字列変数を取得する `getVar()` を呼び出すと、返されるタイプは `string` になります。 Web SDK変数データ要素を返す `getVar()` を呼び出すと、返されるタイプは、その変数に設定されたすべてのプロパティを含む `object` になります。

```js
// Retrieves the value in the `Example` data element
_satellite.getVar('Example');

// Execute custom logic depending on the numeric value in the `cartTotal` data element
const cartValue = Number(_satellite.getVar('cartTotal'));
if (cartValue > 100) {
  _satellite.logger.info('Purchase larger than $100 detected');
}

// Some data elements like Web SDK variables support deeply nested properties
const pageName = _satellite.getVar('Data layer')?.web?.webPageDetails?.name;
if (!pageName) {
  _satellite.logger.warn('Page name is not set');
}

// Custom code data elements support a second argument
// Works in a Launch custom code block where `event` is provided by the rule engine.
const rule = _satellite.getVar('Return event rule', event);
```

## 使用可能なフィールド

`getVar()` メソッドでは、2 つの引数がサポートされています。 ほとんどのユースケースには、2 番目のオプション引数は含まれていません。

| 名前 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| **`name`** | `string` | ○ | 取得するデータ要素名。 データ要素名では大文字と小文字が区別されます。 |
| **`event`** | `object` | × | トリガールールからのイベントコンテキスト。 [&#x200B; カスタムコード &#x200B;](/help/tags/ui/managing-resources/data-elements.md#custom-code) またはカスタム拡張機能を使用したデータ要素による高度なユースケースでのみ使用されます。 イベント（クリック、フォーム送信、カスタム JavaScript ディスパッチなど）によってルールがトリガーされると、関連するイベントオブジェクトがここに含まれます。 データ要素では、この情報を使用して、クリックされた要素の詳細やカスタムイベントからのプロパティなど、コンテキスト値を返すことができます。 |
