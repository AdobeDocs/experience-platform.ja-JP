---
title: アラートテストリファレンス
description: Adobe Experience Platform Debugger での Auditor のアラートのテスト機能について説明します。
exl-id: ac6f8675-6c34-48b4-b5dd-48e92af217fd
source-git-commit: 10a5605c40143b58f6ba0108cc087956aa929866
workflow-type: tm+mt
source-wordcount: '634'
ht-degree: 35%

---

# アラートテストリファレンス

このリファレンスでは、Adobe Experience Platform Debugger の Auditor 機能でアラートテストを実行する方法に関する詳細を提供します。

>[!NOTE]
>
>Platform Debugger での監査テストについて詳しくは、 [auditor 機能の概要](./overview.md).

アラートは、認識する必要があるが、スコアには影響しない問題を示します。これらは、ベストプラクティスの推奨事項ですが、お客様の実装に適用されない場合があります。

| テスト | 重み付け | 条件 | 推奨 |
| --- | --- | --- | --- |
| Advertising Cloud - 正しいコンバージョンタグが実装されている | 0 | 正しいコンバージョンタグが使用されているかどうかを確認します。<br><br>**警告**:非推奨の TubeMogul コンバージョンタグを使用すると、データが失われる可能性があります。 | コンバージョンピクセルを新しい Advertising Cloud 画像専用コンバージョンタグにアップグレードします。これは、 [Advertising Cloudタグ拡張機能](../../destinations/catalog/advertising/adobe-advertising-cloud.md). |
| Advertising Cloud - 正しい JS タグを使用している | 0 | Advertising Cloudでは、最新の JavaScript タグを使用する必要があります。 | Advertising Cloud JavaScript を最新バージョンにアップグレードします。非推奨の JavaScript バージョンを使用すると、機能が失われる可能性があります。これは、 [Advertising Cloudタグ拡張機能](../../destinations/catalog/advertising/adobe-advertising-cloud.md). |
| Advertising Cloud - 画像専用タグ | 0 | Advertising Cloud の画像ピクセル形式は、次の推奨形式のいずれかと一致する必要があります。 <ul><li>`http(s)://rtd.tubemogul.com/upi/?sid=<HASH_VALUE>`</li><li>`http(s)://rtd-tm.everesttech.net/upi/?sid=<HASH_VALUE>`</li><li>`http(s)://pixel.everesttech.net/px2/<NUMERIC_ID>?`</li></ul> | Advertising Cloud の全機能を活用できるよう、Advertising Cloud のピクセルを新しい Advertising Cloud の画像専用タグにアップグレードします。これは、 [Advertising Cloudタグ拡張機能](../../destinations/catalog/advertising/adobe-advertising-cloud.md). |
| Advertising Cloud - セグメントピクセルの DSP 同期が有効になっている | 0 | TubeMogul セグメントピクセルに DSP 同期設定が含まれているかどうかを確認し、その設定をピクセルに追加することをお勧めします。DSP Syncing 設定は、クエリー文字列パラメーターを使用して決定されます。 以下に要約を示します。 <ul><li>タグが次のいずれかに対して実行される場合：<ul><li>`https://rtd.tubemogul.com/upi/?sid=<HASH_VALUE>`</li><li>`http(s)://rtd-tm.everesttech.net/upi/?sid=<HASH_VALUE>`</li><li>`http(s)://pixel.everesttech.net/px2/<NUMERIC_ID>?`</li></ul></li><li>およびタグに URL パラメーターが含まれる `sid=`</li><li>THEN URL パラメーターが `cs=0` または `cs=1` が存在し、を推奨しない場合は `cs=1` オーディエンスの一致率を向上させるために、をこれらのピクセルに追加します。</li></ul> | URL パラメーターを追加する `cs=1` をAdvertising Cloudピクセルに追加して、DSP同期を実行できるようにすることで、オーディエンスの一致率が向上します。 これは、 [Advertising Cloudタグ拡張機能](../../destinations/catalog/advertising/adobe-advertising-cloud.md). |
| Experience Cloud ID サービス - 1 つの AdobeOrg のみを使用する | 0 | 通常の ECID 実装では、1 つの AdobeOrg を使用する必要があります。 | この実装に複数の AdobeOrg ID が存在することを検証します。<br><br>[追加情報](https://experienceleague.adobe.com/docs/id-service/using/intro/id-request.html?lang=ja) |
| 起動 — `pageBottom` コールバック配置 | 0 | この `_satellite.pageBottom()` タグを機能させるには、関数が存在する必要があります。 終了の直前にインラインスクリプトを追加する `</body>` タグを使用して、適切な DTM 機能を確保してください。 注意：ベストプラクティスは、このタグを `<body>`. これが `<body>` タグを使用すると、機能する可能性がありますが、ベストプラクティスではないので、正しく機能しない場合や、予期しない結果や望ましくない結果が生じる場合があります。 | 終了の直前にインラインスクリプトを追加する `</body>` タグを使用して、適切な DTM 機能を確保してください。 <br><br>[追加情報](../../tags/ui/client-side/asynchronous-deployment.md) |
| Launch - 自己ホストされている | 0 | タグライブラリは、Adobeの Akamai インスタンス上 ( ) でホストされています。 `assets.adobedtm.com`. タグの読み込みには、キャッシュ制御を通じた Web サイトのパフォーマンスの制御、サードパーティスクリプトの依存関係の軽減、および公開プロセスの制御の向上を提供する自己ホスト型アプローチが推奨されます。 タグライブラリは、お客様独自の Web ホスティングまたは CDN を使用してホストおよび管理できます。 | ページにタグを読み込む場合は、自己ホスト型に切り替える方法があります。 Akamai CDN 経由の ホスティングはほとんどの場合において機能しますが、自己ホスティングを使用すると、ページのパフォーマンスが向上します。<br><br>追加情報:<ul><li>[タグクイックスタートガイド](../../tags/ui/client-side/asynchronous-deployment.md)</li><li>[非同期デプロイメント](../../tags/ui/client-side/asynchronous-deployment.md)</li></ul> |
| Launch - 非同期的にデプロイする必要がある | 0 | 最適なパフォーマンスを得るには、タグを非同期的にデプロイする必要があります。 | 次を含める： `async` パラメーターを使用して、適切なタグ機能を確保する <br><br>[追加情報](../../tags/ui/client-side/asynchronous-deployment.md) |
| Target — 内のコンテンツ `mboxDefault` | 0 | コンテンツは次の場所に存在する必要があります： `mboxDefault` 使用時 `at.js`. | コンテンツが使用可能であることを確認します。<br><br>[追加情報](https://experienceleague.adobe.com/docs/target/using/implement-target/implementing-target.html) |

{style="table-layout:auto"}
