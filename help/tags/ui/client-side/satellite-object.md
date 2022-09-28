---
title: サテライトオブジェクトのリファレンス
description: クライアントサイドの _satellite オブジェクトと、それを使用してタグで実行できる様々な関数について説明します。
exl-id: f8b31c23-409b-471e-bbbc-b8f24d254761
source-git-commit: 8ded2aed32dffa4f0923fedac7baf798e68a9ec9
workflow-type: tm+mt
source-wordcount: '1290'
ht-degree: 95%

---

# サテライトオブジェクトのリファレンス

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

このドキュメントは、クライアントサイドの `_satellite` オブジェクトと、それを使用して実行できる様々な機能のリファレンスとして機能します。

## `track`

**コード**

```javascript
_satellite.track(identifier: string [, detail: *] )
```

**例**

```javascript
_satellite.track('contact_submit', { name: 'John Doe' });
```

`track` は、コアタグ拡張機能から指定された識別子を使用して設定されたダイレクトコールイベントタイプを使用するすべてのルールを実行します。上記の例では、ダイレクト型イベントタイプを使用する（`contact_submit` 識別子が設定されている）すべてのルールをトリガーします。関連情報を含むオプションのオブジェクトも渡されます。詳細オブジェクトにアクセスするには、条件やアクションのテキストフィールド内で「`%event.detail%`」と入力するか、カスタムコード条件またはアクションのコードエディター内で「`event.detail`」と入力します。

## `getVar`

**コード**

```javascript
_satellite.getVar(name: string) => *
```

**例**

```javascript
var product = _satellite.getVar('product');
```

この例では、名前が一致するデータ要素が存在する場合、そのデータ要素の値が返されます。一致するデータ要素が存在しない場合は、一致する名前のカスタム変数が、以前に `_satellite.setVar()` を使用して設定されているかどうかを確認します。一致するカスタム変数が見つかると、その値が返されます。

>[!NOTE]
>
>パーセント (`%`) 構文を使用して、タグ実装内の多くのフォームフィールドの変数を参照し、を呼び出す必要がなくなります。 `_satellite.getVar()`. 例えば、 `%product%` は、製品データ要素またはカスタム変数の値にアクセスします。

イベントがルールをトリガーする際に、ルールの対応する `event` ～に向かう `_satellite.getVar()` 次のようにします。

```javascript
// event refers to the calling rule's event
var rule = _satellite.getVar('return event rule', event);
```

## `setVar`

**コード**

```javascript
_satellite.setVar(name: string, value: *)
```

**例**

```javascript
_satellite.setVar('product', 'Circuit Pro');
```

`setVar()` は、指定した名前と値でカスタム変数を設定します。変数の値は、`_satellite.getVar()` を使用して後でアクセスできます。

オプションとして、キーが変数名、値がそれぞれの変数値であるオブジェクトを渡すことにより、複数の変数を一度に設定できます。

```javascript
_satellite.setVar({ 'product': 'Circuit Pro', 'category': 'hobby' });
```

## `getVisitorId`

**コード**

```javascript
_satellite.getVisitorId() => Object
```

**例**

```javascript
var visitorIdInstance = _satellite.getVisitorId();
```

プロパティに [!DNL Adobe Experience Cloud ID] 拡張機能がインストールされている場合、このメソッドは訪問者 ID インスタンスを返します。詳しくは、 [Experience Cloud ID サービスのドキュメント](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja) を参照してください。

## `logger`

**コード**

```javascript
_satellite.logger.log(message: string)
```

```javascript
_satellite.logger.info(message: string)
```

```javascript
_satellite.logger.warn(message: string)
```

```javascript
_satellite.logger.error(message: string)
```

**例**

```javascript
_satellite.logger.error('No product ID found.');
```

`logger` オブジェクトを使用すると、メッセージをブラウザーコンソールに記録できます。このメッセージは、ユーザーが（`_satellite.setDebug(true)` を呼び出す、または適切なブラウザー拡張機能を使用して）タグのデバッグを有効にした場合にのみ表示されます。

### ログ取得廃止の警告

```javascript
_satellite.logger.deprecation(message: string)
```

**例**

```javascript
_satellite.logger.deprecation('This method is no longer supported, please use [new example] instead.');
```

これにより、ブラウザーコンソールに警告が記録されます。ユーザーがタグのデバッグを有効にしているかどうかに関係なく、メッセージが表示されます。

## `cookie` {#cookie}

