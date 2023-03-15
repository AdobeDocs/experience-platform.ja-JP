---
title: 非同期デプロイメント
description: Web サイトで Adobe Experience Platform タブライブラリを非同期でデプロイする方法について説明します。
exl-id: ed117d3a-7370-42aa-9bc9-2a01b8e7794e
source-git-commit: 88939d674c0002590939004e0235d3da8b072118
workflow-type: tm+mt
source-wordcount: '1079'
ht-degree: 100%

---

# 非同期デプロイメント {#asynchronous-deployment}

>[!CONTEXTUALHELP]
>id="platform_tags_asynchronous_deployment"
>title="非同期デプロイメント"
>abstract="このオプションを有効にすると、このスクリプトタグが解析される際に、ブラウザーは JavaScript ファイルの読み込みを開始しますが、ライブラリが読み込まれて実行されるのを待たずに、ドキュメントの残りの部分の解析とレンダリングを続行します。これにより、web ページのパフォーマンスが向上しますが、特定のルールの実行方法に関して、重要な影響を与えます。 詳しくは、 ドキュメント を参照してください。"

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語が変更されました。用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

製品で必要となる JavaScript ライブラリのパフォーマンスとノンブロッキングデプロイメントは、 Adobe Experience Cloud ユーザーにとってますます重要となっています。[[!DNL Google PageSpeed]](https://developers.google.com/speed/pagespeed/insights/) などのツールでは、ユーザーがサイトにアドビライブラリをデプロイする方法を変更することを推奨しています。この記事では、非同期的に Adobe JavaScript ライブラリを使用する方法について説明します。

## 同期と非同期

### 同期デプロイメント

多くの場合、ライブラリはページの `<head>` タグ内で同期的に読み込まれます。次に例を示します。

```markup
<script src="example.js"></script>
```

デフォルトでは、ブラウザーはドキュメントを解析してこの行に到達し、サーバーから JavaScript ファイルの取得を開始します。ブラウザーは、ファイルが返されるまで待機し、JavaScript ファイルを解析して実行します。最後に、残りの HTML ドキュメントの解析を続行します。

表示コンテンツをレンダリングする前にパーサーが `<script>` タグに遭遇すると、コンテンツの表示が遅延します。読み込まれる JavaScript ファイルが、ユーザーにコンテンツを表示するために必ずしも必要ではない場合、訪問者はコンテンツを待機する必要がなくなります。ライブラリが大きいほど、遅延時間も長くなります。この理由により、[!DNL Google PageSpeed] や [!DNL Lighthouse] などの Web サイトパフォーマンスベンチマークツールは、同期的に読み込まれたスクリプトにフラグを付けることが頻繁に発生します。

管理するタグが多数ある場合、タグ管理ライブラリはすぐに大きくなってしまう可能性があります。

### 非同期デプロイメント

`<script>`タグに `async` 属性を追加すると、任意のライブラリを非同期で読み込むことができます。次に例を示します。

```markup
<script src="example.js" async></script>
```

このコードは、スクリプトタグが解析されたら、ブラウザーは JavaScript ファイルの読み込みを開始しますが、ライブラリが読み込まれて実行されるのを待たずに、ドキュメントの残りの部分の解析とレンダリングを続行する必要があることをブラウザーに示します。

## 非同期デプロイメントに関する考慮事項

上述のように、同期デプロイメントでは、Adobe Experience Platform タブライブラリの読み込み中および実行中、ブラウザーはページの解析とレンダリングを一時停止します。一方、非同期デプロイメントでは、ライブラリの読み込み中、ブラウザーはページの解析とレンダリングを続行します。ページの解析とレンダリングに関連して、タグライブラリの読み込みが完了するタイミングのばらつきを考慮する必要があります。

まず、タグライブラリの読み込みは、ページの下部が解析および実行される前に終わることもあれば、後に終わることもあるため、ページコードから `_satellite.pageBottom()` を呼び出さないでください（`_satellite` はライブラリが読み込まれるまで利用できません）。これについては、[タグ埋め込みコードの非同期読み込み](#loading-the-tags-embed-code-asynchronously)で説明しています。

次に、タグライブラリは、[`DOMContentLoaded`](https://developer.mozilla.org/ja-JP/docs/Web/Events/DOMContentLoaded) ブラウザーイベント（DOM Ready）の発生前または発生後に読み込みを終了できます。

これら 2 つの点により、 ライブラリを非同期的に読み込む際、コア拡張機能の [Library Loaded](../../extensions/client/core/overview.md#library-loaded-page-top)、[Page Bottom](../../extensions/client/core/overview.md#page-bottom)、[DOM Ready](../../extensions/client/core/overview.md#page-bottom) および [Window Loaded](../../extensions/client/core/overview.md#window-loaded) イベントタイプがどのように機能するかを示すとよいでしょう。

タグプロパティに次の 4 つのルールが含まれる場合：

* ルール A：イベントタイプ「Library Loaded」を使用
* ルール B：イベントタイプ「Page Bottom」を使用
* ルール C：イベントタイプ「DOM Ready」を使用
* ルール D：イベントタイプ「Window Loaded」を使用

タグライブラリの読み込みの終了時期に関係なく、すべてのルールが必ず実行されます。実行順序は次のとおりです。

ルール A → ルール B → ルール C → ルール D

順序は常に適用されますが、ルールにはタグライブラリの読み込みが終了するとすぐに実行されるものと、後で実行されるものがあります。タグライブラリ読み込みが終了すると、次のことが発生します。

1. ルール A はすぐに実行されます。
1. `DOMContentLoaded` ブラウザーイベント（DOM Ready）が既に発生している場合は、ルール B とルール C がすぐに実行されます。それ以外の場合は、ルール B とルール C は後で [`DOMContentLoaded`](https://developer.mozilla.org/ja-JP/docs/Web/Events/DOMContentLoaded) ブラウザーイベントが発生したときに実行されます。
1. [`load`](https://developer.mozilla.org/ja-JP/docs/Web/Events/load) ブラウザーイベント（Window Loaded）が既に発生している場合は、ルール D が即座に実行されます。それ以外の場合は、ルール D は後で [`load`](https://developer.mozilla.org/ja-JP/docs/Web/Events/load) ブラウザーイベントが発生したときに実行されます。指示に従ってタグライブラリをインストールした場合、 タグライブラリでは&#x200B;*常に*、 [`load`](https://developer.mozilla.org/ja-JP/docs/Web/Events/load) ブラウザーイベントが発生する前に読み込みが完了することに注意してください。

これらの原則を独自の Web サイトに適用する際には、次の点に注意してください。

* **「Library Loaded」イベントタイプを使用するルールは、データレイヤーが完全に読み込まれる前に実行される場合があります。**&#x200B;これにより、データがまだページ上で使用できないという理由で、ルールのアクションが実行されないことがあります。これらのタイプの問題は、ルール設定のトゥイーンによって軽減できます。例えば、「Library Loaded」イベントタイプによってルールをトリガーする代わりに、データレイヤーの読み込みが終了するとすぐに、ページコードによってトリガーされる「Custom Event」または「Direct Call」イベントタイプを使用することができます。
* **「Page Bottom」イベントタイプでは、ライブラリが非同期で読み込まれる際、値の提供はおこないません。**  代わりに、Library Loaded、DOM Ready、Window Loaded、またはその他のイベントタイプを検討してください。

順序に問題が発生した場合、タイミングの問題が発生している可能性が高くなります。正確なタイミングを必要とするデプロイメントでは、イベントリスナーと Custom Event または Direct Call イベントタイプを使用して、実装の堅牢性と一貫性を高める必要があります。

## 非同期でのタグ埋め込みコードの読み込み

タグでは、[環境](../publishing/environments.md)を設定する時に埋め込みコードを作成する際、非同期での読み込みをオンにする切り替えを提供します。また、非同期読み込みを自分で設定することもできます。

1. 非同期属性を `<script>` タグに追加して、非同期的にスクリプトを読み込みます。

   タグ埋め込みコードの場合、次のコードを変更します。

   ```markup
   <script src="//www.yoururl.com/launch-EN1a3807879cfd4acdc492427deca6c74e.min.js"></script>
   ```

   を次のように変更します。

   ```markup
   <script src="//www.yoururl.com/launch-EN1a3807879cfd4acdc492427deca6c74e.min.js" async></script>
   ```

1. 以前タグの末尾に追加した可能性のあるコードをすべて削除します。

   ```markup
   <script type="text/javascript">_satellite.pageBottom();</script>
   ```

   このコードは、ブラウザーパーサーがページの最下部に到達したことを Platform に伝えます。この時間までにタグが読み込みおよび実行されていない可能性があるため、`_satellite.pageBottom()` を呼び出すとエラーが発生し、「Page Bottom」イベントタイプが期待どおりに動作しない可能性があります。
