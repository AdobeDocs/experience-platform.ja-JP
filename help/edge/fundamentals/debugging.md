---
title: Adobe Experience Platform Web SDK でのデバッグ
description: Experience PlatformWeb SDK でデバッグ機能を切り替える方法を説明します。
keywords: Web SDK のデバッグ；デバッグ；設定；コマンドの設定；debug コマンド；edgeConfigId;setDebug;debugEnabled;debug;
exl-id: 4e893af8-a48e-48dc-9737-4c61b3355f03
source-git-commit: c1e6b1519bc40e7d36bd83dc49e442d3d5583fed
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 61%

---

# デバッグ

デバッグが有効になっている場合、SDK は、実装のデバッグや SDK の動作の理解に役立つメッセージをブラウザーコンソールに出力します。

デバッグはデフォルトで無効になっていますが、次の 4 つの方法でオンに切り替えることができます。

* `configure` コマンド
* `setDebug` コマンド
* クエリ文字列パラメーター
* Adobe Experience Platform Debugger でのデバッグの有効化をオンに切り替える Adobe Experience Platformは、Web ページを調べ、Experience Cloud製品の実装の問題をデバッグできる強力なツールです。 Adobe Experience Platform Debugger は、 [クロム](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) および [Firefox](https://addons.mozilla.org/ja/firefox/addon/adobe-experience-platform-dbg/) 拡張子。 デバッグは、AEP Web SDK セクションの「設定」タブで有効にできます。

![](../images/enable-debugging.png)

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

デバッグを `debug` コマンドまたはクエリ文字列パラメーターで設定すると、`configure` コマンドで設定された `debug` オプションが上書きされます。この 2 つの場合、セッション中もデバッグはオンのままになります。 つまり、debug コマンドまたはクエリ文字列パラメーターを使用してデバッグを有効にした場合、次のいずれかが実行されるまで、デバッグは有効なままになります。

* セッションの終了
* `debug` コマンドを実行します
* クエリ文字列パラメーターを再度設定する

## ライブラリ情報の取得

多くの場合、Web サイトに読み込んだライブラリの背後にある詳細にアクセスすると便利です。これをおこなうには、次のように、`getLibraryInfo` コマンドを実行します。

```js
alloy("getLibraryInfo").then(function(result) {
  console.log(result.libraryInfo.version);
  console.log(result.libraryInfo.commands);
  console.log(result.libraryInfo.configs);
});
```

現在、指定された `libraryInfo` オブジェクトには次のプロパティが含まれています。

* `version`:読み込まれたライブラリのバージョンです。 例えば、読み込まれるライブラリのバージョンが 1.0.0 の場合、値は `1.0.0` になります。ライブラリをタグ拡張 (「AEP Web SDK」) 内で実行する場合、バージョンはライブラリバージョンで、タグ拡張バージョンは「+」記号で結合されたバージョンです。 例えば、ライブラリのバージョンが 1.0.0 で、タグ拡張のバージョンが 1.2.0 の場合、値はになります。 `1.0.0+1.2.0`.
* `commands`:これらは、読み込まれたライブラリでサポートされている使用可能なコマンドのすべてです。
* `configs`:これらは、読み込まれたライブラリ内の現在の設定すべてです。
