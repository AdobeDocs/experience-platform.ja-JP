---
title: setVar （）
description: getVar （）を使用して後で取得できる値を設定します。
source-git-commit: 54c32803136bf37a13bb9ca14b1d1c7b09a2041c
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# `setVar()`

`_satellite.setVar()` メソッドを使用すると、後で [`_satellite.getVar()`](getvar.md) で参照できる 1 つ以上のカスタム名と値のペアを設定できます。

```ts
// Set a single name/value pair
_satellite.setVar(name: string, value: unknown): void

// Set multiple name/value pairs at once
_satellite.setVar(vars: { [name: string]: unknown }): void;
```

>[!IMPORTANT]
>
>`getVar()` メソッドでは、データ要素と `setVar()` で設定された値の両方を取得できますが、これら 2 つのエンティティタイプは別々です。 `setVar()` を使用してタグ UI のデータ要素と同じ名前の値を設定しても、上書きされません。

## 永続性とスコープ

`setVar()` 値はページメモリ内にのみ存在します。

* **現在のページのみ**：値はページがアンロードされるまで保持されます。 単一ページアプリケーションでは、完全なリロードまたは上書き/クリアが行われるまで保持されます。
* **ブラウザーストレージなし**:Cookie、ローカルストレージ、セッションストレージには何も書き込まれません。

## `setVar()` を使用して設定された値の参照

`getVar()` を使用して、カスタムコードの値を取得できます。

```js
// Set a custom variable using setVar()
_satellite.setVar('Ad location','Banner advertisement');

// Returns the string 'Banner advertisement'
_satellite.getVar('Ad location');
```

また、データ要素表記をサポートするフィールドのタグ UI で、これらの変数を参照することもできます。

```text
%Ad location%
```

>[!NOTE]
>
>`setVar()` を使用して設定された値がデータ要素と同じ名前を使用しており、データ要素表記でその名前を参照している場合は、データ要素が優先されます。

## 例

```js
// Set a single name/value pair
_satellite.setVar('product', 'Circuit Pro');

// Set multiple name/value pairs at once (same as calling setVar() three times)
_satellite.setVar({ 'title': 'Blinding Light', 'category': 'Game', 'genre': 'Tower defense' });

// Retrieve each value
_satellite.getVar('title'); // Blinding Light
_satellite.getVar('category'); // Game
_satellite.getVar('genre'); // Tower defense
```

>[!NOTE]
>
>このメソッドを使用して変数名を設定する場合は、ピリオド（`.`）を使用しないでください。 `getVar()` メソッドでは、`setVar()` を使用して設定されたピリオドを含む変数は認識されません。 ただし、タグ UI`getVar()` 定義する際にピリオドを使用するデータ要素は _認識_ されません。
