---
title: setDebug
description: Web SDK デバッグ設定を有効または無効にします。
exl-id: 9faac147-b7c7-4732-8454-35102970dae0
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# `setDebug`

`setDebug` コマンドを使用すると、Web SDK のデバッグモードに入ったり、デバッグモードを終了したりできます。 これは、[ デバッグ ](../use-cases/debugging.md) に使用できる複数のオプションの 1 つです。 Adobeでは、開発環境または実稼動環境内のローカルマシン上でのみデバッグモードを有効にすることを推奨しています。

## Web SDK タグ拡張機能を使用したデバッグの設定

タグ拡張機能には、UI 内のデバッグオプションを切り替える機能はありません。 JavaScript構文を使用してカスタムコードエディターを使用するか、サイトでブラウザーコンソール内にJavaScript コードを入力できます。

## Web SDK JavaScript ライブラリを使用したデバッグの設定

設定済みの Web SDK インスタンスを呼び出す際に、`setDebug` コマンドを実行します。 設定オブジェクトの唯一のオプションは `enabled` です。これは、デバッグモードが有効かどうかを判断するブール値です。

```js
alloy("setDebug", {"enabled": true});
```
