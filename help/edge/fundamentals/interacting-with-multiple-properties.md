---
title: 複数のプロパティの操作
seo-title: Adobe Experience Platform Web SDK：複数のプロパティの操作
description: 複数の Experience Platform Web SDK プロパティの操作方法について説明します
seo-description: 複数の Experience Platform Web SDK プロパティの操作方法について説明します
keywords: 複数のプロパティ；設定；sendEvent;edgeConfigId;orgId;
translation-type: tm+mt
source-git-commit: 8c256b010d5540ea0872fa7e660f71f2903bfb04
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 85%

---


# 複数のプロパティの操作

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
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg"
});

mycustomname1("sendEvent", {
  "data": {
    "key": "value"
  }
});

mycustomname2("configure", {
  "edgeConfigId": "f46e981f-fd03-4bdd-a9d9-73ce4447f870",
  "orgId": "ADB3NUMBERSANDLETTERS2@AdobeOrg"
});

mycustomname2("sendEvent", {
  "data": {
    "key": "value"
  }
});
```

同じインスタンスで他のコマンドを実行する前に必ず、各インスタンスで `configure` コマンドを実行してください。

## 制限事項

cookieとの競合を避けるために、1つのページ内のAdobe Experience Platform[!DNL Web SDK]の1つのインスタンスのみが特定の`edgeConfigId`を持つことができます。  同様に、Adobe Experience Platform[!DNL Web SDK]の1つのインスタンスのみが特定の`orgId`を持つことができます。
