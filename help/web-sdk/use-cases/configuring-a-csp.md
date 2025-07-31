---
title: CSP の設定
seo-title: Configuring a CSP for Adobe Experience Platform Web SDK
description: Experience Platform Web SDKの CSP を設定する方法について説明します
seo-description: Learn how to configure a CSP for the Experience Platform Web SDK
keywords: 設定；設定；SDK；エッジ；Web SDK；設定；コンテキスト；web；デバイス；環境；web sdk 設定；コンテンツセキュリティポリシー；
exl-id: 661d0001-9e10-479e-84c1-80e58f0e9c0b
source-git-commit: 35429ec2dffacb9c0f2c60b608561988ea487606
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 0%

---

# CSP の設定

[ コンテンツセキュリティポリシー ](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy) （CSP）は、ブラウザーが使用できるリソースを制限するために使用されます。 また、CSP はスクリプトリソースとスタイルリソースの機能を制限することもできます。 Adobe Experience Platform Web SDKには CSP は必要ありませんが、CSP を追加すると攻撃サーフェスが減少して、悪意のある攻撃に対する防御が強化されます。

CSP は、[!DNL Experience Platform Web SDK] のデプロイ方法と設定方法を反映する必要があります。 次の CSP は、SDKが正しく機能するために必要な可能性のある変更を示しています。 具体的な環境によっては、追加の CSP 設定が必要になる場合があります。

## コンテンツセキュリティポリシーの例

次の例は、CSP の設定方法を示しています。

### Edge ドメインへのアクセスを許可

```
default-src 'self';
connect-src 'self' EDGE-DOMAIN
```

上記の例では、`EDGE-DOMAIN` をファーストパーティドメインに置き換える必要があります。 ファーストパーティドメインが [edgeDomain](../commands/configure/edgedomain.md) 設定用に設定されます。 ファーストパーティドメインを設定していない場合は、`EDGE-DOMAIN` に置き換える必要 `*.adobedc.net` あります。 [idMigrationEnabled](../commands/configure/idmigrationenabled.md) を使用して訪問者の移行をオンにする場合、`connect-src` ディレクティブにも `*.demdex.net` を含める必要があります。

### NONCE を使用してインラインのスクリプト要素とスタイル要素を許可する

ページのコンテンツを変更で [!DNL Experience Platform Web SDK] ます。インラインスクリプトおよびスタイルタグの作成を承認する必要があります。 これを実現するために、Adobeでは、CSP ディレクティブ [default-src](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) に nonce を使用することをお勧めします。 nonce は、一意のページビューごとに 1 回生成される、サーバー生成の暗号的に強力なランダムトークンです。

```
default-src 'nonce-SERVER-GENERATED-NONCE'
```

さらに、CSP nonce を、[!DNL Experience Platform Web SDK] [ ベースコード ](../install/library.md) スクリプトタグの属性として追加する必要があります。 [!DNL Experience Platform Web SDK] の後、ページにインラインスクリプトタグまたはスタイルタグを追加する際に、nonce が使用されます。

```
<script nonce="SERVER-GENERATED-NONCE">
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["alloy"]);
</script>
```

nonce を使用しない場合、もう 1 つのオプションは、`unsafe-inline` および `script-src` の CSP 指令に `style-src` を追加することです。

```
script-src 'unsafe-inline'
style-src 'unsafe-inline'
```

>[!NOTE]
>
>Adobeでは、ページ上で任意のスクリプトを実行でき **CSP のメリットが制限されるので、** を指定することは `unsafe-inline` お勧めしません。

## アプリ内メッセージ用の CSP の設定 {#in-app-messaging}

[Web アプリ内メッセージ ](../personalization/web-in-app-messaging.md) を設定する場合は、CSP に次のディレクティブを含める必要があります。

```
default-src  blob:;
```
