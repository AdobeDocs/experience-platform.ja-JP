---
title: デバッグ
seo-title: Adobe Experience Platform Web SDK のデバッグ
description: Experience Platform Web SDK のデバッグを切り替える方法について説明します
seo-description: Experience Platform Web SDK のデバッグを切り替える方法について説明します
keywords: debugging web sdk;debugging;configure;configure command;debug command;edgeConfigId;setDebug;debugEnabled;debug;
translation-type: tm+mt
source-git-commit: e21374eb51ec1d572f6a4973d33cadf9ae17969b
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 92%

---


# デバッグ

デバッグが有効になっている場合、SDK は、実装のデバッグや SDK の動作の理解に役立つメッセージをブラウザーコンソールに出力します。また、デバッグをおこなうと、設定したスキーマに対して収集されるデータの、サーバーサイド同期検証がおこなわれます。

デバッグはデフォルトで無効になっていますが、次の3つの方法でオンに切り替えることができます。

* `configure` コマンド
* `setDebug` コマンド
* クエリ文字列パラメーター

## configure コマンドを使用したデバッグの切り替え

`configure` コマンドを使用して SDK を設定する場合は、`debugEnabled` オプションを `true` に設定してデバッグを有効にしてください。

```javascript
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId":"ADB3LETTERSANDNUMBERS@AdobeOrg",
  "debugEnabled": true
});
```

>[!TIP]
>
>自分のブラウザーだけでなく、Web ページのすべてのユーザーに対するデバッグを可能にします。

## debug コマンドを使用したデバッグの切り替え

次のように、別の `debug` コマンドでデバッグを切り替えます。

```javascript
alloy("setDebug", {
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

デバッグを `debug` コマンドまたはクエリ文字列パラメーターで設定すると、`configure` コマンドで設定された `debug` オプションが上書きされます。この2つの場合、セッション中、デバッグもオンのままです。 つまり、debug コマンドまたはクエリ文字列パラメーターを使用してデバッグを有効にした場合、次のいずれかが実行されるまで、デバッグは有効なままになります。

* セッションの終了
* `debug` コマンドを実行します
* クエリ文字列パラメーターを再度設定する

## ライブラリ情報の取得

多くの場合、Web サイトに読み込んだライブラリの背後にある詳細にアクセスすると便利です。これをおこなうには、次のように、`getLibraryInfo` コマンドを実行します。

```js
alloy("getLibraryInfo").then(function(libraryInfo) {
  console.log(libraryInfo.version);
});
```

現在、指定された `libraryInfo` オブジェクトには次のプロパティが含まれています。

* `version`：読み込まれたライブラリのバージョンです。例えば、読み込まれるライブラリのバージョンが 1.0.0 の場合、値は `1.0.0` になります。
