---
title: CSP の設定
seo-title: Configuring a CSP for Adobe Experience Platform Web SDK
description: Experience PlatformWeb SDK 用の CSP の設定方法を説明します
seo-description: Learn how to configure a CSP for the Experience Platform Web SDK
keywords: 設定；設定；SDK；エッジ；Web SDK；設定；コンテキスト；Web；デバイス；環境；Web SDK 設定；コンテンツセキュリティポリシー；
exl-id: 661d0001-9e10-479e-84c1-80e58f0e9c0b
source-git-commit: 0085306a2f5172eb19590cc12bc9645278bd2b42
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 2%

---

# CSP の設定

[ コンテンツセキュリティポリシー ](https://developer.mozilla.org/ja/docs/Web/HTTP/Headers/Content-Security-Policy)(CSP) は、ブラウザーが使用できるリソースを制限するために使用されます。 また、CSP は、スクリプトおよびスタイルリソースの機能を制限することもできます。 Adobe Experience Platform Web SDK には CSP は必要ありませんが、CSP を追加すると攻撃の対象面が減り、悪意のある攻撃を防ぐことができます。

CSP は、[!DNL Platform Web SDK] のデプロイ方法と設定方法を反映する必要があります。 次の CSP は、SDK が正しく機能するために必要な変更を示しています。 特定の環境に応じて、追加の CSP 設定が必要になる場合があります。

## コンテンツセキュリティポリシーの例

次の例は、CSP の設定方法を示しています。

### エッジドメインへのアクセスを許可

```
default-src 'self';
connect-src 'self' EDGE-DOMAIN
```

上記の例では、`EDGE-DOMAIN` をファーストパーティドメインに置き換える必要があります。 ファーストパーティドメインは、[edgeDomain](configuring-the-sdk.md#edge-domain) 設定用に設定されます。 ファーストパーティドメインが設定されていない場合は、`EDGE-DOMAIN` を `*.adobedc.net` に置き換える必要があります。 [idMigrationEnabled](configuring-the-sdk.md#id-migration-enabled) を使用して訪問者の移行を有効にする場合は、`connect-src` ディレクティブにも `*.demdex.net` を含める必要があります。

### NONCE を使用してインラインスクリプトおよびスタイル要素を許可する

[!DNL Platform Web SDK] ではページコンテンツを変更でき、インラインスクリプトおよびスタイルタグの作成には承認が必要です。これを実現するために、Adobeでは、[default-src](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) CSP ディレクティブに nonce を使用することをお勧めします。 nonce は、一意のページビューごとに 1 回生成される、サーバー生成の暗号的に強いランダムトークンです。

```
default-src 'nonce-SERVER-GENERATED-NONCE'
```

さらに、CSP nonce を [!DNL Platform Web SDK] [ ベースコード ](installing-the-sdk.md#adding-the-code) スクリプトタグの属性として追加する必要があります。 [!DNL Platform Web SDK] 次に、ページにインラインスクリプトまたはスタイルタグを追加する際に、そのナンスを使用します。

```
<script nonce="SERVER-GENERATED-NONCE">
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["alloy"]);
</script>
```

nonce を使用しない場合、もう 1 つのオプションは `unsafe-inline` を `script-src` および `style-src` CSP ディレクティブに追加することです。

```
script-src 'unsafe-inline'
style-src 'unsafe-inline'
```

>[!NOTE]
>
>Adobeは **** を指定しないことをお勧めします。これは、ページ上で任意のスクリプトを実行できるので、CSP のメリットが少ないからです。`unsafe-inline`
