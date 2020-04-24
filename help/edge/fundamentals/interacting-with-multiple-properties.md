---
title: 複数のプロパティの操作
seo-title: Adobe Experience Platform Web SDK：複数のプロパティの操作
description: 複数の Experience Platform Web SDK プロパティの操作方法について説明します
seo-description: 複数の Experience Platform Web SDK プロパティの操作方法について説明します
translation-type: tm+mt
source-git-commit: 0cc6e233646134be073d20e2acd1702d345ff35f

---


# （ベータ版）複数のプロパティの操作

>[!IMPORTANT]
>
>Adobe Experience Platform Web SDK は現在ベータ版で、すべてのユーザーが利用できるわけではありません。ドキュメントと機能は変更される場合があります。

同じページで 2 つの異なるプロパティを操作する場合があります。これには、以下が含まれます。

* 買収され、共同で　Web サイトの統合に取り組んでいる企業
* 複数の企業間でのデータ共有関係
* 新しい Adobe ソリューションをテストしていて、既存の実装を中断したくないお客様

SDK では、ベースコード内の配列に別の名前を追加することで、各プロパティに個別のインスタンスを作成できます。次の例では、`mycustomname1` と　`mycustomname2`　という 2 つの名前を指定します。

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["mycustomname1", "mycustomname2"]);
</script>
<script src="alloy.js" async></script>
```

その結果、スクリプトは 2 つの SDK インスタンスを作成します。最初のインスタンスと対話するためのグローバル関数に `mycustomname1`、2 番目のインスタンスと対話するためのグローバル関数が `mycustomname2` という名前が付けられます。

2 つの異なるインスタンスを作成すると、異なるプロパティに対してそれぞれを設定できます。`mycustomname1` との対話によって生じる通信やデータの持続性は、`mycustomname2` と切り離されます。

上の例に続き、次のように各インスタンスを使用してコマンドを実行できます。

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

同じインスタンスで他のコマンドを実行する前に必ず、各インスタンスで `configure` コマンドを実行してください。

## 制限事項

Cookie との競合を避けるために、1 つのページ内の Adobe Experience Platform Web SDK のインスタンスのうち、いずれか 1 つにのみ特定の `configId` を含めることができます。同様に、特定の `orgId` を持つことのできる Adobe Experience Platform Web SDK のインスタンスは 1 つだけです。
