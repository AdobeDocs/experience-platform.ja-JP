---
title: 複数の Web SDK インスタンスの使用
description: 複数のExperience Platform Web SDK プロパティを操作する方法を説明します。
keywords: 複数のプロパティ；設定；sendEvent;edgeConfigId;orgId;
exl-id: e07afb0d-3490-414f-bc9c-f71bc04fe664
source-git-commit: f75dcfc945be2f45c1638bdd4d670288aef6e1e6
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 63%

---

# 複数の Web SDK インスタンスの使用

同じページで 2 つの異なるプロパティを操作する場合があります。その例を次に示します。

* 買収され、共同で　Web サイトの統合に取り組んでいる企業
* 複数の企業間でのデータ共有関係
* 新しい Adobe ソリューションをテストしていて、既存の実装を中断したくないお客様

SDK では、ベースコード内の配列に別の名前を追加することで、各プロパティに個別のインスタンスを作成できます。次の例では、`titanium` と `copper` という 2 つの名前を指定しています。

```html
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["titanium", "copper"]);
</script>
<script src="alloy.js" async></script>
```

その結果、スクリプトは 2 つの SDK インスタンスを作成します。最初のインスタンスと対話するためのグローバル関数に `titanium`、2 番目のインスタンスと対話するためのグローバル関数が `copper` という名前が付けられます。

2 つの異なるインスタンスを作成すると、異なるプロパティに対してそれぞれを設定できます。`titanium` とのインタラクションによって発生した通信やデータの永続性は、`copper` から分離された状態が維持されます。

上記の例に続いて、各インスタンスを使用してコマンドを実行できます。

```javascript
titanium("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg"
});

titanium("sendEvent", {
  "data": {
    "key": "value"
  }
});

copper("configure", {
  "edgeConfigId": "f46e981f-fd03-4bdd-a9d9-73ce4447f870",
  "orgId": "ADB3NUMBERSANDLETTERS2@AdobeOrg"
});

copper("sendEvent", {
  "data": {
    "key": "value"
  }
});
```

同じインスタンスで他のコマンドを実行する前に必ず、各インスタンスで `configure` コマンドを実行してください。

>[!IMPORTANT]
>
>Cookie との競合を避けるために、各 Web SDK インスタンスは、独自の `edgeConfigId` と独自の `orgId` を持つ必要があります。
