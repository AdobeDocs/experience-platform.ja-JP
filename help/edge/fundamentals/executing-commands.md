---
title: Adobe Experience Platform Web SDK コマンドの実行
description: Experience Platform Web SDK コマンドの実行方法について説明します
keywords: コマンドの実行；commandName;Promise;getLibraryInfo；応答オブジェクト；同意；
exl-id: dda98b3e-3e37-48ac-afd7-d8852b785b83
source-git-commit: ca3ee230d510dfb9de400b6f573a612ec33c8f7a
workflow-type: tm+mt
source-wordcount: '416'
ht-degree: 71%

---

# コマンドの実行


Web ページにベースコードが実装されたら、SDK を使用してコマンドの実行を開始できます。コマンドを実行する前に、外部ファイル (`alloy.js`) がサーバーから読み込まれるのを待つ必要はありません。 SDK が読み込みを完了していない場合、コマンドはキューに追加され、できるだけ早く SDK によって処理されます。

コマンドは、次の構文を使用して実行されます。

```javascript
alloy("commandName", options);
```

`commandName` は SDK に何を実行するかを伝え、`options` はコマンドに渡すパラメーターとデータです。使用可能なオプションはコマンドによって異なります。各コマンドの詳細については、ドキュメントを参照してください。

## Promise に関する注記事項

[Promise](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/Promise) は、SDK が Web ページ上のコードと通信する際の基本です。Promise は一般的なプログラミング構造であり、この SDK や JavaScript に固有のものではありません。ある Promise は、promise の作成時に不明な値のプロキシとしての役割を果たします。値が判明すると、その値を使用して promise が「解決」されます。ハンドラー関数を promise に関連付けると、promise が解決されたときや、promise の解決中にエラーが発生したときに通知を受け取ることができます。Promise の詳細については、[このチュートリアル](https://javascript.info/promise-basics)や、Web 上の他のリソースをお読みください。

## 成功または失敗の処理 {#handling-success-or-failure}

コマンドが実行されるたびに、promise が返されます。promise は、コマンドの最終的な完了を表します。次の例では、`then` および `catch` メソッドを使用して、コマンドが成功したか失敗したかを判断できます。

```javascript
alloy("commandName", options)
  .then(function(result) {
    // The command succeeded.
    // "value" is whatever the command returned
  })
  .catch(function(error) {
    // The command failed.
    // "error" is an error object with additional information
  });
```

コマンドが正常に実行されたかどうかを把握することが重要でない場合は、`then` 呼び出しを削除してもかまいません。

```javascript
alloy("commandName", options)
  .catch(function(error) {
    // The command failed.
    // "error" is an error object with additional information
  });
```

同様に、コマンドが失敗したかどうかを把握することが重要でない場合は、`catch` 呼び出しを削除してもかまいません。

```javascript
alloy("commandName", options)
  .then(function(result) {
    // The command succeeded.
    // "value" will be whatever the command returned
  });
```

### 応答オブジェクト

コマンドから返されるすべての promise は `result` オブジェクトを使用して解決されます。 結果オブジェクトには、コマンドとユーザーの同意に応じたデータが含まれます。 例えば、次のコマンドでは、ライブラリ情報が results オブジェクトのプロパティとして渡されます。

```js
alloy("getLibraryInfo")
  .then(function(result) {
    console.log(result.libraryInfo.version);
  });
```

### 同意

ユーザーが特定の目的に対して同意しない場合、プロミスは解決されます。ただし、応答オブジェクトには、ユーザーが同意した内容のコンテキストで提供できる情報のみが含まれます。
