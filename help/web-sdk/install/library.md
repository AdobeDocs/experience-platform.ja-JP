---
title: JavaScript ライブラリを使用した Web SDK のインストール
description: スタンドアロンの CDN ファイルを使用して、Web SDK ライブラリを参照します。
exl-id: bacfe938-4326-48f6-a321-bd16970e77eb
source-git-commit: 58cd6300307881c3de7c52e07c401bf2ed908517
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# JavaScript ライブラリを使用した Web SDK のインストール

を使用せずに Web SDK をインストールする代わりの方法 [タグ拡張機能の使用](extension.md) は、CDN でホストされる JavaScript ライブラリを参照します。 ライブラリを直接参照することも、ライブラリをダウンロードして独自のインフラストラクチャ上でホストすることもできます。 縮小形式およびフル形式で使用できます。

Web SDK ライブラリは、次の URL 構造を使用して使用できます。

* **縮小済み**: `https://cdn1.adoberesources.net/alloy/[VERSION]/alloy.min.js`
* **完全**: `https://cdn1.adoberesources.net/alloy/[VERSION]/alloy.js`

詳しくは、 [リリースノート](../release-notes.md) を使用して、URL に含める最新バージョンを指定します。 例えば、バージョン2.19.1の完全版の URL は次のようになります。 `https://cdn1.adoberesources.net/alloy/2.19.1/alloy.js`.

## コードの追加

次のコードブロックを、できる限り高い位置に追加します。 `<head>` タグのHTML:

```html
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["alloy"]);
</script>
<script src="https://cdn1.adoberesources.net/alloy/2.19.1/alloy.min.js" async></script>
```

このコードは、 `alloy` オブジェクトに含まれます。 Web SDK を同期的に読み込む場合は、 `async` 属性を設定します。 の削除 `async` 属性は、ライブラリが読み込まれて実行されるまで、HTMLドキュメントの残りの部分がブラウザーで解析およびレンダリングされるのをブロックします。 通常、プライマリコンテンツをユーザーに表示する前にこの遅延が発生するのはお勧めしませんが、組織のニーズに応じて適切になる場合があります。
