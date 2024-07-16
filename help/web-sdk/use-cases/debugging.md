---
title: デバッグメソッド
description: Web SDK でデバッグ機能を切り替える方法について説明します。
keywords: web sdk のデバッグ；デバッグ；設定；configure コマンド；debug コマンド；edgeConfigId;setDebug;debugEnabled；デバッグ；
exl-id: 4e893af8-a48e-48dc-9737-4c61b3355f03
source-git-commit: b6e084d2beed58339191b53d0f97b93943154f7c
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---

# デバッグメソッド

デバッグが有効な場合、Web SDK はブラウザーコンソールにメッセージを出力します。これは、実装のデバッグに役立ちます。 デバッグは、確立したルールとデータ要素に従って SDK がどのように動作するかを理解する際に役立ちます。

デバッグはデフォルトで無効になっていますが、4 つの異なる方法でオンに切り替えることができます。 これらのメソッドを任意に組み合わせて、開発ワークフローにとって最も便利なデバッグを有効または無効にすることができます。

## `configure` コマンドで `debugEnabled` を使用する

拡張機能を設定する際は、`debugEnabled` ブール値を true に設定します。 このオプションは、サイトの任意のページにアクセスするすべてのユーザーがデバッグを利用できるので、通常は開発環境で使用されます。

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "debugEnabled": true
});
```

詳細は、[`debugEnabled`](../commands/configure/debugenabled.md) を参照してください。

## `setDebug` コマンドの使用

上記のブール値と同様に、このコマンドは、ページへのすべての訪問者にわたってデバッグを有効にします。

```js
alloy("setDebug", {"enabled": true});
```

詳細については、[`setDebug`](../commands/setdebug.md) コマンドを参照してください。

## クエリ文字列パラメーターの設定

デバッグを有効にするには、任意の URL の末尾にクエリ文字列 `?alloy_debug=true` を追加します。 例：

`http://example.com/?alloy_debug=true`

この方法はローカルマシンにのみ適用され、すべてのユーザーがデバッグを有効にせずに実稼動 web サイトをデバッグできます。 この方法でのデバッグの有効化は、ブラウジングセッションの残りの部分、または無効化するまで有効のままです。

## Adobe Experience Platform Debuggerの使用

Adobe Experience Platform Debuggerは web ページを調べる強力なツールで、Experience Cloud製品の実装のデバッグに役立ちます。 デバッグは、「AEP Web SDK」セクションの「設定」タブで有効にすることができます。

![ デバッガーの有効化 ](../assets/enable-debugging.png)

詳しくは、[Adobe Experience Platform Debuggerの概要 ](/help/debugger/home.md) を参照してください。
