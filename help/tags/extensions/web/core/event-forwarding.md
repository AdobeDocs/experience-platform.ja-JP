---
title: コアイベント転送拡張機能の概要
description: Adobe Experience Platform のコアイベント拡張機能について説明します。
exl-id: b5ee4ccf-6fa5-4472-be04-782930f07e20
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '1724'
ht-degree: 100%

---

# コアイベント転送拡張機能の概要

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

コアイベント転送拡張機能は、Adobe Experience Platform でのイベント転送のデフォルトのイベント、条件、データタイプを提供します。

このリファレンスは、この拡張機能を使用してルールを作成するときに使用できるオプションに関する情報です。

## Core 拡張機能の条件のタイプ

ここでは、Core 拡張機能で使用できる条件のタイプについて説明します。これらの条件タイプは、標準ロジックと例外ロジックのどちらででも使用できます。

### カスタムコード

イベントの条件として使用する必要があるカスタムコードを指定します。組み込みコードエディターを使用してカスタムコードを入力します。Adobe Experience Platform でのイベント転送は、ES6 をサポートしています。

1. 「**[!UICONTROL エディターを開く]**」を選択します。
1. カスタムコードを入力します。
1. 「**[!UICONTROL 保存]**」を選択します。

カスタムコード内のデータ要素の値にアクセスするには、`getDataElementValue` メソッドを使用します。例えば、`productName` という名前のデータ要素の値を取得するには、次のように記述します。

```javascript
getDataElementValue('productName') 
```

#### ruleStash オブジェクト

カスタムコードでは、`ruleStash` オブジェクトも使用できます。

