---
title: Web 拡張機能の共有モジュール
description: Adobe Experience Platform で Web 拡張機能の共有ライブラリモジュールを定義する方法について説明します。
exl-id: ec013a39-966c-43f3-bc36-31198990a17e
source-git-commit: 44e2b8241a8c348d155df3061d398c4fa43adcea
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 100%

---

# Web 拡張機能の共有モジュール

共有モジュールは、他の拡張機能と通信するためのメカニズムです。例えば、拡張機能 A はデータを非同期的に読み込み、[promise](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/Promise) を介して拡張機能 B で使用できるようにします。

JavaScript の実装では、すべての共有モジュールが `turbine` 自由変数によって提供される [`getSharedModule`](../turbine.md#shared) メソッドを使用してインスタンス化されます。

共有モジュールは、他の拡張機能内から呼び出されることがない場合でもタグライブラリに含まれます。ライブラリのサイズを不必要に大きくしないようにするには、共有モジュールとして公開する内容に注意する必要があります。

共有モジュールにはビューコンポーネントはありません。

独自のタグ拡張機能を開発する場合、提供したい共有モジュールを定義できます。例えば、ユーザー ID を非同期で読み込み、プロミスを介して他の拡張機能とユーザー ID を共有するモジュールを作成できます。

```javascript
var userIdPromise = new Promise(/* load user ID, then resolve promise */);
module.exports = userIdPromise;
```

[拡張機能マニフェスト](../manifest.md) で、この共有モジュールの名前を指定する必要があります。`user-id-promise` という名前を付けた場合、次のように指定することで、別の拡張機能がこの共有モジュールにアクセスできます。

```javascript
var userIdPromise = turbine.getSharedModule('user-extension', 'user-id-promise');
```

通常 CommonJS モジュールからエクスポートできるもの（関数、オブジェクト、文字列、数値、ブール値など）を共有モジュールにすることができます。
