---
title: 複数のプロパティの操作
seo-title: 複数のプロパティとの対話操作を行うAdobe Experience Platform Web SDK
description: 複数のエクスペリエンスプラットフォームWeb SDKプロパティを操作する方法を説明します。
seo-description: 複数のエクスペリエンスプラットフォームWeb SDKプロパティを操作する方法を説明します。
translation-type: tm+mt
source-git-commit: 0cc6e233646134be073d20e2acd1702d345ff35f

---


# （ベータ版）複数のプロパティの操作

>[!IMPORTANT]
>
>Adobe Experience Platform Web SDKは現在ベータ版で、すべてのユーザーが利用できるわけではありません。 ドキュメントと機能は変更される場合があります。

同じページで2つの異なるプロパティを操作する場合があります。 これには、以下が含まれます。

* 自社のウェブサイトの統合に取り組んでいる企業
* 複数の企業間でのデータ共有関係
* 新しいアドビソリューションをテストしていて、既存の導入を中断したくないお客様

SDKを使用すると、ベースコード内の配列に別の名前を追加することで、各プロパティに対して個別のインスタンスを作成できます。 次の例では、という2つの名前を指定してい `mycustomname1` ます `mycustomname2`。

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["mycustomname1", "mycustomname2"]);
</script>
<script src="alloy.js" async></script>
```

その結果、スクリプトはSDKの2つのインスタンスを作成します。 第1のインスタンスと対話するためのグローバル関数が名 `mycustomname1` 前付けられ、第2のインスタンスと対話するためのグローバル関数が名付けられ `mycustomname2`る。

2つの異なるインスタンスを作成することで、それぞれ異なるプロパティに対して設定できます。 との対話によって生じる通信やデータの持続性は、との間で切り離さ `mycustomname1` れた状態に保たれ、またその逆の `mycustomname2` 状態に保たれます。

上の例の後に、次のように各インスタンスを使用してコマンドを実行できます。

```javascript
mycustomname1("configure", {
  "configId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg"
});

mycustomname1("event", {
  "data": {
    "key": "value"
  }
});

mycustomname2("configure", {
  "configId": "f46e981f-fd03-4bdd-a9d9-73ce4447f870",
  "orgId": "ADB3NUMBERSANDLETTERS2@AdobeOrg"
});

mycustomname2("event", {
  "data": {
    "key": "value"
  }
});
```

同じインスタンスで他のコマ `configure` ンドを実行する前に、各インスタンスのコマンドを必ず実行してください。

## 制限事項

cookieとの競合を避けるために、特定のCookieを含めることができるのは1つのページ内のAdobe Experience Platform Web SDKの1つのインスタンスのみで `configId`す。  同様に、特定のSDKを持つことのできるAdobe Experience Platform Web SDKのインスタンスは1つだけで `orgId`す。
