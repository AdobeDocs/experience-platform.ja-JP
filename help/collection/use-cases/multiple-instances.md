---
title: 複数の Web SDK インスタンスの使用
description: 複数のExperience Platform Web SDK プロパティを操作する方法を説明します。
keywords: 複数のプロパティ
exl-id: e07afb0d-3490-414f-bc9c-f71bc04fe664
source-git-commit: 192739967e6b050bb04893ee7bab5119dd7f870c
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 23%

---

# 複数の Web SDK インスタンスの使用

同じページで 2 つの異なるプロパティを操作する場合があります。考えられるシナリオは次のとおりです。

* 買収され、共同で　Web サイトの統合に取り組んでいる企業
* 複数の企業間でのデータ共有関係
* 新しいAdobe ソリューションをテストする際に、既存の実装を中断したくない顧客

SDKでは、[ ベースコード ](../js/install/base-code.md) の配列に別の名前を追加することで、プロパティごとに個別のインスタンスを作成できます。 次の例では、`titanium` と `copper` という 2 つの名前を指定しています。

```html
<!-- Base code -->
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n.setTimeout(function(){n[o].q.push([i,l,u])})})},n[o].q=[])})}
  (window,["titanium", "copper"]);
</script>

<!-- Load the Web SDK (JavaScript library loader or Tags embed code) -->
<!-- <script src=".../alloy.min.js" async></script> -->
<!-- <script src=".../launch-<ENV>.min.js" async></script> -->
```

その結果、スクリプトは、ライブラリの初期化時に 2 つのSDK インスタンスになる 2 つのグローバル関数（上記の例では `titanium` と `copper`）を作成します。 各インスタンスは、独自の設定と状態を保持します。`titanium` を使用するコマンドは、`copper` から分離された状態が維持されます。

>[!TIP]
>
>タグでベースコードを使用する場合は、タグ拡張機能を設定する際に、設定したすべてのインスタンス名がすべての [SDK インスタンス名 ](/help/tags/extensions/client/web-sdk/configure/general.md) と一致することを確認します。

`titanium` および `copper` の命名パターンの例に従って、Web SDK インスタンスとして、コマンドを独立して実行できます。

```javascript
titanium("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg"
});

titanium("sendEvent", {
  data: {
    key: "value"
  }
});

copper("configure", {
  datastreamId: "f46e981f-fd03-4bdd-a9d9-73ce4447f870",
  orgId: "ADB3NUMBERSANDLETTERS2@AdobeOrg"
});

copper("sendEvent", {
  data: {
    key: "value"
  }
});
```

同じインスタンスで他のコマンドを実行する前に必ず、各インスタンスで [`configure`](../js/commands/configure/overview.md) コマンドを実行してください。

>[!IMPORTANT]
>
>Cookie との競合を避けるために、各 web SDK インスタンスには、独自の `datastreamId` と一意の `orgId` が必要です。
