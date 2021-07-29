---
title: コンテンツセキュリティポリシー（CSP）のサポート
description: Adobe Experience PlatformでWebサイトをタグと統合する際の、コンテンツセキュリティポリシー(CSP)の制限への対処方法について説明します。
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '1080'
ht-degree: 57%

---

# コンテンツセキュリティポリシー（CSP）のサポート

>[!NOTE]
>
>Adobe Experience Platform Launchは、Adobe Experience Platformのデータ収集テクノロジーのスイートとしてリブランドされました。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

コンテンツセキュリティポリシー（CSP）は、クロスサイトスクリプティング攻撃（XSS）を防ぐセキュリティ機能です。これは、悪意のあるコンテンツが、実際は別の場所から送られたのに、信頼できるソースから来たように見せかけ、ブラウザーを騙して実行させた場合に発生します。 CSP を使用すれば、ブラウザーは（ユーザーの代理として）、スクリプトが実際に信頼できるソースから送られたものであることを検証できます。

CSP は、サーバー応答に `Content-Security-Policy` HTTP ヘッダーを追加するか、HTML ファイルの `<meta>` セクションに設定済みの `<head>` 要素を追加することで実装されます。

>[!NOTE]
>
> CSP の詳細については、「[MDN Web ドキュメント](https://developer.mozilla.org/ja/docs/Web/HTTP/CSP)」を参照してください。

Adobe Experience Platformのタグは、Webサイトにスクリプトを動的に読み込むように設計されたタグ管理システムです。 セキュリティ上の問題が発生する可能性があるため、デフォルトの CSP は、動的に読み込まれるこれらのスクリプトをブロックします。このドキュメントでは、タグから動的に読み込まれるスクリプトを許可するようCSPを設定する方法のガイダンスを提供します。

タグをCSPと連携させるには、2つの主な課題を克服する必要があります。

* **タグライブラリのソースを信頼する必要があります。** この条件が満たされない場合、タグライブラリおよびその他の必要なJavaScriptファイルはブラウザーでブロックされ、ページに読み込まれません。
* **インラインスクリプトを許可する必要があります。**&#x200B;この条件が満たされない場合、カスタムコードルールのアクションはページ上でブロックされ、正しく実行されません。

セキュリティの向上には、コンテンツ作成者に代わって作業量の増加が必要です。 タグを使用し、さらにCSPを配置する場合、他のスクリプトを誤って「安全」とすることなく、これらの問題の両方を解決する必要があります。 このドキュメントの残りの部分では、これを実現する方法に関するガイダンスを提供します。

## タグを信頼できるソースとして追加する

CSP を使用する場合は、信頼されたドメインを `Content-Security-Policy` ヘッダーの値に含める必要があります。タグに指定する値は、使用しているホスティングのタイプによって異なります。

### 自己ホスト

ライブラリを[自己ホスト](../publishing/hosts/self-hosting-libraries.md)している場合、ライブラリのソースはおそらくユーザー自身のビルドになります。次の設定を使用して、ホストドメインが安全なソースであることを指定できます。

**HTTP ヘッダー**

```http
Content-Security-Policy: script-src 'self'
```

**HTML `<meta>`タグ**

```html
<meta http-equiv="Content-Security-Policy" content="script-src 'self'">
```

### アドビ管理ホスティング

[アドビ管理ホスト](../publishing/hosts/managed-by-adobe-host.md)を使用している場合、`assets.adobedtm.com` でビルドが保持されます。既に読み込んでいるスクリプトを壊さないように、`self`を安全なドメインとして指定する必要がありますが、`assets.adobedtm.com`を「安全」としてリストする必要もあります。そうしないと、タグライブラリがページに読み込まれません。 この場合、次の設定を使用する必要があります。

**HTTP ヘッダー**

```http
Content-Security-Policy: script-src 'self' assets.adobedtm.com
```

**HTML `<meta>`タグ**


非常に重要な前提条件は次のとおりです。タグライブラリ[非同期で](./asynchronous-deployment.md)を読み込む必要があります。 タグライブラリの同期読み込みでは動作しません（コンソールエラーやルールが正しく実行されない原因となります）。

```html
<meta http-equiv="Content-Security-Policy" content="script-src 'self' assets.adobedtm.com">
```

既に読み込んだスクリプトが引き続き機能するよう、安全なドメインとして `self` を指定する必要がありますが、`assets.adobedtm.com` を「安全」としてもリストする必要があります。そうしない場合、ライブラリビルドがページに読み込まれません。

## インラインスクリプト

CSP はデフフォルトでインラインスクリプトを許可しないため、インラインスクリプトを許可するには手動で設定する必要があります。インラインスクリプトを許可するには、次の 2 つのオプションがあります。

* [nonce で許可](#nonce)（セキュリティレベル高）
* [すべてのインラインスクリプトを許可する](#unsafe-inline)（セキュリティレベル最低）

>[!NOTE]
>
>CSPの仕様には、ハッシュを使用する3つ目のオプションの詳細が含まれていますが、このアプローチは、タグなどのタグ管理システムでは使用できません。 Platformでタグでハッシュを使用する際の制限事項について詳しくは、[Subresource Integrity(SRI)のガイド](./sri.md)を参照してください。

### nonce で許可 {#nonce}

この方法では、暗号化 nonce を生成して、CSP およびサイト上のすべてのインラインスクリプトに追加します。ブラウザーは、nonce を含むインラインスクリプトを読み込む命令を受け取ると、nonce 値と CSP ヘッダー内に含まれる値を比較します。一致すると、スクリプトが読み込まれます。この nonce は、新しいページが読み込まれるたびに変更される必要があります。

>[!IMPORTANT]
>
>この方法を使用するには、ビルドを非同期で読み込む必要があります。ビルドを同期的に読み込む際にはこれが動作しないため、コンソールのエラーやルールが正しく実行されません。詳しくは、[非同期デプロイメント](./asynchronous-deployment.md)のガイドを参照してください。

次の例は、アドビ管理ホストの CSP 設定に nonce を追加する方法を示しています。自己ホスティングを使用している場合は、`assets.adobedtm.com` を除外できます。

**HTTP ヘッダー**

```http
Content-Security-Policy: script-src 'self' assets.adobedtm.com 'nonce-2726c7f26c'
```

**HTML `<meta>`タグ**

```html
<meta http-equiv="Content-Security-Policy" content="script-src 'self' assets.adobedtm.com 'nonce-2726c7f26c'">
```

ヘッダーまたはHTMLタグを設定したら、インラインスクリプトを読み込む際にnonceを見つける場所をタグに伝える必要があります。 タグがスクリプトの読み込み時にnonceを使用するには、次の操作を行う必要があります。

1. データレイヤー内の nonce の位置を参照するデータ要素を作成します。
1. コア拡張機能を設定し、使用したデータ要素を指定します。
1. データ要素とコア拡張機能の変更を公開します。

>[!NOTE]
>
>上記のプロセスは、カスタムコードが何を実行するかではなく、カスタムコードの読み込みのみを処理します。CSP に準拠していないカスタムコードがインラインスクリプトに含まれている場合は、CSP が優先されます。例えば、カスタムコードを使用してインラインスクリプトをDOMに追加して読み込む場合、タグはnonceを正しく追加できないので、特定のカスタムコードアクションが期待どおりに動作しません。

### すべてのインラインスクリプトを許可する {#unsafe-inline}

nonce の使用が機能しない場合は、すべてのインラインスクリプトを許可するよう CSP を設定できます。このオプションの安全性はあまり高くありませんが、実施やメンテナンスは容易です。

次の例は、CSP ヘッダー内のすべてのインラインスクリプトを許可する方法を示しています。

#### 自己ホスト

自己ホスティングを使用する場合は、次の設定を使用します。

**HTTP ヘッダー**

```http
Content-Security-Policy: script-src 'self' 'unsafe-inline'
```

**HTML `<meta>`タグ**

```html
<meta http-equiv="Content-Security-Policy" content="script-src 'self' 'unsafe-inline'">
```

#### アドビ管理ホスティング

アドビ管理ホスティングを使用している場合は、次の設定を使用します。

**HTTP ヘッダー**

```http
Content-Security-Policy: script-src 'self' assets.adobedtm.com 'unsafe-inline'
```

**HTML `<meta>`タグ**

```html
<meta http-equiv="Content-Security-Policy" content="script-src 'self' assets.adobedtm.com 'unsafe-inline'">
```

## 次の手順

このドキュメントでは、タグライブラリファイルとインラインスクリプトを受け入れるようにCSPヘッダーを設定する方法を説明します。

追加のセキュリティ対策として、Subresource Integrity（SRI）を使用して、取得したライブラリビルドを検証することもできます。ただし、この機能をタグなどのタグ管理システムで使用する場合、いくつかの大きな制限があります。 詳しくは、[プラットフォームでのSRIの互換性に関するガイド](./sri.md)を参照してください。
