---
title: タグプレゼンスのテストリファレンス
description: Adobe Experience Platform Debuggerで監査機能がタグの有無をテストする方法について説明します。
exl-id: 8f01f89e-2a3b-41bc-b971-f3c60d0ae3fa
source-git-commit: 36871289743f384207bb149df6e5e1af14d4d371
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 13%

---

# タグプレゼンスのテストリファレンス

このリファレンスでは、Adobe Experience Platform Debuggerの監査機能がタグの有無をテストする方法について詳しく説明します。

>[!NOTE]
>
>Experience Platform Debuggerでの監査テストについて詳しくは、[監査機能の概要](./overview.md)を参照してください。

タグプレゼンスのテスト特定のタグがページ上に存在するかどうか、ページコード内の適切な場所に存在するかどうかを評価します。

| テスト | 重み | 条件 | レコメンデーション |
| --- | --- | --- | --- |
| Adobe Advertising - コードプレゼンス | 5 | Adobe Advertising タグはDOMでは使用できません。 | [Adobe Advertising タグ拡張機能](../../destinations/catalog/advertising/adobe-advertising-cloud.md)を使用してAdobe Advertising タグを実装します。 |
| Adobe Advertising - セグメントピクセル実装 | 5 | Adobe Advertising セグメントピクセルを新しいAdobe Advertising画像のみのタグにアップグレードします。 非推奨の AMO セグメントタグを使用すると、データが失われる可能性があります。 | [Adobe Advertising タグ拡張機能](../../destinations/catalog/advertising/adobe-advertising-cloud.md)を使用して、Adobe Advertising セグメントピクセルを実装します。 |
| Analytics - DOMに読み込まれています | 5 | Adobe Analytics タグが検出されませんでした。 | 最新バージョンのAnalyticsをインストールします。 <br><br>[追加情報](https://experienceleague.adobe.com/docs/analytics/implementation/home.html?lang=ja) |
| 起動 – ライブラリが読み込まれました | 5 | DOMに`global _satellite` オブジェクトが見つかりませんでした。タグライブラリがインストールされていないか、実行に失敗しています。 | タグライブラリがページに実装されており、後続のスクリプトアクティビティによってブロックされていないことを確認します。 |
| Launch – 複数の埋め込みスクリプトがない | 5 | 実稼動サイトでは、ページごとに1つの埋め込みコードのみを読み込む必要があります。 | 本番稼働ライブラリのみがページに読み込まれていることを確認してください。 |
| 起動 – `pageBottom` コールバックが`<body>`に存在します | 5 | 必要な`_satellite.pageBottom()` コールバックがページの`<body>`内に見つかりませんでした。 このテストは、`pageBottom`呼び出しがページ上にまったく見つからない場合、または`<head>` タグ（またはその他の予期しない場所）に存在する場合に失敗します。 `pageBottom`が`<body>` タグ内のどこかに見つかった場合にのみ渡されます。 | 終了`</body>` タグの直前にインラインスクリプトを追加して、適切なタグ機能を確保します。<br><br>[追加情報](../../tags/ui/client-side/asynchronous-deployment.md) |
| Launch - `pageBottom` コールバックは、非同期でデプロイされたときに存在しません | 5 | `_satellite.pageBottom()` コールバックがページ上に見つかりました。タグが非同期でデプロイされている場合は、この問題は発生しません。 | 適切なタグ機能を有効にするには、`_satellite.pageBottom()` スクリプトを削除します。 <br><br>[追加情報](../../tags/ui/client-side/asynchronous-deployment.md) |
| Experience Cloud ID サービス – コードプレゼンス | 5 | Experience Cloud ID サービスコードが見つかりませんでした。Experience Cloud ID （ECID）の使用は、Experience Cloud ソリューションから最大限の価値を引き出すために強くお勧めし、Experience Cloud ソリューション全体のID管理に不可欠です。 | 最新バージョンのECIDをインストールします。<br><br>[追加情報](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html?lang=ja) |
| Experience Cloud ID サービス - Cookie プレゼンス | 5 | `AMCV_` Cookieが見つかりませんでした。 `VisitorAPI.js` コードから訪問者オブジェクトをインスタンス化する必要があります。 | タグ実装の場合は、AdobeOrg IDがECID ツールに正しく入力されていることを確認します。 <br><br>[追加情報](https://experienceleague.adobe.com/docs/id-service/using/intro/cookies.html?lang=ja) |
| Experience Cloud ID サービス - MID値があります | 5 | MID値が`AMCV_` Cookieに見つかりませんでした。 | ECID APIの遅延を確認するには、もう一度テストします。 この状態が続く場合は、Adobe カスタマーケアにお問い合わせください。 <br><br>[追加情報](https://experienceleague.adobe.com/docs/id-service/using/intro/cookies.html?lang=ja) |
| Target - コードプレゼンス | 5 | Adobe TargetはDOMで定義する必要があります。 | Targetの最新バージョン（at.js）をインストールします。 <br><br>[追加情報](https://experienceleague.adobe.com/docs/target/using/implement-target/implementing-target.html?lang=ja) |
| ターゲット – ライブラリが`<head>`に読み込まれました | 4 | ターゲット ライブラリは、`<head>` タグに読み込む必要があります。 | Target ライブラリが`<head>` タグに読み込まれていることを確認してください。 <br><br>[追加情報](https://experienceleague.adobe.com/docs/target/using/implement-target/implementing-target.html?lang=ja) |

{style="table-layout:auto"}