`_satellite.cookie` には、cookie の読み取りと書き込みをおこなう関数が含まれます。これは、サードパーティのライブラリ js-cookie の公開済みコピーです。このライブラリのより高度な使用方法の詳細については、[js-cookie のドキュメント](https://www.npmjs.com/package/js-cookie#basic-usage)を参照してください。

### cookie の設定 {#cookie-set}

cookie を設定するには、`_satellite.cookie.set()` を使用します。

**コード**

```javascript
_satellite.cookie.set(name: string, value: string[, attributes: Object])
```

>[!NOTE]
>
>古い [`setCookie`](#setCookie) メソッドを使う cookie の設定方法では、この関数呼び出しの 3 番目（オプション）の引数は、cookie の有効期間（TTL）を日数で示す整数でした。 この新しいメソッドでは、「attributes」オブジェクトを 3 番目の引数として受け取ります。 新しいメソッドを使用して cookie の TTL を設定するには、attributes オブジェクトに `expires` プロパティを指定し、目的の値に設定する必要があります。以下に例を示します。

**例**

次の関数呼び出しは、1 週間で期限切れになる cookie を書き込みます。

```javascript
_satellite.cookie.set('product', 'Circuit Pro', { expires: 7 });
```

### cookie の取得 {#cookie-get}

cookie を取得するには、`_satellite.cookie.get()` を使用します。

**コード**

```javascript
_satellite.cookie.get(name: string) => string
```

**例**

次の関数呼び出しは、以前に設定された cookie を読み取ります。

```javascript
var product = _satellite.cookie.get('product');
```

### cookie の削除 {#cookie-remove}

cookie を削除するには、`_satellite.cookie.remove()` を使用します。

**コード**

```javascript
_satellite.cookie.remove(name: string)
```

**例**

次の関数呼び出しは、以前に設定された cookie を削除します。

```javascript
_satellite.cookie.remove('product');
```

## `buildInfo`

**コード**

```javascript
_satellite.buildInfo
```

このオブジェクトには、現在のタグのランタイムライブラリのビルドに関する情報が含まれています。オブジェクトには、次のプロパティが含まれています。

### `turbineVersion`

これにより、現在のライブラリ内で使用されている [Turbine](https://www.npmjs.com/package/@adobe/reactor-turbine) バージョンが提供されます。

### `turbineBuildDate`

コンテナ内で使用されている [Turbine](https://www.npmjs.com/package/@adobe/reactor-turbine) のバージョンが作成された日付（ISO 8601 形式）。

### `buildDate`

現在のライブラリが構築された日付（ISO 8601 形式）。

この例では、オブジェクトの値を示します。

```javascript
{
  turbineVersion: "14.0.0",
  turbineBuildDate: "2016-07-01T18:10:34Z",
  buildDate: "2016-03-30T16:27:10Z"
}
```

## `environment`

このオブジェクトには、現在のタグランタイムライブラリがデプロイされている環境に関する情報が含まれています。

**コード**

```javascript
_satellite.environment
```

オブジェクトには、次のプロパティが含まれています。

```javascript
{
  id: "ENbe322acb4fc64dfdb603254ffe98b5d3",
  stage: "development"
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | 環境の ID。 |
| `stage` | このライブラリが構築された環境。使用できる値は、`development`、`staging`、`production` です。 |

## `notify`

>[!NOTE]
>
>このメソッドは廃止されました。代わりに `_satellite.logger.log()` を使用してください。

**コード**

```javascript
_satellite.notify(message: string[, level: number])
```

**例**

```javascript
_satellite.notify('Hello world!');
```

`notify` は、ブラウザーコンソールにメッセージを記録します。このメッセージは、ユーザーが（`_satellite.setDebug(true)` を呼び出すか、適切なブラウザ拡張機能を使用して）タグのデバッグを有効にした場合にのみ表示されます。

ログに記録されるメッセージのスタイル設定とフィルタリングに影響を与えるオプションのログレベルを渡すことができます。サポートされているレベルは次のとおりです。

3 - 情報メッセージ。

4 - 警告メッセージ。

5 - エラーメッセージ。

ログレベルを指定しない場合や、その他のレベルの値を渡す場合、メッセージは通常のメッセージとして記録されます。

## `setCookie` {#setCookie}

>[!IMPORTANT]
>
>このメソッドは廃止されました。代わりに [`_satellite.cookie.set()`](#cookie-set) を使用してください。

**コード**

```javascript
_satellite.setCookie(name: string, value: string, days: number)
```

**例**

```javascript
_satellite.setCookie('product', 'Circuit Pro', 3);
```

これにより、ユーザーのブラウザーに cookie が設定されます。cookie は指定した日数の間持続します。

## `readCookie`

>[!IMPORTANT]
>
>このメソッドは廃止されました。代わりに [`_satellite.cookie.get()`](#cookie-get) を使用してください。

**コード**

```javascript
_satellite.readCookie(name: string) => string
```

**例**

```javascript
var product = _satellite.readCookie('product');
```

これにより、ユーザーのブラウザーから cookie を読み取ります。

## `removeCookie`

>[!NOTE]
>
>このメソッドは廃止されました。代わりに [`_satellite.cookie.remove()`](#cookie-remove) を使用してください。

**コード**

```javascript
_satellite.removeCookie(name: string)
```

**例**

```javascript
_satellite.removeCookie('product');
```

これにより、ユーザーのブラウザーから cookie を削除します。

## デバッグ関数

次の関数は、実稼動コードからはアクセスできません。これらはデバッグ目的のみを目的としており、必要に応じて時間の経過と共に変化します。

### `container`

**コード**

```javascript
_satellite._container
```

**例**

>[!IMPORTANT]
>
>この関数は、実稼動コードからはアクセスできません。デバッグ目的のみを目的としており、必要に応じて時間の経過と共に変化します。

### `monitor`

**コード**

```javascript
_satellite._monitors
```

**例**

>[!IMPORTANT]
>
>この関数は、実稼動コードからはアクセスできません。デバッグ目的のみを目的としており、必要に応じて時間の経過と共に変化します。

**サンプル**

タグライブラリを実行する Web ページで、コードのスニペットを HTML に追加します。通常、コードは、タグライブラリを読み込む `<script>` 要素の前の `<head>` 要素に挿入されます。これにより、タグライブラリで発生する最も古いシステムイベントを監視できます。次に例を示します。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
  <script>
    window._satellite = window._satellite || {};
    window._satellite._monitors = window._satellite._monitors || [];
    window._satellite._monitors.push({
      ruleTriggered: function (event) {
        console.log(
          'rule triggered',
          event.rule
        );
      },
      ruleCompleted: function (event) {
        console.log(
          'rule completed',
          event.rule
        );
      },
      ruleConditionFailed: function (event) {
        console.log(
          'rule condition failed',
          event.rule,
          event.condition
        );
      }
    });
  </script>
  <script src="//assets.adobedtm.com/launch-EN5bfa516febde4b22b3e7c6f96f6b439f.min.js"
          async></script>
</head>
<body>
  <h1>Click me!</h1>
</body>
</html>
```

最初のスクリプト要素では、タグライブラリがまだ読み込まれていないので、最初の `_satellite` オブジェクトが作成され、`_satellite._monitors` の配列が初期化されます。次に、スクリプトがそ配列にモニターオブジェクトを追加します。モニターオブジェクトは、後でタグライブラリによって呼び出される次のメソッドを指定できます。

### `ruleTriggered`

この関数は、イベントがルールをトリガーした後、ルールの条件とアクションが処理される前に呼び出されます。`ruleTriggered` に渡されたイベントオブジェクトには、トリガーされたルールに関する情報が含まれます。

### `ruleCompleted`

この関数は、ルールが完全に処理された後に呼び出されます。 （つまり、イベントが発生し、すべての条件をクリアし、すべてのアクションが実行された後）`ruleCompleted` に渡された event オブジェクトには、完了したルールに関する情報が含まれます。

### `ruleConditionFailed`

この関数は、ルールがトリガーされ、その条件の 1 つが失敗した後に呼び出されます。`ruleConditionFailed` に渡されたイベントオブジェクトには、トリガーされたルールと、失敗した条件に関する情報が含まれます。

`ruleTriggered` が呼び出されると、その直後に `ruleCompleted` または `ruleConditionFailed` が呼び出されます。

>[!NOTE]
>
>1 つのモニターで、3 つのメソッドすべて（`ruleTriggered`、`ruleCompleted`、および `ruleConditionFailed`）を指定する必要はありません。Adobe Experience Platform のタグは、モニターで提供されている、あらゆるサポートメソッドで機能します。

### モニターのテスト

上記の例では、モニターで 3 つのメソッドをすべて指定しています。呼び出されると、モニターは関連情報を記録します。これをテストするには、タグライブラリで 2 つのルールを設定します。

1. クリックイベントと、ブラウザー情報（ブラウザーが [!DNL Chrome] の場合にのみ渡されます）を含むルール。
1. クリックイベントと、ブラウザー情報（ブラウザーが [!DNL Firefox] の場合にのみ渡されます）を含むルール。

[!DNL Chrome] でページを開く場合は、ブラウザーコンソールを開いて、ページを選択すると、コンソールに以下が表示されます。

![](../../images/debug.png)

これらのハンドラーには、必要に応じて、さらにフックや情報が追加される場合があります。
