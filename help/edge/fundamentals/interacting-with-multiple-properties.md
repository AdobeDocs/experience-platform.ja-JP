---
title: Adobe Experience Platform Web SDK での複数のプロパティの操作
description: 複数の Experience Platform Web SDK プロパティの操作方法について説明します.
keywords: 複数のプロパティ；設定；sendEvent;edgeConfigId;orgId;
exl-id: e07afb0d-3490-414f-bc9c-f71bc04fe664
source-git-commit: 0085306a2f5172eb19590cc12bc9645278bd2b42
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 69%

---

# 複数のプロパティの操作

同じページで 2 つの異なるプロパティを操作する場合があります。次のような場合が考えられます。

* 買収され、共同で　Web サイトの統合に取り組んでいる企業
* 複数の企業間でのデータ共有関係
* 新しい Adobe ソリューションをテストしていて、既存の実装を中断したくないお客様

SDK では、ベースコード内の配列に別の名前を追加することで、各プロパティに個別のインスタンスを作成できます。次の例では、2 つの名前が示されています。 `mycustomname1` および `mycustomname2`.

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

2 つの異なるインスタンスを作成すると、異なるプロパティに対してそれぞれを設定できます。とのやり取りによって生じる通信やデータの持続性 `mycustomname1` は次の場所から切り離されます： `mycustomname2`.

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

Cookie との競合を避けるには、Adobe Experience Platformのインスタンスを 1 つだけにします [!DNL Web SDK] ページ内で、特定の `edgeConfigId`. 同様に、Adobe Experience Platformのインスタンスは 1 つだけです [!DNL Web SDK] 特定の `orgId`.
