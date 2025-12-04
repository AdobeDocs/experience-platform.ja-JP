---
title: setDebug
description: Web SDKのデバッグ設定を有効または無効にします。
exl-id: 9faac147-b7c7-4732-8454-35102970dae0
source-git-commit: 6f8bdfd09023ea48962a40a9539afe017bc108cc
workflow-type: tm+mt
source-wordcount: '121'
ht-degree: 0%

---

# `setDebug`

`setDebug` コマンドを使用すると、web SDKでデバッグモードに入ったり、デバッグモードを終了したりできます。 これは、[&#x200B; デバッグ &#x200B;](../../use-cases/debugging.md) に使用できる複数のオプションの 1 つです。 Adobeでは、開発環境または実稼動環境内のローカルマシン上でのみデバッグモードを有効にすることを推奨しています。

設定済みの Web SDK インスタンスを呼び出す際に、`setDebug` コマンドを実行します。 設定オブジェクトの唯一のオプションは `enabled` です。これは、デバッグモードが有効かどうかを判断するブール値です。

```js
alloy("setDebug", {"enabled": true});
```

## Web SDK タグ拡張機能を使用したデバッグの設定

タグライブラリが実装されているページのブラウザーコンソール内で [`_satellite.setDebug()`](/help/collection/tags/setdebug.md) を呼び出します。 Web SDK タグ拡張機能には、タグ UI 内でデバッグオプションを切り替える機能はありません。
