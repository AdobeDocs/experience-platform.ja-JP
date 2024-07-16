---
title: JavaScript ライブラリを使用して Web SDK をインストールします
description: スタンドアロン CDN ファイルを使用して Web SDK ライブラリを参照します。
exl-id: bacfe938-4326-48f6-a321-bd16970e77eb
source-git-commit: 9876390f7ba34c312f2ce4c00fe39e3ea1ef1ace
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# JavaScript ライブラリを使用して Web SDK をインストールします

[ タグ拡張機能を使用 ](extension.md) せずに Web SDK をインストールする代わりに、CDN でホストされているJavaScript ライブラリを参照することもできます。 ライブラリを直接参照するか、ダウンロードして独自のインフラストラクチャにホストすることができます。 縮小された形式と完全な形式で利用できます。

Web SDK ライブラリは、次の URL 構造を使用して利用できます。

* **縮小**: `https://cdn1.adoberesources.net/alloy/[VERSION]/alloy.min.js`
* **フル**: `https://cdn1.adoberesources.net/alloy/[VERSION]/alloy.js`

URL に含める最新バージョンについては、[ リリースノート ](../release-notes.md) を参照してください。 例えば、バージョン 2.19.1 のフルバージョンの URL は `https://cdn1.adoberesources.net/alloy/2.19.1/alloy.js` です。

## コードの追加

次のコードブロックをできるだけHTMLの `<head>` タグに追加します。

```html
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n.setTimeout(function(){n[o].q.push([i,l,u])})})},n[o].q=[])})}
  (window,["alloy"]);
</script>
<script src="https://cdn1.adoberesources.net/alloy/2.19.1/alloy.min.js" async></script>
```

このコードは、任意の Web SDK コマンドを呼び出すことができる `alloy` オブジェクトを非同期で作成します。 Web SDK を同期的に読み込む場合は、コードブロックの最終行で `async` 属性を削除できます。 `async` 属性を削除すると、ライブラリが読み込まれて実行されるまで、HTMLドキュメントの残りの部分がブラウザーで解析およびレンダリングされなくなります。 プライマリコンテンツをユーザーに表示する前にこのような追加の遅延を設けることは、通常はお勧めしませんが、組織のニーズに応じて合理的な場合があります。
