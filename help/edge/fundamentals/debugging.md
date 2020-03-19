---
title: デバッグ
seo-title: Adobe Experience Platform Web SDKのデバッグ
description: エクスペリエンスプラットフォームWeb SDKのデバッグを切り替える方法を説明します。
seo-description: エクスペリエンスプラットフォームWeb SDKのデバッグを切り替える方法を説明します。
translation-type: tm+mt
source-git-commit: 0cc6e233646134be073d20e2acd1702d345ff35f

---


# （ベータ）デバッグ

>[!IMPORTANT]
>
>Adobe Experience Platform Web SDKは現在ベータ版で、すべてのユーザーが利用できるわけではありません。 ドキュメントと機能は変更される場合があります。

デバッグが有効な場合、SDKは、実装のデバッグやSDKの動作の理解に役立つメッセージをブラウザーコンソールに出力します。 また、デバッグを行うと、サーバー側で、設定したスキーマに対して収集されるデータの同期検証が行われます。

デバッグはデフォルトで無効になっていますが、次の3つの方法で切り替えることができます。

* `configure` command
* `debug` command
* クエリ文字列パラメータ

## 「設定」コマンドによるデバッグの切り替え

コマンドを使用してSDKを設定する `configure` 場合は、オプションをに設定してデバッグを有 `debugEnabled` 効にしてくださ `true`い。

```javascript
alloy("configure", {
  "configId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId":"ADB3LETTERSANDNUMBERS@AdobeOrg",
  "debugEnabled": true
});
```

>[!Hint]
>これにより、個人のブラウザーだけでなく、Webページのすべてのユーザーに対するデバッグが可能になります。

## デバッグコマンドを使用したデバッグの切り替え

次の別のコマンドでデバッグを切 `debug` り替えます。

```javascript
alloy("debug", {
  "enabled": true
});
```

Webページのコードを変更しない場合や、Webサイトのすべてのユーザーに対してメッセージのログを作成しない場合は、ブラウザーのJavaScriptコンソール内でいつでもコマンドを実行できるので、この機能が特に役立ちます。 `debug`

## クエリ文字列パラメーターを使用したデバッグの切り替え

クエリ文字列パラメーターを次のよ `alloy_debug` うに設定して、デバッグを切 `true` り替える `false` ことができます。

```HTTP
http://example.com/?alloy_debug=true
```

コマンドと同様 `debug` に、Webページのコードを変更しない場合や、Webサイトのすべてのユーザに対してログメッセージを生成したくない場合は、ブラウザにWebページを読み込む際にクエリ文字列パラメータを設定できるので、これは特に便利です。

## 優先度と期間

デバッグをコマンドまたはクエリ文 `debug` 字列パラメータで設定すると、コマンドで設定され `debug` たオプションが上書きさ `configure` れます。 この2つの場合、セッション中もデバッグは切り替えられたままになります。 つまり、debugコマンドまたはクエリ文字列パラメーターを使用してデバッグを有効にした場合、次のいずれかが実行されるまで、デバッグは有効なままです。

* セッションの終了
* コマンドを実行し `debug` ます
* クエリ文字列パラメータを再度設定する
