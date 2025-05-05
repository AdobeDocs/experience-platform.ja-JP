---
title: アラートテストリファレンス
description: auditor 機能がAdobe Experience Platform Debuggerでアラートをテストする方法を説明します。
exl-id: ac6f8675-6c34-48b4-b5dd-48e92af217fd
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '623'
ht-degree: 13%

---

# アラートテストリファレンス

このリファレンスでは、Adobe Experience Platform Debuggerの auditor 機能によるアラートテストの実行方法について詳しく説明します。

>[!NOTE]
>
>Experience Platform Debugger の auditor テストについて詳しくは、[auditor 機能の概要 ](./overview.md) を参照してください。

アラートは、認識する必要があるが、スコアには影響しない問題を示します。これらは、ベストプラクティスのレコメンデーションですが、お客様の実装に適用されない場合があります。

| テスト | 重み | 条件 | レコメンデーション |
| --- | --- | --- | --- |
| Advertising Cloud – 実装された正しいコンバージョンタグ | 0 | 正しいコンバージョンタグが使用されているかどうかを確認します。<br><br>**警告**：非推奨（廃止予定）の TubeMogul コンバージョンタグを使用すると、データが失われる可能性があります。 | コンバージョンピクセルを新しい Advertising Cloud 画像のみのコンバージョンタグにアップグレードします。 これは、[Advertising Cloud タグ拡張機能 ](../../destinations/catalog/advertising/adobe-advertising-cloud.md) で最も簡単に実現できます。 |
| Advertising Cloud – 正しい使用された JS タグ | 0 | Advertising Cloud は、最新のJavaScript タグを使用する必要があります。 | Advertising Cloud JavaScript を最新バージョンにアップグレードします。非推奨（廃止予定）のJavaScript バージョンを使用すると、機能が失われる可能性があります。 これは、[Advertising Cloud タグ拡張機能 ](../../destinations/catalog/advertising/adobe-advertising-cloud.md) を使用すると、より簡単に実現できます。 |
| Advertising Cloud – 画像のみのタグ | 0 | Advertising Cloud の画像ピクセル形式は、次の推奨形式のいずれかと一致する必要があります。 <ul><li>`http(s)://rtd.tubemogul.com/upi/?sid=<HASH_VALUE>`</li><li>`http(s)://rtd-tm.everesttech.net/upi/?sid=<HASH_VALUE>`</li><li>`http(s)://pixel.everesttech.net/px2/<NUMERIC_ID>?`</li></ul> | Advertising Cloud ピクセルを新しい Advertising Cloud 画像専用タグにアップグレードして、Advertising Cloud の完全な機能を確実に活用できるようにします。 これは、[Advertising Cloud タグ拡張機能 ](../../destinations/catalog/advertising/adobe-advertising-cloud.md) で最も簡単に実現できます。 |
| Advertising Cloud - セグメントピクセル数DSP同期が有効 | 0 | TubeMogul セグメントピクセルにDSP同期設定が含まれているかどうかを確認し、その設定をピクセルに追加することをお勧めします。 DSPの同期設定は、クエリ文字列パラメーターを使用して決定されます。 以下に要約を示します。 <ul><li>タグが次のいずれかに対して実行されている場合：<ul><li>`https://rtd.tubemogul.com/upi/?sid=<HASH_VALUE>`</li><li>`http(s)://rtd-tm.everesttech.net/upi/?sid=<HASH_VALUE>`</li><li>`http(s)://pixel.everesttech.net/px2/<NUMERIC_ID>?`</li></ul></li><li>また、タグには URL パラメーター `sid=` が含まれます</li><li>次に、URL パラメーター `cs=0` または `cs=1` が存在するかどうかを確認し、存在しない場合は、オーディエンスの一致率を向上さ `cs=1` るために、これらのピクセルに追加することを推奨します。</li></ul> | DSP同期を実行してオーディエンスの一致率を高められるように、URL パラメーター `cs=1` を Advertising Cloud ピクセルに追加します。 これは、[Advertising Cloud タグ拡張機能 ](../../destinations/catalog/advertising/adobe-advertising-cloud.md) で最も簡単に実現できます。 |
| Experience Cloud ID サービス - 1 つの AdobeOrg のみを使用します | 0 | 通常の ECID 実装では、1 つの AdobeOrg を使用する必要があります。 | この実装に複数の AdobeOrg ID が存在することを検証します。 <br><br>[追加情報](https://experienceleague.adobe.com/docs/id-service/using/intro/id-request.html?lang=ja) |
| Launch - `pageBottom` コールバックの配置 | 0 | タグを機能させるには、`_satellite.pageBottom()` 関数が存在する必要があります。 DTM が適切に機能するように、`</body>` タグを閉じる直前にインラインスクリプトを追加します。 メモ：タグは `<body>` の最後のタグにすることをお勧めします。 `<body>` タグ内で見つかった場合は、機能する可能性がありますが、ベストプラクティスではないので、誤って機能したり、予期しない結果や望ましくない結果になる可能性があります。 | DTM が適切に機能するように、`</body>` タグを閉じる直前にインラインスクリプトを追加します。 <br><br>[追加情報](../../tags/ui/client-side/asynchronous-deployment.md) |
| Launch - セルフホスト | 0 | タグライブラリは、Adobeの Akamai インスタンス（`assets.adobedtm.com`）でホストされています。 タグを読み込む場合は、セルフホスティングをお勧めします。セルフホスティングを使用すると、キャッシュの制御、サードパーティのスクリプトの依存関係の削減、公開プロセスの制御が行えるため、web サイトのパフォーマンスをより詳細に制御できます。 タグライブラリは、独自の web ホスティングまたは CDN を通じてホストおよび管理できます。 | ページにタグを読み込むための自己ホスト型アプローチに切り替えることです。 Akamai CDN を介したホスティングはほとんどの場合うまく機能しますが、自己ホスティングはページのパフォーマンスを向上させます。 <br><br> 追加情報：<ul><li>[ タグクイックスタートガイド ](../../tags/ui/client-side/asynchronous-deployment.md)</li><li>[非同期デプロイメント](../../tags/ui/client-side/asynchronous-deployment.md)</li></ul> |
| ローンチ – 非同期にデプロイする必要があります | 0 | 最適なパフォーマンスを得るには、タグを非同期でデプロイする必要があります。 | インラインスクリプトに `async` パラメーターを含めて、タグが適切に機能するようにします <br><br>[ 追加情報 ](../../tags/ui/client-side/asynchronous-deployment.md) |
| Target - `mboxDefault` のコンテンツ | 0 | `at.js` を使用する場合、コンテンツは `mboxDefault` に存在する必要があります。 | コンテンツが使用可能であることを確認します。 <br><br>[追加情報](https://experienceleague.adobe.com/docs/target/using/implement-target/implementing-target.html?lang=ja) |

{style="table-layout:auto"}
