---
title: アラートテスト参照
description: Adobe Experience Platform Debuggerのアラートに対する監査機能のテスト方法について説明します。
exl-id: ac6f8675-6c34-48b4-b5dd-48e92af217fd
source-git-commit: 36871289743f384207bb149df6e5e1af14d4d371
workflow-type: tm+mt
source-wordcount: '615'
ht-degree: 9%

---

# アラートテストの参照

このリファレンスでは、Adobe Experience Platform Debuggerの監査機能がアラートテストを実行する方法について詳しく説明します。

>[!NOTE]
>
>Experience Platform Debuggerでの監査テストについて詳しくは、[監査機能の概要](./overview.md)を参照してください。

アラートは、認識する必要があるが、スコアには影響しない問題を示します。これらは、ベストプラクティスのレコメンデーションですが、お客様の実装に適用されない場合があります。

| テスト | 重み | 条件 | レコメンデーション |
| --- | --- | --- | --- |
| Adobe Advertising – 正しいコンバージョンタグの実装 | 0 | 正しいコンバージョンタグが使用されているかどうかを確認します。<br><br>**警告**：非推奨のTubeMogul変換タグを使用すると、データが失われる可能性があります。 | コンバージョンピクセルを新しいAdvertising画像のみのコンバージョンタグにアップグレードします。 これは、[Adobe Advertising タグ拡張機能](../../destinations/catalog/advertising/adobe-advertising-cloud.md)を使用すると、最も簡単に実現できます。 |
| Adobe Advertising – 正しいJS タグを使用 | 0 | Advertisingでは、最新のJavaScript タグを使用する必要があります。 | Advertising JavaScriptを最新バージョンにアップグレードします。 非推奨のJavaScript バージョンを使用すると、機能が失われる可能性があります。 これは、[Adobe Advertising タグ拡張機能](../../destinations/catalog/advertising/adobe-advertising-cloud.md)を使用することで、より簡単に実現できます。 |
| Adobe Advertising – 画像のみのタグ | 0 | Advertisingの画像ピクセルフォーマットは、次のいずれかの推奨フォーマットに一致する必要があります。 <ul><li>`http(s)://rtd.tubemogul.com/upi/?sid=<HASH_VALUE>`</li><li>`http(s)://rtd-tm.everesttech.net/upi/?sid=<HASH_VALUE>`</li><li>`http(s)://pixel.everesttech.net/px2/<NUMERIC_ID>?`</li></ul> | Advertising ピクセルを新しいAdvertising画像専用タグにアップグレードすると、Advertisingの全機能を活用できます。 これは、[Adobe Advertising タグ拡張機能](../../destinations/catalog/advertising/adobe-advertising-cloud.md)を使用すると、最も簡単に実現できます。 |
| Adobe Advertising - セグメントピクセル DSPの同期が有効 | 0 | TubeMogul セグメントピクセルにDSP Syncing設定が含まれているかどうかを確認し、この設定をピクセルに追加することをお勧めします。 DSPの同期設定は、クエリ文字列パラメーターの使用によって決まります。 以下に要約を示します。 <ul><li>タグが次のいずれかに対して発行されている場合：<ul><li>`https://rtd.tubemogul.com/upi/?sid=<HASH_VALUE>`</li><li>`http(s)://rtd-tm.everesttech.net/upi/?sid=<HASH_VALUE>`</li><li>`http(s)://pixel.everesttech.net/px2/<NUMERIC_ID>?`</li></ul></li><li>タグにURL パラメーター`sid=`が含まれています</li><li>次に、URL パラメーター`cs=0`または`cs=1`が存在するかどうかを確認します。また、オーディエンスの一致率を向上できるように、これらのピクセルに`cs=1`を追加することをお勧めしない場合は、これらのピクセルに追加します。</li></ul> | Advertising ピクセルにURL パラメーター`cs=1`を追加して、DSPの同期が行われるようにします。これにより、オーディエンスの一致率が向上します。 これは、[Adobe Advertising タグ拡張機能](../../destinations/catalog/advertising/adobe-advertising-cloud.md)を使用することで最も簡単に実現できます。 |
| Experience Cloud ID サービス - 1つのAdobeOrgのみを使用 | 0 | 通常のECID実装では、単一のAdobeOrgを使用する必要があります。 | この実装に複数のAdobeOrg IDが存在することを検証します。 <br><br>[追加情報](https://experienceleague.adobe.com/docs/id-service/using/intro/id-request.html) |
| 起動 – `pageBottom` コールバック プレースメント | 0 | タグを機能させるには、`_satellite.pageBottom()`関数が存在する必要があります。 閉じる`</body>` タグの直前にインラインスクリプトを追加して、適切なDTM機能を確保します。 メモ：タグは`<body>`の最後のタグであることがベストプラクティスです。 `<body>` タグ内で見つかった場合は、機能する可能性がありますが、ベストプラクティスではないため、正しく機能しないか、予期しない結果や望ましくない結果が得られる可能性があります。 | 閉じる`</body>` タグの直前にインラインスクリプトを追加して、適切なDTM機能を確保します。 <br><br>[追加情報](../../tags/ui/client-side/asynchronous-deployment.md) |
| Launch - セルフホスト | 0 | タグライブラリは、`assets.adobedtm.com`でAdobeのAkamai インスタンスでホストされています。 セルフホスティングは、タグの読み込みに最適なアプローチです。キャッシュ制御、サードパーティのスクリプトへの依存関係の軽減、公開プロセスの制御を強化することで、web サイトのパフォーマンスをより詳細に制御できるからです。 タグライブラリは、独自のweb ホスティングまたはCDNを通じてホストおよび管理できます。 | セルフホスティングへの切り替えは、ページにタグを読み込むためのアプローチです。 Akamai CDNを介したホスティングはほとんどの場合に機能しますが、セルフホスティングはページパフォーマンスを向上させます。 <br><br>追加情報：<ul><li>[ タグのクイックスタートガイド ](../../tags/ui/client-side/asynchronous-deployment.md)</li><li>[非同期デプロイメント](../../tags/ui/client-side/asynchronous-deployment.md)</li></ul> |
| Launch – 非同期でデプロイする必要があります | 0 | タグは、パフォーマンスを最適化するために非同期でデプロイする必要があります。 | インラインスクリプトに`async` パラメーターを含めて、適切なタグ機能を確保します<br><br>[追加情報](../../tags/ui/client-side/asynchronous-deployment.md) |
| Target - `mboxDefault`のコンテンツ | 0 | `mboxDefault`を使用する場合、コンテンツは`at.js`に存在する必要があります。 | コンテンツが使用可能であることを確認します。 <br><br>[追加情報](https://experienceleague.adobe.com/docs/target/using/implement-target/implementing-target.html) |

{style="table-layout:auto"}