```javascript
arc.ruleStash: Object<string, *>`
```

```javascript
logger.log(context.arc.ruleStash);
```

`ruleStash` は、アクションモジュールからすべての結果を収集するオブジェクトです。

各拡張機能には独自の名前空間があります。例えば、拡張機能の名前が `send-beacon` である場合、`send-beacon` アクションの結果はすべて `ruleStash['send-beacon']` 名前空間に保存されます。

名前空間は拡張機能ごとに一意で、初めは `undefined` 値が含まれています。

名前空間は、各アクションから返された結果で上書きされます。名前空間では、特別な処理は実行されません。例えば、`generate-fullname` と `generate-fulladdress` という 2 つのアクションを含む `transform` 拡張機能がある場合、これら 2 つのアクションを 1 つのルールに追加します。

`generate-fullname` アクションの結果が `Firstname Lastname` の場合、アクションが完了すると、ルールが次のように表示されます。

```js
{
  transform: 'Firstname Lastname`
}
```

`generate-address` アクションの結果が `3900 Adobe Way` の場合、アクションが完了すると、ルールが次のように表示されます。

```js
{
  transform: '3900 Adobe Way`
}
```

ルール内に `Firstname Lastname` が存在していないことに注意してください。これは、`generate-address` アクションによって、この部分が住所で上書きされたためです。

`ruleStash` の `transform` 名前空間内に両方のアクションの結果を格納したい場合は、次の例のようなアクションモジュールを記述します。

```javascript
module.exports = (context) => {
  let transformRuleStash = context.arc.ruleStash.transform;
  if (!transformRuleStash) {
    transformRuleStash = {};
  }
  transformRuleStash.fullName = 'Firstname Lastname';
  return transformRuleStash;
}
```

このアクションを初めて実行する場合、`ruleStash` は `undefined` となり、空のオブジェクトで初期化されます。次回アクションが実行されると、以前に呼び出されたときの `ruleStash` がアクションによって返されます。オブジェクトを `ruleStash` として使用すると、他のアクションによって以前設定されたデータが拡張機能から失われることなく、新しいデータを追加できます。

この場合は、常に完全な拡張機能ルールを返すように注意する必要があります。5 などの値のみを返す場合、ルールは次のようになります。

```js
{
  transform: 5
}
```

### 値の比較 {#value-comparison}

2 つの値を比較して、この条件が true を返すかどうかを判断します。

複数の条件を持つルールがある場合、この条件で true が返されても、他の条件が false と評価されるか、いずれかの例外が true と評価されて、ルールが実行されない可能性があります。

1. 値を指定します。
1. 演算子を選択します。詳細については、次の値比較演算子のリストを参照してください。
1. 比較用に別の値を指定します。

次の値比較演算子を使用できます。

**Equal：**&#x200B;厳密でない比較を使用して 2 つの値が等しい場合（JavaScript では == 演算子）、true を返します。値には任意のタイプを指定できます。値フィールドに _true_、_false_、_null_&#x200B;または _undefined_ などの単語を入力すると、その単語は文字列として比較され、同等の JavaScript には変換されません。

**Does Not Equal：**&#x200B;厳密でない比較で 2 つの値が等しくない場合（JavaScript では= 演算子）、条件は true を返します。値には任意のタイプを指定できます。値フィールドに _true_、_false_、_null_&#x200B;または _undefined_ などの単語を入力すると、その単語は文字列として比較され、同等の JavaScript には変換されません。

**Contains：**&#x200B;最初の値に 2 番目の値が含まれている場合、条件は true を返します。数値は文字列に変換されます。数値または文字列以外の値を指定すると、条件は false を返します。

**Does Not Contain：**&#x200B;最初の値に 2 番目の値が含まれていない場合、条件は true を返します。数値は文字列に変換されます。数字または文字列以外の値を指定すると、条件は true を返します。

**Starts With：**&#x200B;最初の値が 2 番目の値で開始する場合、条件は true を返します。数値は文字列に変換されます。数値または文字列以外の値を指定すると、条件は false を返します。

**Does Not Start With：**&#x200B;最初の値が 2 番目の値で始まらない場合、条件は true を返します。数値は文字列に変換されます。数値または文字列以外の値を指定すると、条件は true を返します。

**Ends With：**&#x200B;最初の値が 2 番目の値で終わる場合、条件は true を返します。数値は文字列に変換されます。数値または文字列以外の値を指定すると、条件は false を返します。

**Does Not End With：**&#x200B;最初の値が 2 番目の値で終わらない場合、条件は true を返します。数値は文字列に変換されます。数値または文字列以外の値を指定すると、条件は true を返します。

**Matches Regex：**&#x200B;最初の値が正規表現と一致する場合、条件は true を返します。数値は文字列に変換されます。数値または文字列以外の値を指定すると、条件は false を返します。

**Does Not Match Regex：**&#x200B;最初の値が正規表現と一致しない場合、条件は true を返します。数値は文字列に変換されます。数値または文字列以外の値を指定すると、条件は true を返します。

**Is Less Than：**&#x200B;最初の値が 2 番目の値より小さい場合、条件は true を返します。数字を表す文字列は数値に変換されます。数値または変換可能な文字列以外の値を指定すると、条件は false を返します。

**Is Less Than Or Equal To：**&#x200B;最初の値が 2 番目の値以下の場合、条件は true を返します。数字を表す文字列は数値に変換されます。数値または変換可能な文字列以外の値を指定すると、条件は false を返します。

**Is Greater Than：**&#x200B;最初の値が 2 番目の値以上の場合、条件は true を返します。数字を表す文字列は数値に変換されます。数値または変換可能な文字列以外の値を指定すると、条件は false を返します。

**Is Greater Than Or Equal To：**&#x200B;最初の値が 2 番目の値以上の場合、条件は true を返します。数字を表す文字列は数値に変換されます。数値または変換可能な文字列以外の値を指定すると、条件は false を返します。

**Is True：**&#x200B;値が true のブール値の場合、条件は true を返します。他のタイプの場合、指定した値はブール値に変換されません。値が true のブール値以外の値を指定すると、条件は false を返します。

**Is Truthy：**&#x200B;ブール値に変換した後、値が true の場合、条件は true を返します。Truthy な値の例については、[MDN の Truthy に関するドキュメント](https://developer.mozilla.org/ja-JP/docs/Glossary/Truthy)を参照してください。

**Is False：**&#x200B;値が false のブール値の場合、条件は true を返します。他のタイプの場合、指定した値はブール値に変換されません。値が false のブール値以外の値を指定すると、条件は false を返します。

**Is Falsy：**&#x200B;ブール値に変換した後、値が false の場合は、条件は true を返します。Falsy な値の例については、[MDN の Falsy に関するドキュメント](https://developer.mozilla.org/ja-JP/docs/Glossary/Falsy)を参照してください。



## Core 拡張機能のアクションタイプ

ここでは、Core 拡張機能で使用できるアクションタイプについて説明します。

### カスタムコード

イベントのトリガー後に実行して条件を評価するコードを提供します。Adobe Experience Platform でのイベント転送は、ES6 をサポートしています。

1. アクションコードに名前を付けます。
1. 「**[!UICONTROL エディターを開く]**」を選択します。
1. コードを編集して、「**[!UICONTROL 保存]**」を選択します。

カスタムコード内のデータ要素の値にアクセスするには、`getDataElementValue` メソッドを使用します。例えば、`productName` という名前のデータ要素の値を取得するには、次のように記述します。

```javascript
getDataElementValue('productName') 
```

イベント転送アクションが順番に実行されます。また、1 つのアクション内のカスタムコードで、後続のアクションに使用できる値を返すこともできます。返される値は、そのアクション内のコード、または外部ソースに対する呼び出しの応答の本文から取得できます。Core 拡張機能が使用される 1 つのルール内で以前に実行したアクションのデータを参照するには、`Path` タイプのデータ要素を作成し、次のパスを使用して、Core 拡張機能内のカスタムコードで定義された `productCategory` という変数の値を参照します。

```javascript
arc.ruleStash.[Extension-Name].[key-as-defined-by-action] 

