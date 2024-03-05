---
title: デバッグメソッド
description: Web SDK でデバッグ機能を切り替える方法について説明します。
keywords: Web SDK のデバッグ；デバッグ；設定；コマンドの設定；debug コマンド；edgeConfigId;setDebug;debugEnabled;debug;
exl-id: 4e893af8-a48e-48dc-9737-4c61b3355f03
source-git-commit: b6e084d2beed58339191b53d0f97b93943154f7c
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---

# デバッグメソッド

デバッグが有効な場合、Web SDK は、実装のデバッグに役立つメッセージをブラウザーコンソールに出力します。 デバッグは、確立したルールやデータ要素に従って SDK がどのように動作するかを理解したい場合に役立ちます。

デバッグはデフォルトで無効になっていますが、4 つの方法でオンに切り替えることができます。 これらのメソッドを任意に組み合わせて使用し、開発ワークフローに最も便利なデバッグを有効または無効にできます。

## 用途 `debugEnabled` （内） `configure` command

を設定します。 `debugEnabled` boolean を true に設定して拡張機能を設定する場合に使用します。 このオプションは、サイトの任意のページを訪問するすべてのユーザーに対してデバッグを有効にするので、通常、開発環境に使用されます。

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "debugEnabled": true
});
```

詳しくは、 [`debugEnabled`](../commands/configure/debugenabled.md) を参照してください。

## 以下を使用します。 `setDebug` command

上記のブール値と同様に、このコマンドを使用すると、ページへのすべての訪問者に対するデバッグが有効になります。

```js
alloy("setDebug", {"enabled": true});
```

詳しくは、 [`setDebug`](../commands/setdebug.md) コマンドを使用して、詳細を確認できます。

## クエリー文字列パラメーターの設定

クエリ文字列を追加することで、デバッグを有効にすることができます `?alloy_debug=true` を URL の末尾に追加します。 以下に例を示します。

`http://example.com/?alloy_debug=true`

この方法は、ローカルマシンにのみ適用され、すべてのユーザーに対してデバッグを有効にすることなく、実稼動用 Web サイトをデバッグできます。 この方法でデバッグを有効にしても、閲覧セッションの残りの間、または無効にするまで、有効のままになります。

## Adobe Experience Platform Debugger

このAdobe Experience Platform Debuggerは、Web ページを調べ、Experience Cloud製品の実装のデバッグを支援する強力なツールです。 AEP Web SDK セクションの「設定」タブからデバッグを有効にすることができます。

![デバッガーを有効にする](../assets/enable-debugging.png)

詳しくは、 [Adobe Experience Platform Debuggerの概要](/help/debugger/home.md) を参照してください。
