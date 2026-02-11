---
title: ベースコード
description: データ収集ライブラリが非同期で読み込まれる際のキューコマンド（ブートストラップ）。
source-git-commit: 0a45b688243b17766143b950994f0837dc0d0b48
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 0%

---

# ベースコード

ベースコードは、Adobe Experience Platform Web SDKが非同期で読み込まれる際に、コマンドをキューに入れることで、ブートストラップを可能にします。 ベースコードを実行した後は、競合状態やライブラリの読み込みが完了したときのタイミングを気にすることなく、[`configure`](../commands/configure/overview.md) や [`sendEvent`](../commands/sendevent/overview.md) などのコマンドをすぐに呼び出すことができます。 Web SDKが読み込みを完了すると、キューに入れられたコマンドは、先入れ先出しの順序（キューに入れたのと同じ順序）で実行されます。

コマンドは、キューに入れられている場合でも Promises を返します。 コマンドがキューに入っている場合、Web SDKの読み込みが完了すると、コマンドが実行された後に、Promise が解決または拒否されます。 Web SDKが読み込みを完了しない場合（ライブラリの読み込みに失敗した場合など）、キュー内の Promises は保留中のままになります。

## ベースコードを追加

Web SDKを呼び出す可能性のあるスクリプトの前に、ベースコードをできるだけ高く、`<head>` タグに配置します。

```html
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n.setTimeout(function(){n[o].q.push([i,l,u])})})},n[o].q=[])})}
  (window,["alloy"]);
</script>
```

ベースコードを追加した後、選択したメソッド（[JavaScript ライブラリローダー &#x200B;](library.md) または [&#x200B; タグ埋め込みコード &#x200B;](/help/tags/extensions/client/web-sdk/getting-started.md)）を使用して web SDKを読み込みます。 タグベースの実装の場合、ベースコードは、Web SDK タグ拡張機能 2.34.0 以降でサポートされます。

このベースコードは、次のシナリオでは必要 **ありません**。

* JavaScript ライブラリを同期的に読み込む場合。 ライブラリの取得および実行中にブロックの同期読み込みを解析しています。
* タグ拡張機能を使用している場合、web SDKへのすべての呼び出しは、タグルールまたはアクション内で行われます。 実装で、タグライブラリの外部で web SDKが参照されている場合にのみ、ベースコードを含める必要があります。 ほとんどのタグ実装では、通常、タグライブラリの外部で Web SDKを呼び出さないので、ほとんどのタグ実装ではベースコードは必要ありません。

## 例

コマンドがキューに入れられ、解決されるタイミングについては、このコード例のコメントを参照してください。 この例は、JavaScript ライブラリとタグ拡張機能の両方に適用されます。

```html
<head>
  <script>
    // Calls made before the base code runs are not captured (alloy is not yet defined).
    // Always make sure that the base code runs before any attempt to call commands.
    // alloy("getLibraryInfo").then(console.log).catch(console.error);
  </script>

  <!-- Base code -->
  <script>
    !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
    []).push(o),n[o]=function(){var u=arguments;return new Promise(
    function(i,l){n.setTimeout(function(){n[o].q.push([i,l,u])})})},n[o].q=[])})}
    (window,["alloy"]);
  </script>

  <!-- Queued command -->
  <script>
    alloy("getLibraryInfo").then(result => {
      console.log("Queued call resolved:", result);
    }).catch(console.error);
  </script>

  <!-- Load the Web SDK using the JavaScript loader -->
  <script src="https://cdn1.adoberesources.net/alloy/<VERSION>/alloy.min.js" async></script>
  <!-- or the tag extension -->
  <!-- <script src=".../launch-<ENV>.min.js" async></script> -->

  <!-- Another call (queued if the library is still loading; immediate if it is ready) -->
  <script>
    alloy("getLibraryInfo").then(result => {
      console.log("Immediate call resolved:", result);
    }).catch(console.error);
  </script>
</head>
```

## SDK インスタンスの名前を変更

ベースコードの最後の行を変更することで、呼び出すグローバル関数の名前を変更できます。 変更：

```js
(window,["alloy"]);
```

終了：

```js
(window,["ingot"]);
```

この変更により、`ingot` の代わりに `alloy` を使用してコマンドを呼び出すことができます。

```js
ingot("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg"
});
```

## 複数のSDK インスタンス

必要に応じて、ベースコードを使用して、ページ上に複数のSDK インスタンスを設定できます。 詳しくは [&#x200B; 複数の web SDK インスタンスの使用 &#x200B;](../../use-cases/multiple-instances.md) を参照してください。