arc.ruleStash.core.productCategory  
```

## Core 拡張機能データ要素のタイプ

データ要素のタイプは、拡張機能によって決まります。タイプはいくつでも作成することができます。

以降の節では、Core 拡張機能で使用できるデータ要素のタイプについて説明します。その他の拡張機能は、他のタイプのデータ要素を使用します。

### カスタムコード

カスタム JavaScript を UI に入力するには、「**[!UICONTROL エディターを開く]**」を選択してエディターウィンドウにコードを挿入します。

データ要素の値として使用する値を示すために、エディターウィンドウで return ステートメントを記述する必要があります。return ステートメントを含めない場合、または値 `null` または `undefined` が返される場合、データ要素のデフォルト値には `null` または `undefined` が反映されます。

カスタムコード内のデータ要素の値にアクセスするには、`getDataElementValue` メソッドを使用します。例えば、`productName` という名前のデータ要素の値を取得するには、次のように記述します。

```javascript
getDataElementValue('productName') 
```

**例：**

```javascript
return getDataElementValue('section').concat(getDataElementValue('pName')); 
```

#### Path

Adobe Experience Platform Edge ネットワークに送信されるイベントのキーと値のペアへのパスは、パスデータ要素タイプを使用して参照できます。

イベントのオブジェクト全体を参照するには、パスとして「`arc`」を入力します。`arc` は「Adobe Resource Context」を表し、Adobe Experience Platform Edge ネットワークに送信されるイベントの最上位パスです。

例えば、クライアントから Edge ネットワークへの `interact` 呼び出しには、ブラウザーコンソールから確認できる次のリクエストが含まれます。

```javascript
"events": [ 
        { 
             "xdm": { 
                    "page": { 
                            "btnHover": false, 
                            "pageName": "We Travel Home Page", 
                            "siteSection": "Landing Page" 
                     }] 
```

`pageName` を参照するパスを入力するには、パスフィールドに次のように入力します。

```javascript
arc.event.xdm.page.pageName 
```

>[!NOTE]
>
>クライアントからの `interact` 呼び出しには `events` が含まれていますが、イベント転送の場合は `event` が必要です。これは、イベント転送では、クライアントで表示される複数のイベントのバッチとしてではなく、各イベントが個別に調査されるからです。
