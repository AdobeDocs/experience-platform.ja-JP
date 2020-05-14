---
title: デバッグ
seo-title: Adobe Experience Platform Web SDK のデバッグ
description: Experience Platform Web SDK のデバッグを切り替える方法について説明します
seo-description: Experience Platform Web SDK のデバッグを切り替える方法について説明します
translation-type: tm+mt
source-git-commit: e9fb726ddb84d7a08afb8c0f083a643025b0f903
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 100%

---


# デバッグ

デバッグが有効になっている場合、SDK は、実装のデバッグや SDK の動作の理解に役立つメッセージをブラウザーコンソールに出力します。また、デバッグをおこなうと、設定したスキーマに対して収集されるデータの、サーバーサイド同期検証がおこなわれます。

デバッグはデフォルトで無効になっていますが、次の 3 つの方法で切り替えることができます。

* `configure` コマンド
* `debug` コマンド
* クエリ文字列パラメーター

## configure コマンドを使用したデバッグの切り替え

`configure` コマンドを使用して SDK を設定する場合は、`debugEnabled` オプションを `true` に設定してデバッグを有効にしてください。

```javascript
alloy("configure", {
  "configId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId":"ADB3LETTERSANDNUMBERS@AdobeOrg",
  "debugEnabled": true
});
```

>[!Hint]
>自分のブラウザーだけでなく、Web ページのすべてのユーザーに対するデバッグを可能にします。

## debug コマンドを使用したデバッグの切り替え

次のように、別の `debug` コマンドでデバッグを切り替えます。

```javascript
alloy("debug", {
  "enabled": true
});
```

Web ページのコードを変更しない場合や、Web サイトのすべてのユーザーに対してメッセージのログを作成しない場合は、ブラウザーの JavaScript コンソール内でいつでも `debug` コマンドを実行できるので、この機能が特に便利です。

## クエリ文字列パラメーターを使用したデバッグの切り替え

次のように、`alloy_debug` クエリ文字列パラメーターを `true` または `false` に設定して、デバッグを切り替えることができます。

```HTTP
http://example.com/?alloy_debug=true
```

`debug` コマンドと同様に、Web ページのコードを変更しない場合や、Web サイトのすべてのユーザーに対してログメッセージを作成しない場合は、ブラウザーに Web ページを読み込む際にクエリ文字列パラメーターを設定できるので、この機能が特に便利です。

## 優先度と期間

デバッグを `debug` コマンドまたはクエリ文字列パラメーターで設定すると、`configure` コマンドで設定された `debug` オプションが上書きされます。この 2 つの場合、セッション中もデバッグは切り替えられたままになります。つまり、debug コマンドまたはクエリ文字列パラメーターを使用してデバッグを有効にした場合、次のいずれかが実行されるまで、デバッグは有効なままになります。

* セッションの終了
* `debug` コマンドを実行します
* クエリ文字列パラメーターを再度設定する
