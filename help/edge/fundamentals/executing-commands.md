---
title: コマンドの実行
seo-title: Adobe Experience Platform Web SDKコマンドの実行
description: エクスペリエンスプラットフォームWeb SDKコマンドの実行方法を説明します。
seo-description: エクスペリエンスプラットフォームWeb SDKコマンドの実行方法を説明します。
translation-type: tm+mt
source-git-commit: 0cc6e233646134be073d20e2acd1702d345ff35f

---


# （ベータ）コマンドの実行

>[!IMPORTANT]
>
>Adobe Experience Platform Web SDKは現在ベータ版で、すべてのユーザーが利用できるわけではありません。 ドキュメントと機能は変更される場合があります。

Webページにベースコードが実装されたら、SDKを使用してコマンドの実行を開始できます。 コマンドを実行する前に、外部ファイル\(`alloy.js`\)がサーバから読み込まれるのを待つ必要はありません。 SDKが読み込みを完了していない場合、コマンドはSDKによって可能な限り早くキューに格納され、処理されます。

コマンドは、次の構文を使用して実行されます。

```javascript
alloy("commandName", options);
```

コマ `commandName` ンドに渡すパラメーターとデ `options` ータは、SDKに対して何を行うかを指示します。 使用可能なオプションはコマンドによって異なるので、各コマンドの詳細については、ドキュメントを参照してください。

## 約束の手紙

[約束は](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) 、SDKがWebページ上のコードとどのように通信するかに関する基本原則です。 約束は、共通のプログラミング構造であり、このSDKやJavaScriptに固有のものではありません。 約束は、約束の作成時に不明な値の代理として機能します。 値が認識されると、その値でプロミスが「解決」されます。 ハンドラー関数を約束に関連付けると、約束が解決されたときや、約束の解決中にエラーが発生したときに通知を受け取ることができます。 公約の詳細については、このチュートリ [アルや](https://javascript.info/promise-basics) 、Web上の他のリソースをお読みください。

## 成功または失敗の処理

コマンドが実行されるたびに、プロミスが返されます。 promiseは、コマンドの最終的な完了を表します。 次の例では、およびメソッドを使用して、コ `then` マンド `catch` が成功したか失敗したかを判断できます。

```javascript
alloy("commandName", options)
  .then(function(value) {
    // The command succeeded.
    // "value" is whatever the command returned
  })
  .catch(function(error) {
    // The command failed.
    // "error" is an error object with additional information
  })
```

コマンドが正常に実行されたかどうかを知ることが重要でない場合は、呼び出しを削除で `then` きます。

```javascript
alloy("commandName", options)
  .catch(function(error) {
    // The command failed.
    // "error" is an error object with additional information
  })
```

同様に、コマンドが失敗したタイミングを把握することが重要でない場合は、呼び出しを削除で `catch` きます。

```javascript
alloy("commandName", options)
  .then(function(value) {
    // The command succeeded.
    // "value" will be whatever the command returned
  })
```

## エラーの抑制

約束が拒否され、呼び出しを追加しなかった場合、エラーは「バブルアップ」され、Adobe Experience Platform Web SDKでログが有効になっているかどうかに関係なく、ブラウザーの開発者コンソールに記録されます。 `catch` これが問題となる場合は、SDKの設定で説明されているよ `suppressErrors` うに、設定オ `true` プションをに設 [定できます](configuring-the-sdk.md)。
