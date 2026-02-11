---
title: Web SDK JavaScript ライブラリのインストール
description: スタンドアロン CDN ファイルを使用して web SDK ライブラリを参照します。
exl-id: bacfe938-4326-48f6-a321-bd16970e77eb
source-git-commit: 010192e91185c11d5454d4153913c06b90fe2122
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 0%

---

# Web SDK JavaScript ライブラリのインストール

[Web SDK タグ拡張機能 &#x200B;](/help/tags/extensions/client/web-sdk/overview.md) を使用しない場合は、Adobe CDN でホストされているスタンドアロンのJavaScript ライブラリを参照して、web SDKをインストールできます。 ライブラリを直接参照するか、ダウンロードして独自のインフラストラクチャにホストすることができます。 縮小された形式と完全な形式で利用できます。

Web SDK ライブラリは、次の URL 構造を使用して利用できます。

* **縮小**: `https://cdn1.adoberesources.net/alloy/<VERSION>/alloy.min.js`
* **フル**: `https://cdn1.adoberesources.net/alloy/<VERSION>/alloy.js`

URL に含める最新バージョンについては、[Web SDK リリースノート &#x200B;](../release-notes.md) を参照してください。 例えば、バージョン 2.19.1 のフルバージョンの URL は `https://cdn1.adoberesources.net/alloy/2.19.1/alloy.js` です。

## ベースコードとライブラリローダーの追加

追加するコードは、次の 2 つのセクションで構成されます。

* **ベースコード**:Web SDKが非同期で読み込まれるときに、コマンドをキューに入れることでブートストラップを可能にします。 詳しくは、[&#x200B; ベースコード &#x200B;](base-code.md) を参照してください。 Adobeでは、ページ読み込み時に web SDK コマンドを呼び出す際の競合状態を避けるために、ライブラリを非同期で読み込む際にベースコードを使用することをお勧めします。
* **ライブラリローダー**:JavaScript ライブラリ全体を読み込みます。

Web SDKを呼び出す可能性のあるスクリプトの前に、次のコードブロックをできるだけ `<head>` タグの高い位置に追加します。

```html
<!-- Base code -->
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n.setTimeout(function(){n[o].q.push([i,l,u])})})},n[o].q=[])})}
  (window,["alloy"]);
</script>
<!-- Library loader -->
<script src="https://cdn1.adoberesources.net/alloy/<VERSION>/alloy.min.js" async></script>
```

Web SDKを同期的に読み込む場合は、ライブラリを読み込む際に `async` 属性を削除できます。 `async` 属性ブロックを削除すると、ブラウザーがライブラリを取得して実行する間、HTMLは解析を行います。 プライマリコンテンツをユーザーに表示する前にこのような追加の遅延を行うことは、通常はお勧めしませんが、組織のニーズに応じて合理的な場合があります。
