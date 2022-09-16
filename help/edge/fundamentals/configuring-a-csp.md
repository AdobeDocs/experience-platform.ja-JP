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

A [コンテンツセキュリティポリシー](https://developer.mozilla.org/ja/docs/Web/HTTP/Headers/Content-Security-Policy) (CSP) は、ブラウザーが使用できるリソースを制限するために使用されます。 また、CSP はスクリプトおよびスタイルリソースの機能を制限することもできます。 Adobe Experience Platform Web SDK には CSP は必要ありませんが、CSP を追加すると、悪意のある攻撃に対する攻撃の対象が減少し、

CSP は、 [!DNL Platform Web SDK] がデプロイされ、設定されている。 次の CSP は、SDK が正しく機能するために必要となる可能性のある変更を示しています。 特定の環境に応じて、追加の CSP 設定が必要になる場合があります。

## コンテンツセキュリティポリシーの例

次の例は、CSP の設定方法を示しています。

### Edge ドメインへのアクセスを許可

```
default-src 'self';
connect-src 'self' EDGE-DOMAIN
```

上記の例では、 `EDGE-DOMAIN` はファーストパーティドメインに置き換える必要があります。 ファーストパーティドメインは、 [edgeDomain](configuring-the-sdk.md#edge-domain) 設定。 ファーストパーティドメインが設定されていない場合、 `EDGE-DOMAIN` は、 `*.adobedc.net`. 訪問者の移行が [idMigrationEnabled](configuring-the-sdk.md#id-migration-enabled)、 `connect-src` 指令も含める必要があります `*.demdex.net`.

### NONCE を使用してインラインスクリプトおよびスタイル要素を許可します

[!DNL Platform Web SDK] でページコンテンツを変更できるので、インラインスクリプトおよびスタイルタグの作成を承認する必要があります。 これを実現するために、Adobeでは、に nonce を使用することをお勧めします。 [default-src](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) CSP ディレクティブ。 nonce は、一意のページビューごとに 1 回生成される、サーバー生成の暗号として強固なランダムトークンです。

```
default-src 'nonce-SERVER-GENERATED-NONCE'
```

さらに、CSP nonce を属性として [!DNL Platform Web SDK] [ベースコード](installing-the-sdk.md#adding-the-code) スクリプトタグ。 [!DNL Platform Web SDK] その後、ページにインラインスクリプトまたはスタイルタグを追加する際に、そのナンスを使用します。

```
<script nonce="SERVER-GENERATED-NONCE">
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["alloy"]);
</script>
```

nonce を使用しない場合、他のオプションは `unsafe-inline` から `script-src` および `style-src` CSP ディレクティブ：

```
script-src 'unsafe-inline'
style-src 'unsafe-inline'
```

>[!NOTE]
>
>Adobeが実行 **not** ～を指定する `unsafe-inline` これは、CSP のメリットが制限されるページ上で任意のスクリプトを実行できるためです。
