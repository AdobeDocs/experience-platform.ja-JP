---
title: CSPの構成
seo-title: Adobe Experience PlatformWeb SDK用のCSPの構成
description: Experience PlatformWeb SDK用のCSPを構成する方法を学びます
seo-description: Experience PlatformWeb SDK用のCSPを構成する方法を学びます
keywords: 設定；設定；SDK；エッジ；Web SDK；設定；コンテキスト；Web；デバイス；環境;Web sdk設定；コンテンツセキュリティポリシー；
translation-type: tm+mt
source-git-commit: 4f07d41197add406fbdd82caee5177a1ddaa7d7e
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 2%

---


# CSPの構成

[コンテンツセキュリティポリシー](https://developer.mozilla.org/ja/docs/Web/HTTP/Headers/Content-Security-Policy) (CSP)は、ブラウザーが使用できるリソースを制限するために使用されます。 CSPは、スクリプトおよびスタイルリソースの機能を制限することもできます。 Adobe Experience PlatformWeb SDKにはCSPは必要ありませんが、CSPを追加すると攻撃の対象となる脆弱性が減り、悪意のある攻撃に対する防止に役立ちます。

CSPは、[!DNL Platform Web SDK]の展開方法と構成方法を反映する必要があります。 次のCSPは、SDKが正しく機能するために必要な変更を示しています。 特定の環境に応じて、追加のCSP設定が必要になる場合があります。

## コンテンツセキュリティポリシーの例

次の例は、CSPを構成する方法を示します。

### エッジドメインへのアクセスを許可

```
default-src 'self';
connect-src 'self' EDGE-DOMAIN
```

上記の例では、`EDGE-DOMAIN`をファーストパーティドメインに置き換える必要があります。 ファーストパーティドメインは、[edgeDomain](configuring-the-sdk.md#edge-domain)設定用に構成されています。 ファーストパーティドメインが設定されていない場合は、`EDGE-DOMAIN`を`*.adobedc.net`に置き換える必要があります。 [idMigrationEnabled](configuring-the-sdk.md#id-migration-enabled)を使用して訪問者の移行を有効にする場合は、`connect-src`ディレクティブにも`*.demdex.net`を含める必要があります。

### NONCEを使用してインラインスクリプトおよびスタイル要素を許可する

[!DNL Platform Web SDK] でページのコンテンツを変更できます。また、インラインスクリプトおよびスタイルタグを作成するには、承認が必要です。これを実現するために、Adobeでは、[default-src](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) CSPディレクティブにnonceを使用することをお勧めします。 nonceは、一意のページ表示ごとに1回生成される、サーバ生成の暗号的に強力なランダムなトークンです。

```
default-src 'nonce-SERVER-GENERATED-NONCE'
```

さらに、CSP nonceを[!DNL Platform Web SDK] [ベースコード](installing-the-sdk.md#adding-the-code)スクリプトタグの属性として追加する必要があります。 [!DNL Platform Web SDK] 次に、ページにインラインスクリプトまたはスタイルタグを追加する際に、このnoceを使用します。

```
<script nonce="SERVER-GENERATED-NONCE">
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["alloy"]);
</script>
```

nonceを使用しない場合、もう1つのオプションは`unsafe-inline`を`script-src`および`style-src` CSPディレクティブに追加します。

```
script-src 'unsafe-inline'
style-src 'unsafe-inline'
```

>[!NOTE]
>
>Adobeは&#x200B;****&#x200B;を指定しないことをお勧めします。これは、CSPの利点を制限する、ページ上で任意のスクリプトを実行できるからです。`unsafe-inline`
