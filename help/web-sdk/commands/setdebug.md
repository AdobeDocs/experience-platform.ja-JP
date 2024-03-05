---
title: setDebug
description: Web SDK のデバッグ設定を有効または無効にします。
source-git-commit: b6e084d2beed58339191b53d0f97b93943154f7c
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# `setDebug`

The `setDebug` コマンドを使用すると、Web SDK でデバッグモードを開始または終了できます。 これは、次のオプションの 1 つです。 [デバッグ](../use-cases/debugging.md). Adobeでは、開発環境内、または実稼動環境内のローカルマシン上でのみデバッグモードを有効にすることをお勧めします。

## Web SDK タグ拡張機能を使用したデバッグの設定

タグ拡張機能には、UI 内のデバッグオプションを切り替える機能はありません。 サイト上で JavaScript 構文を使用してカスタムコードエディターを使用するか、またはブラウザーコンソール内に JavaScript コードを入力できます。

## Web SDK JavaScript ライブラリを使用したデバッグの設定

を実行します。 `setDebug` コマンドを使用して、Web SDK の設定済みインスタンスを呼び出す際に呼び出すことができます。 設定オブジェクトの唯一のオプションは、 `enabled`：デバッグモードが有効かどうかを指定するブール値です。

```js
alloy("setDebug", {"enabled": true});
```
